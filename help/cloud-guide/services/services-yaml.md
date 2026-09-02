---
title: Configurar serviços
description: Saiba como configurar serviços usados pelo Adobe Commerce na infraestrutura em nuvem, como MySQL, Redis e Elasticsearch.
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
TQID: https://experienceleague.adobe.com/qvCjqNc8E9QGme-zM42vMg-kb1WjwTlWUqjbm-NI2bg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
last-update: 2026-09-01
source-git-commit: 0f88ef7d75bc2a02eb7988dc815071c5894a4662
workflow-type: tm+mt
source-wordcount: 1176
ht-degree: 0%

---

# Configurar serviços

O arquivo `services.yaml` define os serviços com suporte e usados pelo Adobe Commerce na infraestrutura em nuvem, como MySQL, Redis ou Valkey, Elasticsearch ou OpenSearch. Não é necessário assinar provedores de serviços externos.

>[!NOTE]
>
>O arquivo `.magento/services.yaml` é gerenciado localmente no diretório `.magento` do seu projeto. Durante a implantação, o Adobe Commerce na infraestrutura em nuvem usa essa configuração para provisionar serviços compatíveis com o ambiente de destino. O diretório `.magento` é removido do servidor remoto após a implantação, portanto, `services.yaml` não existe no ambiente implantado.

O script de implantação usa os arquivos de configuração no diretório `.magento` para provisionar o ambiente com os serviços configurados. Um serviço ficará disponível para o aplicativo se for incluído na propriedade [`relationships`](../application/properties.md#relationships) do arquivo `.magento.app.yaml`. O arquivo `services.yaml` contém os valores de _tipo_ e _disco_. O tipo de serviço define o serviço _nome_ e _versão_.

A configuração do serviço em `.magento/services.yaml` é separada das dependências do pacote PHP e Composer definidas em `composer.json` e bloqueadas em `composer.lock`.

## Quando as alterações de serviço se aplicam

Alterar uma configuração de serviço faz com que uma implantação provisione o ambiente com os serviços atualizados, o que afeta os seguintes ambientes:

- Todos os ambientes iniciais, incluindo Produção `master`
- Ambientes de integração Pro

{{$include /help/_includes/pro-services-support.md}}

## Serviços padrão e compatíveis

A infraestrutura do Adobe Commerce na nuvem é compatível com os seguintes serviços, que podem ser configurados para o seu projeto:

- [AtiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md) ou [Valkey](valkey.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

>[!NOTE]
>[Atualizar RabbitMQ sequencialmente entre as versões disponíveis](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/rabbitmq#upgrading-the-rabbitmq-service). Por exemplo, não atualize da 3.9 diretamente para a 4.1.
>
>Para garantir que suas filas de mensagens personalizadas sejam recriadas no RabbitMQ depois da atualização para uma nova versão, acione uma implantação completa.

## Exibir versões e serviços configurados

Você pode exibir exemplos de definições de serviço e valores de disco no arquivo de modelo atual [`services.yaml` ](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). As versões padrão e compatíveis do serviço dependem da versão do Adobe Commerce e do modelo de nuvem atual.

O exemplo a seguir mostra definições de serviço no arquivo de configuração `services.yaml`:

```yaml
mysql:
    type: mysql:11.8
    disk: 5120

cache:
    type: valkey:9.0

opensearch:
    type: opensearch:3  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:4.3
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## Valores de serviço

Forneça a ID de serviço e a configuração de tipo de serviço `type: <name>:<version>`. Se o serviço usar armazenamento persistente, você deverá fornecer um valor de disco.

Usar o seguinte formato:

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

O valor `service-id` identifica o serviço no projeto. Você só pode usar caracteres alfanuméricos minúsculos: `a` a `z` e `0` a `9`, como `valkey`.

O valor _service-id_ é usado na propriedade [`relationships`](../application/properties.md#relationships) do arquivo de configuração `.magento.app.yaml`:

```yaml
relationships:
    valkey: "valkey:valkey"
```

Você pode nomear várias instâncias de cada tipo de serviço. Por exemplo, você pode usar várias instâncias Valkey, uma para sessão e outra para cache.

```yaml
valkey:
    type: valkey:<version>

valkey2:
    type: valkey:<version>
```

Renomeando um serviço no arquivo `services.yaml`:

- O serviço existente antes de criar um serviço com o novo nome especificado.
- Todos os dados existentes para o serviço são removidos. A Adobe recomenda que você [faça backup do ambiente de Início](../storage/snapshots.md) antes de alterar o nome de um serviço existente.

### `type`

O valor `type` especifica o nome e a versão do serviço. Por exemplo:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

O valor `disk` especifica o tamanho do armazenamento de disco persistente (em MB) a ser alocado para o serviço. Os serviços que usam armazenamento persistente, como o MySQL, devem fornecer um valor de disco. Os serviços que usam memória em vez de armazenamento persistente, como Valkey, não exigem um valor de disco.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

A quantidade de armazenamento padrão atual por projeto é de 5 GB ou 5120 MB. Você pode distribuir essa quantidade entre seu aplicativo e cada um de seus serviços.

## Relacionamentos de serviço

Em projetos de infraestrutura na nuvem do Adobe Commerce, as [relações](../application/properties.md#relationships) do serviço configuradas no arquivo `.magento.app.yaml` determinam quais serviços estão disponíveis para o seu aplicativo.

Você pode recuperar os dados de configuração de todas as relações de serviço da variável de ambiente [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md). Os dados de configuração incluem o nome, o tipo e a versão do serviço, juntamente com todos os detalhes de conexão necessários, como o número da porta e as credenciais de logon.

### Verificar relações do ambiente de desenvolvimento local

1. No ambiente de desenvolvimento local, mostre as relações do ambiente ativo.

   ```bash
   magento-cloud relationships
   ```

1. Confirme os `service` e `type` da resposta. A resposta fornece informações de conexão, como endereço IP e número da porta.

   >Resposta de amostra abreviada

   ```yaml
   valkey:
       -
   ...
           type: 'valkey:8.0'
           port: 6379
   opensearch:
       -
   ...
           type: 'opensearch:3'
           port: 9200
   database:
       -
   ...
           type: 'mysql:11.8'
           port: 3306
   ```

### Verificar relações em ambientes remotos

1. Use o SSH para fazer logon no ambiente remoto.

1. Listar os dados de configuração de relacionamentos para todos os serviços configurados no ambiente.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou use o seguinte comando `ece-tools` para exibir relações:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Confirme os `service` e `type` da resposta. A resposta fornece informações de conexão, como endereço IP, número da porta e credenciais de nome de usuário e senha necessárias.

## Versões de serviço

As versões implantadas e testadas na infraestrutura em nuvem determinam a versão do serviço e o suporte à compatibilidade do Adobe Commerce na infraestrutura em nuvem, que às vezes diferem das versões compatíveis com implantações locais do Adobe Commerce. Consulte [Requisitos do sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) no guia _Instalação_ para obter uma lista de dependências de software de terceiros que a Adobe testou com versões específicas do Adobe Commerce e do Magento Open Source.

### Verificações de EOL de software

Durante o processo de implantação, o pacote `ece-tools` verifica as versões de serviço instaladas em relação às datas de fim da vida útil (EOL) de cada serviço.

- Se uma versão do serviço estiver dentro de três meses da data EOL, uma notificação será exibida no log de implantação.
- Se a data EOL estiver no passado, uma notificação de aviso será exibida.

Para manter a segurança da loja, atualize as versões de software instaladas antes que elas atinjam o fim da vida útil. Você pode examinar as datas de fim de vida útil no [arquivo de `eol.yaml` das ferramentas ece](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Migrar para o OpenSearch

{{elasticsearch-support}}

Para o Adobe Commerce versão 2.4.4 e posterior, consulte [Configurar o serviço OpenSearch](opensearch.md).

## Alterar versão do serviço

É possível atualizar a versão do serviço instalada para compatibilidade com a versão do Adobe Commerce implantada no ambiente de nuvem.

Não é possível fazer o downgrade da versão de serviço de um serviço instalado diretamente. No entanto, é possível criar um serviço com a versão necessária. Consulte [Rebaixar versão de serviço](#downgrade-version).

### Atualizar versão do serviço instalado

Você pode atualizar a versão do serviço instalado atualizando a configuração do serviço no arquivo `services.yaml`.

1. Alterar o valor [`type`](#type) do serviço no arquivo `.magento/services.yaml`:

   > Definição do serviço original

   ```yaml
   mysql:
       type: mysql:11.8
       disk: 2048
   ```

   > Definição de serviço atualizada

   ```yaml
   mysql:
       type: mysql:12.3
       disk: 5120
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 11.8 to 12.3."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Versão de downgrade

Não é possível fazer o downgrade de um serviço instalado diretamente. Você tem duas opções:

1. Renomeie um serviço existente com a nova versão, que remove o serviço e os dados existentes, e adiciona um novo.

1. Crie um serviço e salve os dados do serviço existente.

Ao alterar a versão do serviço, você deve atualizar a configuração do serviço no arquivo `services.yaml` e atualizar as relações no arquivo `.magento.app.yaml`.

#### Fazer downgrade de uma versão de serviço renomeando um serviço existente

1. Renomeie o serviço existente no arquivo `.magento/services.yaml` e altere a versão.

   >[!WARNING]
   >
   >A renomeação de um serviço existente o substitui e exclui todos os dados. Se precisar manter os dados, crie um serviço em vez de renomear o existente.

   Por exemplo, para baixar a versão do MariaDB para o serviço _mysql_ da versão 10.4 para 10.3, altere a configuração existente do _service-id_ e do _type_.

   > Definição original de `services.yaml`

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Nova definição `services.yaml`

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Atualizar as relações no arquivo `.magento.app.yaml`.

   > Configuração original de `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Atualização da configuração `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

#### Fazer downgrade de um serviço criando um serviço

1. Adicione uma definição de serviço ao arquivo `services.yaml` para seu projeto com a especificação de versão rebaixada. Consulte _mysql2_ no seguinte exemplo:

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Para usar o novo serviço, altere a configuração de relações no arquivo `.magento.app.yaml`.

   > Configuração original de `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Nova configuração `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.
