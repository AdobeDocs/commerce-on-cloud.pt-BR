---
title: Notas de versão do Cloud Tools Suite
description: Saiba mais sobre as melhorias mais recentes do conjunto de ferramentas da nuvem para o Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
TQID: https://experienceleague.adobe.com/eQQvGGEwj4D6pOlhZqNA-SMdc6JxH-Wg-hBRZaR1C-M
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 3%

---

# Notas de versão do Commerce Cloud Tools Suite

Estas informações de versão detalham as melhorias mais recentes nos pacotes do Cloud Tools Suite for Commerce, que foram projetados para implantar e gerenciar instalações e atualizações do Adobe Commerce na plataforma Cloud.

| Notas de versão | Versão | Descrição | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [pacote de ferramentas ece](ece-tools-package.md) | 2002.2.11 | Um conjunto de scripts e ferramentas projetado para gerenciar e implantar projetos na nuvem | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.11) |
| [Patches da nuvem para o Commerce](cloud-patches.md) | 1.1.14 | Um conjunto de patches que melhoram a integração de todas as versões do Adobe Commerce com ambientes na nuvem. Este pacote inclui patches do Adobe Commerce e hotfixes disponíveis que são aplicados quando você usa o `ece-tools` para implantar | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.14) |
| [Cloud Docker para Commerce](cloud-docker.md) | 1.4.8 | Arquivos de funcionalidade e configuração para imagens do Docker para implantar o Adobe Commerce em um ambiente de nuvem local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.8) |
| [Componentes na nuvem do Commerce](cloud-components.md) | 1.1.4 | Funcionalidade principal estendida do Adobe Commerce para sites implantados na infraestrutura em nuvem | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

Ao atualizar para ECE-Tools 2002.1.0 ou posterior, você atualiza automaticamente para as versões mais recentes dos outros pacotes, que são dependências do pacote `ece-tools`. Consulte [Metapackage da nuvem](../development/overview.md#cloud-metapackage) para obter uma lista de dependências.
