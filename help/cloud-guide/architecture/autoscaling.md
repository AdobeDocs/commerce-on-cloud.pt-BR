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
source-git-commit: a542dac902dc0de7c0836c1e5e4aece40fc6cbee
workflow-type: tm+mt
source-wordcount: 979
ht-degree: 0%

---

# Dimensionamento automático

O dimensionamento automático adiciona ou remove automaticamente recursos da infraestrutura em nuvem para manter o desempenho ideal e custos razoáveis. A Adobe oferece dois tipos de dimensionamento automático para [!DNL Adobe Commerce on cloud infrastructure] projetos:

- [Dimensionamento automático horizontal](#horizontal-auto-scaling) (Disponível somente para arquitetura dimensionada) — Adiciona ou remove nós de servidor Web para projetos de arquitetura dimensionada.
- [Dimensionamento automático vertical](#vertical-auto-scaling) (Disponível para arquitetura Pro padrão ou dimensionada) — Redimensiona a capacidade de CPU dos nós existentes para acomodar alterações na demanda.


## Ativar dimensionamento automático

Para habilitar ou desabilitar o dimensionamento automático horizontal ou vertical para o projeto [!DNL Adobe Commerce on cloud infrastructure], [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Escolha os seguintes motivos no ticket:

- **Motivo do contato**: Solicitação de Alteração de Infraestrutura
- **Motivo do Contato do Adobe Commerce Infrastructure**: Outra Solicitação de Alteração de Infraestrutura

>[!IMPORTANT]
>
>O recurso de dimensionamento automático captura eventos imprevistos. Mesmo que o dimensionamento automático esteja habilitado, a Adobe recomenda que você continue a [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) se esperar um evento futuro.

### Teste de carga

O Adobe habilita o dimensionamento automático primeiro no cluster _preparo_ do projeto na nuvem. Depois de executar e concluir o teste de carga em seu ambiente, o Adobe habilita o dimensionamento automático em seu cluster de produção. Para obter orientação sobre teste de carga, consulte [Teste de desempenho](../launch/checklist.md#performance-testing).

## Dimensionamento automático horizontal

Atualmente, este recurso só está disponível para projetos configurados com uma [Arquitetura em escala](scaled-architecture.md).

O dimensionamento automático horizontal adiciona ou remove os nós do servidor Web para projetos de arquitetura dimensionada. Como alternativa, o [dimensionamento automático vertical](#vertical-auto-scaling) redimensiona a capacidade de CPU dos nós existentes para acomodar as alterações na demanda.

### Nós do servidor da Web

A [camada da Web](scaled-architecture.md#web-tier) é dimensionada para acomodar um aumento nas solicitações de processo e requisitos de tráfego mais altos. Atualmente, o recurso de dimensionamento automático só é dimensionado horizontalmente adicionando ou removendo nós do servidor da Web.

Um evento de dimensionamento automático ocorre quando o uso e o tráfego do CPU atingem um limite predefinido:

- **Nós adicionados** — as CPUs/núcleos em todos os nós ativos da Web têm 75% de capacidade por 1 minuto e o tráfego está aumentando 20% em 5 minutos consecutivos.
- **Nós removidos** — CPUs/núcleos em todos os nós da Web ativos são carregados a 60% por 20 minutos. Os nós são removidos na ordem em que foram adicionados.

Os limites mínimo e máximo são determinados e definidos com base nos limites de recursos contratados de cada comerciante; isso reduz o risco de escalonamento infinito.

### Monitorar limites com o New Relic

Você pode usar o [serviço New Relic](../monitor/new-relic-service.md) para monitorar determinados limites, como a contagem de hosts e o uso do CPU. As consultas New Relic a seguir usam uma notação variável para `cluster-id` somente para fins de exemplo.

>[!TIP]
>
>Para obter uma referência sobre criação de consultas, consulte [sintaxe, cláusulas e funções do NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) na documentação do _New Relic_.
>Use suas consultas para criar um [painel do New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

#### Contagem de hosts

O exemplo de consulta New Relic a seguir mostra a contagem de hosts no ambiente:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

Na captura de tela a seguir, **hosts APM vistos** refere-se ao número de hosts com transações registradas durante o período selecionado.

![Contagem de hosts do New Relic](../../assets/new-relic/host-count.png)

#### Uso do CPU

O exemplo de consulta do New Relic a seguir mostra o uso do CPU para nós da Web:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Uso do CPU em nós da Web do New Relic](../../assets/new-relic/web-node-cpu-usage.png)

### INCLUO NA LISTA DE PERMISSÕES IP

Após ativar o dimensionamento automático, o tráfego do nó da Web de saída é originado dos endereços IP dos nós de serviço. Se você usar um incluo na lista de permissões com um serviço de terceiros que não esteja incluído no seu projeto do Adobe Commerce na infraestrutura em nuvem, verifique os endereços IP no incluo na lista de permissões de serviços de terceiros.

Por exemplo:

- Se o incluo na lista de permissões contiver os endereços IP dos nós de serviço (1, 2 e 3), nenhuma ação será necessária.
- Se o incluo na lista de permissões contiver os endereços IP dos nós de serviço (1, 2 e 3) e dos nós da Web (4, 5 e 6), nesse caso, todos os seis nós, nenhuma ação será necessária.
- Se o incluo na lista de permissões contiver os endereços IP _only_ dos nós da Web (4, 5 e 6), você deverá atualizar o incluo na lista de permissões para incluir os endereços IP dos nós de serviço.

## Dimensionamento automático vertical

Além do [dimensionamento automático horizontal](#auto-scaling) tradicional, o [!DNL Adobe Commerce on cloud infrastructure] também oferece dimensionamento automático vertical para projetos de arquitetura pro padrão e dimensionada.

Em vez de adicionar ou remover nós, o dimensionamento automático vertical redimensiona a capacidade do CPU dos nós existentes para acomodar as alterações na demanda. Isso complementa o dimensionamento automático horizontal, que adiciona ou remove nós do servidor da Web para projetos de arquitetura dimensionada.

- **Nós adicionados**: Não aplicável. O dimensionamento automático vertical redimensiona os nós existentes em vez de adicionar novos.
- **Upsize de nó**: um nó é redimensionado para o próximo tamanho maior de instância quando a pressão de memória ultrapassa o limite definido. Somente um aumento de tamanho é aplicado por evento de dimensionamento.
- **Redução de nó**: a redução dos nós é automática após o fim da demanda. Os tamanhos mínimo e máximo são definidos com base no padrão de uso de cada projeto e nos limites de recursos contratados, o que reduz o risco de dimensionamento desnecessário.

### Limites de dimensionamento automático

Os eventos verticais de dimensionamento automático são acionados usando as PSI (Pressure Stall Information, informações de paralisação por pressão) para memória no Linux, que medem quanto tempo um sistema gasta parado devido à pressão da memória. O Adobe define limites com base nos limites de recursos contratados e padrões de uso do seu projeto; os comerciantes não podem configurá-los no momento.

### Monitorar limites com o New Relic

Você pode usar o serviço [!DNL New Relic] para monitorar os detalhes da instância de infraestrutura, incluindo o tamanho e o tipo da instância. Configure alertas no New Relic para serem notificados sempre que um evento de dimensionamento automático vertical alterar o tamanho ou o tipo de uma instância.

### Impacto no seu ambiente

O dimensionamento automático vertical tem o seguinte impacto no seu ambiente:

- **Tempo de inatividade**: nenhum tempo de inatividade é antecipado quando um nó é redimensionado.
- **Tempo**: redimensionar um nó normalmente leva de 20 a 30 minutos. O nó é temporariamente removido do balanceador de carga enquanto o redimensionamento está em andamento.
