---
title: Assimilação de dados
description: Saiba como visualizar e gerenciar a assimilação de dados do Commerce no New Relic.
feature: Cloud, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Assimilação de dados

A New Relic depende de dados avançados para fornecer monitoramento e análise eficazes, mas grandes conjuntos de dados podem afetar resultados oportunos, desempenho e conformidade. Este tópico fornece algumas orientações sobre como gerenciar a assimilação de dados e estratégias para refinar seus dados de forma a torná-los mais eficazes.

A New Relic fornece uma exibição de _Gerenciamento de dados_ que resume o uso do plano por fonte de dados.

**Para exibir seus dados e fontes de assimilação**:

1. No menu de usuário do New Relic, clique em **[!UICONTROL Manage your data]**.
1. Clique em **[!UICONTROL Data management]** na lista _Administração_.

   ![Gerenciamento de dados](../../assets/new-relic/data-ingestion.png)

   A guia **[!UICONTROL Data ingestion]** exibe os dados assimilados do dia e a fonte de dados.
A guia de retenção de dados exibe e controla por quanto tempo os dados são armazenados.

1. Selecione a guia **[!UICONTROL Limits]** e veja os limites para sua conta.

As fontes de dados para o Adobe Commerce incluem:

- **Eventos de APM** — dados de eventos usados em gráficos e painéis
- **Infraestrutura** — métricas de processo e host, como CPU, armazenamento, rede
- **Logs** — logs para CDN, APM e servidor de aplicativos

Os dados de log contribuem para uma grande parte da assimilação. Veja como [Exibir e analisar dados de log](log-management.md#view-and-analyze-log-data) e trabalhar com o representante da Adobe para formar uma estratégia para as necessidades de assimilação e retenção de dados. Leia mais sobre [gerenciamento de assimilação de dados](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) na _documentação do New Relic_.
