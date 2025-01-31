---
title: Variáveis pós-implantação
description: Consulte a lista de variáveis de ambiente que controlam ações na fase de pós-implantação do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variáveis pós-implantação

As variáveis _pós-implantação_ a seguir controlam ações na fase de pós-implantação e podem herdar e substituir valores das [Variáveis globais](variables-global.md). Insira estas variáveis no estágio `post-deploy` do arquivo `.magento.env.yaml`:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Para obter mais informações sobre como personalizar o processo de criação e implantação:

- [Configuração de implantação](configure-env-yaml.md)
- [Processo de implantação](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Padrão**— `[]` (uma matriz vazia)
- **Versão** — Adobe Commerce 2.1.4 e posterior

Configure o teste TTFB (Time To First Byte, tempo até o primeiro byte) de _tempo para páginas especificadas para testar o desempenho do site._ Especifique uma referência de caminho absoluto, ou URL com protocolo e host, para cada página que requer o teste.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Depois que você especificar as páginas para testar e confirmar suas alterações, o teste _Tempo até o Primeiro Byte_ será executado durante a fase de pós-implantação e publicará os resultados para cada caminho no log de nuvem:

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Para caminhos redirecionados, o log relata o caminho do destino de redirecionamento em vez do configurado na variável de ambiente. Se você especificar um caminho inválido, o log exibirá uma mensagem de aviso.

## `WARM_UP_CONCURRENCY`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Especifique o limite de solicitações simultâneas a serem enviadas durante operações de aquecimento de cache para reduzir a carga do servidor. Esse valor limita o número de conexões paralelas e é útil para configurações de ambiente em que a variável de pós-implantação `WARM_UP_PAGES` especifica várias páginas para pré-carregamento de cache.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Padrão**— `index.php`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Personalize a lista de páginas usadas para pré-carregar o cache no estágio `post_deploy`. Você deve configurar o gancho pós-implantação. Consulte a [seção de ganchos](../application/hooks-property.md) do arquivo `.magento.app.yaml`.

- **páginas únicas** — Especifique uma única página para adicionar ao cache. Não é necessário indicar o URL de base padrão. O exemplo a seguir armazena em cache a página `BASE_URL/index.php`:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **vários domínios** — Lista várias URLs. O exemplo a seguir armazena páginas em cache de dois domínios:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **várias páginas** — Use o formato a seguir para armazenar em cache várias páginas de acordo com um padrão de expressão regular específico:

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: Possíveis variantes `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: Use um padrão `regexp` ou uma correspondência exata `url` para filtrar as URLs, ou use um asterisco (\*) para todas as páginas. Usar SKU do produto para o tipo de entidade `product`
   - `store_id|store_code`: Use a ID ou o Código do armazenamento ou um asterisco (\*) para todos os armazenamentos. Você pode passar várias IDs de armazenamento ou códigos separados por `|`

  O exemplo a seguir armazena em cache os tipos de entidade `category` e `cms-page` com base nesses critérios:
   - todas as páginas de categoria do armazenamento com ID `1`
   - todas as páginas de categoria para lojas com código `store1` e `store2`
   - página de categoria `cars` para armazenamento com código `store_en`
   - página cms `contact` para todas as lojas
   - página cms `contact` para armazenamentos com ID `1` e `2`
   - qualquer página de categoria que contenha `car_` e termine com `html` para armazenamento com ID 2
   - qualquer página de categoria que contenha `tires_` para armazenamento com código `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  O exemplo a seguir armazena em cache o tipo de entidade `product` com base nesses critérios:
   - todos os produtos para todas as lojas (limitado programaticamente a 100 por loja para evitar problemas de desempenho)
   - todos os produtos da loja `store1`
   - produtos com `sku1` para todas as lojas
   - produtos com `sku1` para lojas com código `store1` e `store2`
   - produtos com `sku1`, `sku2` e `sku3` para lojas com código `store1` e `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  O exemplo a seguir armazena em cache o tipo de entidade `store-page` com base nesses critérios:
   - página `/contact-us` para todas as lojas
   - página `/contact-us` do armazenamento com ID `1`
   - página `/contact-us` para lojas com código `code1` e `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
