---
title: Pilha de tecnologia
description: Consulte a pilha de tecnologia que forma a infraestrutura do Commerce na nuvem.
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Pilha de tecnologia

Considere a Adobe Commerce na infraestrutura em nuvem como cinco camadas funcionais, conforme mostrado abaixo:

![Pilha na nuvem](../../assets/CloudStack.svg)

1. [**Infraestrutura em nuvem**](pro-architecture.md): escolha Amazon Web Services (AWS) ou Microsoft Azure como a base da sua Infraestrutura como um Serviço (IaaS) para os projetos do Adobe Commerce no Cloud Infrastructure Pro.

   A Adobe analisa seu uso de recurso de computação virtual (vCPU) com frequência e aloca recursos automaticamente para otimizar seu uso a longo prazo e reduzir o risco de exceder sua permissão diária máxima anual de vCPU. Se você espera um aumento no tráfego do site por períodos específicos, continue abrindo um tíquete de Suporte para [solicitar um upsize temporário](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=pt-BR).

1. [**Platform as a Service**](cloud-architecture.md): cada projeto de infraestrutura do Adobe Commerce na nuvem fornece um ambiente de Integração do Platform as a Service (PaaS) para desenvolvimento, teste e integração de serviços.
1. [**Adobe Commerce**](../project/overview.md): o Adobe Commerce na infraestrutura em nuvem fornece uma infraestrutura pré-provisionada que inclui PHP, MySQL (MariaDB), Redis, serviços de fila de mensagens ([!DNL RabbitMQ] ou [!DNL ActiveMQ]) e tecnologias de mecanismo de pesquisa com suporte.
1. [**Ferramentas de Desempenho**](../monitor/new-relic-service.md): as ferramentas de desempenho do New Relic permitem que você depure, monitore e gerencie seus aplicativos e infraestrutura coletando, analisando e exibindo dados da sua Adobe Commerce em projetos de infraestrutura em nuvem.
1. [**Rede de Entrega de Conteúdo (CDN), Firewall do Aplicativo Web ([!DNL WAF]) e Otimização de Imagem (IO)**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) — Fornece serviços de CDN seguros com proteção interna contra ataques de DDoS (Negação de Serviço Distribuído) como [!DNL Ping of Death], [!DNL Smurf] ataques e outros ataques de inundação baseados em ICMP.
   * [WAF (Web Application Firewall)](../cdn/fastly-waf-service.md) — os serviços da WAF garantem a conformidade com o PCI para vitrines da Adobe Commerce em ambientes de produção e com as políticas da WAF que protegem os aplicativos Web da Adobe Commerce contra ataques de injeção, entradas mal-intencionadas, scripts entre sites, exfiltração de dados, violações de protocolo HTTP e outras [[!DNL OWASP] Dez maiores ameaças à segurança](https://owasp.org/www-project-top-ten/).
   * [Otimização de imagem (IO)](../cdn/fastly-image-optimization.md) — Fornece manipulação e otimização de imagem em tempo real para acelerar a entrega de imagens e simplificar a manutenção de conjuntos de fontes de imagem para aplicativos Web responsivos. O Fastly IO descarrega o processamento de imagens e a carga de redimensionamento, liberando servidores para processar pedidos e conversões com eficiência.

Os aplicativos monolíticos consomem muitos recursos e são difíceis de dimensionar e atender rapidamente. Com a infraestrutura em nuvem, os clientes da Commerce obtêm acesso inigualável a microsserviços baseados em SaaS, que são avançados, inteligentes e eficientes. Consulte [Software e serviços com suporte](cloud-architecture.md#supported-software-and-services).

Use o [Guia de Introdução do Commerce](../../get-started/overview.md) para configurar seu novo programa na Nuvem e começar a gerenciar seu aplicativo do [!DNL Commerce] em um ambiente nativo da nuvem.
