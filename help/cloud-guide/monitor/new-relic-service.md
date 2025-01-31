---
title: serviço New Relic
description: Saiba mais sobre o serviço do New Relic disponível com seu projeto do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Visão geral do serviço New Relic

Todos os projetos do Adobe Commerce na infraestrutura em nuvem incluem acesso ao serviço New Relic para ajudar a monitorar o desempenho e investigar eventos do aplicativo [!DNL Commerce] e da infraestrutura em nuvem.

Os seguintes recursos do New Relic estão disponíveis para uso com ambientes de produção e de preparo:

- [APM do New Relic](#new-relic-apm) (Pro e Starter)
- [Infraestrutura New Relic](#new-relic-infrastructure) (somente Pro)
- [Gerenciamento de logs do New Relic](#new-relic-log-management) (somente Pro)

>[!INFO]
>
>Outros recursos do New Relic não estão disponíveis em projetos do Adobe Commerce.

## APM do New Relic

O [New Relic for application performance management (APM)](https://docs.newrelic.com/introduction-apm/) é um produto de análise de software que ajuda você a analisar e melhorar as interações dos aplicativos. O APM do New Relic está disponível para todos os projetos de infraestrutura em nuvem do Adobe Commerce e fornece os seguintes recursos:

- **Concentre-se em transações específicas** — Marque e monitore ativamente as principais ações do cliente no seu site, como adicionar ao carrinho, fazer check-out ou processar um pagamento.
- **Monitoramento de consulta de banco de dados** — Localize e monitore consultas de banco de dados que afetam o desempenho.
- **Mapa de Aplicativos** — Exiba todas as dependências de aplicativos no site, nas extensões e nos serviços externos.
- **[!DNL Apdex]pontuações** — Avalie o desempenho e crie alertas que identificam problemas e o notificam quando ocorrem, como o desempenho do site afetado por uma promoção rápida ou um evento da web. Consulte [Pontuação do Apdex](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Alertas gerenciados para Adobe Commerce** - Use esta política de alerta do New Relic para monitorar o desempenho do aplicativo e da infraestrutura com base nas práticas recomendadas do setor. Consulte [Monitorar o desempenho com a diretiva de alertas Gerenciados da Adobe Commerce](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Rastrear implantações** — Monitore eventos de implantação e analise o impacto da implantação no desempenho geral. Consulte [Rastrear implantações](track-deployments.md).

Seu projeto de infraestrutura do Adobe Commerce na nuvem inclui o software para o serviço de APM da New Relic, juntamente com uma chave de licença. Não é necessário adquirir ou instalar nenhum software adicional.

## Infraestrutura New Relic

Os projetos profissionais incluem o serviço [New Relic Infrastructure (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/), que se conecta automaticamente aos dados do aplicativo e à análise de desempenho para fornecer monitoramento dinâmico do servidor. Esse serviço está disponível em ambientes de produção e preparo profissionais.

## Gerenciamento de logs do New Relic

Todos os projetos de infraestrutura em nuvem incluem o [gerenciamento de logs do New Relic](log-management.md). O serviço é pré-configurado para agregar todos os dados de log de seus ambientes de preparo e produção e exibi-los em um painel de gerenciamento de log centralizado.
