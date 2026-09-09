---
title: Noções básicas sobre o aplicativo
description: Saiba mais sobre como o Adobe Commerce Traffic Insights funciona, como direcioná-lo com filtros, como seus dados são medidos e suas limitações de dados e desempenho.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Noções básicas sobre o aplicativo

O aplicativo [!DNL Adobe Commerce Traffic Insights] visualiza logs de acesso brutos da Rede de Entrega de Conteúdo do Fastly (CDN) em uma imagem do tráfego de borda de uma loja. Os gráficos são agrupados nas seguintes guias:

- **Largura de banda** — Como a largura de banda de tráfego é distribuída entre domínios, tipos de conteúdo e recursos e projetos na nuvem ao longo do tempo.
- **Desempenho do Cache de Página Inteira** — Com que eficiência o HTML de vitrine dinâmica para páginas de detalhes do produto (PDP), páginas de listagem de produtos (PLP) e páginas do sistema de gerenciamento de conteúdo (CMS) é armazenado em cache na borda.
- **Análise de Atividade e Solicitações de Bots** — Tráfego detalhado por agentes de bot conhecidos, geolocalização, IPs/sub-redes, URLs e sinais do WAF (Next-Gen Web Application Firewall) do Fastly.

Uma quarta guia **Documentação** no aplicativo contém observações conceituais e o [Manual de investigação](investigation-playbook.md).

## Para quem este guia se destina?

- **Operadores de site e SREs (Engenharia de Confiabilidade de Site)** investigando a sobrecarga de largura de banda, picos de tráfego ou carga de origem da CDN.
- **Desenvolvedores** ajustando a cobertura e a taxa de ocorrência do Cache de Página Completa (FPC) ou implementando as regras do Fastly Varnish Configuration Language (VCL).
- **Administradores e engenheiros de segurança** identificando e mitigando bots indesejados, raspadores e tráfego automatizado mal-intencionado.

Supõe-se que haja familiaridade com [!DNL Adobe Commerce on Cloud Infrastructure], conceitos do Fastly CDN e navegação básica no New Relic.

## Como funciona

Selecione uma conta e um intervalo de tempo nos controles de plataforma na parte superior da página. Um **ID de Projeto** opcional pode restringir ainda mais os gráficos a projetos na Nuvem específicos. Em uma configuração de conta principal ou parceria, poder visualizar uma conta na lista suspensa não significa que você possa consultá-la. Se um gráfico relatar um erro de permissão, alterne para uma conta à qual você tenha acesso ao New Relic Query Language (NRQL).

Continue a aplicar filtros para transformar uma visão geral ampla em uma investigação focada. Clique em um valor em uma coluna de facetas, como um bot, IP, sub-rede, país ou tipo de conteúdo, para adicionar um [filtro global](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use). Os filtros ativos são exibidos na parte superior da grade e aplicados em cada widget em cada guia simultaneamente. Para ampliar o escopo, remova um filtro.

**Apresentação** - Considere um cenário em que a *Largura de Banda Total* esteja ultrapassando a tendência da permissão contratual e você queira saber quem a está conduzindo:

1. Abra a guia **Análise de atividade e solicitações de bots** e leia **Estrutura de largura de banda** para ver quanto do tráfego é automatizado versus orgânico.
1. Se os bots parecerem ter mais tráfego, abra **Bots conhecidos por largura de banda** e clique no bot nomeado mais pesado, por exemplo, um scraper. Isso adiciona um novo filtro, o que significa que agora cada widget tem seu escopo definido para esse bot.
1. Leia **Detalhes do impacto de bots conhecidos** para obter sua taxa de solicitação, combinação de status e taxa de ocorrência de FPC.
1. Para ver a origem do bot, verifique **Largura de Banda por País**. Para ver o que o bot está buscando, consulte **URLs por largura de banda**.
1. Se o tráfego se concentrar em uma rede, clique em **Estatísticas por sub-redes IP** para confirmar a rotação de um ator pelos endereços em um único bloco.
1. Agora você tem quem, o que e onde é necessário para escrever uma mitigação direcionada. Prossiga para o [Manual de investigação](investigation-playbook.md) para saber como proceder.

O mesmo método de filtragem funciona de qualquer faceta inicial: um país suspeito, um único IP, um tipo de conteúdo ou um segmento de caminho de URL.

## Como os dados são medidos

Compreender algumas opções de medição facilita a confiança e a interpretação dos números.

- **Largura de Banda (BW)** é o total de bytes que a CDN forneceu para as solicitações correspondentes, contando **os cabeçalhos de resposta e o corpo**. É a métrica de custo de título que conta em relação à bonificação do contrato.
- **Solicitações (Requisição)** é o número de solicitações distintas, entretanto, com o Fastly [shielding](https://www.fastly.com/documentation/guides/concepts/shielding/) habilitado, uma única solicitação é registrada **duas vezes**, uma vez em cada um dos itens a seguir:
  - Blindagem interna [Ponto de Presença (POP)](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)
  - POP EDGE
    Isso acontece a menos que a resposta venha diretamente do cache POP local ou o escudo esteja agindo como o POP para a localização do remetente. Para evitar a dupla contagem desses `HIT,MISS` e `MISS,MISS` casos, as consultas do aplicativo são agregadas com [`uniqueCount`](https://docs.newrelic.com/docs/nrql/nrql-syntax-clauses-functions/#func-uniqueCount) sobre o campo `request_id`. Isso retorna uma **aproximação** de fechamento com uma margem de erro esperada de **~5%**, não uma contagem exata.
- **Segmentos de rede CDN** são compactados de forma diferente. A resposta entregue ao cliente está compactada, mas o tráfego shield-to-POP está [não compactado](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) para preservar o suporte a [Edge Side Includes (ESI)](https://www.fastly.com/documentation/reference/vcl/statements/esi/). Uma baixa taxa de acerto de cache, portanto, aumenta o segmento interno mais do que o voltado para o cliente, já que o conteúdo não armazenado em cache deve ser puxado pela blindagem repetidamente em tamanho máximo e descompactado. Essa compactação é o motivo pelo qual o widget **Largura de banda de segmento de rede** da CDN e a taxa de ocorrência do FPC são duas exibições do mesmo custo subjacente.

## Limitações de dados e desempenho

- **Retenção de 30 dias** - Os logs do Fastly CDN ficam retidos no New Relic por **30 dias** de acordo com o plano de assinatura. Qualquer janela que você escolher deverá estar dentro dos últimos 30 dias. Para largura de banda de longo prazo de **total**, use a integração direta do Fastly no painel [!DNL Adobe Commerce admin], **Dashboard > Fastly > Largura de Banda > Total**, mas considere que ela reporta o ID por serviço, portanto, os dados devem ser coletados por ambiente e agregados para comparação com a permissão de contrato.
- **Limite de consulta de 60 segundos** - Cada NRQL do gráfico tem um [limite de execução de 60 segundos](https://docs.newrelic.com/docs/nrql/using-nrql/rate-limits-nrql-queries/#query-duration). Para contas de tráfego muito alto, um widget pode expirar enquanto verifica muitos registros de log. Se isso acontecer, reduza o intervalo de tempo e recarregue os gráficos. É possível expandi-la novamente para guias mais claras.
