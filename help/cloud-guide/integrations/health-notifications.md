---
title: Notificações de integridade
description: Saiba como configurar notificações do Slack, de email e do PagerDuty para o uso do espaço em disco no seu projeto Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Observability, Integration
exl-id: 5a7f37e9-e8f9-4b6b-b628-60dcaa60cc64
source-git-commit: c3c708656e3d79c0893d1c02e60dcdf2ad8d7c7c
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Notificações de integridade

O Adobe Commerce na infraestrutura em nuvem monitora o uso do espaço em disco em todos os aplicativos e serviços no ambiente de Início ou no ambiente de integração Pro. Um disco de banco de dados com espaço insuficiente pode causar corrupção de dados. A verificação de status de integridade ocorre a cada 5 minutos e pode notificá-lo por email ou outro serviço externo. Há três avisos de disco baixo para notificações de integridade:

- **Aviso**—o espaço disponível em disco cai abaixo de 20%
- **Crítico**—o espaço disponível em disco cai abaixo de 10%
- **Limpar tudo**—o espaço disponível em disco retorna mais de 20%, após um evento de disco baixo

>[!NOTE]
>
>Em ambientes de produção Pro, você pode usar a política de alerta Managed alerts for Adobe Commerce da New Relic para monitorar o espaço em disco. Consulte [Monitorar o desempenho com Alertas Gerenciados](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

## Notificações por email

A integração do email de integridade requer um endereço de origem e pelo menos um endereço de destinatário. Você pode usar o mesmo endereço de email para o `from-address` e o `recipients`. O exemplo a seguir registra uma integração de email de integridade com dois recipients:

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Notificações de canal do Slack

O Slack é um serviço externo que usa aplicativos interativos chamados bots para postar mensagens em uma sala de chat. Antes de receber notificações de integridade no Slack, você deve criar um [usuário de bot](https://api.slack.com/bot-users) personalizado para o seu grupo do Slack. Depois de configurar o usuário de bot para seu canal ou canais, salve o [token de bot](https://api.slack.com/docs/token-types#bot) fornecido pela Slack para registrar sua integração. O exemplo a seguir registra notificações de integridade em um canal do Slack:

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## Notificações PagerDuty

PagerDuty é um serviço externo que pode notificar membros da equipe sobre problemas importantes. Antes de receber notificações de integridade no PagerDuty, você deve criar uma [integração PagerDuty](https://developer.pagerduty.com/v2/docs/integrating) que use a API de eventos versão 2. Use a Chave de integração ou a _Chave de roteamento_ para registrar sua integração. O exemplo a seguir registra notificações para PagerDuty usando uma chave de roteamento:

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```

## Gerenciamento de logs

Para aumentar o espaço disponível em disco, você pode truncar ou remover arquivos de log de seu ambiente. Se o logrotate estiver ativado, primeiro baixe uma cópia de backup dos logs e, em seguida, remova-os:

```bash
rm -rf some-log-file.log.gz
```

Como alternativa, você pode truncar arquivos de log individuais para reduzir seu tamanho. Para obter um exemplo detalhado de truncamento de arquivos de log, consulte o tutorial em vídeo Truncar Arquivos de Log{target="_blank"}.
