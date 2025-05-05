---
title: Configurar serviço Elasticsearch
description: Saiba como habilitar o serviço Elasticsearch para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Configurar serviço Elasticsearch

[Elasticsearch](https://www.elastic.co) é um produto de código aberto que permite que você obtenha dados de qualquer fonte, qualquer formato e os pesquise e visualize em tempo real.

{{elasticsearch-support}}

Para o Adobe Commerce versão 2.4.4 e posterior, consulte [Configurar o serviço OpenSearch](opensearch.md).

- O Elasticsearch faz pesquisas rápidas e avançadas em produtos do catálogo de produtos
- Os analisadores de Elasticsearch suportam vários idiomas
- Suporta palavras de interrupção e sinônimos
- A indexação não afeta os clientes até que a operação de reindexação seja concluída

>[!TIP]
>
>A Adobe recomenda que você sempre configure o Elasticsearch para seu projeto do Adobe Commerce na infraestrutura em nuvem, mesmo que planeje configurar uma ferramenta de pesquisa de terceiros para seu aplicativo do Adobe Commerce. A configuração do Elasticsearch fornece uma opção de fallback caso a ferramenta de pesquisa de terceiros falhe.

{{service-instruction}}

**Para habilitar o Elasticsearch**:

1. Para projetos Iniciais, adicione o serviço `elasticsearch` ao arquivo `.magento/services.yaml` com a versão do Elasticsearch e o espaço em disco alocado em MB.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Para projetos Pro, você deve enviar um tíquete de Suporte da Adobe Commerce para alterar a versão do Elasticsearch nos ambientes de Preparo e Produção.

1. Definir a propriedade `relationships` no arquivo `.magento.app.yaml`.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   Para obter informações sobre como essas alterações afetam seus ambientes, consulte [Serviços](services-yaml.md).

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

## Compatibilidade de software de Elasticsearch

Ao instalar ou atualizar seu projeto Adobe Commerce na infraestrutura em nuvem, sempre verifique a compatibilidade entre a versão do serviço Elasticsearch e o cliente PHP[&#128279;](https://github.com/elastic/elasticsearch-php) do Elasticsearch para Adobe Commerce.

- **Configuração pela primeira vez**-Confirme se a versão do Elasticsearch especificada no arquivo `services.yaml` é compatível com o cliente Elasticsearch PHP configurado para Adobe Commerce.

- **Atualização de projeto**-Verifique se o cliente PHP do Elasticsearch na nova versão do aplicativo é compatível com a versão do serviço Elasticsearch instalada na infraestrutura de nuvem.

O suporte à versão do serviço e à compatibilidade do Adobe Commerce na infraestrutura em nuvem é determinado pelas versões implantadas na infraestrutura em nuvem e, às vezes, difere das versões compatíveis com implantações locais do Adobe Commerce. Consulte [Versões de serviço](services-yaml.md#service-versions).

**Para verificar a compatibilidade de software de Elasticsearch**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Mostrar os detalhes do Elasticsearch para o ambiente ativo.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. Como alternativa, você pode usar o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Verifique a versão do pacote do Composer para `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   Na resposta, verifique a versão instalada na propriedade `versions`.

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   Além disso, você pode encontrar a versão do cliente PHP do Elasticsearch no arquivo `composer.lock` no diretório raiz do ambiente.

1. Na linha de comando, recupere os detalhes de conexão do serviço de Elasticsearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Na resposta, localize o endereço IP do ponto final do serviço Elasticsearch:

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. Recupere o serviço de Elasticsearch instalado `version:number` do ponto de extremidade de serviço.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Verifique a compatibilidade da versão entre o serviço Elasticsearch e o cliente PHP.

   Se as versões forem incompatíveis, faça uma das seguintes atualizações na configuração do seu ambiente:

   - Mude o cliente PHP Elasticsearch para uma versão compatível com a versão do serviço Elasticsearch.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - Altere a versão do serviço Elasticsearch no arquivo `services.yaml` para uma versão que seja compatível com o cliente PHP Elasticsearch.

     {{pro-update-service}}

## Reinicie o serviço Elasticsearch

Se você precisar reiniciar o serviço [Elasticsearch](https://www.elastic.co), entre em contato com o suporte da Adobe Commerce.

## Configuração de pesquisa adicional

- Por padrão, a configuração de pesquisa para ambientes em nuvem é gerada novamente sempre que você implanta. Você pode usar a variável de implantação `SEARCH_CONFIGURATION` para manter configurações de pesquisa personalizadas entre implantações. Consulte [Implantar variáveis](../environment/variables-deploy.md#search_configuration).

- Depois de configurar o serviço Elasticsearch para o seu projeto, use a interface do Administrador para testar a conexão de Elasticsearch e personalizar as configurações de Elasticsearch para o Adobe Commerce.

### Adicionar plug-ins para o Elasticsearch

Como opção, você pode adicionar plug-ins para o Elasticsearch, adicionando a seção `configuration:plugins` ao serviço Elasticsearch no arquivo `.magento/services.yaml`. Por exemplo, o código a seguir habilita os plug-ins de análise ICU e Análise fonética.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Se você usar o plug-in de terceiros do Elastic Suite, deverá [atualizar o pacote `ece-tools`](../dev-tools/update-package.md) para a versão 2002.0.19 ou posterior.
Ao configurar o Elastic Suite, adicione as definições de configuração à variável de implantação `ELASTICSUITE_CONFIGURATION`. Essa configuração salva as configurações nas implantações.

### Remover plug-ins do Elasticsearch

Remover as entradas de plug-in de `elasticsearch:` em `.magento/services.yaml` não as desinstala ou desabilita conforme o esperado. Você deve reindexar os dados de Elasticsearch. Esse comportamento é intencional para evitar possível perda ou corrupção de dados que dependem desses plug-ins.

**Para remover os plug-ins de Elasticsearch**:

1. Remova as entradas do plug-in Elasticsearch do seu arquivo `.magento/services.yaml`.
1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Confirme as `.magento/services.yaml` alterações no repositório de nuvem.
1. Reindexe o índice de pesquisa do catálogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Limpe o cache.

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Para obter detalhes sobre como usar ou solucionar problemas do plug-in do Elastic Suite com o Adobe Commerce, consulte a [documentação do Elastic Suite](https://github.com/Smile-SA/elasticsuite).

## Solução de problemas

Consulte os seguintes artigos de suporte da Adobe Commerce para obter ajuda com a solução de problemas de Elasticsearch:

- [Elasticsearch 5 está configurado, mas a página de pesquisa não carrega com o erro &quot;Fielddata está desabilitado...&quot;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html)
- [Elasticsearch na solução de problemas do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- O Status do Índice [Elasticsearch é `yellow` ou `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html)
