---
title: Arquitetura em nuvem do Commerce
description: Saiba como as arquiteturas de projeto Starter e Pro contrastam para a infraestrutura do Commerce na nuvem.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 7c1e895d-0f88-4f11-919a-b3b5748ca5f0
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# Arquitetura em nuvem do Commerce

A infraestrutura do Adobe Commerce na nuvem tem um plano Starter e um Pro. Cada plano tem uma arquitetura exclusiva para orientar o processo de desenvolvimento e implantação do Adobe Commerce. Tanto o plano Starter quanto a arquitetura de plano Pro implantam bancos de dados, servidores da Web e servidores de cache em vários ambientes para testes completos e, ao mesmo tempo, oferecem suporte à integração contínua.

Para comparação, cada plano inclui os seguintes recursos de infraestrutura e produtos compatíveis.

|          | Início | Pro |
| -------- | --------------------| ------------------ |
| Recursos principais | <ul><li>[Todos os recursos do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=pt-BR)</li><li>Ferramenta de integração do PayPal</li><li>[Relatórios do Commerce](https://business.adobe.com/br/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Todos os recursos do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=pt-BR)</li><li>Ferramenta de integração do PayPal</li><li>[Relatórios do Commerce](https://business.adobe.com/br/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[Módulo B2B](https://business.adobe.com/br/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infraestrutura e implantação | <ul><li>Ferramentas de integração contínua na nuvem com usuários ilimitados</li><li>Fastly Content Delivery Network (CDN), Image Otimization (IO) e segurança adicional com amplas concessões de largura de banda. O serviço Web Application Firewall (WAF) está disponível somente em ambientes de produção.</li><li>[New Relic](../monitor/new-relic-service.md) APM (Monitoramento de desempenho) em 3 ramificações: `master` e 2 de sua escolha<br>Ambientes de produção, preparo e desenvolvimento da Platform as a service (PaaS) (4 ambientes ativos totais) otimizados para o Adobe Commerce</li><li>Filtragem de saída (firewall de saída)</li></ul> | <ul><li>Ferramentas de integração contínua na nuvem com usuários ilimitados</li><li>Fastly Content Delivery Network (CDN), Image Otimization (IO) e segurança adicional com amplas concessões de largura de banda. O serviço Web Application Firewall (WAF) está disponível somente em ambientes de produção.</li><li>[New Relic](../monitor/new-relic-service.md) Infraestrutura em Produção + APM (Monitoramento de Desempenho) em preparo e produção. A [política de alertas gerenciados](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) para Adobe Commerce implementa as práticas recomendadas de monitoramento para notificá-lo proativamente sobre problemas de aplicativo e infraestrutura que afetam o desempenho do site.</li><li>Ambientes [de desenvolvimento de integração](pro-architecture.md#integration-environment) baseados em PaaS (2 ambientes ativos totais) otimizados para o Adobe Commerce</li><li>Infraestrutura como um serviço (IaaS) — infraestrutura virtual dedicada para ambientes de preparo e produção</li></ul> |
| Infraestrutura de alta disponibilidade | | [Arquitetura de alta disponibilidade](pro-architecture.md#redundant-hardware) com uma configuração de três servidores na Infraestrutura como um serviço (IaaS) subjacente para fornecer confiabilidade e disponibilidade de nível empresarial |
| Hardware dedicado | | Hardware isolado e dedicado na infraestrutura subjacente como um serviço (IaaS) para fornecer níveis ainda mais altos de confiabilidade e disponibilidade |
| Suporte por email 24 horas por dia, 7 dias por semana | Suporte de e-mail e monitoramento 24 horas por dia, 7 dias por semana, para o aplicativo principal e a infraestrutura em nuvem | Suporte de e-mail e monitoramento 24 horas por dia, 7 dias por semana, para o aplicativo principal e a infraestrutura em nuvem |
| Um consultor técnico dedicado ao cliente (CTA) | | Gerenciamento de conta técnica dedicado para o período de lançamento inicial, começando com sua assinatura até o lançamento inicial do site |
| Complementos\* | <ul><li>[Módulo B2B](https://business.adobe.com/br/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Disponível por uma taxa adicional_

## Projetos iniciais

A [Arquitetura do plano inicial](starter-architecture.md) tem quatro ambientes:

- **Integração** — O ambiente de integração fornece dois ambientes testáveis. Cada ambiente inclui uma ramificação Git ativa, um banco de dados, um servidor da Web, armazenamento em cache, alguns serviços, variáveis de ambiente e configurações.

- **Preparo** — À medida que o código e as extensões passam nos testes, você pode mesclar a ramificação `integration` ao ambiente de Preparo, que se torna o ambiente de teste de pré-produção. Inclui a ramificação ativa `staging`, o banco de dados, o servidor Web, o armazenamento em cache, serviços de terceiros, variáveis de ambiente, configurações e serviços, como o Fastly e o New Relic.

- **Produção** — Quando o código estiver pronto e testado, todo o código será mesclado ao `master` para implantação no site ativo de produção. Este ambiente inclui ramificação, banco de dados, servidor Web, armazenamento em cache, serviços de terceiros, variáveis de ambiente e configurações ativos do `master`.

- **Inativo** — Você tem um número ilimitado de ramificações inativas.

## Projetos Pro

A [arquitetura do plano Pro](pro-architecture.md) tem um `master` global com três ambientes:

- **Integração** — O ambiente de integração fornece um ambiente testável que inclui um banco de dados, servidor Web, armazenamento em cache, alguns serviços, variáveis de ambiente e configurações. Você pode desenvolver, implantar e testar seu código antes de mesclar ao ambiente de preparo.

   - _Inativo_ — Você pode ter um número ilimitado de ramificações inativas com base no ambiente `integration`, mas apenas uma ramificação ativa (sem incluir `integration` ).

- **Estágios** — O ambiente de preparo é para testes de pré-produção e inclui um banco de dados, servidor Web, armazenamento em cache, serviços de terceiros, variáveis de ambiente, configurações e serviços, como o Fastly.

- **Produção** — O ambiente de produção inclui uma arquitetura de três nós e alta disponibilidade para seus dados, serviços, armazenamento em cache e armazenamento. A produção é o ambiente de armazenamento público e ativo com variáveis de ambiente, configurações e serviços de terceiros.

## Software e serviços compatíveis

O Adobe Commerce na infraestrutura em nuvem usa:

- Sistema operacional: Debian GNU/Linux
- Servidor da Web: Nginx
- Banco de dados: MySQL (MariaDB)
- Rede de entrega de conteúdo (CDN): Fastly CDN

Você pode configurar os seguintes serviços:

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [AtiveMQ](../services/activemq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>Consulte [Requisitos do sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=pt-BR) no _Guia de instalação_ para obter as versões recomendadas.

O módulo Fastly CDN é usado para serviços de CDN e cache em ambientes de preparo e produção. Consulte [Configurar serviços do Fastly](../cdn/fastly.md).

Para obter informações sobre como configurar as versões de software a serem usadas na implementação, consulte os seguintes arquivos de configuração do Adobe Commerce na infraestrutura em nuvem:

- [Configuração do aplicativo (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Configuração de ambiente (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Configuração de rotas (route.yaml)](../routes/routes-yaml.md)
- [Configuração de serviços (services.yaml)](../services/services-yaml.md)
