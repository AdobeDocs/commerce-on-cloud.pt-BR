---
title: Provisionamento do Commerce na nuvem
description: Saiba como preparar um consultor técnico do cliente da Adobe para provisionar seu projeto de infraestrutura em nuvem do Adobe Commerce.
recommendations: noDisplay, catalog
role: Admin
exl-id: 77e8c9fb-8c4a-4c98-adbc-e57871c5bdbc
TQID: https://experienceleague.adobe.com/GzCPqYxn0-ACS34UfvHypqv6BveTRpbLJNN2jNmCL-E
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: cc72dcf1-72e1-48cc-b434-e7c27d62d67c
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 774
ht-degree: 0%

---

# Pré-requisitos de provisionamento do Commerce na nuvem

Vamos começar e inicializar seu projeto do Commerce na infraestrutura em nuvem!

Antes de a Adobe fornecer sua Commerce em ambientes de projeto na nuvem, é recomendável considerar as seguintes estratégias e preparar respostas para sua primeira consulta com a equipe de conta da Adobe. Use as seguintes seções como lista de verificação para ajudá-lo a se preparar para a conversa com um consultor técnico do cliente para provisionar um projeto em nuvem:

## Definição de domínio

**Pergunta 1**: _Que domínio ou domínios você pretende usar para a inicialização do site?_

Prepare-se para integrar os serviços Fastly e nginx definindo seus domínios e subdomínios de nível superior para os ambientes Pro Staging e Production. Após a configuração inicial, só é possível adicionar domínios enviando um tíquete de suporte da Adobe Commerce. Portanto, recomenda-se que as informações de domínio estejam prontas.

Exemplos para domínios de produção e de preparo:

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

Consulte [Configurar vários sites ou lojas](../cloud-guide/store/multiple-sites.md) no guia _Commerce na Infraestrutura na Nuvem_ para obter mais orientações sobre domínios múltiplos ou exclusivos.

Consulte [Várias contas do Fastly e domínios atribuídos](https://experienceleague.adobe.com/pt-br/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"} se você tiver uma conta do Fastly existente que vincula o mesmo ápice e subdomínios usados no site do Adobe Commerce.

## Domínio de email transacional

**Pergunta 2**: _Qual(is) domínio(s) você pretende usar para emails transacionais?_

O Adobe Commerce na nuvem usa o serviço proxy SendGrid SMTP (Simple Mail Transfer Protocol), que fornece autenticação de email de saída e serviços de monitoramento de reputação. O SendGrid envia emails transacionais em seu nome, portanto, requer informações de domínio.

Exemplo do domínio SendGrid: `example@your-store.com`

Consulte [SendGrid mail service](../cloud-guide/project/sendgrid.md) no guia do _Commerce on Cloud Infrastructure_ para obter mais orientações sobre emails transacionais e configurações de domínio.

## Alocação de armazenamento

**Pergunta 3**: _Quanto do armazenamento contratado você planeja alocar para carregamento de arquivo e para banco de dados?_

O Adobe Commerce na infraestrutura em nuvem usa o armazenamento para a estrutura de arquivos, a indexação de pesquisa e o banco de dados. Você pode subdividir o armazenamento conforme necessário, começando com 10 GB para cada partição.

A quantidade de armazenamento que você contratou para seu projeto de nuvem representa o armazenamento total para cada ambiente. Por exemplo, se você comprou 50 GB de espaço de armazenamento, terá 50 GB de armazenamento para cada ambiente. Há 50 GB de armazenamento separado para produção, preparo e cada ambiente de integração, respectivamente.

Você pode aumentar seu armazenamento contratado a qualquer momento. Para ambientes de produção e preparo profissionais, você deve enviar um tíquete de suporte da Adobe Commerce para alterar a alocação de espaço em disco. Um aumento de tamanho dos ambientes de produção Pro e de preparo só pode ocorrer em determinados intervalos. Dependendo do uso atual do espaço em disco, a equipe de suporte pode recomendar o aumento da alocação de espaço em disco em um mínimo de 10 GB. Depois de alocado, o aumento de armazenamento para Preparo e Produção Profissionais pode **não** ser revertido.

Consulte [Gerenciar espaço em disco](../cloud-guide/storage/manage-disk-space.md) no guia do _Commerce on Cloud Infrastructure_.

## Região do Cloud Service

**Pergunta 4**: _Qual região do Cloud Service é mais conveniente para sua proximidade?_

Escolha a base Amazon Web Services (AWS) ou Microsoft Azure as your Infrastructure as a Service (IaaS) para seus projetos do Adobe Commerce on cloud infrastructure Pro. Cada provedor de serviços opera em várias regiões e fornece várias zonas de disponibilidade. Selecione uma região conveniente para sua localização e reduza a possibilidade de latência e custos mais altos.

Consulte [um mapa das regiões de nuvem da Adobe Commerce](../cloud-guide/overview.md).

## Serviço de conexão

**Pergunta 5**: _Você precisa de um serviço PrivateLink? Em caso afirmativo, em qual região está a conexão PrivateLink?_

A infraestrutura do Adobe Commerce na nuvem suporta a integração com o serviço AWS PrivateLink ou Azure Private Link. Embora esse serviço seja opcional, o PrivateLink é usado para estabelecer comunicação segura e privada entre ambientes de infraestrutura em nuvem com serviços e aplicativos hospedados em sistemas externos.

É importante considerar sua estratégia do Cloud Service (AWS ou Azure) para que a instância do Adobe Commerce possa ser acessada na mesma região. Consulte o [serviço PrivateLink](../cloud-guide/development/privatelink-service.md) no guia do _Commerce na Infraestrutura na Nuvem_ para obter mais esclarecimentos sobre acessibilidade regional.

## Inicialização do site do Target

**Pergunta 6**: _Qual é a data de lançamento prevista?_

O lançamento de um site requer configuração iterativa e teste para garantir o sucesso do lançamento do site. Definir uma data de destino ajuda você e sua equipe de conta da Adobe a se prepararem para as atividades finais e de pré-lançamento, que incluem uma chamada para coordenar as etapas finais.

Consulte a [Visão geral do site do Launch](../cloud-guide/launch/overview.md) no guia do _Commerce na Infraestrutura na Nuvem_ para analisar o processo completo e baixar uma cópia da lista de verificação do Launch.

>[!TIP]
>
> Dê uma olhada no portal na nuvem e acesse seu novo projeto.
>
>**Próxima etapa**: [Integração com o Commerce](onboarding.md)
