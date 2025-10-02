---
title: Configurar o serviço OpenSearch
description: Saiba como habilitar o serviço OpenSearch para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Search, Services
exl-id: e704ab2a-2f6b-480b-9b36-1e97c406e873
source-git-commit: a6f927d7931d0fbbf37269cb41b4b6006ac04268
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# Configurar o serviço OpenSearch

O serviço [OpenSearch](https://www.opensearch.org) é uma bifurcação de código aberto do Elasticsearch 7.10.2, seguindo as alterações de licenciamento do Elasticsearch. Consulte o [OpenSource Project](https://github.com/opensearch-project) no GitHub.

{{elasticsearch-support}}

O OpenSearch permite extrair dados de qualquer fonte, qualquer formato e pesquisá-los e visualizá-los em tempo real.

- Pesquisas rápidas e avançadas em produtos do catálogo de produtos
- Os analisadores OpenSearch são compatíveis com vários idiomas
- Suporta palavras de interrupção e sinônimos
- A indexação não afeta os clientes até que a operação de reindexação seja concluída

{{service-instruction}}

>[!TIP]
>
>A menos que você esteja usando o [Live Search](https://experienceleague.adobe.com/en/docs/commerce/live-search/overview), a Adobe recomenda que você sempre configure o OpenSearch para seu projeto do Adobe Commerce na infraestrutura na nuvem, mesmo que planeje configurar uma ferramenta de pesquisa de terceiros para seu aplicativo do Adobe Commerce. A configuração do OpenSearch fornece uma opção de fallback se a ferramenta de pesquisa de terceiros falhar.

**Para habilitar OpenSearch**:

1. Para ambientes de integração, adicione o serviço `opensearch` ao arquivo `.magento/services.yaml` com a versão apropriada e o espaço em disco alocado em MB. Nesse caso, a versão 2 é adequada. A versão secundária não é necessária.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Para projetos Pro, você deve [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para alterar a versão do OpenSearch nos ambientes de Preparo e Produção.

1. Defina ou verifique a propriedade `relationships` no arquivo `.magento.app.yaml`.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Para obter informações sobre como essas alterações afetam seus ambientes, consulte [Configurar serviços](services-yaml.md).

1. Depois que o processo de implantação for concluído, use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Reindexe o índice de pesquisa do catálogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Limpe o cache.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Compatibilidade de software OpenSearch

Ao instalar ou atualizar seu projeto Adobe Commerce na infraestrutura em nuvem, sempre verifique a compatibilidade entre a versão do serviço OpenSearch e o cliente PHP[ do ](https://github.com/opensearch-project/opensearch-php)OpenSearch para Adobe Commerce.

- **Primeira configuração**-Confirme se a versão do OpenSearch especificada no arquivo `services.yaml` é compatível com o cliente OpenSearch PHP configurado para Adobe Commerce.

- **Atualização de projeto**-Verifique se o cliente OpenSearch PHP na nova versão do aplicativo é compatível com a versão do serviço OpenSearch instalada na infraestrutura de nuvem.

O suporte à versão e compatibilidade do serviço é determinado por versões testadas e implantadas na infraestrutura da nuvem e, às vezes, difere das versões compatíveis com implantações locais do Adobe Commerce. Consulte [Requisitos do sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) no _Guia de Instalação_ para obter uma lista de versões com suporte.

**Para verificar a compatibilidade do software OpenSearch**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Mostrar os detalhes do OpenSearch para o ambiente ativo.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. Como alternativa, você pode usar o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recupere os detalhes de conexão do serviço OpenSearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Na resposta, localize o endereço IP e a porta do ponto final do serviço OpenSearch:

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. Recupere o serviço OpenSearch `version:number` instalado do ponto de extremidade de serviço.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## Reiniciar o serviço OpenSearch

Se precisar reiniciar o serviço OpenSearch, entre em contato com o suporte da Adobe Commerce.

## Configuração de pesquisa adicional

- Por padrão, a configuração de pesquisa para ambientes em nuvem é gerada novamente sempre que você implanta. Você pode usar a variável de implantação `SEARCH_CONFIGURATION` para manter configurações de pesquisa personalizadas entre implantações. Consulte [Implantar variáveis](../environment/variables-deploy.md#search_configuration).

- Depois de configurar o serviço OpenSearch para o seu projeto, use a interface do Administrador para testar a conexão OpenSearch e personalizar as configurações OpenSearch para o Adobe Commerce.

### Adicionar plug-ins do OpenSearch

Como opção, você pode adicionar plug-ins para OpenSearch adicionando a seção `configuration:plugins` ao serviço OpenSearch no arquivo `.magento/services.yaml`. Por exemplo, o código a seguir habilita os plug-ins de análise ICU e Análise fonética.

>[!NOTE]
>
>Isso se aplica somente aos ambientes Integration e Starter. Para instalar os plug-ins em um cluster Pro Staging ou Production, [envie uma solicitação de suporte](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case).


```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Consulte o [OpenSearch Project](https://github.com/opensearch-project) para obter mais informações sobre plug-ins.

### Remover plug-ins do OpenSearch

A remoção das entradas de plug-in da seção `opensearch:` do arquivo `.magento/services.yaml` faz **não** desinstalar ou desabilitar o serviço. Para desabilitar totalmente o serviço, você deve reindexar os dados do OpenSearch após remover os plug-ins do arquivo `.magento/services.yaml`. Este design evita a possível perda ou corrupção de dados que dependem desses plug-ins.


**Para remover plug-ins do OpenSearch**:

>[!NOTE]
>
>Essa alteração se aplica somente aos ambientes Integration e Starter. Você terá que [enviar um tíquete de suporte](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) para remover o plug-in em um cluster de Preparo ou Produção Pro.

1. Remova as entradas de plug-in OpenSearch do arquivo `.magento/services.yaml`.
1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Confirme as `.magento/services.yaml` alterações no repositório de nuvem.
1. Reindexe o índice de pesquisa do catálogo (todos os ambientes: Integration, Starter, Pro Staging e Production clusters).

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Limpe o cache.

   ```bash
   bin/magento cache:clean
   ```
