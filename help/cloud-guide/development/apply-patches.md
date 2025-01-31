---
title: Aplicar patches
description: Saiba como aplicar patches no projeto Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Aplicar patches

Os [Patches da Nuvem para o Commerce](https://github.com/magento/magento-cloud-patches) e a [Ferramenta de Patches de Qualidade](https://github.com/magento/quality-patches) fornecem patches para o aplicativo Adobe Commerce instalado.

- O pacote Cloud Patches for Commerce fornece os patches necessários com correções críticas
- Os Patches de qualidade oferecem correções de qualidade opcionais de baixo impacto como [patches individuais](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html#individual-patch) que não contêm alterações incompatíveis com versões anteriores

Consulte [Patches Disponíveis](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html) no _Guia de Ferramentas de Operações do Commerce_ para ver uma lista completa de patches lançados.

Ambos os pacotes melhoram a integração de todas as versões do Adobe Commerce com ambientes na nuvem e permitem a entrega rápida de correções críticas, opcionais e personalizadas. Você pode usar esses pacotes para aplicar, reverter e exibir informações gerais sobre todos os patches individuais disponíveis para o Commerce.

>[!TIP]
>
>Você pode usar a [Ferramenta de patches de qualidade](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html) e Patches na nuvem para Commerce como pacotes autônomos para projetos Magento Open Source e Adobe Commerce. Recomendamos o uso da Ferramenta de correções de qualidade para projetos que não estejam na nuvem.

Quando você implanta alterações no ambiente remoto, o pacote `ece-tools` usa `magento/magento-cloud-patches` e `magento/quality-patches` para verificar patches pendentes e os aplica automaticamente na seguinte ordem:

1. Aplique todos os patches Commerce necessários incluídos no pacote Cloud Patches for Commerce.
1. Aplique os patches Commerce opcionais selecionados incluídos na Ferramenta de patches de qualidade.
1. Aplique patches personalizados no diretório `/m2-hotfixes` em ordem alfabética por nome de patch.

>[!NOTE]
>
>Ao atualizar o pacote `ece-tools` ou o pacote Cloud Patches for Commerce, os patches necessários mais recentes serão aplicados na próxima vez que você implantar o projeto, ou você poderá implantá-los imediatamente usando o comando da CLI `ece-patches apply` e reimplantando o ambiente de Nuvem. Você não pode ignorar [os patches necessários](https://github.com/magento/magento-cloud-patches/tree/develop/patches) durante o processo de implantação.

## Pré-requisitos

{{upgrade-tip}}

A Ferramenta de Patches de Qualidade é uma dependência dos Patches de Nuvem para Commerce e o pacote `ece-tools`. Para aplicar os patches mais recentes, você deve ter a [versão mais recente de ECE-Tools](../dev-tools/update-package.md) instalada. A versão mínima exigida de ECE-Tools é 2002.1.2.

## Exibir patches e status disponíveis

Para exibir a lista de patches individuais disponíveis:

```bash
php ./vendor/bin/ece-patches status
```

Exemplo de resposta:

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

A tabela de status contém os seguintes tipos de informações:

- **Tipo**:
   - `Optional`—Todos os patches da Ferramenta de Patches de Qualidade e do pacote de Patches na Nuvem são opcionais para instalações do Adobe Commerce e do Magento Open Source. Para o Adobe Commerce na infraestrutura em nuvem, todos os patches são opcionais.
   - `Required` — Todos os patches do pacote Cloud Patches for Commerce são necessários para clientes da nuvem.
   - `Deprecated`—O patch individual é marcado como obsoleto e recomendamos revertê-lo se você o tiver aplicado. Após reverter um patch obsoleto, ele não será mais exibido na tabela de status.
   - `Custom` — Todos os patches do diretório &#39;m2-hotfixes&#39;.

- **Status**:
   - `Applied` — O patch foi aplicado.
   - `Not applied`—O patch não foi aplicado.
   - `N/A`—O status do patch não pode ser definido devido a conflitos.

- **Detalhes**:
   - `Affected components` — A lista de módulos afetados.
   - `Required patches` — A lista de patches necessários (dependências).
   - `Recommended replacement` — O patch que é um substituto recomendado para um patch obsoleto.

## Aplicar um patch em um ambiente local

Você pode aplicar patches manualmente em um ambiente local e testá-los antes de implantar.

**Para aplicar patches individuais em um ambiente de desenvolvimento local**:

1. Adicione a variável &#39;QUALITY_PATCH&#39; ao arquivo `.magento.env.yaml` e liste os patches necessários abaixo de.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. Na raiz do projeto, aplique os patches.

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   O comando `ece-patches apply` aplica patches na seguinte ordem:
   - Patches necessários
   - Patches individuais opcionais
   - Patches personalizados do diretório `/m2-hotfixes`

1. Limpe o cache.

   ```bash
   php ./bin/magento cache:clean
   ```

1. Teste os patches e faça as alterações necessárias nos patches personalizados.

## Aplicação de um patch em um ambiente remoto

>[!WARNING]
>
>Recomendamos testar todos os patches em uma integração ou ambientes de preparo antes de implantar no ambiente de produção.

**Para aplicar patches em um ambiente remoto**:

1. Adicione a variável `QUALITY_PATCHES` ao arquivo `.magento.env.yaml` e liste os patches necessários abaixo de.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >Depois de atualizar para uma nova versão do Adobe Commerce, você deverá reaplicar os patches se eles não estiverem incluídos na nova versão.

1. Adicione, confirme e envie por push o arquivo `.magento.env.yaml` atualizado.

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## Aplicar um patch personalizado

Ao implantar, o ECE-Tools aplica todos os patches de Adobe e qualquer patch personalizado que você adicionar ao diretório `/m2-hotfixes` na raiz do projeto.

>[!NOTE]
>
>Todos os nomes de arquivo de patch devem terminar com a extensão `.patch`.

**Para aplicar e testar um patch personalizado em um ambiente na nuvem**:

1. Na raiz do projeto, crie um diretório chamado `m2-hotfixes` se ele não existir

   ```bash
   mkdir m2-hotfixes
   ```

1. Copie o arquivo de patch para o diretório `/m2-hotfixes`.

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Teste todos os patches em um ambiente de pré-produção. Para o Adobe Commerce na infraestrutura em nuvem, você pode criar ramificações com o comando da CLI `magento-cloud environment:branch <branch-name>`.

## Reverter um patch personalizado

Para reverter ou desinstalar um patch personalizado aplicado anteriormente:

1. Exclua o arquivo de patch do diretório `/m2-hotfixes`.

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Teste em um ambiente de pré-produção. Para o Adobe Commerce na infraestrutura em nuvem, você pode criar ramificações com o comando da CLI `magento-cloud environment:branch <branch-name>`.

## Aplicar patches a um projeto que não esteja na nuvem

Use a [Ferramenta de correções de qualidade](https://github.com/magento/quality-patches) para projetos Magento Open Source e Adobe Commerce.

## Reverter um patch em um ambiente local

Você pode reverter todos os patches aplicados anteriormente em um ambiente de desenvolvimento local usando a CLI do `ece-patches`.

Para reverter todos os patches aplicados:

```bash
php ./vendor/bin/ece-patches revert
```

Este comando reverte todos os patches na seguinte ordem:

- Reverte todos os patches personalizados aplicados do diretório /m2-hotfixes.
- Reverte todos os patches individuais aplicados.
- Reverte todos os patches necessários aplicados.

## Logs

A Ferramenta de Patches de Qualidade registra todas as operações no arquivo `<Project_root>/var/log/patch.log`.
