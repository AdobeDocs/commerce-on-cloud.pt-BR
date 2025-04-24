---
title: Notas de versão do Cloud Tools Suite
description: Saiba mais sobre as melhorias mais recentes do conjunto de ferramentas da nuvem para o Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: 52cba17394ad73c772f0a8910cc732fab173ca8d
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---

# Notas de versão do Commerce Cloud Tools Suite

Estas informações de versão detalham as melhorias mais recentes nos pacotes do Cloud Tools Suite for Commerce, que foram projetados para implantar e gerenciar instalações e atualizações do Adobe Commerce na plataforma Cloud.

| Notas de versão | Versão | Descrição | Source |
| ----------------- |-----------| ---------------------------------------- | --------------------------- |
| [`ece-tools` pacote](ece-tools-package.md) | 2002.2.4 | Um conjunto de scripts e ferramentas projetado para gerenciar e implantar projetos na nuvem | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.4) |
| [Patches da nuvem para o Commerce](cloud-patches.md) | 1.1.6 | Um conjunto de patches que melhoram a integração de todas as versões do Adobe Commerce com ambientes na nuvem. Este pacote inclui patches do Adobe Commerce e hotfixes disponíveis que são aplicados quando você usa o `ece-tools` para implantar | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.6) |
| [Cloud Docker para Commerce](cloud-docker.md) | 1.4.2 | Arquivos de funcionalidade e configuração para imagens do Docker para implantar o Adobe Commerce em um ambiente de nuvem local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.1) |
| [Componentes na nuvem do Commerce](cloud-components.md) | 1.1.1 | Funcionalidade principal estendida do Adobe Commerce para sites implantados na infraestrutura em nuvem | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.1) |

Ao atualizar para ECE-Tools 2002.1.0 ou posterior, você atualiza automaticamente para as versões mais recentes dos outros pacotes, que são dependências do pacote `ece-tools`. Consulte [Metapackage da nuvem](../development/overview.md#cloud-metapackage) para obter uma lista de dependências.

A nova versão do `ece-tools` (2002.2.0) está disponível somente no PHP versão 8.1 e superior (8.2, 8.3). As versões anteriores do PHP foram descontinuadas (7.4, 7.3, 7.2). Você pode usar versões anteriores do `ece-tools` com versões antigas do PHP.
