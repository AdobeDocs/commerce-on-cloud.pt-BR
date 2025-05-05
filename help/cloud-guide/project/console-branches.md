---
title: Gerenciar ramificações com o  [!DNL Cloud Console]
description: Saiba como gerenciar as ramificações de ambiente para o Adobe Commerce na infraestrutura em nuvem usando o  [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Gerenciar ramificações com o [!DNL Cloud Console]

Você pode gerenciar seus ambientes usando a CLI do [!DNL Cloud Console] ou do `magento-cloud`. Os arquivos do projeto são armazenados em um repositório Git. Você pode usar comandos do Git para gerenciar seu código, mas a CLI do `magento-cloud` foi projetada para interagir com recursos da plataforma, enquanto os comandos do Git não. Consulte [Comandos do Git](../dev-tools/cloud-cli-overview.md#git-commands) no tópico da CLI da nuvem.

Este tópico discute como usar o [!DNL Cloud Console] para:

- Adicionar ou excluir um ambiente
- Sincronizar (`git pull`) a partir do ambiente pai
- Mesclar (`git push`) com o ambiente pai

>[!TIP]
>
>Não é possível criar ramificações de ambientes de preparo e produção profissionais. Você pode ramificar da ramificação `master`.

## Criar um ambiente

A estratégia de ramificação usa um fluxo de trabalho Git comum, no qual você desenvolve o código e adiciona extensões em uma ramificação de desenvolvimento. Consulte as visões gerais de arquitetura do [Starter](../architecture/starter-architecture.md) e do [Pro](../architecture/starter-develop-deploy-workflow.md).

- Para Iniciante, crie uma ramificação `staging` da ramificação `master` e, em seguida, ramifique de `staging` para desenvolvimento.
- Para Pro, crie uma ramificação de desenvolvimento do ambiente `Integration`.

Sua conta oferece suporte a um número limitado de ![ramificações de desenvolvimento ativas](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (inativas). Gerencie ramificações ativas e inativas adicionando ou excluindo uma ramificação usando somente o [!DNL Cloud Console] ou a CLI da nuvem. Antes de excluir uma ramificação, desative-a, que permanece na lista _Ambientes_ como _inativa_. Você pode reativar a ramificação mais tarde ou [excluir a ramificação](../dev-tools/cloud-cli-overview.md#) nas configurações do ambiente ou usando a CLI da nuvem.

Se você precisar de ambientes ativos adicionais para desenvolvimento, envie um [Tíquete de suporte](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**Para adicionar uma ramificação**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Selecione um ambiente.

   >[!TIP]
   >
   >Sua nova ramificação será clonada deste ambiente. Escolha um ambiente pai semelhante ao ambiente que você está prestes a criar.

1. Clique em **[!UICONTROL Branch]**.

   ![Criar uma ramificação](../../assets/button-branch.png){width="150"}

1. No formulário _Ramificação de ..._, digite um nome de ramificação.

   O ambiente _name_ é diferente do ambiente _ID_ somente se você usar espaços ou letras maiúsculas no nome do ambiente. Uma ID de ambiente consiste em todas as letras minúsculas, números e símbolos permitidos. Letras maiúsculas em um nome de ambiente são convertidas em minúsculas na ID; espaços em um nome de ambiente são convertidos em traços.

   Um nome de ambiente **não pode** incluir caracteres reservados para o shell do Linux ou para expressões regulares. Os caracteres proibidos incluem chaves (`{ }`), parênteses, asterisco (`*`), colchetes (`>`), E comercial (`&`), porcentagem (<code>%</code>) e outros caracteres.

1. Selecione um **[!UICONTROL Environment type]**.

1. Clique em **[!UICONTROL Create Branch]**.

1. Espere enquanto o ambiente é implantado.

   Durante a implantação, o status do ambiente é **Em andamento**. Após uma implantação bem-sucedida, o status muda para uma marca de seleção verde para **sucesso**.

## Criar ramificação inativa

Não é possível criar uma ramificação inativa no console ou na CLI do Adobe Commerce Cloud. Se quiser criar uma ramificação inativa, crie-a no repositório Git e envie por push usando a opção `environment.Parent` no comando.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Excluir um ambiente

Antes de excluir um ambiente, você deve desativá-lo. Depois que um ambiente estiver inativo, você poderá excluí-lo.

**Para desativar um ambiente**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Selecione o ambiente na lista da barra de navegação _Ambiente_.

1. Clique no ícone de configuração no lado direito da barra de navegação superior, que abre as configurações do ambiente.

1. Na guia _[!UICONTROL General]_, role para baixo até a seção&#x200B;_[!UICONTROL Deactivate environment]_, clique em **[!UICONTROL Deactivate environment and delete data]** e siga as instruções.

## Sincronizar um ambiente

A sincronização de um ambiente (ou ramificação) é igual a `git pull origin <parent>`. Você pode sincronizar o código atualizado de um ambiente primário. Você pode usar esse recurso por meio do [!DNL Cloud Console] para todos os ambientes Starter e Pro.

Para o plano Pro, você pode sincronizar do ambiente de preparo e produção para a ramificação `master`. Essa sincronização extrai e envia apenas código, não dados. Para sincronizar dados, descarte os dados do banco de dados e envie-os para o banco de dados de outro ambiente. Consulte [Migrar e implantar arquivos e dados estáticos](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Para sincronizar um ambiente**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Na lista de ambientes, clique no nome da ramificação a ser sincronizada.

1. Clique em (sincronizar).

   ![Sincronizar um ambiente](../../assets/button-sync.png){width="150"}

1. Selecione os itens a serem sincronizados.

   - Substituir os dados — (dados e arquivos) sincroniza alterações no banco de dados e arquivos de conteúdo da ramificação pai.
   - Mesclar — (código) sincroniza o código atualizado da ramificação principal.

   Isso também cria um comando CLI para você copiar e usar.

1. Clique em **Sincronizar**.

## Mesclar com ambiente pai

Mesclar um ambiente (ou ramificação) é o mesmo que `git push origin`. Você mescla para enviar o código atualizado de um ambiente para o ambiente pai. Você pode mesclar este código a `master`. Você pode implantar em Preparo e Produção usando o comando `merge`.

**Para mesclar com o ambiente pai**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Na lista de ambientes, clique no nome da ramificação a ser mesclada.

1. Clique em (mesclar).

   ![Mesclar um ambiente](../../assets/button-merge.png){width="150"}

1. Clique em **Mesclar** e confirme a ação.

## Exibir logs

Por meio do [!DNL Cloud Console], você pode revisar vários logs de ambientes, incluindo histórico de compilação, implantação e implantação.

Para **Início**, você pode revisar os logs de compilação e implantação e o histórico de implantação. Esses ambientes incluem a ramificação `master` (Produção) e todas as ramificações criadas a partir dela.

Para **Pro**, você pode examinar os seguintes logs em cada ambiente:

- Integração — criação, implantação e histórico de implantação
- Preparação — crie logs e histórico de implantação. Use SSH para fazer logon no servidor e exibir logs de implantação.
- Produção — crie logs e histórico de implantação. Use SSH para fazer logon no servidor e exibir logs de implantação.

**Para exibir logs em[!DNL Cloud Console]**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Selecione um ambiente.

   A exibição de ambiente fornece uma [Lista de atividades](activity-stream.md) que mostra eventos _recentes_, com uma tentativa de entrada por ação incluindo sincronizações, mesclagens, ramificações, backups e muito mais. Clique em **Tudo** para obter o histórico completo de implantação.

1. Para exibir o log de compilação, selecione o link Sucesso ou Falha por registro de implantação na conta.

>[!TIP]
>
>Clique no ícone **Filtrar por** para obter uma lista suspensa e selecione o tipo de mensagens a serem exibidas.

## Obter código de um repositório Git privado

Seu projeto de infraestrutura do Adobe Commerce na nuvem pode incluir código de um repositório Git privado. Por exemplo, você pode ter código para um módulo personalizado ou tema em um repositório privado. Para fazer isso, você deve adicionar a chave SSH pública do seu projeto ao seu repositório Git privado e atualizar o arquivo do projeto `composer.json`.

Para adicionar uma chave de implantação ao seu repositório GitHub privado, você deve ser o administrador desse repositório. O GitHub permite usar uma chave de implantação somente para um repositório.

Se preferir que o projeto acesse vários repositórios, é possível anexar uma chave SSH a uma conta de usuário automatizada. Como esta conta não é usada por um humano, ela é chamada de [usuário da máquina](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Adicione a conta da máquina como colaborador ou adicione o usuário da máquina a uma equipe com acesso aos repositórios.

>[!INFO]
>
>A Adobe recomenda adicionar e mesclar esse código aos repositórios Git do projeto. Se você não configurar a conexão, poderá ter problemas de build.

**Para encontrar sua chave pública SSH**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Clique no ícone de configuração no lado direito da barra de navegação superior.

1. Em _Configurações do projeto_, clique em **[!UICONTROL Deploy Key]**.

1. Copie a chave de implantação na área de transferência para uso em um dos seguintes métodos baseados em Git:

>[!BEGINTABS]

>[!TAB GitHub]

### Insira sua chave de implantação do GitHub

No GitHub, as chaves de implantação são somente leitura por padrão.

**Para inserir sua chave pública de projeto como uma chave de implantação do GitHub**:

1. Faça logon no repositório GitHub como administrador.
1. Clique na guia repositório **[!UICONTROL Settings]**.

   >[!NOTE]
   >
   >Caso não veja essa opção, você não está conectado como administrador de repositório e não pode concluir essa tarefa. Peça ao administrador do repositório GitHub para fazer isso.

1. Na guia _Configurações_ da navegação à esquerda, clique em **[!UICONTROL Deploy Keys]**.
1. Clique em **[!UICONTROL Add deploy key]**.
1. Siga as instruções.

Em `composer.json`, use o formato `<user>@<host>:<.git</code>`, ou `ssh://<user>@<host>:<port>/<path>.git` se estiver usando uma porta não padrão.

>[!TAB Bitbucket]

### Insira sua chave de implantação do Bitbucket

**Para inserir sua chave pública do projeto como uma chave de implantação de Bitbucket**:

1. Faça logon no repositório Bitbucket como administrador.
1. Na navegação à esquerda, clique em **[!UICONTROL Settings]**.
1. Clique em Geral > **[!UICONTROL Deployment Keys]**.
1. Clique em **[!UICONTROL Add Key]**.
1. Siga as instruções.

>[!TAB GitLab]

### Insira sua chave de implantação do GitLab

**Para adicionar a chave SSH pública para o seu projeto como uma chave de implantação do GitLab**:

1. Faça logon no repositório do GitLab como proprietário.
1. Verifique se a opção _Pipelines_ está habilitada para o seu projeto:

   1. Nas configurações do projeto, expanda a seção **[!UICONTROL Visibility, project, features, permissions]**.
   1. Se necessário, clique em **[!UICONTROL Pipelines]** para habilitar a opção.

1. Adicione sua chave SSH pública às configurações de CI/CD.

   1. Na navegação à esquerda, clique em Configurações > **[!UICONTROL CI / CD]**.
   1. Clique em Implantar Chaves **Expandir** para configurar a chave.
   1. No formulário _Implantar Chave_, adicione um nome de chave de implantação ao campo **[!UICONTROL Title]** e cole sua chave SSH pública no campo **[!UICONTROL Key]**.
   1. Clique em **[!UICONTROL Add Key]** para salvar a configuração.

>[!ENDTABS]

## Ambientes e ramificações seguros

Você pode acessar seus projetos e ambientes de qualquer local usando um navegador da Web com o [!DNL Cloud Console]. Você pode ter o conjunto de segurança para seu ambiente de produção, lojas e sites. Esta seção ajuda a proteger os ambientes de Integração e Preparo exclusivamente para seus desenvolvedores, DBAs e muito mais.

>[!WARNING]
>
>**NÃO** use os métodos a seguir para proteger ambientes de Preparo e Produção Profissionais. Isso interrompe o armazenamento em cache do Fastly. Use o recurso [Bloqueio](../cdn/fastly-vcl-blocking.md) disponível no Fastly CDN para Adobe Commerce.

**Para proteger ambientes**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Selecione um ambiente e clique no ícone de configuração na barra de navegação.

1. Na guia de configurações do ambiente _Geral_, clique em **LIGADO** para **[!UICONTROL HTTP access control enabled]** habilitar o acesso seguro. Você pode escolher entre credenciais ou endereços IP para filtrar o acesso.

1. Para filtrar por credenciais, clique em **[!UICONTROL Add Login]**, digite um nome de usuário e senha e clique em **[!UICONTROL Add Login]** para adicionar.

1. Para filtrar por endereço IP, insira os endereços IP em uma lista com `deny` ou `allow`. Por exemplo:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Clique em **[!UICONTROL Save]**. Isso reimplanta o ambiente para atualizar a segurança e as configurações. A Adobe recomenda testar o ambiente após concluir as configurações de segurança.
