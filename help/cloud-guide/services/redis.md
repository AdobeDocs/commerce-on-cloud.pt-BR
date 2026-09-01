---
title: Configurar o serviço Redis
description: Saiba como configurar e otimizar o Redis como uma solução de cache de back-end para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# Configurar o serviço Redis

[Redis](https://redis.io) é uma solução de cache back-end opcional que substitui o `Zend Framework Zend_Cache_Backend_File`, que a Adobe Commerce usa por padrão.

>[!IMPORTANT]
>
>O cache Redis não é suportado para o Adobe Commerce 2.4.9 ou versões de patch posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 e 2.4.8-p4. Use [Valkey](valkey.md) para configuração de cache onde Redis não é suportado. Consulte [Requisitos do sistema](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/installation-guide/system-requirements) para obter os serviços de cache com suporte por versão.

{{service-instruction}}

## Ativar Redis

Para habilitar o Redis, atualize os seguintes arquivos:

- `.magento/services.yaml`
- `.magento.app.yaml`

### Configurar o serviço

Em `.magento/services.yaml`, adicione a definição do serviço Redis. Substitua `<version>` por uma versão Redis com suporte pela sua versão do Adobe Commerce e pelo modelo atual da Nuvem.

```yaml
cache:
  type: redis:<version>
```

Por exemplo, para uma versão do Commerce e um modelo da nuvem compatíveis com Redis 7.2:

```yaml
cache:
  type: redis:7.2
```

A versão de exemplo não é universal. As versões de serviço padrão e compatíveis reais dependem da versão do Adobe Commerce, do nível de patch e do modelo atual do Cloud. Verifique a combinação com suporte em [Requisitos do Sistema](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/installation-guide/system-requirements) e o modelo de projeto atual.

### Configurar o relacionamento de serviço

Em `.magento.app.yaml`, configure a relação entre o aplicativo e o serviço Redis:

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

A chave de relação `redis` é o nome usado pelo aplicativo para acessar o serviço. O valor `cache:redis` consiste na ID de serviço (`cache`) e no tipo de serviço (`redis`) definidos em `.magento/services.yaml`.

### Confirmar e implantar as alterações

Adicionar, confirmar e enviar as alterações de configuração:

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

Depois que a implantação for concluída, verifique se o relacionamento do serviço Redis está disponível.

{{service-change-tip}}

## Verificar o relacionamento de serviço

Após implantar a configuração, execute o seguinte comando em um contêiner de aplicativo para exibir o objeto `MAGENTO_CLOUD_RELATIONSHIPS` decodificado:

Use o SSH para se conectar ao ambiente de nuvem remoto e execute:

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

O comando exibe todos os relacionamentos de serviço configurados. Localize a relação `redis` para identificar os detalhes da conexão Redis.

O exemplo abreviado a seguir mostra a relação `redis`. Não é um esquema universal.

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

A saída varia de acordo com a configuração do ambiente e do serviço. Não codifique nomes de host, portas, endereços IP, nomes de cluster, versões de serviço, nomes de usuário ou senhas deste exemplo. Use os valores retornados por `MAGENTO_CLOUD_RELATIONSHIPS` no ambiente de destino.

Se `jq` estiver disponível, use o seguinte comando para exibir somente a relação Redis:

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

Para obter mais informações sobre relações de serviço, consulte [Configurar serviços](services-yaml.md).

## Personalizar a configuração Redis

Para obter recomendações de cache, sessão, L2 e conexão de réplica, consulte [Práticas recomendadas para a configuração do serviço Valkey e Redis](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) no _Guia de Práticas Recomendadas do Manual de Implementação_.

## Usando a CLI Redis

Supondo que sua relação com Redis tenha o nome `redis`, use o host e a porta retornados por `MAGENTO_CLOUD_RELATIONSHIPS` para se conectar ao Redis.

Conecte-se ao ambiente com o Redis instalado e configurado e execute o seguinte comando:

```terminal
redis-cli -h <host> -p <port>
```

**Exemplo**

```terminal
redis-cli -h redis.internal -p 6379
```

## Obtenha a versão instalada do Redis

>[!BEGINTABS]

>[!TAB Ambiente de integração]

Em um ambiente de Integração, use o host e a porta retornados pela relação `redis` para executar:

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**Exemplo de resposta**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

A versão e os detalhes da build variam de acordo com o ambiente. Não trate uma versão de exemplo exibida como uma versão obrigatória ou de serviço universal.

>[!TAB Preparo e produção profissionais]

Em ambientes de preparo e produção profissionais, execute:

```terminal
redis-server -v
```

**Exemplo de resposta**

```text
Redis server v=<installed-version> ...
```

A versão e os detalhes da build variam de acordo com o ambiente. Não trate uma versão de exemplo exibida como uma versão obrigatória ou de serviço universal.

>[!ENDTABS]

## Solução de problemas do Redis

Consulte os seguintes artigos de suporte da Adobe Commerce para obter ajuda com a solução de problemas Redis:

- [Alertas gerenciados no Adobe Commerce: alerta de aviso de memória Redis](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Alertas gerenciados no Adobe Commerce: alerta crítico de memória Redis](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### Erros de limpeza de cache fazem referência a Redis em um cache configurado pelo Valkey

Uma falha na limpeza do cache de pré-implantação pode exibir o código de erro `[107]` (`clean-redis-cache`) e uma mensagem `Connection to Redis` mesmo quando o serviço `cache` está configurado como Valkey. `ece-tools` usa este código de erro orientado por Redis herdado e a mensagem para a etapa de limpeza de cache, independentemente de qual serviço oferece suporte à relação `cache`, portanto, o texto não indica que o Redis está instalado.

Se o erro subjacente for uma falha de DNS, como `Name or service not known`, para o host de relação, a etapa de implantação foi executada antes de a relação de serviço estar disponível, ou o nome da relação em `.magento.app.yaml` não corresponde à ID de serviço em `.magento/services.yaml`. Consulte [Verificar a relação de serviço](#verify-the-service-relationship).
