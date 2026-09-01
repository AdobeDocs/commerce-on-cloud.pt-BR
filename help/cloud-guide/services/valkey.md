---
title: Configurar o serviço Valkey
description: Saiba como configurar e otimizar o Valkey como uma solução de cache de back-end para o Adobe Commerce na infraestrutura em nuvem, incluindo a substituição do Redis e a personalização das configurações de back-end do cache.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# Configurar o serviço Valkey

[Valkey](https://valkey.io) é uma solução de cache de back-end opcional para o Adobe Commerce na infraestrutura em nuvem. O Valkey é necessário ao substituir a configuração de cache padrão no Adobe Commerce 2.4.9 e posterior, ou em versões de patch posteriores a 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 e 2.4.8-p4.

{{service-instruction}}

## Configurar Valkey

Para substituir Redis por Valkey, atualize os seguintes arquivos:

- `.magento/services.yaml`
- `.magento.app.yaml`

### Configurar o serviço

Em `.magento/services.yaml`, substitua a definição do serviço Redis por uma definição de serviço Valkey. Substitua `<version>` por uma versão do Valkey com suporte pela sua versão do Adobe Commerce e pelo modelo atual do Cloud.

```yaml
cache:
  type: valkey:<version>
```

**Exemplo**

```yaml
cache:
  type: valkey:8.0
```

A versão de exemplo não é universal. As versões padrão e compatíveis do serviço dependem da versão do Adobe Commerce e do modelo atual da Cloud. Use a versão especificada pelo modelo de projeto atual. Consulte [Configurar serviços](services-yaml.md#service-versions) para obter mais informações.

>[!WARNING]
>
>Se você alterar a ID do serviço, o serviço existente será removido e um novo serviço será criado. Os dados existentes no serviço removido são excluídos permanentemente. Faça backup do ambiente antes de renomear um serviço.

Não presuma que os dados de cache e sessão persistem quando você altera o valor `type` de `redis:<version>` para `valkey:<version>`, mesmo quando mantém a mesma ID de serviço. Trate a migração como a criação de um novo cache: não há garantia de preservação dos dados existentes do cache e da sessão, e os usuários são desconectados após a conclusão da migração.

### Configurar o relacionamento de serviço

Em `.magento.app.yaml`, configure a relação entre o aplicativo e o serviço Valkey:

```yaml
relationships:
  valkey: "cache:valkey"
```

A chave de relação `valkey` é o nome usado pelo aplicativo para acessar o serviço. O valor `cache:valkey` faz referência à ID de serviço e ao tipo de serviço definidos em `.magento/services.yaml`.

>[!TIP]
>
>O Adobe Commerce se comunica com o Valkey por meio da biblioteca do cliente `credis`, que funciona em soquetes PHP simples por padrão. Para melhorar o desempenho, habilite a extensão PHP `redis` em `.magento.app.yaml`. O `credis` usa a extensão compilada automaticamente quando ela está disponível.
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### Confirmar e implantar as alterações

Adicionar, confirmar e enviar as alterações de configuração:

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

Depois que a implantação for concluída, verifique se o relacionamento de serviço da Valkey está disponível.

{{service-change-tip}}

{{valkey-newrelic}}

## Personalizar a configuração do Valkey

Para obter recomendações de cache, sessão, L2 e conexão de réplica, consulte [Práticas recomendadas para a configuração do serviço Valkey e Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) no _Guia de Práticas Recomendadas do Manual de Implementação_.

## Verificar o relacionamento de serviço

Para exibir o objeto `MAGENTO_CLOUD_RELATIONSHIPS` decodificado, execute o seguinte comando em um contêiner de aplicativo após implantar a configuração:

Use o SSH para se conectar ao ambiente de nuvem remoto e execute:

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

O comando exibe todos os relacionamentos de serviço configurados. Para identificar os detalhes da conexão Valkey, localize a relação valkey.

**Exemplo de saída**

O exemplo abreviado a seguir mostra a relação `valkey`. Não é um esquema universal.

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
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

A saída varia de acordo com a configuração do ambiente e do serviço. Não codifique nomes de host, portas, endereços IP, nomes de cluster, versões de serviço, nomes de usuário ou senhas deste exemplo. Use os valores retornados por `MAGENTO_CLOUD_RELATIONSHIPS` no ambiente de destino.

Se `jq` estiver disponível, exibir somente a relação Valkey:

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

Para obter mais informações sobre relações de serviço, consulte [Configurar serviços](services-yaml.md).

## Uso da CLI do Valkey

Supondo que sua relação com Valkey tenha o nome `valkey`, use o host e a porta retornados por `MAGENTO_CLOUD_RELATIONSHIPS` para se conectar ao Valkey:

```terminal
valkey-cli -h <host> -p <port>
```

**Exemplo**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## Obter a versão instalada do Valkey

>[!BEGINTABS]

>[!TAB Ambiente de integração]

Em um ambiente de Integração, use o host e a porta retornados pela relação `valkey` para executar:

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**Exemplo de resposta**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

A versão e os detalhes da build variam de acordo com o ambiente. Não trate uma versão de exemplo exibida como uma versão obrigatória ou de serviço universal.

>[!TAB Preparo e produção profissionais]

Em ambientes de preparo e produção profissionais, execute:

```terminal
valkey-server -v
```

**Exemplo de resposta**

```text
Valkey server v=<installed-version> ...
```

A versão e os detalhes da build variam de acordo com o ambiente. Não trate uma versão de exemplo exibida como uma versão obrigatória ou de serviço universal.

>[!ENDTABS]

## Solução de problemas do Valkey

### Erros de limpeza de cache fazem referência a Redis em um cache configurado pelo Valkey

Uma falha na limpeza do cache de pré-implantação pode exibir o código de erro `[107]` (`clean-redis-cache`) e uma mensagem `Connection to Redis` mesmo quando o serviço `cache` está configurado como Valkey. `ece-tools` usa este código de erro e mensagem para a etapa de limpeza de cache, independentemente de o serviço de cache de backup ser Redis ou Valkey.

Se o erro subjacente for uma falha de DNS, como `Name or service not known`, para o host de relação, a etapa de implantação foi executada antes de a relação de serviço estar disponível, ou o nome da relação em `.magento.app.yaml` não corresponde à ID de serviço em `.magento/services.yaml`. Consulte [Verificar a relação de serviço](#verify-the-service-relationship).
