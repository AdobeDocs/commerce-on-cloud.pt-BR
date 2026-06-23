---
title: Otimizar a implantação em nuvem
description: Saiba mais sobre maneiras de otimizar o processo de implantação do Adobe Commerce em projetos de infraestrutura em nuvem, incluindo redução do tempo de inatividade, implantação de conteúdo estático, implantação baseada em cenário e assistentes inteligentes.
feature: Cloud, Deploy, SCD
exl-id: 4315e2f4-06af-4a5c-9db9-e7b2f63660df
TQID: https://experienceleague.adobe.com/bd9n9CFrpyn1UZG6SX8qkoZGBOFd2N7z9Hoa1hQ8rew
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 230
ht-degree: 0%

---

# Otimizar implantação

O desempenho do site pode sofrer durante o processo de implantação. O período de tempo em que um site está no modo de manutenção ao implantar em um site de produção depende de muitos fatores, como a configuração do ambiente e a quantidade de conteúdo que um site contém. A primeira prática recomendada para otimizar a implantação na Nuvem é [atualizar para usar o `ece-tools`](../dev-tools/install-package.md) para se beneficiar dos recursos do pacote, como comandos para criar um backup do banco de dados e verificar a configuração do ambiente.

Os seguintes tópicos podem ajudá-lo a entender melhor como otimizar o processo de implantação:

- [Processo de implantação na nuvem](process.md)
Há três fases no processo de implantação da nuvem, e você pode usar os pontos fortes e fracos de cada fase a seu favor.

- [Implantação sem tempo de inatividade](reduce-downtime.md)
Entenda o que acontece durante a implantação e como reduzir a quantidade de tempo de inatividade das experiências da loja durante uma atualização do ambiente de Produção.

- [Implantação de conteúdo estático](static-content.md)
A melhor maneira de otimizar a implantação na nuvem é controlar como e quando gerar conteúdo estático.

- [Assistentes inteligentes](smart-wizards.md)
O pacote `ece-tools` fornece os comandos do assistente inteligente para avaliar rapidamente a configuração do projeto.

- [Rastrear implantações com o New Relic](../monitor/track-deployments.md)
Use o serviço New Relic para monitorar eventos de implantação e analisar o impacto da implantação no desempenho geral.

