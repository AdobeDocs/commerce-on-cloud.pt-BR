---
title: Propriedade de variáveis
description: Use a propriedade variables para personalizar as opções de configuração de armazenamento para o aplicativo  [!DNL Commerce] .
feature: Cloud, Configuration
exl-id: 9909d4a1-d9c8-492b-a5cc-cfbf17b5b7a8
TQID: https://experienceleague.adobe.com/bNdcqNxAAE1E1Qe2C-y1LWpFX-8Kzuwc9rJYEnbRz0U
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 85
ht-degree: 0%

---

# Propriedade de variáveis

Você pode usar variáveis de ambiente baseadas em aplicativos para personalizar as configurações de armazenamento. Essas variáveis usam uma sintaxe específica. Consulte [Substituir configurações](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=pt-BR) no _Guia de configuração_.

As variáveis de ambiente a seguir, incluídas no arquivo `.magento.app.yaml`, são necessárias para versões específicas do aplicativo [!DNL Commerce].

Necessário para o Adobe Commerce 2.2.x a 2.3.x:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Para o Adobe Commerce 2.4.x, defina as seguintes variáveis:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

