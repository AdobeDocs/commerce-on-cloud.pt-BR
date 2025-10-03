---
title: Projeto de infraestrutura em nuvem
description: Leia uma visão geral sobre a Adobe Commerce na infraestrutura em nuvem [!DNL Cloud Console] e saiba como acessar as configurações da conta.
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 8eed04c7-6469-45a4-aa89-dc594c977264
source-git-commit: 00b1b6578c226a304697963d17ba349ea17da260
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# Projeto de infraestrutura em nuvem

O projeto de infraestrutura do Adobe Commerce na nuvem inclui todo o código em ramificações Git, ambientes associados e scripts para implantar o aplicativo [!DNL Commerce]. Os ambientes contêm serviços para dar suporte ao aplicativo [!DNL Commerce], incluindo um banco de dados, servidor Web e servidor de cache.

A Adobe fornece um [!DNL Cloud Console] e ferramentas de desenvolvedor para gerenciar totalmente todos os aspectos do seu projeto. Você, como proprietário da conta, tem acesso total a todos os ambientes.

## [!DNL Cloud Console]

O [!DNL Cloud Console] fornece métodos interativos para compilar, gerenciar e implantar o código Commerce em um formato simples. [Faça logon em  [!DNL Cloud Console]](https://console.adobecommerce.com) para ver sua lista de projetos. Você só pode ver projetos que você tem permissão para acessar como administrador ou para tipos de ambientes específicos. Se você for um Parceiro de soluções da Adobe, poderá ver vários projetos para clientes aos quais oferece suporte.

>[!TIP]
>
>Se você não vir nenhum projeto, entre em contato com o [Proprietário da conta ou administrador do projeto](../project/user-access.md) associado ao projeto e solicite acesso. Para novos usuários, consulte o [Tópico de integração](../../get-started/onboarding.md#cloud-console) no guia _Introdução_.

A exibição _Todos os projetos_ lista todos os projetos aos quais você tem permissão de acesso. Você pode clicar em **[!UICONTROL Show filters]** e filtrar sua lista de projetos por tipo, região ou plano.

![Lista de projetos](../../assets/ui-allprojects-list.png)

### Visão geral do projeto

Selecionar um projeto na lista _Todos os projetos_ abre a visão geral do projeto. A visão geral do projeto sempre exibe uma barra de navegação do projeto, que inclui um seletor de ambiente e um botão de configuração:

![Navegação do projeto](../../assets/project-nav.png)

A visão geral do projeto, desde que você não tenha um ambiente selecionado, mostra um resumo dos detalhes do projeto na área de visualização:

- Nome do projeto
- Região, ID do Projeto
- Planejamento, armazenamento alocado, ambientes, usuários
- URL de vitrine com o botão **[!UICONTROL Set a custom domain]**

E na visão geral do projeto principal:

- A exibição de ambientes mostra uma exibição de lista ou árvore de ![ramificação ativa](../../assets/icon-active.png){width="32"} (ativa) e ![ramificação inativa](../../assets/icon-inactive.png){width="32"} (inativa) ambientes.
- [Fluxo de atividade](activity-stream.md) mostra atividades em execução, pendentes e recentes para o projeto.
<!-- - Apps & Services—Shows a topology of service containers -->

Para projetos **Starter**, há uma hierarquia de ramificações que começa em `master` (Produção). Qualquer ramificação criada é exibida como secundária da ramificação `master`. A Adobe recomenda criar uma ramificação `staging` e, em seguida, criar uma ramificação `integration` para desenvolvimento. Consulte [Arquitetura inicial](../architecture/starter-architecture.md).

Para **Pro**, há uma hierarquia de ramificações que começa de `production` a `staging` até `integration`. O ícone ![Dedicado](../../assets/icon-dedicated.png){width="32"} indica que a ramificação é implantada em um ambiente dedicado. Todas as ramificações criadas são exibidas como filhas da ramificação `integration`. Consulte [Arquitetura Pro](../architecture/pro-architecture.md).

![Lista de ambientes profissionais](../../assets/pro-environments.png)

### Visão geral do ambiente

Selecionar um ambiente na barra de navegação do projeto altera a visão geral e a barra de navegação para focalizar no ambiente selecionado. A barra de navegação inclui controles de ramificação (Ramificação, Mesclagem e Sincronização) e um botão de configuração:

![Ambiente selecionado](../../assets/environment-selected.png)

A visão geral do ambiente mostra um resumo dos detalhes do ambiente na área de pré-visualização:

- Nome do ambiente, tipo
- Região, ID do Projeto
- Data e hora da última atividade, incluindo backup
- Acesso HTTP e status do mecanismo de pesquisa
- Nome da máquina atribuído ao ambiente
- Status do ambiente (ativo ou inativo)
- URL de vitrine com o botão **[!UICONTROL Set a custom domain]**

E na visão geral do ambiente principal:

- [Fluxo de atividade](activity-stream.md) apresenta a visão geral do ambiente principal e mostra as atividades em execução, pendentes e recentes do ambiente selecionado.
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- A [guia Backups](../storage/snapshots.md#create-a-manual-backup) fornece uma lista de backups armazenados, um histórico de ações de backup e o botão Backup.

### Acessar loja

Cada ambiente ativo tem uma vitrine. Selecione um ambiente na navegação superior e clique no URL na visão geral do ambiente. Além disso, há uma lista **[!UICONTROL URLs]** no lado direito acima da lista Atividade.

O URL de acesso à Web pode incluir o seguinte:

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **Identificação exclusiva** = 7 caracteres alfanuméricos aleatórios
- **ID do projeto** = ID do projeto com 13 caracteres
- **Região** = nome da região do AWS ou Azure, consulte [Endereços IP regionais](regional-ip-addresses.md)

Os ambientes de Produção e Preparo Pro incluem três nós que você pode acessar usando os seguintes links:

- URLs do balanceador de carga:

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- Acesso direto a um dos três servidores redundantes:

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  O URL de produção é usado pela rede de entrega de conteúdo (CDN).

## Configurações

Abra o painel _Configurações_ clicando no ícone ![configurar projeto](../../assets/icon-configure.png){width="36"} (configurar) no lado direito da navegação do projeto.

### Configurações do projeto

**[!UICONTROL Project Settings]** expande um menu de controles no nível do projeto para gerenciar usuários, variáveis e muito mais:

| Opção | Descrição |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| Geral | Gerencie o fuso horário para uso com a programação de backups ou manutenção. |
| Access | Gerencie o [acesso de usuário](user-access.md) aos tipos de projeto e ambiente. |
| Certificados | Exibir uma lista de certificados SSL associados ao projeto. |
| Implantar chave | Adicione e exiba a chave pública ao repositório de código do projeto. |
| Domínios | Adicione um nome de domínio ao projeto. Consulte [Gerenciar domínios](../cdn/fastly-custom-cache-configuration.md#manage-domains). |
| Integrações | Adicione e gerencie [integrações](../integrations/overview.md), como notificações de integridade e webhooks. |
| Variáveis | Adicione [variáveis de nível de projeto](../environment/variable-levels.md) que estão disponíveis na compilação e no tempo de execução em todos os ambientes. |

{style="table-layout:auto"}

### Configurações do ambiente

Clique em **[!UICONTROL Environments]** e selecione um ambiente específico na lista para que os controles gerenciem configurações de site, variáveis de ambiente e muito mais:

| Opção | Descrição |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Geral | Configure o nome de exibição, o tipo de ambiente e o ambiente pai.<br>Alternar configurações de ambiente diferentes: |
|           | **Habilitar emails de saída**: enviar [emails de saída](outgoing-emails.md) do ambiente usando o protocolo SMTP. |
|           | **Ocultar dos mecanismos de pesquisa**: bloquear indexadores e rastreadores de mecanismos de pesquisa do site. |
|           | **Controle de acesso HTTP**: habilite a configuração de segurança para [!DNL Cloud Console] usando um controle de acesso de logon e endereço IP. |
|           | O status é `active` ou `inactive`. A maior parte do seu trabalho é em um ambiente ativo. Você pode desativar ou excluir o ambiente. |
| Variáveis | Exibir, criar e gerenciar [variáveis de nível de ambiente](../environment/variable-levels.md) disponíveis em tempo de execução. |
| Domínios | Exiba uma lista de [rotas configuradas](../routes/routes-yaml.md). |

{style="table-layout:auto"}

>[!WARNING]
>
>**NÃO** use o método de controle de acesso HTTP para proteger ambientes de Preparo e Produção Pro. Isso interrompe o armazenamento em cache do Fastly. Em vez disso, use o recurso [Bloqueio](../cdn/fastly-vcl-blocking.md) disponível no Fastly CDN para Adobe Commerce para bloquear o acesso ou implementar o controle de acesso usando o [Fastly Basic Auth](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md).

## Credenciais do Fastly e do New Relic

Seu projeto inclui o [Fastly](../cdn/fastly.md) e o [New Relic](../monitor/new-relic-service.md). Os detalhes do projeto exibem informações para o plano do projeto e licenças e tokens importantes para essas integrações. Somente o Proprietário da licença tem acesso inicial às credenciais e aos serviços. Forneça essas credenciais para os recursos técnicos e do desenvolvedor, conforme necessário.

- O [Fastly](https://www.fastly.com/) fornece entrega de conteúdo (CDN), otimização de imagem e serviços de segurança (DDoS e WAF) para o Adobe Commerce em projetos de infraestrutura na nuvem. Consulte [Obter credenciais do Fastly](../cdn/fastly-configuration.md#get-fastly-credentials).

- O [New Relic](../monitor/new-relic-service.md) fornece informações sobre métricas e desempenho do aplicativo para ambientes de Preparo e Produção.

Use a [CLI da Nuvem](../dev-tools/cloud-cli-overview.md) para examinar seus tokens de integração, IDs e muito mais:

```bash
magento-cloud subscription:info services
```
