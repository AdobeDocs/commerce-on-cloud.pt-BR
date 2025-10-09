---
title: Commerce na infraestrutura em nuvem
description: Saiba mais sobre como criar, implantar e gerenciar a infraestrutura do Commerce na nuvem.
exl-id: a37d0403-df14-4bb9-8cc4-25436560ba0c
source-git-commit: 11d0e86ad4633d9c4232474b9921ed491f285969
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 3%

---


# Commerce na infraestrutura em nuvem

O Adobe Commerce na infraestrutura em nuvem fornece uma plataforma de hospedagem automatizada com uma abordagem de **autoatendimento** para criar, implantar e gerenciar o aplicativo [!DNL Commerce] em um ambiente nativo em nuvem. A infraestrutura do Adobe Commerce na nuvem vem com recursos adicionais que a diferenciam das plataformas locais do Adobe Commerce e do Magento Open Source:

- Uma infraestrutura pré-provisionada que inclui PHP, MySQL (MariaDB), Redis, [!DNL RabbitMQ] e tecnologias de mecanismo de pesquisa compatíveis.
- Fluxo de trabalho baseado em Git com criação e implantação automáticas para desenvolvimento rápido e implantação contínua eficientes sempre que você envia alterações de código em um ambiente de Plataforma como um serviço (PaaS).
- Arquivos de configuração de ambiente altamente personalizáveis e ferramentas de gerenciamento e implantação da interface de linha de comando (CLI).
- Hospedagem do Amazon Web Services (AWS) que oferece um ambiente escalável e seguro para vendas e varejo online.

![Benefícios da nuvem](../assets/CloudBenefits.svg)

>[!NOTE]
>
>Para obter mais informações sobre segurança, consulte a [lista de verificação de inicialização de segurança](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist#security-configuration).

Veja a [Pilha de tecnologia](architecture/tech-stack.md) em detalhes ou saiba mais sobre recursos específicos e produtos com suporte na [Arquitetura de nuvem do Commerce](architecture/cloud-architecture.md).

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## Regiões em nuvem

As seções a seguir fornecem detalhes sobre as diferentes regiões do AWS e do Azure disponíveis para o Adobe Commerce na infraestrutura em nuvem.

## Regiões do AWS

![Diagrama mostrando regiões do AWS](../assets/aws-regions.svg){zoomable="yes"}

>[!NOTE]
>
> Somente no local na China e na Rússia.

## Regiões do Azure

![Diagrama mostrando as regiões do Azure](../assets/azure-regions.svg){zoomable="yes"}

>[!NOTE]
>
> Somente no local na China e na Rússia. Todos os comerciantes que exigem ambientes de integração devem usar regiões dos EUA.

## Documentação do Adobe Commerce

O guia da infraestrutura do Commerce na nuvem presume que você tenha algum conhecimento prático e compreensão do aplicativo do Adobe Commerce. Consulte os Guias do Desenvolvedor e do Usuário do [!DNL Commerce] abaixo:

- [Documentação do desenvolvedor do Adobe Commerce](https://developer.adobe.com/commerce/docs/) (site do Adobe Developer) — desenvolva, personalize, integre, estenda e use recursos avançados

- [Documentação do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce.html) (Adobe Experience League) — planeje, implemente, opere, atualize e mantenha seus projetos do [!DNL Commerce]

{{$include /help/_includes/templated/whats-new.md}}

<!-- Last updated from includes: 2025-10-08 16:18:12 -->
