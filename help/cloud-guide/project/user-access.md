---
title: Gerenciar acesso do usuário
description: Saiba como gerenciar o acesso de usuários ao Adobe Commerce em projetos e ambientes de infraestrutura em nuvem.
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 953593de-f675-49fd-988f-f11306f67fbd
source-git-commit: c972d9f2029499cf53edc334c1d9a40b155a991d
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 0%

---

# Gerenciar acesso do usuário

Os projetos do Adobe Commerce em infraestrutura em nuvem usam acesso com base em funções. Há duas funções disponíveis no nível do projeto:

- **Administrador do projeto**—Grava acesso a todos os ambientes do projeto e pode gerenciar usuários, código de push e atualizar configurações do projeto. (Anteriormente conhecido como **Super admin**)
- **Visualizador do projeto** — Acesso somente para visualização a todos os ambientes do projeto.

Visualizadores de projeto não podem executar tarefas em nenhum ambiente; no entanto, você pode conceder aos visualizadores do projeto acesso de gravação a um tipo de ambiente específico.

O acesso no nível do ambiente é baseado no tipo de ambiente: produção, preparo e desenvolvimento. Conceder a um usuário _visualizador_ permissão para _ambientes de desenvolvimento_ significa que ele pode exibir **todos** ambientes de desenvolvimento no projeto. A tabela a seguir esclarece as habilidades concedidas a cada nível de permissão:

| Nível de permissão | Access | Acesso SSH |
| ------------------ | ----------- | :----------: |
| **Administrador** | Executar tarefas de administrador, como alterar configurações, enviar código, executar tarefas e gerenciamento de ramificações, incluindo mesclagem com o ambiente pai | Sim |
| **Colaborador** | Enviar código e ramificar o ambiente; não é possível alterar as configurações ou executar ações | Sim |
| **Visualizador** | Acesso somente para visualização ao tipo de ambiente | Não |
| **Sem acesso** | Sem acesso ao tipo de ambiente | Não |

{style="table-layout:auto"}

Você pode adicionar usuários e atribuir funções usando a CLI do `magento-cloud` ou o [!DNL Cloud Console].

>[!BEGINSHADEBOX]

**Pré-requisitos:**

- Um usuário registrado com uma Adobe ID. Um usuário deve [se registrar em uma conta do Adobe](https://account.adobe.com) e inicializar sua [conta da nuvem](https://console.adobecommerce.com) visitando [https://console.adobecommerce.com](https://console.adobecommerce.com) antes de adicioná-lo a um projeto da nuvem.
- Um usuário com a função de **Administrador** atribuída não pode gerenciar usuários com a CLI `magento-cloud`. Somente usuários com a função **Proprietário da conta** podem gerenciar usuários.

>[!ENDSHADEBOX]

## Gerenciar usuários com a CLI

Use a CLI do `magento-cloud` para gerenciar usuários e integrar a sistemas automatizados:

- `magento-cloud user:add`-adicionar um usuário ao projeto
- `magento-cloud user:delete`-excluir um usuário
- `magento-cloud user:list [users]` usuários da lista de projetos
- `magento-cloud user:role`-visualizar ou alterar a função de usuário
- `magento-cloud user:update` - atualizar função de usuário em um projeto

Os exemplos a seguir usam a CLI do `magento-cloud` para adicionar um usuário, configurar funções, modificar atribuições de projetos e atribuir funções de usuário.

**Para adicionar um usuário e atribuir funções**:

1. Use a CLI do `magento-cloud` para adicionar o usuário.

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >O usuário deve ter uma Adobe ID; consulte os [pré-requisitos](#add-users-and-manage-access).

1. Siga as instruções: especifique o endereço de email do usuário, defina as funções de projeto e de ambiente e adicione o usuário.

   > Exemplos de prompts

   ```
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   Depois de adicionar o usuário, o Adobe envia um email para o endereço especificado com instruções para acessar o projeto Adobe Commerce na infraestrutura em nuvem.

### Exibir a função de projeto de um usuário

```bash
magento-cloud user:get alice@example.com
```

>Exemplo de resposta:

```
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### Adicionar um usuário a vários ambientes

Para adicionar um usuário como `viewer` em um ambiente `Production` e como `contributor` em um ambiente `Integration`:

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### Atualizar permissões de ambiente do usuário

Para atualizar as permissões do ambiente do usuário para `admin` no ambiente `Production`:

```bash
magento-cloud user:update alice@example.com -r production:a
```

## Gerenciar usuários do [!DNL Cloud Console]

Você pode usar o [[!DNL Cloud Console]](../../get-started/cloud-console.md) para adicionar permissões e usar o recurso _Editar_ para modificar permissões para um usuário existente.

>[!IMPORTANT]
>
>O usuário deve ter uma Adobe ID; consulte os [pré-requisitos](#add-users-and-manage-access).

### Adicionar um usuário ao projeto

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com/).

1. Selecione um projeto na lista _Todos os projetos_.

1. No painel Projeto, clique no ícone de configuração no canto superior direito.

1. Em _Configurações do projeto_, clique em **[!UICONTROL Access]**.

1. No modo de exibição _Acesso_, clique em **[!UICONTROL Add]**.

1. Preencha o formulário _[!UICONTROL Add User]_:

   - Insira o endereço de email do usuário.

   - **[!UICONTROL Project admin]**—conceda direitos de Administrador a todos os tipos de configurações e ambientes.

   - **[!UICONTROL Environment types and permissions]** — conceder acesso e níveis de permissão específicos a determinados tipos de ambientes. _Sem acesso_, _Administrador_ (alterar configurações, executar ação, mesclar código), _Colaborador_ (código de push) ou _Visualizador_ (somente exibição).

   >[!TIP]
   >
   >Somente um **Administrador do projeto** pode gerenciar usuários em qualquer ambiente. Para conceder a um usuário acesso à guia **Acesso**, outro **Administrador de projeto** ou o **Proprietário da conta** deve atribuir a esse usuário a função de **Administrador de projeto**.

1. Clique em **[!UICONTROL Add User]**.

   >[!IMPORTANT]
   >
   >Adicionar um usuário não aciona uma implantação automaticamente.

1. Depois de adicionar usuários, implante novamente todos os ambientes para aplicar as alterações. Adicionar um usuário não aciona uma implantação automaticamente. A reimplantação é uma etapa importante para garantir que o usuário possa acessar um ambiente usando SSH ou executar tarefas de administrador.

Depois de adicionar o usuário, o Adobe envia um email para o endereço especificado com instruções para acessar o projeto Adobe Commerce na infraestrutura em nuvem.

## Requisitos de autenticação do usuário

Para maior segurança, a Adobe fornece imposição de autenticação multifator (MFA) no nível do projeto para exigir autenticação de dois fatores (TFA) para acesso SSH ao código-fonte e aos ambientes do projeto da infraestrutura em nuvem do Adobe Commerce. Consulte [Habilitar MFA para SSH](multi-factor-authentication.md).

Quando a imposição de MFA é habilitada em um projeto de infraestrutura do Adobe Commerce na nuvem, todos os usuários com acesso SSH a um ambiente nesse projeto devem habilitar o TFA em sua conta do Adobe Commerce na infraestrutura da nuvem. Para processos automatizados, você pode criar um usuário de máquina e um token de API para autenticar na linha de comando.

Depois de adicionar um usuário a um projeto na nuvem, peça para o usuário revisar as configurações de segurança da conta e adicionar as seguintes configurações de segurança, conforme necessário:

- **Habilitar TFA**—Atenda aos padrões de segurança e conformidade configurando a autenticação de dois fatores. Projetos configurados com [imposição de MFA](multi-factor-authentication.md) exigem o TFA em contas que usam SSH para acessar os projetos.

- **Habilitar chaves SSH** — Os usuários que exigem acesso ao Adobe Commerce em repositórios de código-fonte de infraestrutura em nuvem devem habilitar chaves SSH em suas contas. Consulte [Conexões seguras](../development/secure-connections.md).

- **Criar um token de API** — Os usuários devem gerar um token de API que seja usado para acesso SSH a um ambiente. Você precisa do token para habilitar fluxos de trabalho de autenticação para processos automatizados.

  Em projetos com imposição de MFA ativada, você deve usar o token da API para autenticar solicitações de acesso SSH de contas automatizadas. O token permite que processos automatizados ignorem workflows de autenticação que exigem o TFA.

### Ativar o TFA para contas na nuvem

O Adobe Commerce na infraestrutura em nuvem é compatível com o TFA usando qualquer um dos seguintes aplicativos:

- [Autenticador do Google (Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [Authy (Android/iPhone)](https://authy.com/features/)
- [FreeOTP (Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth Authenticator (Firefox OS, desktop, outros)](https://github.com/gbraad-apps/gauth)

As instruções para instalar o aplicativo autenticador e habilitar o TFA estão disponíveis na página _Configurações de conta_ em [!DNL Cloud Console].

**Para habilitar o TFA em sua conta de usuário**:

1. Faça logon em [sua conta](https://console.adobecommerce.com).

1. No menu de conta superior direito, clique em **[!UICONTROL My Profile]**.

1. Na guia _Segurança_, clique em **[!UICONTROL Set up application]**.

1. Se você não tiver um aplicativo autenticador aprovado no dispositivo móvel, use as instruções vinculadas para instalar um.

1. Adicione a conta Adobe Commerce na infraestrutura em nuvem ao aplicativo autenticador.

   - No dispositivo móvel, abra o aplicativo autenticador. Em seguida, adicione o código de configuração ao aplicativo.

   - Na página [!UICONTROL **[!UICONTROL TFA set up - Application]**], digite o código do TFA de seu dispositivo móvel no campo **[!UICONTROL Application verification code]**.

   - Clique em **[!UICONTROL Verify and save]**.

     Se o código for válido, a Adobe enviará uma notificação ao endereço de email da conta confirmando que a conta agora tem o TFA.

1. Opcional. Habilite as configurações do _Navegador confiável_ para armazenar em cache o código de autenticação no navegador por 30 dias.

   Essa configuração reduz o número de desafios de autenticação durante o logon no projeto.

1. Clique em **Salvar** ou **Ignorar**.

1. Salve os códigos de recuperação.

   - Na página _Configuração do TFA - Recuperação_ códigos, copie e salve os códigos de recuperação para poder fazer logon no projeto Adobe Commerce na infraestrutura em nuvem quando não for possível acessar o dispositivo móvel ou o aplicativo de autenticação.

   - Copie os códigos de recuperação para outro local ou anote-os caso perca o acesso ao dispositivo ou ao aplicativo de autenticação.

   - Clique em **Salvar** para salvar os códigos em sua conta para que você possa exibi-los e gerenciá-los nas configurações de segurança da conta.

     >[!WARNING]
     >
     >Se você perder o acesso a uma conta com TFA e não tiver a lista de códigos de recuperação, entre em contato com o administrador do projeto ou [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para redefinir o aplicativo TFA.

1. Após concluir a configuração do TFA, clique em **Salvar** para atualizar sua conta.

1. Autentique sua sessão atual com o TFA.

   - Faça logout da sua conta.
   - Faça logon com seu nome de usuário e senha.
   - Quando solicitado, insira o código TFA para a entrada `accounts.magento.cloud` do aplicativo autenticador em seu dispositivo móvel.

### Gerenciar códigos de configuração e recuperação do TFA

Você pode gerenciar a configuração de TFA para uma conta Adobe Commerce na infraestrutura em nuvem na seção _Segurança_ da página _Meu perfil_.

1. Faça logon em [sua conta](https://console.adobecommerce.com).

1. No menu de conta superior direito, clique em **[!UICONTROL My Profile]**.

1. Na página _Meu perfil_, clique na guia **[!UICONTROL Security]**.

1. Use os links disponíveis para atualizar as configurações do TFA para sua conta do Adobe Commerce na infraestrutura em nuvem:

   - Desativar TFA
   - Redefinir o aplicativo autenticador
   - Adicionar ou remover navegadores confiáveis
   - Exibir ou atualizar códigos de recuperação do TFA em sua conta

### Criar um token de API

Um token de API pode ser trocado por um token de acesso OAuth 2, que pode ser usado para autenticar solicitações.

Em projetos que têm a imposição de MFA habilitada, você deve ter um token de API para habilitar o acesso SSH para usuários de máquina e processos automatizados.

>[!IMPORTANT]
>
>Proteja valores de token de API para sua conta. Não exponha o valor em amostras de código, capturas de tela ou comunicações inseguras entre cliente e servidor. Além disso, não exponha o valor no código-fonte armazenado em repositórios públicos.

**Para criar um token de API**:

1. Faça logon em [sua conta](https://console.adobecommerce.com).

1. No menu de conta superior direito, clique em **[!UICONTROL My Profile]**.

1. Na página _Meu perfil_, clique na guia **[!UICONTROL API tokens]**.

1. Clique em **[!UICONTROL Create API token]** e digite um nome, por exemplo, especifique um nome que corresponda ao usuário da máquina ou ao processo automatizado que usa o token de API.

   ![Tokens de API](../../assets/api-token-name.png)

1. Clique em **[!UICONTROL Create API token]**.
