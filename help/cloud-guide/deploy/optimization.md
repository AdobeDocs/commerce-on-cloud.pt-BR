---
title: Otimizar a implantação em nuvem
description: Saiba mais sobre maneiras de otimizar o processo de implantação do Adobe Commerce em projetos de infraestrutura em nuvem, incluindo redução do tempo de inatividade, implantação de conteúdo estático, implantação baseada em cenário e assistentes inteligentes.
feature: Cloud, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Otimizar implantação

O desempenho do site pode sofrer durante o processo de implantação. O período de tempo em que um site está no modo de manutenção ao implantar em um site de produção depende de muitos fatores, como a configuração do ambiente e a quantidade de conteúdo que um site contém. A primeira prática recomendada para otimizar a implantação na Nuvem é [atualizar para usar o `ece-tools`](../dev-tools/install-package.md) para se beneficiar dos recursos do pacote, como comandos para criar um backup do banco de dados e verificar a configuração do ambiente.

Os seguintes tópicos podem ajudá-lo a entender melhor como otimizar o processo de implantação:

- [Processo de implantação na nuvem](process.md)
Há três fases no processo de implantação da nuvem, e você pode usar os pontos fortes e fracos de cada fase a seu favor.

- [Nenhuma implantação de tempo de inatividade](reduce-downtime.md)
Entenda o que acontece durante a implantação e como reduzir a quantidade de tempo de inatividade das experiências da loja durante uma atualização do ambiente de Produção.

- [Implantação de conteúdo estático](static-content.md)
A melhor maneira de otimizar a implantação na nuvem é controlar como e quando gerar conteúdo estático.

- [Assistentes inteligentes](smart-wizards.md)
O pacote `ece-tools` fornece os comandos do assistente inteligente para avaliar rapidamente a configuração do projeto.

- [Rastrear implantações com o New Relic](../monitor/track-deployments.md)
Use o serviço New Relic para monitorar eventos de implantação e analisar o impacto da implantação no desempenho geral.
