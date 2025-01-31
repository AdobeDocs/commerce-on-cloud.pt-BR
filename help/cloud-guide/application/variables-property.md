---
title: Propriedade de variáveis
description: Use a propriedade variables para personalizar as opções de configuração de armazenamento para o aplicativo  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Propriedade de variáveis

Você pode usar variáveis de ambiente baseadas em aplicativos para personalizar as configurações de armazenamento. Essas variáveis usam uma sintaxe específica. Consulte [Substituir configurações](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) no _Guia de configuração_.

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
