---
title: Exemplo de gerenciamento de configurações específicas do sistema
description: Veja um exemplo de como gerenciar e sincronizar as configurações de armazenamento em todos os ambientes do Adobe Commerce na infraestrutura em nuvem.
hidefromtoc: true
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---


# Exemplo de gerenciamento de configurações específicas do sistema

Este exemplo mostra como usar o gerenciamento de configurações para manter as configurações de armazenamento consistentes em todos os ambientes.

O exemplo usa o seguinte procedimento definido em [Configurações de armazenamento](store-settings.md):

1. Insira suas configurações no Admin do armazenamento do ambiente de integração.
1. Crie um arquivo `config.php` e transfira-o para sua estação de trabalho local.
1. Encaminhar `config.php` para o ambiente de integração remota.
1. Verifique se as suas configurações não são editáveis no Admin.
1. Faça as modificações necessárias:

   * Altere as definições de configuração no ambiente de integração.
   * Para adicionar configurações, execute o comando para criar `config.php` novamente. Novas configurações são anexadas ao arquivo.
   * Para remover ou editar configurações existentes, edite manualmente o arquivo.
   * Confirmar e enviar.

Por exemplo, talvez você queira definir as seguintes configurações:

* Desative as configurações de otimização de local e arquivo estático no ambiente de integração
* Permitir otimização de arquivos estáticos em ambientes de preparo e produção
* Configure o Fastly no Preparo e na Produção com credenciais específicas para cada

_Otimização de arquivo estático_ significa mesclar e minificar folhas de estilos em cascata e JavaScript e minificar modelos de HTML. Consulte [Estratégias de implantação de conteúdo estático](../deploy/static-content.md).

## Pré-requisitos

Para concluir essas tarefas de gerenciamento de configuração, você precisa do seguinte:

* Função de leitor de projeto com [privilégios de &quot;administrador&quot;](../project/user-access.md) de ambiente
* URL do administrador e credenciais para ambientes de integração, preparo e produção

## Configurar o administrador do Commerce

No ambiente de integração, é possível fazer logon no Administrador para modificar as configurações do sistema para lojas, sites, módulos ou extensões, otimização de arquivos estáticos e valores do sistema relacionados à implantação de conteúdo estático. Consulte [Dados de configuração](store-settings.md#scd-performance).

**Para alterar as configurações de otimização de local e arquivo estático**:

1. Faça logon no Admin do ambiente de integração. Você pode acessar esta URL por meio do [[!DNL Cloud Console]](../project/overview.md).
1. Navegue até **Lojas** > Configurações > **Configuração** > Geral > **Geral**.
1. Na navegação da página, expanda **Opções de Local**.
1. Na lista **Localidade**, altere a localidade. Você pode alterá-la novamente mais tarde.

   ![Alterar localidade](../../assets/locale-options.png)

1. Clique em **Salvar configuração**.
1. Se solicitado, [limpe o cache](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/cache-management).
1. Faça logout do Administrador.

## Exportar valores e transferir config.php para o seu sistema local

Esta etapa cria e transfere o arquivo de configuração do `config.php` no ambiente de integração usando um comando executado na máquina local.

Este procedimento corresponde à etapa 2 no [procedimento recomendado](store-settings.md). Depois de criar `config.php`, transfira-o para o sistema local para poder adicioná-lo ao Git.

**Para criar e transferir`config.php`**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Alterar para o ambiente de integração.

   ```bash
   magento-cloud environment:checkout integration
   ```

1. Criar um dump local do banco de dados remoto.

   ```bash
   magento-cloud db:dump
   ```

O trecho a seguir de `config.php` mostra um exemplo de alteração da localidade padrão para `en_GB` e das configurações de otimização de arquivo estático:

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## Enviar e implantar config.php em ambientes

Agora que você criou o `config.php` e o transferiu para o sistema local, confirme-o no Git e envie-o por push para seus ambientes. Este procedimento corresponde às etapas 3 e 4 no [procedimento recomendado](store-settings.md).

O comando a seguir adiciona, confirma e envia para a ramificação `master`:

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

Implantação completa de código para preparo e produção. Para começar, você envia por push para `staging` e `master` ramificações. Para obter detalhes sobre comandos de implantação, consulte [Implantar armazenamento](../deploy/staging-production.md).

Aguarde a conclusão da implantação em todos os ambientes.

## Verifique as alterações de configuração

Depois de enviar `config.php` para seus ambientes por push, todos os valores alterados devem ser somente leitura no Administrador. Neste exemplo, as configurações modificadas de localidade padrão e otimização de arquivo estático não devem ser editáveis no Admin. Estas configurações estão definidas em `config.php`.

Para verificar as alterações de configuração:

1. Faça logout do Administrador em um dos ambientes.
1. Faça logon novamente no Administrador.
1. Clique em **Lojas** > Configurações > **Configuração** > Geral > **Geral**.
1. No painel direito, expanda **Opções de Local**.

   Observe que vários campos não podem ser editados, como mostrado no exemplo a seguir. Estas configurações são mantidas por `config.php`.

   ![Determinados valores não são mais editáveis no Administrador](../../assets/locale-options-disabled.png)

1. Faça logout do Administrador.

## Alterar e atualizar definições de configuração específicas do sistema

Se você precisar modificar qualquer uma dessas configurações, modifique o arquivo `config.php` manualmente com um editor de texto. Depois de concluir as edições ou remoções, você pode confirmar e enviar para o ambiente remoto seguindo as etapas anteriores.

Para adicionar configurações, modifique o ambiente de integração e execute o comando novamente para gerar o arquivo. Todas as novas configurações são anexadas ao código no arquivo. Encaminhe-o para o Git seguindo as etapas anteriores.

Para este exemplo, modifique as configurações de otimização de arquivo estático e adicione uma nova configuração para o JavaScript.

### Adicionar configurações na integração

Para adicionar valores de configuração no Admin do ambiente de integração. Este exemplo mescla arquivos do JavaScript.

1. Faça logoff do Administrador de integração.
1. Faça logon novamente no Administrador de integração.
1. Clique em **Lojas** > Configurações > **Configuração** > **Avançado** > **Desenvolvedor**.
1. No painel direito, expanda **Configurações do JavaScript**.
1. Na lista **Mesclar arquivos do JavaScript**, clique em **Sim**.
1. Clique em **Salvar configuração**.
1. Se solicitado, [limpe o cache](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/cache-management).
1. Faça logout do Administrador.

Ao executar o comando dump novamente, a nova configuração é anexada ao arquivo.

```bash
magento-cloud db:dump
```

### Editar config.php com novas configurações

No local, use um editor de texto para editar o arquivo `app/etc/config.php` atualizado. Edite essas configurações para ativar a minificação para arquivos JavaScript, HTML e CSS.

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

Para modificar as configurações para permitir minificação, edite de `'0'` a `'1'` para `'minify_html'` e cada opção `'minify_files'`:

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

Salve as alterações no arquivo.

### Enviar as alterações para o Git

Para enviar suas alterações, insira o seguinte:

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

Aguarde a conclusão da implantação.

Repita o processo de implantação para enviar o código para todos os ambientes.
