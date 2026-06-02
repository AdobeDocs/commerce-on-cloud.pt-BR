---
title: Fluxo de atividade
description: Saiba como ler o fluxo de atividades no  [!DNL Cloud Console] ou na CLI da nuvem para a infraestrutura do Adobe Commerce na nuvem.
last-substantial-update: 2024-02-06T00:00:00.000Z
exl-id: 1acac76e-b088-44ad-9c93-ea27b686b6b4
TQID: https://experienceleague.adobe.com/piZcU6SjL054YJU9yISQJss7LVEHEHZc7zSYa2QGe60
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 448
ht-degree: 0%

---

# Fluxo de atividade

A exibição principal de cada ambiente exibe uma lista de eventos históricos de **Atividade** semelhantes a um log do Git. A lista de Atividades é um fluxo dos eventos recentes para ambientes ativos. Veja a seguir uma lista dos tipos de atividades e seus ícones exibidos no fluxo de Atividades:

![Tipos de atividade](../../assets/activity-types.svg){width="500" align="center"}

## Exibir logs

Na lista Atividade, clique no ícone de status de uma atividade para exibir o log. Como alternativa, clique no menu ![Mais](../../assets/icon-more.png){width="32"} (_mais_) para acessar mais opções para gerenciar a atividade. A seguir, é mostrado um log curto que cria um backup. Você pode [usar a CLI da Nuvem](#activity-stream-with-cloud-cli) para exibir o mesmo log.

![Exibição de log](../../assets/log-view.png)

## Gerenciar uma atividade

Algumas atividades estão com o status _em execução_ ou _pendente_. Você pode agir em uma atividade em execução, como cancelar uma implantação em execução. As guias a seguir mostram dois métodos de cancelamento de uma atividade: o [!DNL Cloud Console] ou a CLI da nuvem.

>[!BEGINTABS]

>[!TAB Console]

**Para cancelar uma atividade no[!DNL Cloud Console]**:

Você pode agir em uma atividade em execução acessando o menu ![Mais](../../assets/icon-more.png){width="32"} (_mais_) e selecionando uma ação, como `Cancel` ou `View log`. Para este exemplo, selecione a opção **Cancelar** para interromper a atividade em execução.

Nem todas as atividades têm a opção de cancelamento. Por exemplo, a opção para cancelar a implantação do aplicativo aparece somente durante a fase _build_. Depois que o aplicativo for movido para a fase _de implantação_, não será mais possível cancelar a atividade. Consulte [Processo de implantação](../deploy/process.md) sobre as diferentes fases.

![Cancelar atividade](../../assets/activity-icons/cancel-activity.png){width="450" align="center"}

Se você tiver um terminal executando a atividade de implantação, cancelar no [!DNL Cloud Console] resultará no cancelamento no terminal:

![Atividade cancelada no terminal](../../assets/activity-icons/activity-cancelled.png){width="300"}

>[!TAB CLI]

**Para cancelar uma atividade na CLI da Nuvem**:

1. Identifique as atividades em execução e selecione uma ID de atividade.

   ```bash
   magento-cloud activity:list --state=in_progress
   ```

1. Cancele a atividade usando a ID da atividade:

   ```bash
   magento-cloud activity:cancel wvl5wm7s5vkhy
   ```

>[!ENDTABS]

## Filtrar fluxo de atividade

A capacidade de filtrar a lista de atividades é útil quando você está procurando algo específico, como um backup ou um evento de mesclagem.

**Para filtrar a lista de atividades no[!DNL Cloud Console]**:

1. Selecione um ambiente e escolha o modo de exibição Atividade **[!UICONTROL All]** para incluir o histórico completo de eventos.

1. Clique em ![Filtrar por](../../assets/icon-filterby.png){width="32"} e selecione as opções **[!UICONTROL Filter by]**:

   ![Filtrar atividades](../../assets/activity-filter.png)

1. Escolha a exibição da Atividade **[!UICONTROL Recent]** e redefina a lista.

## Exibir fluxo com a CLI da nuvem

A CLI do `magento-cloud` fornece a maioria das mesmas capacidades que a [!DNL Cloud Console]. O comando `activity` pode:

- `list` o fluxo de atividades para um ambiente
- `get` detalhes sobre uma atividade específica
- exibir o `log` para uma atividade específica
- `cancel` uma atividade

**Para exibir o fluxo de atividades com a CLI de Nuvem**:

1. Listar as atividades do ambiente atual.

   ```bash
   magento-cloud activity:list
   ```

1. Cada atividade tem uma ID exclusiva. Selecione uma ID na lista anterior e visualize os detalhes dessa atividade.

   ```bash
   magento-cloud activity:get wvl5wm7s5vkhy
   ```

1. Exibir o log completo dessa atividade.

   ```bash
   magento-cloud activity:log wvl5wm7s5vkhy
   ```

   Exemplo de resposta:

   ```bash
   Activity ID: wvl5wm7s5vkhy
   Type: environment.backup
   Description: User created a backup of Master
   Created: 2023-09-08T14:03:33+00:00
   State: complete
   Log:
   Creating backup of master
   Created backup eg5pu63egt2dcojkljalzjdopa
   ```
