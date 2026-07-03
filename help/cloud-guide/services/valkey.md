---
title: Configurar o serviço Valkey
description: Saiba como configurar e otimizar o Valkey como uma solução de cache de back-end para o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: f1e6c9da5dacb144dc3e1a09885c1a9b11ce54ee
workflow-type: tm+mt
source-wordcount: 259
ht-degree: 0%

---

# Configurar o serviço Valkey

[Valkey](https://valkey.io) é uma solução de cache de back-end opcional que substitui o `Zend Framework Zend_Cache_Backend_File`, que a Adobe Commerce usa por padrão. Se você substituir o padrão na versão 2.4.9+ do Commerce ou versões de patch posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 e as linhas de versão 2.4.8-p5, deverá usar Valkey.

Consulte [Configurar Valkey](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration){target="_blank"} no _Guia de práticas recomendadas do manual de implementação_.

{{service-instruction}}

**Para substituir Redis por Valkey, atualize a configuração nos três arquivos a seguir**:

1. Substitua a configuração Redis pelo nome e tipo Valkey necessários no arquivo `.magento/services.yaml`.

   ```yaml
   cache:
       type: valkey:<version>
   ```

   Para fornecer sua própria configuração Valkey, adicione uma chave `core_config` em seu arquivo `.magento/services.yaml`:

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. Configure as relações no arquivo `.magento.app.yaml`.

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. Configure `.magento.env.yaml` para substituir a configuração Redis da seguinte maneira:.

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Verifique as relações de serviço](services-yaml.md#service-relationships).

{{service-change-tip}}

{{valkey-newrelic}}

## Uso da CLI do Valkey

Supondo que sua relação com Valkey tenha o nome `valkey`, você poderá acessá-la usando a ferramenta `valkey-cli`.

1. Use o SSH para se conectar ao ambiente de integração com o Valkey instalado e configurado.

1. Abra um túnel SSH para um host.

   ```bash
   valkey-cli -h valkey.internal
   ```

## Obter versão do Valkey instalada

Use o comando a seguir para instalar a versão do Valkey em um ambiente de integração:

```bash
valkey-cli -h valkey.internal info | grep version
```

Resposta:

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valkey no preparo e produção Pro

Para obter a versão Valkey instalada em um ambiente de preparo ou produção, use o comando `valkey-server`:

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

Use o comando a seguir para obter a configuração do Valkey instalada em um ambiente Pro Staging ou Production:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Resposta:

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
