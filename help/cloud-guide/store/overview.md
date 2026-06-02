---
title: Visão geral das opções de armazenamento e do gerenciamento de configuração
description: Personalize sua loja da Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Services
exl-id: e653172f-7370-4761-b2ce-3a420b33b948
TQID: https://experienceleague.adobe.com/iseYcfjh61-4ArUf9rKBGKFVuOz1dbS1xVq-FYiCxsw
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 193
ht-degree: 0%

---

# Visão geral das opções de armazenamento e do gerenciamento de configuração

Há várias maneiras de personalizar seu armazenamento, como adicionar um tema personalizado, instalar uma extensão ou impor uma configuração específica em ambientes de infraestrutura em nuvem. Você pode definir configurações para serviços específicos diretamente em ambientes de Preparo e Produção. Você pode configurar vários sites e lojas. A configuração da Loja ajuda a definir essas opções na estação de trabalho local e implantar configurações específicas em todos os ambientes.

Para acessar sua vitrine eletrônica, use o comando `magento-cloud url` e responda aos prompts. Ou você pode encontrar a url no [!DNL Cloud Console] em **Site de acesso**.

## Configurar opções de loja

As opções de armazenamento incluem o seguinte:

* [Módulo B2B (B2B)](b2b-module.md)
* [Temas personalizados](custom-theme.md)
* [Extensões](extensions.md)
* [Vários sites](multiple-sites.md)
* [Serviços de pagamento](paypal.md)

## Configurar serviços e integrações

Há [arquivos de configuração](../environment/overview.md) específicos que gerenciam determinados comportamentos de implantação em ambientes remotos. Você pode revisar esses tópicos separadamente:

* [Implantação do aplicativo](../application/configure-app-yaml.md)
* [Ações de criação e implantação de ambientes](../environment/configure-env-yaml.md)
* [Rotas de solicitação de entrada](../routes/routes-yaml.md)
* [Serviços compatíveis](../services/services-yaml.md)

## Gerenciamento de configuração

Após configurar as opções, os serviços e as integrações da loja, use o gerenciamento de configurações para implantar essas configurações em todos os ambientes de forma consistente e com tempo de inatividade mínimo. Consulte [Gerenciamento de Configuração](store-settings.md).
