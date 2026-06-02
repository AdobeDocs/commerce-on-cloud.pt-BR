---
title: Assimilação de dados
description: Saiba como visualizar e gerenciar a assimilação de dados do Commerce no New Relic.
feature: Cloud, Observability
exl-id: b457b4de-deeb-4e92-b95a-c2b89d6f7a05
TQID: https://experienceleague.adobe.com/60hhI0IvazUSrw6cCRoXfbGjA4fkLLPXb8Q4Q08AqeE
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 209
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
