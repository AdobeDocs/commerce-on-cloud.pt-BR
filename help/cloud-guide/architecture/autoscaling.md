---
title: Dimensionamento automático
description: Saiba como o Adobe Commerce na infraestrutura em nuvem pode ser dimensionado para atender às demandas de recursos.
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 605
ht-degree: 0%

---

# Dimensionamento automático

O dimensionamento automático adiciona ou remove automaticamente recursos da infraestrutura em nuvem para manter o desempenho ideal e custos razoáveis. Atualmente, este recurso só está disponível para projetos configurados com uma [Arquitetura em escala](scaled-architecture.md).

## Nós do servidor da Web

A [camada da Web](scaled-architecture.md#web-tier) é dimensionada para acomodar um aumento nas solicitações de processo e requisitos de tráfego mais altos. Atualmente, o recurso de dimensionamento automático só é dimensionado horizontalmente adicionando ou removendo nós do servidor da Web.

Um evento de dimensionamento automático ocorre quando o uso e o tráfego do CPU atingem um limite predefinido:

- **Nós adicionados** — as CPUs/núcleos em todos os nós da Web ativos têm 75% de capacidade por 1 minuto e o tráfego está aumentando 20% em 5 minutos consecutivos.
- **Nós removidos** — CPUs/núcleos em todos os nós da Web ativos são carregados a 60% por 20 minutos. Os nós são removidos na ordem em que foram adicionados.

Os limites mínimo e máximo são determinados e definidos com base nos limites de recursos contratados de cada comerciante; isso reduz o risco de escalonamento infinito.

## Monitorar limites com o New Relic

Você pode usar o [serviço New Relic](../monitor/new-relic-service.md) para monitorar determinados limites, como a contagem de hosts e o uso do CPU. As consultas New Relic a seguir usam uma notação variável para `cluster-id` somente para fins de exemplo.

>[!TIP]
>
>Para obter uma referência sobre criação de consultas, consulte [sintaxe, cláusulas e funções do NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) na documentação do _New Relic_.
>Use suas consultas para criar um [painel do New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Contagem de hosts

O exemplo de consulta New Relic a seguir mostra a contagem de hosts no ambiente:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

Na captura de tela a seguir, **hosts APM vistos** refere-se ao número de hosts com transações registradas durante o período selecionado.

![Contagem de hosts do New Relic](../../assets/new-relic/host-count.png)

### Uso do CPU

O exemplo de consulta do New Relic a seguir mostra o uso do CPU para nós da Web:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Uso do CPU em nós da Web do New Relic](../../assets/new-relic/web-node-cpu-usage.png)

## Ativar dimensionamento automático

Para habilitar ou desabilitar o dimensionamento automático para o projeto de infraestrutura em nuvem do Adobe Commerce, [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket). Escolha os seguintes motivos no ticket:

- **Motivo do contato**: Solicitação de Alteração de Infraestrutura
- **Motivo do Contato do Adobe Commerce Infrastructure**: Outra Solicitação de Alteração de Infraestrutura

>[!IMPORTANT]
>
>O recurso de dimensionamento automático captura eventos imprevistos. Mesmo que o dimensionamento automático esteja habilitado, a Adobe recomenda que você continue a [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) se esperar um evento futuro.

### Teste de carga

O Adobe habilita o dimensionamento automático primeiro no cluster _preparo_ do projeto na nuvem. Depois de executar e concluir o teste de carga em seu ambiente, o Adobe habilita o dimensionamento automático em seu cluster de produção. Para obter orientação sobre teste de carga, consulte [Teste de desempenho](../launch/checklist.md#performance-testing).

### INCLUO NA LISTA DE PERMISSÕES IP

Após ativar o dimensionamento automático, o tráfego do nó da Web de saída é originado dos endereços IP dos nós de serviço. Se você usar um incluo na lista de permissões com um serviço de terceiros que não esteja incluído no seu projeto do Adobe Commerce na infraestrutura em nuvem, verifique os endereços IP no incluo na lista de permissões de serviços de terceiros.

Por exemplo:

- Se o incluo na lista de permissões contiver os endereços IP dos nós de serviço (1, 2 e 3), nenhuma ação será necessária.
- Se o incluo na lista de permissões contiver os endereços IP dos nós de serviço (1, 2 e 3) e dos nós da Web (4, 5 e 6), nesse caso, todos os seis nós, nenhuma ação será necessária.
- Se o incluo na lista de permissões contiver os endereços IP _only_ dos nós da Web (4, 5 e 6), você deverá atualizar o incluo na lista de permissões para incluir os endereços IP dos nós de serviço.

