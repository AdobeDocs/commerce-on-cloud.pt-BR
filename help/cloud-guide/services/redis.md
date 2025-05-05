---
title: Configurar o serviço Redis
description: Saiba como configurar e otimizar o Redis como uma solução de cache de back-end para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Configurar o serviço Redis

[Redis](https://redis.io) é uma solução de cache back-end opcional que substitui o Zend Framework Zend_Cache_Backend_File, que a Adobe Commerce usa por padrão.

Consulte [Configurar Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html?lang=pt-BR) no _Guia de configuração_.

{{service-instruction}}

**Para habilitar Redis**:

1. Adicione o nome e o tipo necessários ao arquivo `.magento/services.yaml`.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   Para fornecer sua própria configuração do Redis, adicione uma chave `core_config` em seu arquivo `.magento/services.yaml`:

   ```yaml
   cache:
       type: redis:<version>
   ```

1. Configure as relações no arquivo `.magento.app.yaml`.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [Verifique as relações de serviço](services-yaml.md#service-relationships).

{{service-change-tip}}

## Usando a CLI Redis

Supondo que sua relação com Redis tenha o nome `redis`, você poderá acessá-la usando a ferramenta `redis-cli`.

1. Use o SSH para se conectar ao ambiente de integração com o Redis instalado e configurado.

1. Abra um túnel SSH para um host.

   ```bash
   redis-cli -h redis.internal
   ```

## Obter a versão do Redis instalada

Use o seguinte comando para obter a versão Redis instalada em um ambiente de integração:

```bash
redis-cli -h redis.internal info | grep version
```

Exemplo de resposta:

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redes em preparo e produção profissionais

Para obter a versão do Redis instalada em um ambiente de preparo ou produção, use o comando `redis-server`:

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

Use o seguinte comando para obter a configuração Redis instalada em um ambiente Pro Staging ou Production:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Exemplo de resposta:

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Solução de problemas do Redis

Consulte os seguintes artigos de suporte da Adobe Commerce para obter ajuda com a solução de problemas Redis:

- [Redis problema atrasar o logon ou o check-out do administrador](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html?lang=pt-BR)
- [Implementação do cache Redis estendido no Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html?lang=pt-BR)
- [Alertas gerenciados no Adobe Commerce: alerta de aviso de memória Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html?lang=pt-BR)
- [Alertas gerenciados no Adobe Commerce: alerta crítico de memória Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html?lang=pt-BR)
