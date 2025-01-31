---
title: Configurar notificações
description: Saiba como configurar notificações para o Adobe Commerce em ambientes de infraestrutura em nuvem.
feature: Cloud, Configuration, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Configurar notificações

Por padrão, o Adobe Commerce na infraestrutura em nuvem grava ações de compilação e implantação no arquivo `app/var/log/cloud.log` dentro do diretório de aplicativo raiz do Adobe Commerce. Como opção, você pode enviar logs para um sistema de mensagens, como Slack e email, para receber notificações em tempo real.

Por exemplo, você pode enviar uma mensagem de Slack para alertar um grupo de pessoas quando uma implantação falhar e solicitar uma investigação sobre o que deu errado.

## Notificações do plano

Antes de configurar notificações, considere o seguinte:

- Que tipo de notificações você deseja receber (mensagens de Slack, email, ambos)?
- Quantos detalhes você deseja ver nos logs?
- Onde você deseja configurar as notificações (Integração, Preparo, Produção)?

Por exemplo, durante o desenvolvimento inicial, você pode preferir notificações por email que mostram logs detalhados sobre o ambiente de integração para ajudar a depurar problemas antes de implantar no ambiente de preparo. Quando estiver pronto para implantar no ambiente de preparo ou produção, você poderá preferir uma mensagem de Slack que contenha informações menos detalhadas.

>[!NOTE]
>
>O arquivo de configuração usado para configurar notificações está na raiz do diretório do projeto, portanto, se aplica quando você envia alterações para qualquer ambiente. Se quiser personalizar notificações por ambiente, você deverá modificar o arquivo de configuração antes de enviá-lo para esse ambiente.

## Configurar notificações

Para configurar notificações:

1. Na estação de trabalho local, altere para o diretório do projeto.
1. No arquivo `.magento.env.yaml`, na raiz do projeto, adicione as configurações do sistema de mensagens, incluindo a notificação preferencial [Níveis de log](log-handlers.md#log-levels).

   Por exemplo, para definir as configurações de email do Slack _e do_, use o seguinte:

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >O Adobe Commerce na infraestrutura em nuvem envia emails somente durante a fase de implantação.

1. Confirme e envie suas alterações para o servidor remoto.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Exemplo de configuração de Slack

O exemplo a seguir mostra uma configuração somente de Slack:

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token`—Seu Slack [token de usuário](https://api.slack.com/docs/token-types#user). O token do usuário autoriza o Adobe Commerce na infraestrutura em nuvem a enviar mensagens.
- `channel` — Nome do canal de Slack Adobe Commerce na infraestrutura em nuvem envia notificações.
- `username`—O nome de usuário Adobe Commerce na infraestrutura em nuvem usa para enviar mensagens de notificação no Slack.
- `min_level` — Nível mínimo de log para mensagens de notificação. Recomendamos usar `info`.

### Exemplo de configuração de email

O exemplo a seguir mostra uma configuração somente de email:

>[!NOTE]
>
>O Adobe Commerce na infraestrutura em nuvem envia emails somente durante a fase de implantação.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` — Endereço de email O Adobe Commerce na infraestrutura em nuvem envia mensagens de notificação.
- `from` — Endereço de email para enviar mensagens de notificação aos destinatários.
- `subject` — Descrição do email.
- `min_level` — Nível mínimo de log para mensagens de notificação. Recomendamos usar `notice` ou `warning`.
