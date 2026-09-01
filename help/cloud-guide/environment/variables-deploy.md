---
title: Implantar variáveis
description: Consulte a lista de variáveis de ambiente que controlam ações na fase de implantação do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
TQID: https://experienceleague.adobe.com/TNuUxXzCiXnKefww0DmKbjfJygEz2HFG-0PjCsCy2nA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: bdc2bedd2696e7dde0ffb55f846a8bced2dbd25d
workflow-type: tm+mt
source-wordcount: 3106
ht-degree: 0%

---

# Implantar variáveis

As variáveis _implantar_ a seguir controlam ações na fase de implantação e podem herdar e substituir valores das [Variáveis globais](variables-global.md). Insira estas variáveis no estágio `deploy` do arquivo `.magento.env.yaml`:

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

Para obter mais informações sobre como personalizar o processo de criação e implantação:

- [Configuração de implantação](configure-env-yaml.md)
- [Processo de implantação](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Padrão**—_Não definido_

Use `CACHE_CONFIGURATION` para mesclar ou substituir as opções de front-end e back-end do cache geradas durante a implantação.

Para Adobe Commerce na infraestrutura em nuvem, não edite `app/etc/env.php` diretamente. O pacote `ece-tools` gera a configuração de implantação de `.magento.env.yaml`, relações de serviço e variáveis de implantação com suporte.

Use `VALKEY_BACKEND` ou `REDIS_BACKEND` para selecionar o cache com suporte ou a implementação L2 para a versão exata do Adobe Commerce. Use o `CACHE_CONFIGURATION` para personalizar opções como tentativas de conexão, tempos limite de leitura, prefixos de cache ou chaves de pré-carregamento.

A combinação de back-end e serviço de cache compatível depende da versão do Commerce e do nível de patch. O Redis não é compatível com o Adobe Commerce 2.4.9 ou versões de patch posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 e 2.4.8-p4. Use Valkey para versões em que os [requisitos do sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) o exijam.

>[!NOTE]
>
>Para obter orientações mais detalhadas sobre a configuração do serviço Redis e Valkey, consulte [Práticas recomendadas para a configuração do serviço Valkey e Valkey](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)

Por padrão, o processo de implantação substitui a configuração de cache correspondente. Para mesclar os valores especificados com a configuração gerada, defina `_merge` como `true`:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            connect_retries: 3
          remote_backend_options:
            read_timeout: 10
```

Para substituir a configuração existente pelos valores especificados em `CACHE_CONFIGURATION`, defina `_merge` como `false`.

>[!IMPORTANT]
>
> Não copie opções `bin/magento setup:config:set` locais, como `cm_cache_backend_redis`, diretamente em `CACHE_CONFIGURATION`. Em projetos na Nuvem, `ece-tools` obtém detalhes de conexão de serviço dos relacionamentos configurados. Use a estrutura documentada para a versão selecionada do Commerce e a implementação do cache.

O exemplo a seguir mescla atribuições de banco de dados em uma configuração de cache existente. Use esse tipo de substituição somente quando o back-end selecionado e a versão do Commerce forem compatíveis. Aplicar configurações de front-end a `symfony_l2` somente se a documentação atual do Symfony L2 oferecer suporte explícito à opção.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

O exemplo a seguir usa o [recurso de pré-carregamento Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache#redis-preload-feature) conforme definido no _Guia de configuração_. Use a orientação correspondente do Valkey para versões que usam o Valkey.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

Para usar um modelo [REDIS_BACKEND](#redis_backend) personalizado que não esteja na lista de permissões, defina `_custom_redis_backend` como `true` para que ece-tools aplique a validação apropriada:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **Padrão**—`true`

Habilita ou desabilita a limpeza de [arquivos de conteúdo estático](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment) gerados durante a fase de compilação ou implantação. Use o valor padrão _true_ em desenvolvimento como uma prática recomendada.

- **`true`** — Remove todo o conteúdo estático existente antes de implantar o conteúdo estático atualizado.
- **`false`** — A implantação somente substitui arquivos de conteúdo estático existentes se o conteúdo gerado contiver uma versão mais recente.

Se você modificar o conteúdo estático por meio de um processo separado, defina o valor como _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Falha ao limpar arquivos de exibição estáticos antes da implantação pode causar problemas se você implantar atualizações em arquivos existentes sem remover as versões anteriores. Devido às regras de [fallback de arquivo estático](https://developer.adobe.com/commerce/frontend-core/guide/css/preprocess#clean-static-view-files), as operações de fallback poderão exibir o arquivo errado se o diretório contiver várias versões do mesmo arquivo.

## `CRON_CONSUMERS_RUNNER`

- **Padrão**—`cron_run = false`, `max_messages = 1000`

Use essa variável de ambiente para confirmar se as filas de mensagens estão em execução após uma implantação.

- `cron_run` — Um valor booleano que habilita ou desabilita o trabalho cron `consumers_runner`. O padrão é `false`.
- `max_messages` — O número máximo de mensagens que cada consumidor processa antes de terminar. O padrão é `1000`. Para evitar que o consumidor seja encerrado, defina-o como `0`.
- `consumers` — Uma matriz de cadeias de caracteres especificando os nomes dos consumidores a serem executados. Uma matriz vazia executa _todos_ consumidores.
- `multiple_processes`-O número de processos a serem gerados para cada consumidor. Essa opção é compatível com o Adobe Commerce 2.4.4 e posterior.

>[!NOTE]
>
>Para listar os consumidores da fila de mensagens disponíveis, execute o comando `./bin/magento queue:consumers:list` no ambiente remoto.

O exemplo a seguir executa consumidores selecionados e inicia vários processos para cada um:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
       example_consumer_1
       example_consumer_2
      multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

O exemplo a seguir executa todos os consumidores:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Por padrão, o processo de implantação substitui as configurações correspondentes no arquivo `env.php`. Consulte [Gerenciar filas de mensagens](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues) no _Guia de Configuração do Commerce_ para Adobe Commerce local.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Padrão**—`false`

Configure como `consumers` processa mensagens da fila de mensagens escolhendo uma das seguintes opções:

- `false`—`Consumers` processar mensagens disponíveis, fechar a conexão TCP e encerrar independentemente do limite `max_messages` especificado na variável de implantação `CRON_CONSUMERS_RUNNER`.

- `true`—`Consumers` continue a processar mensagens da fila de mensagens até atingir o número máximo de mensagens (`max_messages`) especificado na variável de implantação `CRON_CONSUMERS_RUNNER` antes de fechar a conexão TCP e encerrar o processo do consumidor. Se a fila ficar vazia antes de atingir `max_messages`, o consumidor aguarda mais mensagens chegarem.

>[!WARNING]
>
>Se você usar trabalhadores para executar `consumers` em vez de usar um trabalho cron, defina essa variável como true.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Padrão**—_Não definido_

>[!WARNING]
>
>Para evitar a exposição da chave no repositório de código-fonte, defina o valor `CRYPT_KEY` por meio do [!DNL Cloud Console] em vez do arquivo `.magento.env.yaml`. Consulte [Definir variáveis de ambiente e projeto](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/overview#configure-environment).

Ao mover o banco de dados de um ambiente para outro sem um processo de instalação, você precisará das informações criptográficas correspondentes. O Adobe Commerce usa o valor da chave de criptografia definido em [!DNL Cloud Console] como o valor `crypt/key` no arquivo `env.php`.

## `DATABASE_CONFIGURATION`

- **Padrão**—_Não definido_

Se você definiu um banco de dados na [propriedade de relações](../application/properties.md#relationships) do arquivo `.magento.app.yaml`, poderá personalizar suas conexões de banco de dados para implantação.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

O exemplo a seguir mescla novos valores com uma configuração existente:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

Além disso, é possível configurar um prefixo de tabela.

>[!WARNING]
>
>Se você não usar a opção de mesclagem com o prefixo da tabela, deverá fornecer as configurações de conexão padrão ou a validação da implantação falhará.

O exemplo a seguir usa o prefixo de tabela `ece_` com configurações de conexão padrão em vez de usar a opção `_merge`:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

Saída de exemplo:

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **Padrão**—_Não definido_

Mantém as configurações personalizadas do serviço [!DNL Elastic Suite] entre as implantações e as usa na seção &#39;system/default/sorriso_elasticsuite_core_base_settings&#39; da configuração principal do [!DNL Elastic Suite]. Se o pacote do compositor [!DNL Elastic Suite] estiver instalado, ele será configurado automaticamente.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>Em um cluster Pro de Preparo/Produção que tenha três nós (ou três nós de serviço na [Arquitetura em Escala](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)), o `indices_settings` deve ser definido da seguinte maneira:
>
>```yaml
>           indices_settings:
>               number_of_shards: 1
>               number_of_replicas: 2
>```

{{merge-options}}

O exemplo a seguir mescla um novo valor com a configuração existente:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 2
      _merge: true
```

**Limitações conhecidas**:

- Alterar o mecanismo de pesquisa para qualquer tipo diferente de `elasticsuite` causa uma falha de implantação acompanhada de um erro de validação apropriado
- A remoção do serviço Elasticsearch causa uma falha de implantação acompanhada de um erro de validação apropriado

>[!NOTE]
>
>Para obter detalhes sobre como usar ou solucionar problemas do plug-in [!DNL Elastic Suite] com o Adobe Commerce, consulte a [[!DNL Elastic Suite] documentação](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **Padrão**—`false`

Ativa e desativa o Google Analytics ao implantar em ambientes de Preparo e Integração. Por padrão, o Google Analytics é verdadeiro somente para o ambiente de Produção. Para habilitar o Google Analytics nos ambientes de Preparo e Integração, defina esse valor como `true`.

- **`true`** — Habilita o Google Analytics em ambientes de Preparo e Integração.
- **`false`** — Desabilita o Google Analytics em ambientes de Preparo e Integração.

Adicione a variável de ambiente `ENABLE_GOOGLE_ANALYTICS` ao estágio `deploy` no arquivo `.magento.env.yaml`:

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>O processo de implantação sempre habilita o Google Analytics em ambientes de produção.

## `FORCE_UPDATE_URLS`

- **Padrão**—`true`

Na implantação em ambientes Pro ou Starter de Preparo e Produção, essa variável substitui as URLs de base do Adobe Commerce no banco de dados pelas URLs de projeto especificadas pela variável [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Para substituir o comportamento padrão da variável de implantação [UPDATE_URLS](#update_urls), use esta configuração.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Padrão**— Em ambientes de Produção e de Preparo, o padrão é `file` e não pode ser alterado. Para ambientes iniciais e de integração Pro, o padrão é `db`.

O provedor de bloqueio impede a execução de trabalhos cron duplicados e grupos cron. O Adobe Commerce na Nuvem oferece suporte aos provedores de bloqueio `file` e `db`.

Em ambientes de Produção e Preparo Pro, o `MAGENTO_CLOUD_LOCKS_DIR` configura o provedor `file`. Não é possível substituir essa configuração. Em ambientes Pro Integration e Starter, o `ece-tools` define o provedor `db` por padrão. Para otimizar o desempenho local e espelhar a arquitetura de produção, defina o provedor como `file` nesses ambientes.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: 'file'
```

## `MYSQL_USE_SLAVE_CONNECTION`

- **Padrão**—`false`

>[!TIP]
>
>A variável `MYSQL_USE_SLAVE_CONNECTION` tem suporte apenas no Adobe Commerce nos clusters Staging e Production Pro da infraestrutura em nuvem. Não é compatível com projetos iniciais.

O Adobe Commerce pode ler vários bancos de dados de forma assíncrona. Defina como `true` para usar uma conexão _somente leitura_ com o banco de dados automaticamente para receber tráfego somente leitura em um nó não mestre. Essa conexão melhora o desempenho por meio do balanceamento de carga, pois somente um nó lida com o tráfego de leitura-gravação. Para remover qualquer matriz de conexão somente leitura existente do arquivo `env.php`, defina como `false`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Quando a variável `MYSQL_USE_SLAVE_CONNECTION` está definida como `true`, o sistema define o parâmetro `synchronous_replication` como `true` por padrão no arquivo `env.php` em ambientes de Preparo e Produção Profissionais. Quando o `MYSQL_USE_SLAVE_CONNECTION` está definido como `false`, o parâmetro `synchronous_replication` não está configurado.

## `QUEUE_CONFIGURATION`

- **Padrão**—_Não definido_

Use essa variável de ambiente para manter configurações personalizadas de serviço de fila entre implantações. Essa variável é compatível com os protocolos AMQP (para RabbitMQ) e STOMP (para AtiveMQ Artemis). Por exemplo, se você preferir usar um serviço de fila de mensagens existente, em vez de depender da infraestrutura de nuvem para criá-lo para você, use a variável de ambiente `QUEUE_CONFIGURATION` para conectá-lo ao seu site:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

Para AtiveMQ Artemis usando o protocolo STOMP:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      stomp:
        host: activemq.host
        port: 61616
        user: username
        password: password
```

{{merge-options}}

O exemplo a seguir mescla novos valores com uma configuração existente:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **Padrão**—`Cm_Cache_Backend_Redis`

Especifica a configuração do modelo de back-end para o cache Redis.

O cache Redis não é suportado para Adobe Commerce 2.4.9 ou versões de patch posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 e 2.4.8-p4. Para essas versões, use Valkey e a configuração `VALKEY_BACKEND` correspondente. Sempre verifique o serviço de cache com suporte nos [requisitos do sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements).

Para versões compatíveis com o Redis, os modelos de back-end disponíveis incluem:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

O exemplo a seguir habilita o back-end do cache sincronizado remoto e o cache L2:

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
> Quando `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` é selecionado, `ece-tools` gera automaticamente a configuração de cache L2. Para personalizar a configuração gerada, use [`CACHE_CONFIGURATION`](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Padrão**—`false`

>[!TIP]
>
>Há suporte para `REDIS_USE_SLAVE_CONNECTION` somente nos clusters Adobe Commerce em Cloud Staging e Production Pro. Não é compatível com projetos iniciais.

O Adobe Commerce pode ler várias instâncias de Redis de forma assíncrona. Defina essa variável como `true` para usar uma conexão somente leitura com uma réplica Redis para tráfego de leitura enquanto a instância primária manipula tráfego de leitura-gravação. Para remover uma matriz de conexão somente leitura existente de `env.php`, defina-a como `false`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Você deve ter um [serviço Redis configurado](../services/redis.md) nos arquivos `.magento.app.yaml` e `services.yaml`.

[ECE-Tools versão 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) e posterior usa mais configurações tolerantes a falhas. Se o Adobe Commerce não conseguir ler os dados da réplica Redis, ele voltará para a instância primária Redis.

A conexão somente leitura não está disponível no ambiente de integração. Se você usar [`CACHE_CONFIGURATION`](#cache_configuration), mescle as alterações na configuração gerada e verifique se a configuração resultante retém a conexão de réplica.

## `VALKEY_BACKEND`

- **Padrão**—`Cm_Cache_Backend_Redis`
- **Versão** — Versões do Adobe Commerce com suporte para Valkey

`VALKEY_BACKEND` especifica o modelo de back-end para a configuração do cache Valkey. O valor padrão usa um nome de classe compatível com Redis herdado; isso não significa que o serviço deve ser Redis.

Para versões do Adobe Commerce anteriores à 2.4.9 que oferecem suporte ao Valkey, os modelos de back-end incluem:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

O Adobe Commerce 2.4.9 e versões posteriores também oferecem suporte ao `symfony_l2`, a implementação L2 baseada em cache do Symfony. `symfony_l2` é suportado somente com Valkey.

### Configurar cache remoto sincronizado

Para o Adobe Commerce 2.4.8, use a seguinte configuração quando a implementação do cache sincronizado remoto for apropriada:

```yaml
stage:
  deploy:
    VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

A especificação do back-end sincronizado remoto habilita o cache L2 e `ece-tools` gera a configuração do cache automaticamente. Consulte o [exemplo de arquivo de configuração](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#customize-the-symfony-l2-cache-configuration). Para personalizar a configuração gerada, use [`CACHE_CONFIGURATION`](#cache_configuration).

### Configurar a implementação moderna do cache L2 do Symfony

Para o Adobe Commerce 2.4.9 e posterior, use a implementação do Symfony L2:

```yaml
stage:
  deploy:
    VALKEY_BACKEND: 'symfony_l2'
```

Especificar `symfony_l2` como o modelo de back-end do Valkey habilita o cache L2, e `ece-tools` gera a configuração do cache L2 automaticamente a partir dos detalhes de conexão do serviço Valkey, incluindo os front-ends `default` e `stale_cache_enabled`. Defina `CACHE_CONFIGURATION` somente quando precisar personalizar as opções de back-end com suporte, como o diretório de cache local. Consulte [Implementação do cache L2 do Symfony](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#configure-symfony-l2-cache){target="_blank"} no _Guia de Configuração do Adobe Commerce_.

>[!NOTE]
>
>O Adobe Commerce 2.4.9 inclui melhorias no cache L2 do Symfony — incluindo armazenamento, invalidação e compactação de tags de cache — com patch ACP2E-5132, reduzindo a E/S de disco, eliminando entradas de cache obsoletas e reduzindo a sobrecarga da memória e da rede.

## `VALKEY_USE_SLAVE_CONNECTION`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.4.8 e posterior

>[!TIP]
>
>Há suporte para `VALKEY_USE_SLAVE_CONNECTION` somente nos clusters Adobe Commerce em Cloud Staging e Production Pro. Não é compatível com projetos iniciais.

O Adobe Commerce pode ler várias instâncias do Valkey de forma assíncrona. Defina `VALKEY_USE_SLAVE_CONNECTION` como `true` para usar uma conexão _somente leitura_ com uma réplica Valkey para tráfego somente leitura, enquanto a instância primária manipula tráfego leitura-gravação. Essa conexão melhora o desempenho por meio do balanceamento de carga, pois somente um nó lida com o tráfego de leitura-gravação. Para remover uma matriz de conexão somente leitura existente de `env.php`, defina-a como `false`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Você deve ter um [Serviço Valkey configurado](../services/valkey.md) em `.magento.app.yaml` e `.magento/services.yaml`. A disponibilidade de uma conexão de réplica depende da topologia do projeto e da versão `ece-tools` instalada.

Antes de confiar nessa configuração, inspecione o valor `MAGENTO_CLOUD_RELATIONSHIPS` decodificado e confirme se há uma relação de réplica. Por exemplo:

```bash
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

Para `symfony_l2`, o suporte a réplicas requer as atualizações relevantes do `ece-tools` e de Patches de Nuvem. Atualize para a versão mais recente do `ece-tools` antes de habilitar esta configuração. Se nenhuma relação de réplica estiver presente após a reimplantação, entre em contato com o Suporte da Adobe Commerce.

Ao usar [`CACHE_CONFIGURATION`](#cache_configuration), mesclar substituições com suporte na configuração gerada em vez de substituir a estrutura de conexão gerada.

## `RESOURCE_CONFIGURATION`

- **Padrão**—Não definido

Mapeia um nome de recurso para uma conexão de banco de dados. Esta configuração corresponde à seção `resource` do arquivo `env.php`.

{{merge-options}}

O exemplo a seguir mescla novos valores com uma configuração existente:

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **Padrão**—`4`

Especifica qual nível de compactação [gzip](https://www.gnu.org/software/gzip) (`0` a `9`) usar ao compactar conteúdo estático. Defina como `0` para desabilitar a compactação.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Padrão**—`600`

Quando o tempo necessário para compactar os ativos estáticos excede o tempo limite de compactação, ele interrompe o processo de implantação. Defina o tempo máximo de execução, em segundos, para o comando static content compression.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Padrão**—_Não definido_

Você pode configurar vários locais por tema. Essa personalização acelera o processo de implantação, reduzindo o número de arquivos de tema desnecessários. Por exemplo, você pode implantar o tema _magento/backend_ em inglês e um tema personalizado em outros idiomas.

O exemplo a seguir implanta o tema `Magento/backend` com três localidades:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Além disso, você pode optar por _não_ implantar um tema:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Padrão**—_Não definido_

Permite aumentar o tempo de execução máximo esperado para implantação de conteúdo estático.

Por padrão, o Adobe Commerce define a execução máxima esperada para 900 segundos, mas alguns cenários exigem mais tempo para concluir a implantação de conteúdo estático para um projeto na nuvem.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Padrão**—`false`

Na fase de implantação, defina `SCD_NO_PARENT: true` para que a geração de conteúdo estático para temas principais não ocorra durante a fase de implantação. Essa configuração minimiza o tempo de implantação e evita o tempo de inatividade do site que pode ocorrer se a compilação de conteúdo estático falhar durante a implantação. Consulte [Implantação de conteúdo estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Padrão**—`quick`

Permite personalizar a [estratégia de implantação](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy) para conteúdo estático. Consulte [Implantar arquivos de exibição estáticos](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment).

Use estas opções _somente_ se você tiver mais de uma localidade:

- `standard`—implanta todos os arquivos de exibição estática para todos os pacotes.
- `quick`—(_padrão_) minimiza o tempo de implantação.
- `compact` — preserva o espaço em disco no servidor.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Padrão**—Automático

Define o número de threads para implantação de conteúdo estático. O valor padrão é definido com base na contagem de threads do CPU detectada e não excede um valor 4. Aumentar o número de threads acelera a implantação de conteúdo estático. Diminuir o número de threads o torna mais lento. Você pode definir o valor da thread, por exemplo:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Para reduzir ainda mais o tempo de implantação, use o [Gerenciamento de Configuração](../store/store-settings.md) com o comando `scd-dump` para mover a implantação estática para a fase de compilação.

## `SEARCH_CONFIGURATION`

- **Padrão**—_Não definido_

Use essa variável de ambiente para manter configurações personalizadas do serviço de pesquisa entre implantações. Por exemplo:

Configuração do Elasticsearch:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

Configuração do OpenSearch (para Commerce 2.4.6 e posterior):

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

O exemplo a seguir mescla um novo valor com a configuração existente:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Padrão**—_Não definido_

Use `SESSION_CONFIGURATION` para configurar o armazenamento da sessão. O exemplo abaixo usa a estrutura de configuração de sessão compatível com Redis. Use-a somente com a combinação de nome e serviço do armazenamento de sessão compatível com a versão exata do Commerce. Para sessões com suporte de Valkey, siga o [exemplo de armazenamento de sessão Valkey](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#apply-all-best-practice-recommendations).

Não suponha que variáveis de cache como `VALKEY_BACKEND` ou `REDIS_BACKEND` configurem sessões. A configuração de cache e sessão é independente. Em projetos na nuvem, use o relacionamento de serviço e a configuração gerada, quando possível; não codifique valores específicos do ambiente sem substituir o host e a porta de exemplo.

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: 'redis.internal'
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

Substitua `redis.internal` e `6379` pelo host do serviço de sessão e pela porta para o ambiente de destino quando a configuração de implantação exigir detalhes de conexão explícitos.

{{merge-options}}

O exemplo a seguir mescla um novo valor com a configuração existente:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Padrão**— _Não definido_

Defina como `true` para ignorar a implantação de conteúdo estático durante a fase de implantação.

Na fase de implantação, defina `SKIP_SCD: true` para que a compilação de conteúdo estático não ocorra durante a fase de implantação. Essa configuração minimiza o tempo de implantação e evita o tempo de inatividade do site que pode ocorrer se a compilação de conteúdo estático falhar durante a implantação. Consulte [Implantação de conteúdo estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Padrão**—`true`

Na implantação, substitua as URLs de base do Adobe Commerce no banco de dados pelas URLs de projeto especificadas pela variável [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Essa configuração é útil para desenvolvimento local, em que os URLs básicos são configurados para o ambiente local. Quando você implanta em um ambiente de nuvem, os URLs são atualizados para que você possa acessar sua vitrine e o administrador usando os URLs do projeto.

Se você precisar atualizar URLs ao implantar em ambientes Pro ou Starter de Preparo e Produção, use a variável [`FORCE_UPDATE_URLS`](#force_update_urls).

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `USE_LUA`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.4.7 e posterior

Controla a opção de back-end do cache `use_lua` em `env.php` para o front-end do cache padrão (e, ao usar o back-end `symfony_l2`, as opções de back-end remoto do front-end `stale_cache_enabled`). Esta opção não é aplicada ao front-end do `page_cache`.

Use o valor padrão `false`, a menos que o suporte da Adobe direcione explicitamente o contrário.

```yaml
stage:
  deploy:
    USE_LUA: false
```

>[!WARNING]
>
>No Adobe Commerce 2.4.7 e 2.4.8, a configuração `USE_LUA: true` pode causar corrupção de cache e problemas de perda de cache do GraphQL.
>
>A partir do Adobe Commerce 2.4.9, use as orientações de configuração de cache Valkey para a sua versão do Commerce e não confie no `USE_LUA` para novas implantações.

## `LUA_KEY`

A variável `LUA_KEY` está obsoleta. Se `LUA_KEY` estiver incluído em `.magento.env.yaml`, remova-o durante a migração. Em vez disso, use as variáveis `USE_LUA` e `USE_LUA_ON_GC`.

## `USE_LUA_ON_GC`

- **Padrão**—`true`
- **Versão** — Adobe Commerce 2.4.8 e posterior

Controla a opção de back-end do cache `use_lua_on_gc` em `env.php` para o front-end do cache padrão (e, ao usar o back-end `symfony_l2`, as opções de back-end remoto do front-end `stale_cache_enabled`) para a coleta de lixo. Esta opção não é aplicada ao front-end do `page_cache`.

Use o valor padrão `true` para preservar a limpeza de marca de cache atômica durante o trabalho de cron `backend_clean_cache`.

```yaml
stage:
  deploy:
    USE_LUA_ON_GC: true
```

>[!WARNING]
>
>No Adobe Commerce 2.4.8, a configuração `USE_LUA_ON_GC: false` pode fazer com que a invalidação do cache com base em tags falhe silenciosamente e exigir uma liberação de cache completa para recuperação.
>
>Nas versões da 2.4.9 e posteriores, siga as [orientações do serviço de cache](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache) para a versão instalada.

## `VERBOSE_COMMANDS`

- **Padrão**—_Não definido_

Habilite ou desabilite o nível de detalhamento de depuração [Symfony](https://symfony.com/doc/current/console/verbosity.html) para comandos CLI `bin/magento` executados durante a fase de implantação.

>[!NOTE]
>
>Para usar a configuração VERBOSE_COMMANDS para controlar os detalhes na saída do comando para comandos CLI `bin/magento` com êxito e com falha, você deve definir [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Escolha o nível de detalhes fornecido nos logs:

- `-v`= saída normal
- `-vv`= mais saída detalhada
- `-vvv` = saída detalhada ideal para depuração

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
