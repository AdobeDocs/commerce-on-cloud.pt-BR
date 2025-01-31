---
title: Atualizar projeto para usar ECE-Tools
description: Saiba como atualizar seu projeto Adobe Commerce na infraestrutura em nuvem para usar o pacote ECE-Tools e aproveitar as correções e os recursos mais recentes.
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Atualizar projeto para usar o pacote ECE-Tools

O Adobe substituiu os pacotes `magento/magento-cloud-configuration` e `magento/ece-patches` em favor do pacote `ece-tools`, o que simplifica muitos processos de nuvem. Se você usar um projeto mais antigo do Adobe Commerce na infraestrutura em nuvem que contém _não_ o pacote `ece-tools`, será necessário executar um processo único e manual de _atualização_ para o seu projeto.

>[!WARNING]
>
>Se o seu projeto contiver o pacote `ece-tools`, você poderá ignorar a atualização a seguir. Para verificar, recupere a versão [!DNL Commerce] usando o comando `php vendor/bin/ece-tools -V` no diretório raiz do projeto local.

Este processo de atualização de projeto requer que você atualize a restrição de versão `magento/magento-cloud-metapackage` no arquivo `composer.json`, no diretório raiz. Essa restrição permite atualizações para o Adobe Commerce em metapackages de infraestrutura em nuvem, incluindo a remoção de pacotes obsoletos, sem atualizar a versão atual do Adobe Commerce.

{{upgrade-tip}}

## Remover pacotes obsoletos

Antes de executar uma atualização para usar o pacote `ece-tools`, verifique o arquivo `composer.lock` quanto aos seguintes pacotes obsoletos:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Atualizar o metapackage

Cada versão do Adobe Commerce exige uma restrição diferente com base no seguinte:

```
>=current_version <next_version
```

- Para `current_version`, especifique a versão do Adobe Commerce a ser instalada.
- Para `next_version`, especifique a próxima versão de patch após o valor especificado em `current_version`.

Para instalar o Adobe Commerce `2.3.5-p2`, defina `current_version` como `2.3.5` e `next_version` como `2.3.6`. A restrição `">=2.3.5 <2.3.6"` instala o pacote disponível mais recente para a versão 2.3.5.

Você sempre pode encontrar a restrição do metapackage mais recente no [`magento-cloud` modelo](https://github.com/magento/magento-cloud/blob/master/composer.json).

O exemplo a seguir coloca uma restrição para o metapackage do Adobe Commerce na infraestrutura em nuvem para qualquer versão maior ou igual à versão atual 2.4.7 e anterior à próxima versão 2.4.8:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Atualizar o projeto

Para atualizar seu projeto para usar o pacote `ece-tools`, você deve atualizar o metapackage e as propriedades de ganchos `.magento.app.yaml` e executar uma atualização do Composer.

**Para atualizar o projeto para usar ece-tools**:

1. Atualizar a restrição de versão `magento/magento-cloud-metapackage` no arquivo `composer.json`.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Atualize o metapackage.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Modifique os comandos hook no arquivo `magento.app.yaml`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Verifique e remova os [pacotes obsoletos](#remove-deprecated-packages). Os pacotes obsoletos podem impedir uma atualização bem-sucedida.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Talvez seja necessário atualizar o pacote `ece-tools`.

   ```bash
   composer update magento/ece-tools
   ```

1. Adicione e confirme as alterações de código. Neste exemplo, os seguintes arquivos foram atualizados:

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Envie alterações de código por push ao servidor remoto e mescle esta ramificação com a ramificação `integration`.

   ```bash
   git push origin <branch-name>
   ```
