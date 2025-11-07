---
title: Configurar o serviço RabbitMQ
description: Saiba como habilitar o serviço RabbitMQ para gerenciar filas de mensagens para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 76a9721767cbd4328347311cc308810f0f7914c0
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Configurar o serviço [!DNL RabbitMQ]

O [MQF (Estrutura da Fila de Mensagens)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=pt-BR) é um sistema do Adobe Commerce que permite que um [módulo](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/implementation-playbook/glossary#module) publique mensagens em filas. Também define os consumidores que recebem as mensagens de forma assíncrona.

O MQF usa [RabbitMQ](https://www.rabbitmq.com/) como agente de mensagens, o que fornece uma plataforma escalável para enviar e receber mensagens. Também inclui um mecanismo para armazenar mensagens não entregues. [!DNL RabbitMQ] é baseado na especificação AMQP (Advanced Message Queuing Protocol) 0.9.1.

>[!NOTE]
>
>O Adobe Commerce na infraestrutura em nuvem também oferece suporte a [AtiveMQ Artemis](activemq.md) como um serviço alternativo de fila de mensagens usando o protocolo STOMP.

>[!IMPORTANT]
>
>Se preferir usar um serviço existente baseado em AMQP, como o [!DNL RabbitMQ], em vez de depender da Adobe Commerce na infraestrutura de nuvem para criá-lo para você, use a variável de ambiente [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) para conectá-lo ao seu site.

{{service-instruction}}

**Para habilitar RabbitMQ**:

1. Adicione o nome, o tipo e o valor de disco necessários (em MB) ao arquivo `.magento/services.yaml`, juntamente com a versão do RabbitMQ instalada.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. Configure as relações no arquivo `.magento.app.yaml`.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Verifique as relações de serviço](services-yaml.md#service-relationships).

{{service-change-tip}}

## Conectar ao RabbitMQ para depuração

Para fins de depuração, é útil se conectar diretamente a uma instância de serviço de uma das seguintes maneiras:

- Conectar-se a partir do ambiente de desenvolvimento local
- Conectar a partir do aplicativo
- Conectar a partir do aplicativo PHP

### Conectar-se a partir do ambiente de desenvolvimento local

1. Faça logon na CLI do `magento-cloud` e no projeto:

   ```bash
   magento-cloud login
   ```

1. Confira o ambiente com o RabbitMQ instalado e configurado.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Usar SSH para se conectar ao ambiente de nuvem:

   ```bash
   magento-cloud ssh
   ```

1. Recupere os detalhes da conexão RabbitMQ e as credenciais de logon da variável [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships):

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Na resposta, localize as informações de RabbitMQ, por exemplo:

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Habilitar o encaminhamento de porta local para RabbitMQ (se o projeto estiver localizado em uma região diferente, como US-3, EU-5 ou AP-3, substitua ``us-3``/``eu-5``/``ap-3`` por ``us``)

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Um exemplo de acesso à interface da Web de gerenciamento do RabbitMQ em `http://localhost:15672` é:

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. Enquanto a sessão estiver aberta, você poderá iniciar um cliente RabbitMQ de sua escolha na estação de trabalho local, configurado para se conectar ao `localhost:<portnumber>` usando o número da porta, o nome de usuário e as informações de senha da variável MAGENTO_CLOUD_RELATIONSHIPS.

### Conectar a partir do aplicativo

Para se conectar ao RabbitMQ em execução em um aplicativo, instale um cliente, como [amqp-utils](https://github.com/dougbarth/amqp-utils), como uma dependência de projeto no arquivo `.magento.app.yaml`.

Por exemplo,

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

Ao fazer logon no contêiner PHP, digite qualquer comando `amqp-` disponível para gerenciar suas filas.

### Conectar a partir do aplicativo PHP

Para conectar ao RabbitMQ usando seu aplicativo PHP, adicione uma biblioteca PHP à sua árvore de origem.

## Solução de problemas do serviço [!DNL RabbitMQ]

Consulte [Não é possível se conectar ao RabbitMQ na Adobe Commerce Cloud](https://experienceleague.adobe.com/pt-br/docs/experience-cloud-kcs/kbarticles/ka-27688).

## Atualizando o serviço [!DNL RabbitMQ]

Para obter instruções de atualização, consulte [Alterar versão do serviço](https://experienceleague.adobe.com/pt-br/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version).
