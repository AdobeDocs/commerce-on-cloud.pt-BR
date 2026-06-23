---
title: Pilha de tecnologia
description: Consulte a pilha de tecnologia que forma a infraestrutura do Commerce na nuvem.
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
TQID: https://experienceleague.adobe.com/2-uZdx1Oi-3LQUK-L7rC4kZWYcYobEhnVcUDNwOHpFs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: df5e974b-6742-4873-a687-a6bedaafdaa2
  - id: f2261633-201d-46c5-8a66-999e70527a83
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 405
ht-degree: 0%

---

# Pilha de tecnologia

Considere a Adobe Commerce na infraestrutura em nuvem como cinco camadas funcionais, conforme mostrado abaixo:

![Pilha na nuvem](../../assets/CloudStack.png)

1. [**Infraestrutura em nuvem**](pro-architecture.md): escolha Amazon Web Services (AWS) ou Microsoft Azure como base da sua Infraestrutura como um Serviço (IaaS) para o seu Adobe Commerce em projetos Pro de infraestrutura em nuvem.

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

