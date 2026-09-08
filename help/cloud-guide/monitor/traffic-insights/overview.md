---
title: Insights de tráfego do Adobe Commerce
description: Saiba mais sobre a ferramenta Adobe Commerce Traffic Insights e como ela pode ajudar você a entender o tráfego no seu projeto de infraestrutura em nuvem do Adobe Commerce.
feature: Cloud, Observability
role: Admin
source-git-commit: 119c9415abd22221e3ae785445d537f0609eba14
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Insights de tráfego

O Adobe Commerce Traffic Insights é um aplicativo do New Relic One que visualiza o tráfego de CDN do Fastly [!DNL Adobe Commerce on Cloud Infrastructure]. Ele lê as linhas de log de acesso do Fastly CDN que já estão sendo enviadas para o New Relic como `Log` eventos e renderiza um conjunto preparado de gráficos, com escopo para uma conta do New Relic selecionada e o intervalo de tempo da plataforma. Isso visualiza o tráfego de borda de uma loja sem gravar o NRQL, a linguagem de consulta do New Relic, manualmente.

## O que ajuda a investigar

Os Insights de tráfego foram projetados para ajudar você a resolver três problemas comuns:

- **Sobreposição de largura de banda da CDN** — Tráfego com tendência acima da permissão de contrato. Atribua o volume a mídia pesada, arquivos grandes, páginas 404 não armazenáveis em cache ou um cache ineficiente, até um domínio, tipo de conteúdo, URL ou projeto específico.
- **Pesquisar carga de bot e rastreador** — Um mecanismo de pesquisa ou rastreador de IA que gera um compartilhamento desproporcional de solicitações, prejudicando a eficiência do cache e a carga de origem. Veja quais bots nomeados são mais ativos e exatamente o que eles buscam.
- **Scripts e raspadores mal-intencionados** — raspagem, enchimento de credenciais, testes de cartão, criação de contas falsas ou abuso de Camada 7. Mostrar os sinais do Fastly Next-Gen WAF e os IPs, as sub-redes e os países por trás do tráfego suspeito.

Em cada caso, o aplicativo identifica o *quem, o que e onde* do tráfego. Atuando sobre essas informações por meio das regras de VCL do Fastly, otimização de imagem, ajuste de cache, limitação de taxa ou pelo complemento [Segurança Avançada](../../cdn/advanced-security.md) do Adobe em sua configuração do Commerce e do Fastly. O [manual de investigação](investigation-playbook.md) aborda cada um desses problemas.

## Acesso ao aplicativo

- **Link direto:** [Insights de tráfego do Adobe Commerce](https://one.newrelic.com/a9a0c3b8-3844-4ca1-8bad-c6742747be47).
- **Na tela inicial do New Relic One** (one.newrelic.com) — depois que a conta é assinada pelo aplicativo, ela aparece como seu próprio bloco, **Adobe Commerce Traffic Insights** na página inicial.
- **Na barra de pesquisa superior (Localização Rápida)** — pesquise por `Adobe Commerce Traffic Insights` e selecione-o nos resultados.
- **Para fixá-lo para obter acesso mais rápido** - use o controle de estrela ou fixador no bloco ou no cabeçalho da página do aplicativo para adicioná-lo aos favoritos ou à navegação à esquerda. O local exato desse controle depende da versão da interface do usuário do New Relic em uso para a conta.

## Neste guia

- **[Entendendo o aplicativo](understanding-the-app.md)** - O que são os Insights de Tráfego, como conduzi-los com filtros, como os números são medidos e o que os dados podem e não podem dizer a você.
- **[Manual de investigação](investigation-playbook.md)** - Abordagens recomendadas para os três problemas que o aplicativo foi criado para resolver: excesso de largura de banda, carga de rastreador e tráfego mal-intencionado. Cada uma dessas referências ao gráfico que a confirma e especifica o caminho de escalonamento de [Segurança avançada](../../cdn/advanced-security.md) nativo da Adobe para quando a mitigação manual é insuficiente.