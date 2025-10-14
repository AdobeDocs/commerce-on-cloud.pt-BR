---
title: Configurar o serviço AtiveMQ
description: Saiba como habilitar o serviço AtiveMQ Artemis para gerenciar filas de mensagens para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Configurar o serviço [!DNL ActiveMQ]

O [MQF (Estrutura da Fila de Mensagens)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) é um sistema do Adobe Commerce que permite que um [módulo](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module) publique mensagens em filas. Também define os consumidores que recebem as mensagens de forma assíncrona.

O MQF pode usar [AtiveMQ Artemis](https://activemq.apache.org/components/artemis/) como agente de mensagens, o que fornece uma plataforma escalável para enviar e receber mensagens. Também inclui um mecanismo para armazenar mensagens não entregues. [!DNL ActiveMQ Artemis] dá suporte ao protocolo STOMP (Streaming Text Oriented Messaging Protocol) para mensagens.

[!DNL ActiveMQ Artemis] está disponível como uma alternativa ao RabbitMQ para gerenciar filas de mensagens. É particularmente útil quando você precisa de recursos específicos para AtiveMQ ou quer usar o protocolo STOMP.

>[!IMPORTANT]
>
>Se preferir usar um serviço existente de agente de mensagens, como o [!DNL ActiveMQ], em vez de depender da Adobe Commerce na infraestrutura de nuvem para criá-lo para você, use a variável de ambiente [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) para conectá-lo ao seu site.

{{service-instruction}}

**Para habilitar AtiveMQ Artemis**:

1. Adicione o nome, o tipo e o valor de disco necessários (em MB) ao arquivo `.magento/services.yaml` junto com a versão do AtiveMQ instalada.

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. Configure as relações no arquivo `.magento.app.yaml`.

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Verifique as relações de serviço](services-yaml.md#service-relationships).

{{service-change-tip}}

## Conectar ao AtiveMQ para depuração

Para fins de depuração, você pode se conectar diretamente a uma instância de serviço de uma das seguintes maneiras:

- Conectar-se a partir do ambiente de desenvolvimento local
- Conectar a partir do aplicativo
- Conectar a partir do aplicativo PHP

### Conectar-se a partir do ambiente de desenvolvimento local

1. Faça logon na CLI do `magento-cloud` e no projeto:

   ```bash
   magento-cloud login
   ```

1. Confira o ambiente com o AtiveMQ Artemis instalado e configurado.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Usar SSH para se conectar ao ambiente de nuvem:

   ```bash
   magento-cloud ssh
   ```

1. Recupere os detalhes da conexão do AtiveMQ e as credenciais de logon da variável [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships):

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Na resposta, localize as informações do AtiveMQ. O nome da relação depende de como você a configurou no arquivo `.magento.app.yaml`. Por exemplo, se você usou `activemq-artemis` como o nome da relação:

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Habilite o encaminhamento de porta local para AtiveMQ (se o projeto estiver localizado em uma região diferente, como US-3, EU-5 ou AP-3, substitua ``us-3``/``eu-5``/``ap-3`` por ``us``)

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Um exemplo para acessar o console Web AtiveMQ Artemis em `http://localhost:8161` é:

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >O AtiveMQ Artemis usa a porta 61616 para mensagens STOMP e a porta 8161 para o console da web.

1. Enquanto a sessão estiver aberta, você poderá acessar o console da Web do AtiveMQ Artemis em `http://localhost:8161` usando o nome de usuário e a senha da variável MAGENTO_CLOUD_RELATIONSHIPS.

### Conectar a partir do aplicativo

Para se conectar ao AtiveMQ executado em um aplicativo, instale uma biblioteca de cliente STOMP em seu aplicativo PHP.

### Conectar a partir do aplicativo PHP

Para conectar-se ao AtiveMQ usando seu aplicativo PHP, adicione uma biblioteca STOMP PHP à árvore de origem. O Adobe Commerce usa o protocolo STOMP para comunicação AtiveMQ e a configuração é automaticamente definida durante a implantação quando o AtiveMQ Artemis é detectado como um serviço configurado.

## Suporte a protocolo

O AtiveMQ Artemis no Adobe Commerce na infraestrutura em nuvem usa o protocolo STOMP (Streaming Text Oriented Messaging Protocol):

- **STOMP**: o protocolo de mensagens usado para operações de fila (porta 61616)
- **Console da Web**: interface de gerenciamento acessível via HTTP (porta 8161)

## Diferenças de RabbitMQ

Embora [!DNL ActiveMQ Artemis] e [!DNL RabbitMQ] sirvam como agente de mensagens para o Adobe Commerce, há algumas diferenças:

- **Protocolo**: AtiveMQ Artemis usa o protocolo STOMP, enquanto RabbitMQ usa AMQP
- **Configuração**: quando o AtiveMQ Artemis é configurado, o Adobe Commerce usa automaticamente o protocolo STOMP
- **Prioridade**: se AtiveMQ e RabbitMQ estiverem configurados, AtiveMQ terá prioridade para operações baseadas em STOMP, enquanto operações AMQP usarão RabbitMQ
- **Console da Web**: o AtiveMQ fornece um console de gerenciamento baseado na Web para monitoramento de filas e mensagens

## Configuração da fila

Quando o AtiveMQ Artemis é configurado como um serviço, o Adobe Commerce configura automaticamente o sistema de fila para usar o protocolo STOMP. A configuração é gravada no arquivo `app/etc/env.php` durante a implantação:

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

Você pode substituir essa configuração usando a variável de ambiente [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration), se necessário.

