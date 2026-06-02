---
title: Variáveis de ambiente
description: Consulte uma lista de variáveis de ambiente específicas do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Build, Configuration, Deploy
exl-id: 38b2cdc2-1a98-48bd-90b2-13ef179da26f
TQID: https://experienceleague.adobe.com/qRdv72nxgkwRjRz0lXqs33rSmZKc3akq2W0pJK4CM7k
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 266
ht-degree: 0%

---

# Variáveis de ambiente

A infraestrutura do Adobe Commerce na nuvem permite atribuir variáveis de ambiente para substituir opções de configuração. O pacote `ece-tools` define valores no arquivo `env.php` com base nos valores das [Variáveis de nuvem](variables-cloud.md), variáveis definidas no [!DNL Cloud Console] e no arquivo de configuração `.magento.env.yaml`.

As variáveis de ambiente no arquivo `.magento.env.yaml` personalizam o ambiente de nuvem, substituindo a configuração existente do Commerce. Se um valor padrão for `Not Set`, o pacote `ece-tools` executará a ação **NO** e usará o valor padrão [!DNL Commerce] ou o valor da configuração `MAGENTO_CLOUD_RELATIONSHIPS`. Se o valor padrão estiver definido, o pacote `ece-tools` atuará para definir esse padrão.

Os tipos de variáveis de ambiente incluem:

- [ADMIN](variables-admin.md) — as variáveis substituem as variáveis ADMIN do projeto
- [MAGENTO_CLOUD](variables-cloud.md) — variáveis específicas para a infraestrutura em nuvem
- Variáveis usadas no arquivo `.magento.env.yaml`:
   - [Global](variables-global.md)—as variáveis afetam os estágios de compilação, implantação e pós-implantação
   - [Build](variables-build.md) — as variáveis controlam ações de compilação
   - [Implantar](variables-deploy.md) — variáveis controlam ações de implantação
   - [Pós-implantação](variables-post-deploy.md)—as variáveis controlam ações após a implantação

As variáveis são _hierárquicas_, o que significa que, se uma variável não for substituída, ela será herdada do ambiente pai.

Você pode definir [variáveis ADMIN](variables-admin.md) de [!DNL Cloud Console] ou usando a CLI do Adobe Commerce. É possível gerenciar outras variáveis de ambiente a partir do arquivo [`.magento.env.yaml`](configure-env-yaml.md) para gerenciar ações de compilação e implantação em todos os seus ambientes, incluindo o Pro Staging e o Production, sem precisar de um tíquete de suporte.

>[!TIP]
>
>Os arquivos YAML diferenciam maiúsculas de minúsculas e não permitem tabulações. Tenha cuidado para usar recuo consistente em todo o arquivo `.magento.env.yaml` ou sua configuração pode não funcionar como esperado. Os exemplos nesta documentação e no arquivo de exemplo usam o recuo _two-space_. Use o [comando de validação de ferramentas ece](configure-env-yaml.md#validate-configuration-file) para verificar sua configuração.
