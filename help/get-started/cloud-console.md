---
title: Fazer logon em  [!DNL Cloud Console]
description: Saiba mais sobre o [!DNL Cloud Console] for Adobe Commerce na infraestrutura em nuvem.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# Fazer logon em [!DNL Cloud Console]

O [!DNL Cloud Console] fornece métodos interativos para compilar, gerenciar e implantar o código Commerce. O [!DNL Cloud Console] é uma experiência mais moderna e fácil de usar e cria a base para aprimoramentos futuros na interface.

[Faça logon em  [!DNL Cloud Console]](https://console.adobecommerce.com) para ver sua lista de projetos.

![Lista de projetos](../assets/ui-allprojects-list.png)

## Recursos

Os recursos novos ou aprimorados incluem:

- Visão geral clara das características do projeto e do ambiente
- Fluxo de atividades com histórico classificável
- Gerenciamento manual de backup e histórico para projetos iniciais
- Exibições de log aprimoradas
- Listas classificáveis
- Formulários simples e orientação para adicionar integrações
- Conformidade com as Diretrizes de acessibilidade de conteúdo da Web (WCAG)

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

Os recursos novos ou aprimorados são os seguintes:

| Recurso | Melhorias |
| -------------- | ----------------------------------- |
| [Fluxo de atividade](../cloud-guide/project/activity-stream.md) | Interaja com uma lista classificável de ações em execução, pendentes ou históricas. Selecione uma atividade e visualize logs ou cancele um build em execução. |
| [Visões gerais do projeto e do ambiente](../cloud-guide/project/overview.md#project-overview) | Abra o projeto e veja a visão geral dos detalhes do projeto e a lista de ambientes. A visão geral do ambiente fornece mais detalhes sobre o status do ambiente, o acesso ao aplicativo e as atividades recentes. |
| [Formulários de integração](../cloud-guide/integrations/overview.md) | Use formulários simples e orientações para adicionar integrações, como notificações de Bitbucket ou Slack. |
| [Lista de projetos](../cloud-guide/project/overview.md#cloud-console) | A exibição _Todos os projetos_ lista todos os projetos aos quais você tem permissão de acesso. Você pode clicar em **[!UICONTROL Show filters]** e filtrar sua lista de projetos por tipo, região ou plano. |
| [Opções de visibilidade de variáveis](../cloud-guide/environment/variable-levels.md) | Limitar a visibilidade de uma variável de nível de projeto ou de ambiente durante a compilação ou o tempo de execução. |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## Perguntas sobre o console

**_Onde posso encontrar o recurso Instantâneos_**?

Para projetos [!DNL Starter], o recurso Instantâneos agora é chamado _Backups_. Você pode criar um backup manual do seu ambiente [!DNL Starter] a partir do [!DNL Cloud Console] ou criar um instantâneo a partir da CLI da nuvem. Você deve ter uma função de Administrador para o ambiente.

Selecione um ambiente na barra de navegação do projeto. O ambiente deve estar ativo. Selecione a guia **[!UICONTROL Backups]**. Atualmente, essa opção não está disponível para ambientes Pro.

**_Onde está a lista de rotas configuradas para o ambiente_**?

Você pode encontrar a lista de rotas configuradas na guia _Serviços_ para um ambiente.

Selecione um ambiente na barra de navegação do projeto. Selecione a guia **[!UICONTROL Services]**. A visão geral do **Roteador** exibe as rotas configuradas. Atualmente, não é possível adicionar uma rota a partir do novo [!DNL Cloud Console].

## Menu Conta

No canto superior direito, encontra-se o menu da conta. Clique na seta para baixo do menu e selecione **[!UICONTROL My Profile]**. Na exibição _Meu Perfil_, você pode controlar seus detalhes de usuário e configurações de exibição, gerenciar [autenticação de segurança](../cloud-guide/project/user-access.md#user-authentication-requirements), [tokens de API](../cloud-guide/project/user-access.md#create-an-api-token) e [chaves SSH](../cloud-guide/development/secure-connections.md).
