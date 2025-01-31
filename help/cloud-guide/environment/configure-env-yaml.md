---
title: Configurar ambiente
description: Saiba como configurar ações de criação e implantação em toda a Commerce em ambientes de infraestrutura em nuvem, incluindo preparo e produção profissionais, usando variáveis de ambiente.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Configurar variáveis de ambiente para implantação

O arquivo `.magento.env.yaml` usa variáveis de ambiente para centralizar o gerenciamento de ações de compilação e implantação em todos os seus ambientes, incluindo o armazenamento temporário e a produção. Para configurar ações exclusivas em cada ambiente, você deve modificar esse arquivo em cada ambiente.

>[!TIP]
>
>Os arquivos YAML diferenciam maiúsculas de minúsculas e não permitem tabulações. Tenha cuidado para usar recuo consistente em todo o arquivo `.magento.env.yaml` ou sua configuração pode não funcionar como esperado. Os exemplos na documentação e no arquivo de exemplo usam o recuo _two-space_. Use o [comando de validação de ferramentas ece](#validate-configuration-file) para verificar sua configuração.

## Estrutura de arquivo

O arquivo `.magento.env.yaml` contém duas seções: `stage` e `log`. A seção `stage` controla as ações que ocorrem durante as fases do [processo de implantação da nuvem](../deploy/process.md).

- `stage`—Use a seção de preparo para definir determinadas ações para os seguintes estágios de implantação:
   - `global` — Controla ações nas fases de compilação, implantação e pós-implantação. Você pode substituir essas configurações nas seções de criação, implantação e pós-implantação.
   - `build` — Controla ações somente na fase de compilação. Se você não especificar configurações nesta seção, a fase de criação usará configurações da seção global.
   - `deploy` — Controla ações somente na fase de implantação. Se você não especificar configurações nesta seção, a fase de implantação usará configurações da seção global.
   - `post-deploy` — Controla as ações _após_ a implantação do aplicativo e _após_ o contêiner começa a aceitar conexões.
- `log`—Use a seção de log para configurar [notificações](set-up-notifications.md), incluindo tipos de notificação e nível de detalhes.
   - `slack` — Configurar uma mensagem para enviar a um bot de Slack.
   - `email` — Configure um email para enviar a um ou mais destinatários de email.
   - [manipuladores de log](log-handlers.md) — Configure as mensagens de aplicativos de hardware e software enviadas a um servidor de log remoto.

### Variáveis de ambiente

O pacote `ece-tools` define valores no arquivo `env.php` com base nos valores das [Variáveis de nuvem](variables-cloud.md), variáveis definidas no [!DNL Cloud Console] e no arquivo de configuração `.magento.env.yaml`. As variáveis de ambiente no arquivo `.magento.env.yaml` personalizam o ambiente de nuvem, substituindo a configuração existente do Commerce. Se um valor padrão for `Not Set`, o pacote `ece-tools` executará a ação **NO** e usará o valor padrão [!DNL Commerce] ou o valor da configuração MAGENTO_CLOUD_RELATIONSHIPS. Se o valor padrão estiver definido, o pacote `ece-tools` atuará para definir esse padrão.

Os tópicos a seguir contêm definições detalhadas de todas as variáveis que podem ser usadas no arquivo `.magento.env.yaml`, como se um valor padrão esteja definido ou não:

- [Global](variables-global.md)—as variáveis controlam ações em cada fase: compilação, implantação e pós-implantação
- [Build](variables-build.md) — as variáveis controlam ações de compilação
- [Implantar](variables-deploy.md) — variáveis controlam ações de implantação
- [Pós-implantação](variables-post-deploy.md)—as variáveis controlam ações após a implantação

### Criar arquivo de configuração da CLI

Você pode gerar um arquivo de configuração `.magento.env.yaml` para um ambiente em nuvem usando os seguintes comandos `ece-tools`.

>Cria um arquivo de configuração

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Atualizar valores no arquivo de configuração

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Ambos os comandos exigem um único argumento: uma matriz formatada em JSON que especifica um valor para pelo menos uma variável de build, implantação ou pós-implantação. Por exemplo, o comando a seguir define valores para as variáveis `SCD_THREADS` e `CLEAN_STATIC_FILES`:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

E cria um arquivo `.magento.env.yaml` com as seguintes configurações:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

Você pode usar o comando `cloud:config:update` para atualizar o novo arquivo. Por exemplo, o comando a seguir altera o valor `SCD_THREADS` e adiciona a configuração `SCD_COMPRESSION_TIMEOUT`:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

O arquivo atualizado contém a seguinte configuração:

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Validar arquivo de configuração

Use o seguinte comando `ece-tools` para validar o arquivo de configuração `.magento.env.yaml` antes de enviar as alterações para o ambiente de Nuvem remoto.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

O exemplo de resposta a seguir fornece uma lista de itens que devem ser corrigidos:

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## Constantes de PHP

Você pode usar constantes PHP em definições de arquivo `.magento.env.yaml` ao invés de codificar valores. O exemplo a seguir define o `driver_options` usando uma constante PHP:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>A análise constante não funciona ao usar uma versão do pacote `symfony/yaml` anterior à 3.2.

## Tratamento de erros

Quando ocorrer uma falha devido a um valor inesperado no arquivo de configuração `.magento.env.yaml`, você receberá uma mensagem de erro. Por exemplo, a seguinte mensagem de erro apresenta uma lista de alterações sugeridas para cada item com um valor inesperado, às vezes fornecendo opções válidas:

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Faça as correções, confirme e envie as alterações. Se você não receber uma mensagem de erro, as alterações no arquivo de configuração passarão na validação.

## Otimização do gerenciamento de configuração

Se você tiver ativado o Gerenciamento de configurações após despejar as configurações, mova as variáveis SCD_* da implantação para o estágio de criação. Consulte [Estratégias de implantação de conteúdo estático](../deploy/static-content.md).

>Antes do gerenciamento de configuração:

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

>Depois de habilitar o Gerenciamento de configuração, mova as variáveis SCD_* para o estágio de criação:

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
