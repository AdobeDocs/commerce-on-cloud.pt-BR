---
title: Trabalhadores
description: Saiba como configurar a propriedade workers no arquivo de configuração do aplicativo  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Propriedade dos trabalhadores

Você pode definir um trabalhador para ser executado independentemente da instância da Web sem uma instância do Nginx em execução; no entanto, o trabalhador usa o mesmo armazenamento de rede usado pelo aplicativo [!DNL Commerce]. Você não precisa configurar um servidor Web na instância do worker (usando Node.js ou Go) porque o roteador não pode direcionar solicitações públicas ao worker. Isso torna a instância do trabalhador ideal para tarefas em segundo plano ou tarefas em execução continuamente que correm o risco de bloquear uma implantação.

## Configurar um trabalhador

Os funcionários estão disponíveis para uso somente com ambientes de preparo e produção profissionais. Os ambientes de integração Pro e Starter podem optar por usar a variável [CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner).

Para configurar um trabalhador no Pro Staging ou na Produção, [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) e inclua as seguintes informações:

- ID do projeto
- ID do ambiente
- Nome do trabalhador
- Comandos Start

É possível configurar um processo por trabalhador. Uma configuração básica de trabalho comum no arquivo `.magento.app.yaml` pode ser semelhante ao seguinte:

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

Este exemplo define um único trabalhador chamado `queue`, com um pequeno nível (tamanho S) de alocação de recursos, e executa o comando `php ./bin/magento` na inicialização. O trabalhador `queue` é executado em cada nó como um processo de trabalho. Se o comando for encerrado, ele será reiniciado automaticamente.

## Comandos e substituições

A chave `commands.start` é necessária para iniciar comandos com o aplicativo de trabalho. Você pode usar qualquer comando shell válido, embora seja ideal usar o idioma do aplicativo. Se o comando especificado pela chave `start` for encerrado, ele será reiniciado automaticamente.

>[!IMPORTANT]
>
>Os ganchos `deploy` e `post_deploy` e os comandos `crons` são executados somente no contêiner da Web, não em instâncias de trabalho.

### Herança

As definições das propriedades `size`, `relationships`, `access`, `disk` e `mount` e `variables` são herdadas por um trabalhador, a menos que sejam explicitamente substituídas.

As propriedades a seguir são as mais usadas para substituir [configurações de nível superior](properties.md):

- `size`—alocar menos recursos para um único processo em segundo plano
- `variables`—instrui o aplicativo a ser executado de forma diferente

### Tempo e enfileiramento

Embora cada operador enfileire atrás de outro, a configuração a seguir produz uma separação consistente de dois segundos nos carimbos de data e hora no arquivo `var/time.txt`, independentemente da espera de oito segundos no código PHP:

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
