---
title: Gerenciamento de configuração de armazenamento
description: Saiba como gerenciar e sincronizar configurações de armazenamento em todos os ambientes de infraestrutura em nuvem do Adobe Commerce.
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Gerenciamento de configuração de armazenamento

As configurações padrão da loja são armazenadas em um `config.xml` para o módulo apropriado. Quando você altera as configurações no Commerce Admin ou no comando `bin/magento config:set` da CLI, as alterações são refletidas no banco de dados principal, especificamente na tabela `core_config_data`. Essas configurações substituem as configurações padrão armazenadas no arquivo `config.xml`.

As configurações de armazenamento, que se referem às configurações na seção Admin **Lojas** > **Configurações** > **Configuração**, são armazenadas nos arquivos de configuração de implantação com base no tipo de configuração:

- `app/etc/config.php` — definições de configuração para armazenamentos, sites, módulos ou extensões, otimização de arquivo estático e valores de sistema relacionados à implantação de conteúdo estático. Consulte a [referência do config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) no _Guia de Configuração_.
- `app/etc/env.php` — valores para substituições específicas do sistema e configurações confidenciais que deveriam _NÃO_ ser armazenadas no controle de origem. Consulte a [referência do arquivo env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) no _Guia de Configuração_.

>[!NOTE]
>
>Como a infraestrutura do Adobe Commerce na nuvem oferece suporte apenas aos modos de produção e manutenção, a seção **Avançado** > **Desenvolvedor** não está acessível no Administrador. Você deve ter [privilégios de Administrador do ambiente](../project/user-access.md) para concluir tarefas de gerenciamento de configuração. Você pode definir configurações adicionais usando [variáveis de ambiente](../environment/configure-env-yaml.md).

O gerenciamento de configurações fornece uma maneira de implantar configurações de armazenamento consistentes em seus ambientes com tempo de inatividade mínimo usando a implantação de pipeline. O projeto de infraestrutura do Adobe Commerce na nuvem inclui o servidor de compilação, scripts de compilação e implantação e ambientes de implantação criados com a [estratégia de implantação de pipeline](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) em mente.

## Esquema de substituição de configuração

Todas as configurações do sistema são definidas durante as fases de criação e implantação de acordo com o seguinte esquema de substituição:

1. Se uma variável de ambiente existir, use a configuração personalizada e ignore a configuração padrão.
1. Se uma variável de ambiente não existir, use a configuração de um par de nome-valor `MAGENTO_CLOUD_RELATIONSHIPS` no arquivo [`.magento.app.yaml` ](../application/configure-app-yaml.md). Ignorar a configuração padrão.
1. Se uma variável de ambiente não existir e `MAGENTO_CLOUD_RELATIONSHIPS` não contiver um par nome-valor, remova todas as configurações personalizadas e use os valores da configuração padrão.

Em resumo, as variáveis de ambiente substituem todos os outros valores.

>[!TIP]
>
>Consulte [Gerenciamento de configuração](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) no _Guia de configuração_ para obter mais informações sobre o esquema de substituição para implantação de pipeline.

Se a mesma configuração for definida em vários locais, a aplicação dependerá da seguinte hierarquia de configuração para determinar qual valor aplicar ao ambiente:

| Prioridade | Método de configuração<br> | Descrição |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>variáveis de ambiente | Valores adicionados da guia _Variáveis_ da configuração de ambiente em [!DNL Cloud Console]. Especifique valores aqui para configurações confidenciais ou específicas do ambiente. As configurações especificadas aqui não podem ser editadas no Administrador. Consulte [Variáveis de configuração de ambiente](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Valores adicionados na seção `variables` do arquivo `.magento.app.yaml`. Especifique valores aqui para garantir uma configuração consistente em todos os ambientes. **Não especifique valores confidenciais no arquivo `.magento.app.yaml`.** Consulte [Configurações do aplicativo](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | Os valores de configuração específicos do ambiente armazenados aqui são adicionados com o comando `app:config:dump`. Defina os valores específicos do sistema e confidenciais usando variáveis de ambiente ou a CLI. Consulte [Dados confidenciais](#sensitive-data). O arquivo `env.php` é **não** incluído no controle do código-fonte. |
| 4 | `app/etc/config.php` | Os valores armazenados aqui são adicionados usando o comando `app:config:dump`. Os valores de configuração compartilhados são adicionados a `config.php`. Defina a configuração compartilhada pelo Admin ou usando a CLI. O arquivo `config.php` está incluído no controle do código-fonte. |
| 5 | Banco de dados | Os valores armazenados aqui são adicionados definindo configurações no Administrador. As configurações definidas usando qualquer um dos métodos anteriores são bloqueadas (esmaecidas) e não podem ser editadas pelo administrador. |
| 6 | `config.xml` | Muitas configurações têm valores padrão definidos no arquivo `config.xml` para um módulo. Se o Adobe Commerce não conseguir encontrar nenhum valor definido por nenhum dos métodos anteriores, ele voltará ao valor padrão, se definido. |

{style="table-layout:auto"}

## Despejo de configuração

Você pode usar o seguinte comando `ece-tools` para gerar um arquivo `config.php` que contenha todas as configurações de armazenamento atuais:

```bash
./vendor/bin/ece-tools config:dump
```

Os dados &quot;despejados&quot; para o arquivo `app/etc/config.php` ficam _bloqueados_, o que significa que o campo correspondente no Administrador do Commerce torna-se **somente leitura**. O arquivo `config.php` inclui apenas as configurações que você define. Ela não bloqueia os valores padrão. Bloquear apenas os valores atualizados também garante que todas as extensões usadas nos ambientes de Preparo e Produção não sejam interrompidas devido a configurações somente leitura, especialmente o Fastly.

>[!WARNING]
>
>O comando `ece-tools config:dump` não recupera configurações detalhadas para módulos, como B2B. Se você precisar de um despejo de configuração abrangente, use o comando `app:config:dump`, mas esse comando bloqueará os valores de configuração em um estado somente leitura.

### Dados sensíveis

Todas as configurações confidenciais são exportadas para o arquivo `app/etc/env.php` quando você usa o comando `bin/magento app:config:dump`. Você pode definir valores confidenciais usando o comando da CLI: `bin/magento config:sensitive:set`. Consulte [Configurações sensíveis e específicas do ambiente](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) no guia _Commerce PHP Extensions_ para saber como designar configurações sensíveis ou específicas do sistema.

Consulte uma lista de [Configurações sensíveis ou específicas do sistema](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) no _Guia de Configuração_.

### Desempenho do SCD

Dependendo do tamanho do armazenamento, você pode ter um grande número de arquivos de conteúdo estático para implantar. Normalmente, o conteúdo estático é implantado durante a fase de implantação, quando o aplicativo está no modo de manutenção. A configuração mais ideal é gerar conteúdo estático durante a fase de criação. Consulte [Escolha de uma estratégia de implantação](../deploy/static-content.md).

Se você tiver ativado o Gerenciamento de configurações após despejar as configurações, mova as variáveis SCD_* do estágio de implantação para o estágio de criação para ativar adequadamente a geração de conteúdo estático durante a fase de criação. Consulte [Variáveis de ambiente](../environment/configure-env-yaml.md#environment-variables).

**Antes do Gerenciamento de Configuração**:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

**Depois de habilitar o Gerenciamento de Configuração**:

Mova as variáveis SCD_* para o estágio de criação:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```

>[!NOTE]
>
>Antes de implantar arquivos estáticos, as fases de criação e implantação compactam o conteúdo estático usando GZIP. A compactação de arquivos estáticos reduz as cargas do servidor e aumenta o desempenho do site. Consulte [opções de compilação](../environment/variables-build.md) para saber mais sobre como personalizar ou desabilitar a compactação de arquivos.

## Procedimento para gerenciar suas configurações

A seguir, uma visão geral de alto nível desse processo:

![Visão geral do gerenciamento de configuração de Início](../../assets/starter/configuration-management-flow.png)

**Para configurar seu armazenamento e gerar um arquivo de configuração**:

1. Conclua todas as configurações das lojas no Administrador para um dos ambientes:

   - Início: uma ramificação de desenvolvimento ativa
   - Pro: uma filial ativa no ambiente de integração

   Essas configurações não incluem os produtos reais, a menos que você planeje despejar o banco de dados desse ambiente para ambientes de preparo e produção. Normalmente, os bancos de dados de desenvolvimento não incluem os dados completos da loja.

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Criar um dump local do banco de dados remoto.

   ```bash
   magento-cloud db:dump
   ```

1. Adicionar, confirmar e enviar alterações de código para atualizar um ambiente remoto.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

Depois que a implantação for concluída, faça logon no Admin do ambiente atualizado para verificar as configurações. Continue a mesclar todas as configurações adicionais com os ambientes de preparo e produção, conforme necessário.

### Atualizar configurações

Quando você modifica seu ambiente por meio do Administrador e executa o comando novamente, novas configurações são anexadas ao código no arquivo `config.php`.

>[!WARNING]
>
>Embora você possa editar manualmente o arquivo `config.php` nos ambientes de Preparo e Produção, ele **não** é recomendado. O arquivo ajuda a manter todas as configurações consistentes em todos os ambientes. Nunca exclua o arquivo `config.php` para recriá-lo. A exclusão do arquivo pode remover configurações específicas necessárias para processos de compilação e implantação.

### Restaurar arquivos de configuração

Cópias dos arquivos `app/etc/env.php` e `app/etc/config.php` originais foram criadas durante o processo de implantação e armazenadas na mesma pasta. O código a seguir mostra o BAK (arquivos de backup) e o PHP (arquivos originais) na mesma pasta `app/etc`:

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

Configurações mais antigas usaram o arquivo `app/etc/config.local.php`. Consulte [Migrar configurações mais antigas](#migrate-older-configurations).

**Para restaurar os arquivos de configuração**:

1. Na estação de trabalho local, use o SSH para fazer logon no projeto e no ambiente remotos.

   ```bash
   magento-cloud ssh
   ```

1. Verifique o local e a disponibilidade dos arquivos de backup.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Exemplo de resposta:

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Restaurar arquivos de backup.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migrar configurações mais antigas

Se você atualizar para o Adobe Commerce na infraestrutura na nuvem 2.2 ou posterior, talvez queira migrar as configurações do arquivo `config.local.php` para o novo arquivo `config.php`. Se as configurações no Administrador corresponderem ao conteúdo do arquivo, siga as instruções para gerar e adicionar o arquivo `config.php`.

Se forem diferentes, você poderá anexar o conteúdo do arquivo `config.local.php` ao novo arquivo `config.php`:

1. Siga as instruções para gerar o arquivo `config.php`.

1. Abra o arquivo `config.php` e exclua a última linha.

1. Abra o arquivo `config.local.php` e copie o conteúdo.

1. Cole o conteúdo no arquivo `config.php`, salve-o e conclua a adição ao Git.

1. Implante em seus ambientes.

Você só conclui essa migração uma vez. Após a migração, use o arquivo `config.php`.

### Alterar localidades

Você pode alterar as localidades de armazenamento sem seguir um processo complexo de importação e exportação de configuração, _se_ você tiver o [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) habilitado. Você pode atualizar as localidades usando o Administrador.

Você pode adicionar outra localidade ao ambiente de preparo ou produção habilitando `SCD_ON_DEMAND` em uma ramificação de integração, gerar um arquivo `config.php` atualizado com as novas informações de localidade e copiar o arquivo de configuração para o ambiente de destino.

>[!WARNING]
>
>Este processo **substitui** a configuração de armazenamento; faça o seguinte somente se os ambientes contiverem os mesmos armazenamentos.

1. No ambiente de integração, habilite a variável `SCD_ON_DEMAND` usando o [`.magento.env.yaml` arquivo](../environment/configure-env-yaml.md).

1. Adicione os locais necessários usando o Administrador.

1. Use SSH para fazer logon no ambiente remoto e gerar o arquivo `/app/etc/config.php` contendo todas as localidades.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Copie o novo arquivo de configuração do ambiente de integração remota para o diretório do ambiente local.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Adicionar, confirmar e enviar alterações de código para atualizar um ambiente remoto.
