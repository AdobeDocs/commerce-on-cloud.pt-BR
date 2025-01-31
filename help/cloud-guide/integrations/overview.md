---
title: Visão geral das integrações
description: Saiba mais sobre as opções de integração de terceiros para seu projeto do Adobe Commerce na infraestrutura em nuvem.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Visão geral das integrações

As integrações são úteis para usar serviços externos, como hospedagem do Git ou bots de Slack, e manter seus processos de desenvolvimento atuais, como usar a função de solicitação de pull de revisão de código no GitHub. Você pode adicionar as seguintes integrações ao seu projeto do Adobe Commerce na infraestrutura em nuvem:

![Integrações](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Para adicionar uma integração usando a CLI da Nuvem**:

O comando a seguir inicia prompts interativos para selecionar o tipo e as opções da nova integração.

```bash
magento-cloud integration:add
```

**Para listar as integrações configuradas para o seu projeto**:

```bash
magento-cloud integration:list
```

Exemplo de resposta:

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB Console]

**Para adicionar uma integração usando o[!DNL Cloud Console]**:

1. Em _Configurações do projeto_, clique em **[!UICONTROL Integrations]**.

1. Clique em um tipo de integração ou clique em **[!UICONTROL Add integration]**.

1. Percorra as etapas de seleção e configuração do tipo de integração.

1. Depois de adicionar a integração, ela aparece na lista da visualização de Integrações.

>[!ENDTABS]

## Webhooks do Commerce

Você pode configurar webhooks do Commerce no seu projeto na nuvem com a [variável global ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks). Os webhooks do Commerce enviam solicitações para um servidor externo em resposta a eventos gerados pelo Commerce. O [_Guia de Webhooks_](https://developer.adobe.com/commerce/extensibility/webhooks) descreve esse recurso detalhadamente.

## Webhooks genéricos

Você pode capturar e relatar eventos de repositório e infraestrutura da nuvem usando uma integração de webhook personalizada para `POST` mensagens JSON para uma URL de _webhook_.

**Para adicionar uma URL de webhook, use a seguinte sintaxe**:

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` — Especifique o tipo de integração `webhook`.
- `url` — Forneça o URL do webhook que pode receber mensagens JSON.

O exemplo de resposta mostra uma série de prompts que fornecem uma oportunidade de personalizar a integração. Usar a resposta padrão (em branco) envia mensagens sobre todos os eventos em todos os ambientes em um projeto.

Você pode personalizar a integração para relatar [eventos](#events-to-report) específicos, como o envio de código para uma ramificação. Por exemplo, você pode especificar o evento `environment.push` para enviar uma mensagem quando um usuário envia código para uma ramificação:

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

Você pode optar por relatar eventos em um estado `pending`, `in_progress` ou `complete`:

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

E você pode _incluir_ ou _excluir_ mensagens para ambientes específicos:

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

Quando a integração for concluída, você receberá um resumo dos valores:

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Atualizar integração existente

Você pode atualizar uma integração existente. Por exemplo, altere os estados de `complete` para `pending` usando o seguinte:

```bash
magento-cloud integration:update --states=pending <int-id>
```

Exemplo de resposta:

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Eventos a relatar

| Evento | Descrição |
| ----- | :-----------|
| `environment.access.add` | Um usuário recebeu acesso ao ambiente |
| `environment.access.remove` | Um usuário foi removido do ambiente |
| `environment.activate` | Uma ramificação foi &quot;ativada&quot; com um ambiente |
| `environment.backup` | Um usuário acionou um instantâneo |
| `environment.branch` | Uma ramificação foi criada usando o console de gerenciamento |
| `environment.deactivate` | Uma ramificação foi &quot;desativada&quot;. O código ainda está lá, mas o ambiente foi destruído |
| `environment.delete` | Uma filial foi excluída |
| `environment.initialize` | A ramificação `master` do projeto foi inicializada com uma primeira confirmação |
| `environment.merge` | Uma ramificação ativa foi mesclada usando o console de gerenciamento ou a API |
| `environment.push` | Um usuário enviou o código a uma ramificação |
| `environment.restore` | Um usuário restaurou um instantâneo |
| `environment.route.create` | Uma rota foi criada usando o console de gerenciamento |
| `environment.route.delete` | Uma rota foi excluída usando o console de gerenciamento |
| `environment.route.update` | Uma rota foi modificada usando o console de gerenciamento |
| `environment.subscription.update` | O ambiente `master` foi redimensionado porque a assinatura foi alterada, mas não há alterações de conteúdo |
| `environment.synchronize` | Um ambiente teve dados ou código copiado novamente de seu ambiente pai |
| `environment.update.http_access` | As regras de acesso HTTP para um ambiente foram modificadas |
| `environment.update.restrict_robots` | O recurso de bloquear todos os robôs foi ativado ou desativado |
| `environment.update.smtp` | O envio de emails foi ativado ou desativado em um ambiente |
| `environment.variable.create` | Uma variável foi criada |
| `environment.variable.delete` | Uma variável foi excluída |
| `environment.variable.update` | Uma variável foi modificada |
| `project.domain.create` | Um domínio foi criado e adicionado ao projeto |
| `project.domain.delete` | Um domínio associado ao projeto foi removido |
| `project.domain.update` | Um domínio associado ao projeto foi atualizado |
