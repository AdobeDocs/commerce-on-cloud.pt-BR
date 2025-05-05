---
title: Monitoramento do New Relic
description: Saiba como acessar o painel do New Relic e analisar dados do seu projeto do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Observability
topic: Performance
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# Monitoramento do New Relic

O New Relic conecta e monitora sua infraestrutura e o aplicativo [!DNL Commerce] usando agentes PHP. Depois que um ambiente da nuvem se conecta ao New Relic, você pode fazer logon na sua conta da New Relic para analisar os dados coletados pelo agente.

Na página _APM e Serviços_, selecione o **Resumo** para exibir as informações transacionais sobre o seu aplicativo. Essa visualização ajuda a identificar possíveis falhas e verificar a integridade geral de seus aplicativos e serviços.

![Página de visão geral do New Relic do projeto na nuvem](../../assets/new-relic/dashboard.png)

Nesta visualização, você pode rastrear transações que encontram respostas lentas ou gargalos, throughput de aplicativo, erros da Web e muito mais.

Revisar dados rastreados:

- **Mais demorado** — Determine o consumo de tempo rastreando solicitações em paralelo. Por exemplo, você pode ter o maior tempo de transação gasto nas visualizações de produto e categoria. Se uma página de conta de cliente apresentar um alto nível de consumo de tempo de repente, seu aplicativo pode ser afetado por uma chamada ou desempenho de arrastar a consulta.

- **Taxa de transferência mais alta**—Identifique as páginas que mais acessam com base no tamanho e na frequência dos bytes transmitidos.

Todos os dados coletados detalham o tempo gasto em ações que transmitem dados, consultas ou dados _Redis_. Se as consultas causarem problemas, a New Relic fornecerá informações para rastrear e responder a esses problemas.

>[!TIP]
>
>Para obter detalhes sobre como usar esses dados para solucionar problemas de desempenho do aplicativo, consulte [Solução de problemas de desempenho usando o New Relic](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html?lang=pt-BR) na _Central de Ajuda do Adobe Commerce_.

## Monitorar o desempenho com alertas gerenciados

O Adobe fornece a _Política de alerta de Alertas Gerenciados para Adobe Commerce_ para rastrear métricas de desempenho. A política inclui uma coleção de alertas que definem limites e acionam avisos e notificações críticas quando problemas de infraestrutura ou aplicativos afetam o desempenho do local. A política rastreia as seguintes métricas em ambientes de produção:

| Métrica | Coleta de dados | Disponibilidade |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex] pontuação | APM | Pro e Starter |
| Uso do CPU | NRI | Pro |
| Espaço em disco | NRI | Pro |
| Taxa de erros | APM | Pro e Starter |
| Uso de memória | NRI | Pro |
| Carga de consulta MariaDB | NRI | Pro |
| Memória Redis | NRI | Pro |

Quando a infraestrutura do site ou as condições do aplicativo acionam um limite de alerta, o New Relic envia notificações de alerta para que você possa resolver o problema de forma proativa. Consulte [Alertas Gerenciados para Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=pt-BR) na _Central de Ajuda da Adobe Commerce_ para obter detalhes sobre limites de alerta e etapas de solução de problemas para resolver os problemas que dispararam o alerta.

>[!TIP]
>
>Para ambientes Pro Staging e de integração e ambientes Starter, use as [Notificações de integridade](../integrations/health-notifications.md) para monitorar o espaço em disco.

>[!PREREQUISITES]
>
>- **Credenciais do New Relic** — Credenciais para fazer logon na conta do New Relic do projeto na nuvem
>- **Integração ativa com o New Relic**—Verifique se o ambiente da sua nuvem está conectado ao New Relic
>- **Notificação do fluxo de trabalho**—Configure pelo menos um [fluxo de trabalho](#set-up-a-workflow-for-notifications) para receber as notificações de alerta

**Para examinar a política Alertas Gerenciados para Adobe Commerce**:

1. Faça logon em sua [conta do New Relic](https://login.newrelic.com/login).

1. Localize os _Alertas Gerenciados para a política do Adobe Commerce_:

   - No menu de navegação do Explorer, clique em **[!UICONTROL Alerts & AI]**.

   - Em _Detectar_, clique em **[!UICONTROL Alert Conditions & Policies]**.

   - Verifique se sua conta está selecionada na parte superior da exibição _Condições e políticas de alerta_.

   - Na lista _Política_, selecione **Alertas Gerenciados para a política do Adobe Commerce**.

     ![Políticas de alerta geradas](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >Se a política _Alertas Gerenciados para Adobe Commerce_ não estiver disponível, consulte [Alertas Gerenciados para Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=pt-BR) na _Central de Ajuda da Adobe Commerce_.

1. Clique na guia **[!UICONTROL Alert conditions]** para revisar as condições de alerta definidas na política.

## Criar políticas de alerta

Não modifique nenhum alerta incluído na política Alertas gerenciados para Adobe Commerce. O Adobe atualiza e melhora as condições de alerta nesta política ao longo do tempo, substituindo qualquer personalização adicionada à política.

Em vez de modificar um alerta existente, você pode criar uma política de alertas. Em seguida, copie as condições de alerta para a nova regra.

>[!TIP]
>
>Consulte [Introdução aos alertas](https://docs.newrelic.com/docs/alerts/overview/) na documentação do _New Relic_ para obter informações mais detalhadas sobre Alertas, políticas de alerta e fluxos de trabalho.

## Configurar um fluxo de trabalho para notificações

Agora você pode configurar um _fluxo de trabalho_, anteriormente chamado de canal de notificação, para receber notificações sobre o desempenho do site com base em dados filtrados, como uma política de alerta. As notificações sobre problemas de desempenho vão para todos os workflows associados a uma política de alerta quando as condições no aplicativo ou na infraestrutura acionam um alerta. Você também recebe notificações quando um problema é confirmado e fechado.

O New Relic fornece modelos para configurar diferentes tipos de notificações de workflow, incluindo email, Slack, PagerDuty, webhooks e muito mais.

**Para configurar um fluxo de trabalho**:

1. Faça logon em sua [conta do New Relic](https://login.newrelic.com/login).

1. Crie um workflow.

   - No menu de navegação do Explorer, clique em **[!UICONTROL Alerts & AI]**.

   - Na navegação à esquerda em _Enriquecer e notificar_, clique em **[!UICONTROL Workflows]**.

   - Clique em **[!UICONTROL Add a workflow]** no lado direito.

     ![New Relic adicionar um fluxo de trabalho](../../assets/new-relic/add-a-workflow.png)

   - Na página _Configurar o fluxo de trabalho_, digite um nome para o fluxo de trabalho.

   - Na seção _Filtrar dados_, selecione **[!UICONTROL Managed Alerts for Adobe Commerce]** na lista suspensa **[!UICONTROL Policy]**.

   - Na seção _Notificar_, selecione um canal e siga as instruções.

   - Clique em **[!UICONTROL Test workflow]** para verificar sua configuração.

1. Clique em **[!UICONTROL Activate workflow]**.

Consulte a documentação do New Relic sobre [Workflows](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Os alertas na política Alertas gerenciados para Adobe Commerce têm fluxos de trabalho padrão configurados para notificar equipes Adobe que oferecem suporte a clientes de infraestrutura em nuvem da Adobe Commerce. Não modifique a configuração desses canais padrão e não remova as políticas de alerta atribuídas a eles.
