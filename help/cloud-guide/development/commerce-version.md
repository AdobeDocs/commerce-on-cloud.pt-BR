---
title: Atualizar versão do Commerce
description: Saiba como atualizar a versão do Adobe Commerce no projeto de infraestrutura em nuvem.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: bcb5b00f7f203b53eae5c1bc1037cdb1837ad473
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---

# Atualizar versão do Commerce

Você pode atualizar a base de código do Adobe Commerce para uma versão mais recente. Antes de atualizar seu projeto, verifique os [requisitos de sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=pt-BR) no guia _Instalação_ para obter os requisitos de versão de software mais recentes.

Dependendo da configuração do seu projeto, suas tarefas de atualização podem incluir o seguinte:

- Atualize o arquivo `.magento/services.yaml` com novas versões para MariaDB (MySQL), OpenSearch, RabbitMQ e Redis para compatibilidade com novas versões do Adobe Commerce.
- Atualize o arquivo `.magento.app.yaml` com novas configurações para ganchos e variáveis de ambiente.
- Atualize extensões de terceiros para a versão mais recente com suporte.

{{upgrade-tip}}

{{pro-update-service}}

## Arquivos de configuração

Antes de atualizar o aplicativo, você deve atualizar os arquivos de configuração do projeto para levar em conta as alterações nas configurações padrão do Adobe Commerce na infraestrutura em nuvem ou no aplicativo. Os padrões mais recentes podem ser encontrados no [repositório GitHub da magento-cloud](https://github.com/magento/magento-cloud).

### composer.json

Antes de atualizar, sempre verifique se as dependências no arquivo `composer.json` são compatíveis com a versão do Adobe Commerce.

Para atualizar o arquivo `composer.json` para o Adobe Commerce versão 2.4.4 e posterior**:

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

1. Defina a [restrição de versão](overview.md#cloud-metapackage) para a versão de atualização de destino. Esta etapa só será necessária se a versão de destino estiver fora da restrição existente.

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Você deve usar a sintaxe de restrição de versão para atualizar o pacote `ece-tools` com êxito. Você pode localizar a restrição de versão no arquivo `composer.json` da versão do [modelo de aplicativo](https://github.com/magento/magento-cloud/blob/master/composer.json) que você está usando para a atualização.

1. Atualize seu arquivo `composer.json` com a versão principal de atualização do Commerce.

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. Se você estiver usando B2B, atualize seu arquivo `composer.json` com a [versão com suporte](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/release/product-availability#adobe-authored-extensions) para Commerce.

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. Atualizar dependências do projeto.

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

### Atualizar extensões

Revise suas páginas de extensão e módulo de terceiros no Marketplace ou outros sites da empresa e verifique o suporte para o Adobe Commerce e o Adobe Commerce na infraestrutura em nuvem. Se for necessário atualizar extensões e módulos de terceiros, a Adobe recomenda trabalhar em uma nova ramificação de integração com as extensões desativadas.

**Para verificar e atualizar suas extensões**:

1. Crie uma ramificação na estação de trabalho local.

1. Desative as extensões conforme necessário.

1. Quando disponível, baixar atualizações de extensão.

1. Instale a atualização conforme documentado pela documentação de terceiros.

1. Ative e teste a extensão.

1. Adicione, confirme e envie as alterações de código para o remoto.

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
