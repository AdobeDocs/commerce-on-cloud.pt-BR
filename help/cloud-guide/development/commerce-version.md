---
title: Atualizar versão do Commerce
description: Saiba como atualizar a versão do Adobe Commerce no projeto de infraestrutura em nuvem.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 0%

---

# Atualizar versão do Commerce

Você pode atualizar a base de código do Adobe Commerce para uma versão mais recente. Antes de atualizar seu projeto, verifique os [requisitos de sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=pt-BR) no guia _Instalação_ para obter os requisitos de versão de software mais recentes.

Dependendo da configuração do seu projeto, suas tarefas de atualização podem incluir o seguinte:

- Update services—such as MariaDB (MySQL), OpenSearch, RabbitMQ, and Redis—for compatibility with new Adobe Commerce versions.
- Converta um arquivo de gerenciamento de configuração mais antigo.
- Atualize o arquivo `.magento.app.yaml` com novas configurações para ganchos e variáveis de ambiente.
- Atualize extensões de terceiros para a versão mais recente com suporte.
- Atualize o arquivo `.gitignore`.

{{upgrade-tip}}

{{pro-update-service}}

## Atualizar a partir de versões anteriores

Se você estiver iniciando uma atualização de uma versão do Commerce anterior à versão 2.1, algumas restrições na base de código do Adobe Commerce poderão afetar sua capacidade de _atualizar_ para uma versão específica das Ferramentas ECE ou de _atualizar_ para a próxima versão do Commerce com suporte. Use a tabela a seguir para determinar o melhor caminho:

| Versão atual | Caminho de atualização |
| ----------------- | ------------ |
| 2.1.3 e anterior | Atualize o Adobe Commerce para a versão 2.1.4 ou posterior antes de continuar. Em seguida, execute uma [atualização única para instalar o ECE-Tools](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [Atualizar pacote ECE-Tools](../dev-tools/update-package.md).<br>Consulte as notas de versão de [2002.0.9](../release-notes/cloud-release-archive.md#v200209) e versões posteriores de 2002.0.x. |
| 2.1.15 - 2.1.16 | [Atualizar pacote ECE-Tools](../dev-tools/update-package.md).<br>Consulte as notas de versão de[2002.0.9](../release-notes/cloud-release-archive.md#v200209) e posterior. |
| 2.2.x e posterior | [Atualizar pacote ECE-Tools](../dev-tools/update-package.md).<br>Consulte as notas de versão de[2002.0.8](../release-notes/cloud-release-archive.md#v200208) e posterior. |

{style="table-layout:auto"}

{{ece-tools-package}}

## Gerenciamento de configuração

Versões anteriores do Adobe Commerce, como 2.1.4 ou posterior a 2.2.x ou posterior, usavam um arquivo `config.local.php` para o Gerenciamento de configuração. O Adobe Commerce versão 2.2.0 e posterior usa o arquivo `config.php`, que funciona exatamente como o arquivo `config.local.php`, mas tem definições de configuração diferentes que incluem uma lista de módulos habilitados e opções de configuração adicionais.

Ao atualizar de uma versão mais antiga, você deve migrar o arquivo `config.local.php` para usar o arquivo `config.php` mais recente. Use as etapas a seguir para fazer backup do arquivo de configuração e criar um.

**Para criar um `config.php` arquivo temporário**:

1. Crie uma cópia do arquivo `config.local.php` e nomeie-a como `config.php`.

1. Adicione este arquivo à pasta `app/etc` do seu projeto.

1. Adicione e confirme o arquivo na ramificação.

1. Envie o arquivo para a ramificação de integração.

1. Continue com o processo de atualização.

>[!WARNING]
>
>Depois da atualização, você pode remover o arquivo `config.php` e gerar um arquivo novo e completo. Você só pode excluir este arquivo para substituí-lo desta vez. Após gerar um novo arquivo `config.php` completo, você não pode excluir o arquivo para gerar um novo. Consulte [Gerenciamento de configuração e implantação de pipeline](../store/store-settings.md).

### Verificar dependências do Zend Framework Composer

Ao atualizar de 2.2.x **para** 2.3.x ou posterior, verifique se as dependências do Zend Framework foram adicionadas à propriedade `autoload` do arquivo `composer.json` para dar suporte ao Laminas. Este plug-in suporta novos requisitos para o Zend Framework, que migrou para o projeto Laminas. Consulte [Migration of Zend Framework to the Laminas Project](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) no _Magento DevBlog_.

**Para verificar a `auto-load:psr-4` configuração**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Confira a ramificação de integração.

1. Abra o arquivo `composer.json` em um editor de texto.

1. Verifique a seção `autoload:psr-4` para a implementação do gerenciador de plug-in Zend para dependência de controladores.

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Se a dependência Zend estiver ausente, atualize o arquivo `composer.json`:

   - Adicione a seguinte linha à seção `autoload:psr-4`.

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - Atualize as dependências do projeto.

     ```bash
     composer update
     ```

   - Adicionar, confirmar e enviar alterações de código.

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - Mescle as alterações no ambiente de preparo e, em seguida, na produção.

## Arquivos de configuração

Antes de atualizar o aplicativo, você deve atualizar os arquivos de configuração do projeto para levar em conta as alterações nas configurações padrão do Adobe Commerce na infraestrutura em nuvem ou no aplicativo. Os padrões mais recentes podem ser encontrados no [repositório GitHub da magento-cloud](https://github.com/magento/magento-cloud).

### .magento.app.yaml

Sempre examine os valores contidos no arquivo [.magento.app.yaml](../application/configure-app-yaml.md) para a sua versão instalada, pois ele controla a maneira como o aplicativo é criado e implantado na infraestrutura de nuvem. O exemplo a seguir é para a versão 2.4.8 e usa o Composer 2.8.4. A propriedade `build: flavor:` não é usada para o Composer 2.x; consulte [Instalando e usando o Composer 2](../application/properties.md#installing-and-using-composer-2).

**Para atualizar o `.magento.app.yaml` arquivo**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Abra e edite o arquivo `magento.app.yaml`.

1. Atualize as opções do PHP.

   ```yaml
   type: php:8.4
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.8.4'
   ```

1. Modifique os comandos `build` e `deploy` da propriedade `hooks`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Adicione as variáveis de ambiente a seguir ao final do arquivo.

   Para o Adobe Commerce 2.2.x a 2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   Para o Adobe Commerce 2.4.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. Salve o arquivo. Não confirme ou envie alterações para o ambiente remoto ainda.

1. Continue com o processo de atualização.

### composer.json

Antes de atualizar, sempre verifique se as dependências no arquivo `composer.json` são compatíveis com a versão do Adobe Commerce.

**Para atualizar o arquivo `composer.json` para o Adobe Commerce versão 2.4.4 e posterior**:

1. Adicionar o seguinte `allow-plugins` à seção `config`:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Adicionar o seguinte plug-in à seção `require`:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Adicionar o seguinte componente à seção `extra:component_paths`:

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Salve o arquivo. Não confirme ou envie alterações para sua ramificação ainda.

1. Continue com o processo de atualização.

## Backup do projeto

Recomendamos criar um backup do seu projeto antes de uma atualização. Use as etapas a seguir para fazer backup dos ambientes de integração, de preparo e de produção.

**Para fazer backup do banco de dados e do código do ambiente de integração**:

1. Crie um backup local do banco de dados remoto.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >O comando `magento-cloud db:dump` executa o comando [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) com o sinalizador `--single-transaction`, que permite fazer backup do banco de dados sem bloquear as tabelas.

1. Faça backup do código e da mídia.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Opcionalmente, você pode omitir `[--media]` se tiver um grande número de arquivos estáticos que já estão no controle do código-fonte.

**Para fazer backup do banco de dados do ambiente de Preparo ou Produção antes de implantar**:

1. Use o SSH para fazer logon no ambiente remoto.

1. Criar um [despejo de banco de dados](../storage/database-dump.md). Para escolher um diretório de destino para o despejo do banco de dados, use a opção `--dump-directory`.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   A operação de despejo cria um arquivo morto `dump-<timestamp>.sql.gz` no diretório do projeto remoto. Consulte [Fazer backup do banco de dados](../storage/database-dump.md).

## Atualização de aplicativo

Examine as informações das [versões de serviço](../services/services-yaml.md#service-versions) para obter os requisitos de versão de software mais recentes antes de atualizar seu aplicativo.

**Para atualizar a versão do aplicativo**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Defina a versão de atualização usando a [sintaxe de restrição de versão](overview.md#cloud-metapackage).

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Você deve usar a sintaxe de restrição de versão para atualizar o pacote `ece-tools` com êxito. Você pode localizar a restrição de versão no arquivo `composer.json` da versão do [modelo de aplicativo](https://github.com/magento/magento-cloud/blob/master/composer.json) que você está usando para a atualização.

1. Atualize o projeto.

   ```bash
   composer update
   ```

1. Revise os patches atualmente aplicados:

   - Se houver patches instalados no diretório `m2-hotfixes`, [envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) e trabalhe com o Suporte da Adobe Commerce para verificar quais patches ainda podem ser aplicados à nova versão. Remova os patches não aplicáveis do diretório `m2-hotfixes`.

   - Se houver [Patches de Qualidade] aplicados no arquivo `.magento.env.yaml`, verifique se eles ainda podem ser aplicados à nova versão. Remova os patches não aplicáveis da seção `QUALITY_PATCHES` do arquivo `.magento.env.yaml`.

   **Método 1**: [Verifique as versões aplicáveis nas notas de versão de Patches de Qualidade](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Método 2**: [Exibir patches e status disponíveis](https://experienceleague.adobe.com/pt-br/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Método 3**: [Pesquisar patches](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=pt-BR)


1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` é necessário para adicionar todos os arquivos alterados ao controle do código-fonte devido à forma como o Composer realiza marshaling nos pacotes base. Os arquivos de marshaling de `composer install` e `composer update` do pacote base (`magento/magento2-base` e `magento/magento2-ee-base`) para a raiz do pacote.

   Os arquivos que o Composer empacota pertencem à nova versão do Adobe Commerce, para substituir a versão desatualizada desses mesmos arquivos. Atualmente, o empacotamento está desativado no Adobe Commerce, portanto, você deve adicionar os arquivos empacotados ao controle do código-fonte.

1. Aguarde a conclusão da implantação.

1. Verifique a atualização em seu ambiente de integração, preparo ou produção usando SSH para fazer logon e verificar a versão.

   ```bash
   php bin/magento --version
   ```

### Criar um arquivo config.php

Como mencionado em [Gerenciamento de configuração](#configuration-management), após a atualização, você deve criar um arquivo `config.php` atualizado. Conclua todas as alterações adicionais de configuração por meio do Administrador no ambiente de integração.

**Para criar um arquivo de configuração específico do sistema**:

1. No terminal, use um comando SSH para gerar o arquivo `/app/etc/config.php` para o ambiente.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   Por exemplo, para Pro, para executar o `scd-dump` na ramificação `integration`:

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. Transfira o arquivo `config.php` para suas estações de trabalho locais usando `rsync` ou `scp`. Você só pode adicionar este arquivo à ramificação localmente.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   Isso gera um arquivo `/app/etc/config.php` atualizado com uma lista de módulos e definições de configuração.

>[!WARNING]
>
>Para uma atualização, você exclui o arquivo `config.php`. Depois que este arquivo for adicionado ao seu código, você deve **não** excluí-lo. Se precisar remover ou editar configurações, edite o arquivo manualmente.

### Atualizar extensões

Revise suas páginas de extensão e módulo de terceiros no Marketplace ou outros sites da empresa e verifique o suporte para o Adobe Commerce e o Adobe Commerce na infraestrutura em nuvem. Se for necessário atualizar extensões e módulos de terceiros, a Adobe recomenda trabalhar em uma nova ramificação de integração com as extensões desativadas.

**Para verificar e atualizar suas extensões**:

1. Crie uma ramificação na estação de trabalho local.

1. Desative as extensões conforme necessário.

1. Quando disponível, baixar atualizações de extensão.

1. Instale a atualização conforme documentado pela documentação de terceiros.

1. Enable and test the extension.

1. Add, commit, and push the code changes to the remote.

1. Encaminhar e testar no ambiente de integração.

1. Encaminhar para o ambiente de preparo para testar em um ambiente de pré-produção.

A Adobe recomenda que você atualize seu ambiente de Produção _antes_, incluindo as extensões atualizadas em seu processo de inicialização do site.

>[!NOTE]
>
>Quando você atualiza a versão do seu aplicativo, o processo de atualização atualiza para a versão mais recente do [módulo CDN Fastly](../cdn/fastly.md#fastly-cdn-module-for-magento-2) automaticamente.

## Solução de problemas de atualização

Se a atualização falhar, você receberá uma mensagem de erro no navegador indicando que não é possível acessar sua loja ou o painel Administrador:

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**Para resolver o erro**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Abra o arquivo `./app/var/report/<error number>`.

1. [Examine os logs](../test/log-locations.md) e determine a origem do problema.

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
