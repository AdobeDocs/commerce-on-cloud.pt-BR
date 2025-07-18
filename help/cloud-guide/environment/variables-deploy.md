---
title: Implantar variáveis
description: Consulte a lista de variáveis de ambiente que controlam ações na fase de implantação do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 3f2a4f7dc9c23afb3af80304023d9e742c974ccd
workflow-type: tm+mt
source-wordcount: '2483'
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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Configurar página Redis e cache padrão. Ao definir o parâmetro `cm_cache_backend_redis`, especifique as opções `server`, `port` e `database`.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

O exemplo a seguir mescla novos valores com uma configuração existente:

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

O exemplo a seguir usa o [recurso de pré-carregamento Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html?lang=pt-BR#redis-preload-feature) conforme definido no _Guia de configuração_:

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

Para usar um modelo [REDIS_BACKEND](#redis_backend) personalizado (não apenas da lista de permissões), defina a opção `_custom_redis_backend` como `true` para habilitar a validação correta, como no exemplo a seguir:

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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Habilita ou desabilita a limpeza de [arquivos de conteúdo estático](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=pt-BR) gerados durante a fase de compilação ou implantação. Use o valor padrão _true_ em desenvolvimento como uma prática recomendada.

- **`true`** — Remove todo o conteúdo estático existente antes de implantar o conteúdo estático atualizado.
- **`false`** — A implantação somente substitui arquivos de conteúdo estático existentes se o conteúdo gerado contiver uma versão mais recente.

Se você modificar o conteúdo estático por meio de um processo separado, defina o valor como _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Falha ao limpar arquivos de exibição estáticos antes da implantação pode causar problemas se você implantar atualizações em arquivos existentes sem remover as versões anteriores. Devido às regras de [fallback de arquivo estático](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache), as operações de fallback poderão exibir o arquivo errado se o diretório contiver várias versões do mesmo arquivo.

## `CRON_CONSUMERS_RUNNER`

- **Padrão**—`cron_run = false`, `max_messages = 1000`
- **Versão** — Adobe Commerce 2.2.0 e posterior

Use essa variável de ambiente para confirmar se as filas de mensagens estão em execução após uma implantação.

- `cron_run` — Um valor booleano que habilita ou desabilita o trabalho cron `consumers_runner` (padrão = `false`).
- `max_messages` — Um número que especifica o número máximo de mensagens que cada consumidor deve processar antes de terminar (padrão = `1000`). Você pode definir o valor como `0` para impedir que o consumidor seja encerrado.
- `consumers` — Uma matriz de cadeias de caracteres que especifica quais consumidores executar. Uma matriz vazia executa _todos_ consumidores.

- `multiple_processes`-Um número que especifica o número de processos a serem gerados para cada consumidor. Com suporte no Commerce **2.4.4** ou posterior.

>[!NOTE]
>
>Para retornar uma lista da fila de mensagens `consumers`, execute o comando `./bin/magento queue:consumers:list` no ambiente remoto.

Exemplo de matriz que executa o `consumers` específico e o `multiple_processes` para gerar para cada consumidor:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Exemplo de uma matriz vazia que executa todos os `consumers`:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Por padrão, o processo de implantação substitui todas as configurações no arquivo `env.php`. Consulte [Gerenciar filas de mensagens](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html?lang=pt-BR) no _Guia de Configuração do Commerce_ para Adobe Commerce local.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.2.0 e posterior

Configure como `consumers` processa mensagens da fila de mensagens escolhendo uma das seguintes opções:

- `false`—`Consumers` processar mensagens disponíveis na fila, fechar a conexão TCP e encerrar. `Consumers` não espere mensagens adicionais entrarem na fila, mesmo se o número de mensagens processadas for menor que o valor `max_messages` especificado na variável de implantação `CRON_CONSUMERS_RUNNER`.

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
- **Versão** — Adobe Commerce 2.1.4 e posterior

>[!WARNING]
>
>Defina o valor `CRYPT_KEY` por meio do [!DNL Cloud Console] em vez do arquivo `.magento.env.yaml` para evitar a exposição da chave no repositório de código-fonte de seu ambiente. Consulte [Definir variáveis de ambiente e projeto](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html?lang=pt-BR#configure-environment).

Ao mover o banco de dados de um ambiente para outro sem um processo de instalação, você precisará das informações criptográficas correspondentes. O Adobe Commerce usa o valor da chave de criptografia definido em [!DNL Cloud Console] como o valor `crypt/key` no arquivo `env.php`.

## `DATABASE_CONFIGURATION`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

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
- **Versão** — Adobe Commerce 2.2.0 e posterior

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
>Em um cluster Pro de Preparo/Produção que tenha três nós (ou três nós de serviço na [Arquitetura em Escala](https://experienceleague.adobe.com/pt-br/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)), o `indices_settings` deve ser definido da seguinte maneira:
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

O exemplo a seguir mescla um novo valor com a configuração existente:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Ativa e desativa o Google Analytics ao implantar em ambientes de Preparo e Integração. Por padrão, o Google Analytics é verdadeiro somente para o ambiente de Produção. Defina esse valor como `true` para habilitar o Google Analytics nos ambientes de Preparo e Integração.

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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Na implantação em ambientes Pro ou Starter de Preparo e Produção, essa variável substitui as URLs de base do Adobe Commerce no banco de dados pelas URLs de projeto especificadas pela variável [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Use esta configuração para substituir o comportamento padrão da variável de implantação [UPDATE_URLS](#update_urls), que é ignorada ao implantar em ambientes de Preparo ou Produção.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Padrão**—`file`
- **Versão** — Adobe Commerce 2.2.5 e posterior

O provedor de bloqueio impede a inicialização de trabalhos cron duplicados e grupos cron. Use o provedor de bloqueio `file` no ambiente de Produção. Os ambientes iniciais e o ambiente de integração Pro não usam a variável [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md), portanto, o `ece-tools` aplica automaticamente o provedor de bloqueio `db`.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Consulte [Configurar o bloqueio](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html?lang=pt-BR) no _Guia de instalação_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.1.4 e posterior

>[!TIP]
>
>A variável `MYSQL_USE_SLAVE_CONNECTION` é compatível somente com ambientes de cluster Adobe Commerce em Staging e Production Pro de infraestrutura em nuvem e não é compatível com projetos iniciais.

O Adobe Commerce pode ler vários bancos de dados de forma assíncrona. Defina como `true` para usar automaticamente uma conexão _somente leitura_ com o banco de dados para receber tráfego somente leitura em um nó não mestre. Essa conexão melhora o desempenho por meio do balanceamento de carga, pois somente um nó lida com o tráfego de leitura-gravação. Defina como `false` para remover qualquer matriz de conexão somente leitura existente do arquivo `env.php`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Quando a variável `MYSQL_USE_SLAVE_CONNECTION` está definida como `true`, o parâmetro `synchronous_replication` é definido como `true` por padrão no arquivo `env.php` nos ambientes de Preparo e Produção Profissionais. Quando o `MYSQL_USE_SLAVE_CONNECTION` está definido como `false`, o parâmetro `synchronous_replication` não está configurado.

## `QUEUE_CONFIGURATION`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Use essa variável de ambiente para manter configurações de serviço AMQP personalizadas entre implantações. Por exemplo, se você preferir usar um serviço de fila de mensagens existente, em vez de depender da infraestrutura de nuvem para criá-lo para você, use a variável de ambiente `QUEUE_CONFIGURATION` para conectá-lo ao seu site:

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
- **Versão** — Adobe Commerce 2.3.0 e posterior

Especifica a configuração do modelo de back-end para o cache Redis.

O Adobe Commerce versão 2.3.0 e posterior inclui os seguintes modelos de back-end:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

O exemplo de como definir `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Se você especificar `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` como o modelo de back-end do Redis para habilitar o [cache L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=pt-BR), o `ece-tools` gerará a configuração do cache automaticamente. Veja um exemplo de [arquivo de configuração](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=pt-BR#configuration-example) no _Guia de Configuração do Adobe Commerce_. Para substituir a configuração de cache gerada, use a variável de implantação [CACHE_CONFIGURATION](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.1.16 e posterior

>[!WARNING]
>
>_não_ habilite esta variável em um projeto de [arquitetura escalonada](../architecture/scaled-architecture.md). Isso causa erros de conexão Redis. Os escravos Redis ainda estão ativos, mas não são usados para leituras Redis. Como alternativa, a Adobe recomenda o uso do Adobe Commerce 2.3.5 ou posterior, a implementação de uma nova configuração de back-end do Redis e a implementação do cache L2 para Redis.

>[!TIP]
>
>A variável `REDIS_USE_SLAVE_CONNECTION` é compatível somente com ambientes de cluster Adobe Commerce em Staging e Production Pro de infraestrutura em nuvem e não é compatível com projetos iniciais.

O Adobe Commerce pode ler várias instâncias de Redis de forma assíncrona. Defina como `true` para usar automaticamente uma conexão _somente leitura_ com uma instância Redis para receber tráfego somente leitura em um nó não mestre. Essa conexão melhora o desempenho por meio do balanceamento de carga, pois somente um nó lida com o tráfego de leitura-gravação. Defina como `false` para remover qualquer matriz de conexão somente leitura existente do arquivo `env.php`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Você deve ter um serviço Redis configurado no arquivo `.magento.app.yaml` e no arquivo `services.yaml`.

[ECE-Tools versão 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) e posterior usa mais configurações tolerantes a falhas. Se o Adobe Commerce não puder ler dados da instância _slave_ do Redis, ele lerá os dados da instância _master_ do Redis.

A conexão somente leitura não está disponível para uso no ambiente de integração ou se você usar a variável [`CACHE_CONFIGURATION` ](#cache_configuration).

## `VALKEY_BACKEND`

- **Padrão**—`Cm_Cache_Backend_Redis`
- **Versão** — Adobe Commerce 2.8.0 e posterior

`VALKEY_BACKEND` especifica a configuração do modelo de back-end para o cache Valkey.

O Adobe Commerce versão 2.8.0 e posterior inclui os seguintes modelos de back-end:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

O exemplo a seguir descreve como definir `VALKEY_BACKEND`:

```yaml
stage:
  deploy:
  VALKEY_USE_SLAVE_CONNECTION: true
  VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Se você especificar `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` como o modelo de back-end Valkey para habilitar o [cache L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=pt-BR), o `ece-tools` gerará a configuração de cache automaticamente. Veja um exemplo de [arquivo de configuração](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=pt-BR#configuration-example) no _Guia de Configuração do Adobe Commerce_. Para substituir a configuração de cache gerada, use a variável de implantação [CACHE_CONFIGURATION](#cache_configuration).

## `VALKEY_USE_SLAVE_CONNECTION`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.4.8 e posterior

>[!WARNING]
>
>_não_ habilite esta variável em um projeto de [arquitetura escalonada](../architecture/scaled-architecture.md). Isso causa erros de conexão do Valkey. Os escravos Redis ainda estão ativos, mas não são usados para leituras Redis. Como alternativa, a Adobe recomenda usar o Adobe Commerce 2.4.8 ou posterior, implementando uma nova configuração de back-end do Valkey e implementando o cache L2 para o Valkey.

>[!TIP]
>
>A variável `VALKEY_USE_SLAVE_CONNECTION` é compatível somente com ambientes de cluster Adobe Commerce em Staging e Production Pro de infraestrutura em nuvem e não é compatível com projetos iniciais.

O Adobe Commerce pode ler várias instâncias de Redis de forma assíncrona. `VALKEY_USE_SLAVE_CONNECTION` Defina como `true` para usar automaticamente uma conexão _somente leitura_ com uma instância Redis para receber tráfego somente leitura em um nó não mestre. Essa conexão melhora o desempenho por meio do balanceamento de carga, pois somente um nó lida com o tráfego de leitura-gravação. Defina `VALKEY_USE_SLAVE_CONNECTION` como `false` para remover qualquer matriz de conexão somente leitura existente do arquivo `env.php`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Você deve ter um serviço Redis configurado no arquivo `.magento.app.yaml` e no arquivo `services.yaml`.

[ECE-Tools versão 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) e posterior usa mais configurações tolerantes a falhas. Se o Adobe Commerce não puder ler dados da instância Valkey _slave_, ele lerá dados da instância Redis _master_.

A conexão somente leitura não está disponível para uso no ambiente de integração ou se você usar a variável [`CACHE_CONFIGURATION` ](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Padrão**—Não definido
- **Versão** — Adobe Commerce 2.1.4 e posterior

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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Especifica qual nível de compactação [gzip](https://www.gnu.org/software/gzip) (`0` a `9`) usar ao compactar conteúdo estático; `0` desabilita a compactação.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Padrão**—`600`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Quando o tempo necessário para compactar os ativos estáticos excede o tempo limite de compactação, ele interrompe o processo de implantação. Defina o tempo máximo de execução, em segundos, para o comando static content compression.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

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
- **Versão** — Adobe Commerce 2.2.0 e posterior

Permite aumentar o tempo de execução máximo esperado para implantação de conteúdo estático.

Por padrão, o Adobe Commerce define a execução máxima esperada para 900 segundos, mas em alguns cenários, pode ser necessário mais tempo para concluir a implantação do conteúdo estático para um projeto na nuvem.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.4.2 e posterior

Na fase de implantação, defina `SCD_NO_PARENT: true` para que a geração de conteúdo estático para temas principais não ocorra durante a fase de implantação. Essa configuração minimiza o tempo de implantação e evita o tempo de inatividade do site que pode ocorrer se a compilação de conteúdo estático falhar durante a implantação. Consulte [Implantação de conteúdo estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Padrão**—`quick`
- **Versão** — Adobe Commerce 2.2.0 e posterior

Permite personalizar a [estratégia de implantação](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=pt-BR) para conteúdo estático. Consulte [Implantar arquivos de exibição estáticos](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=pt-BR).

Use estas opções _somente_ se você tiver mais de uma localidade:

- `standard`—implanta todos os arquivos de exibição estática para todos os pacotes.
- `quick`—(_padrão_) minimiza o tempo de implantação.
- `compact` — preserva o espaço em disco no servidor. No Adobe Commerce versão 2.2.4 e anterior, essa configuração substitui o valor de `scd_threads` por um valor de `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Padrão**—Automático
- **Versão** — Adobe Commerce 2.1.4 e posterior

Define o número de threads para implantação de conteúdo estático. O valor padrão é definido com base na contagem de threads do CPU detectada e não excede um valor 4. Aumentar o número de threads acelera a implantação de conteúdo estático; diminuir o número de threads o torna mais lento. Você pode definir o valor da thread, por exemplo:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Para reduzir ainda mais o tempo de implantação, use o [Gerenciamento de Configuração](../store/store-settings.md) com o comando `scd-dump` para mover a implantação estática para a fase de compilação.

## `SEARCH_CONFIGURATION`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Configurar o armazenamento de sessão Redis. Requer as opções `save`, `redis`, `host`, `port` e `database` para a variável de armazenamento de sessão. Por exemplo:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

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
- **Versão** — Adobe Commerce 2.1.4 e posterior

Defina como `true` para ignorar a implantação de conteúdo estático durante a fase de implantação.

Na fase de implantação, defina `SKIP_SCD: true` para que a compilação de conteúdo estático não ocorra durante a fase de implantação. Essa configuração minimiza o tempo de implantação e evita o tempo de inatividade do site que pode ocorrer se a compilação de conteúdo estático falhar durante a implantação. Consulte [Implantação de conteúdo estático](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Padrão**—`true`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Na implantação, substitua as URLs de base do Adobe Commerce no banco de dados pelas URLs de projeto especificadas pela variável [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Essa configuração é útil para desenvolvimento local, em que os URLs básicos são configurados para o ambiente local. Quando você implanta em um ambiente de nuvem, os URLs são atualizados para que você possa acessar sua vitrine e o administrador usando os URLs do projeto.

Se você precisar atualizar URLs ao implantar em ambientes Pro ou Starter de Preparo e Produção, use a variável [`FORCE_UPDATE_URLS`](#force_update_urls).

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

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
