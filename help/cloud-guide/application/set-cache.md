---
title: Definir cache para arquivos estáticos
description: Saiba como definir opções de armazenamento em cache no arquivo de configuração do aplicativo [!DNL Commerce].
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: 'https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: d05f97c9-0a96-5792-92cf-f66ce7326e3a
    internal-label: SCD
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%
---
# Definir cache para arquivos estáticos

O TTL (tempo de vida útil) de cache para seus arquivos de mídia e estáticos está definido no arquivo de configuração `.magento.app.yaml` usando a chave `expires`.

>[!NOTE]
>
>Antes de atualizar o ambiente de Produção, é importante testar as alterações no ambiente de Preparo. [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) para obter ajuda para atualizar a configuração nesses ambientes.

1. Especifique o tempo de TTL (em segundos) na propriedade [`web` &#x200B;](web-property.md) do arquivo `.magento.app.yaml`. Você pode adicionar a chave `expires` em `locations` ou em `"/media"` e `"/static"`.

   Para evitar que o cache expire, use o par de valor-chave `expires: -1`. Consulte o exemplo a seguir:

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```

