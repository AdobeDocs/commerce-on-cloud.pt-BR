---
title: Gerenciar ramificações com a CLI
description: Saiba como gerenciar as ramificações de ambiente para o Adobe Commerce na infraestrutura em nuvem usando a CLI da nuvem.
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Gerenciar ramificações com a CLI

Para instalar a CLI do `magento-cloud`, consulte a [Referência da CLI da Nuvem](../dev-tools/cloud-cli-overview.md). Depois de instalar a CLI do `magento-cloud` e configurar chaves SSH para acesso remoto à sua infraestrutura de nuvem, você pode usar comandos da CLI do `magento-cloud` para gerenciar os ambientes dos seus projetos. Para obter informações sobre a arquitetura de ambiente, consulte [Arquitetura de início](../architecture/starter-architecture.md) ou [Arquitetura Pro](../architecture/pro-architecture.md).

Para gerenciar ramificações e ambientes com o [!DNL Cloud Console], consulte [Gerenciar ramificações com o [!DNL Cloud Console]](../project/console-branches.md).

## Usar comandos da CLI

Os comandos da CLI `magento-cloud` são semelhantes aos comandos do Git. Você pode usá-los para se conectar ao seu projeto e gerenciar seus ambientes. Embora seja possível executar os comandos a partir de qualquer diretório, é recomendável executá-los a partir de um diretório do projeto. Quando executado de um diretório de projeto, você pode omitir o parâmetro `-p <project-ID>`. Consulte a [Referência da CLI da nuvem](../dev-tools/cloud-cli-overview.md).

## Clonar o projeto

As instruções a seguir usam uma combinação de comandos da CLI do `magento-cloud` e comandos do Git para clonar seu projeto em sua estação de trabalho local. Para ver uma lista completa de comandos CLI do `magento-cloud`, use o comando `magento-cloud list`.

>[!IMPORTANT]
>
>Alguns comandos do Git não podem concluir uma ação no projeto Adobe Commerce na infraestrutura em nuvem. Por exemplo, você pode criar uma ramificação usando um comando Git, mas não pode criar e ativar um novo ambiente. Você deve criar um ambiente usando o comando `magento-cloud environment:branch <branch-name>` para que o ambiente se torne _ativo_. Como alternativa, você pode usar o [!DNL Cloud Console] para criar ambientes ativos. Consulte [Referência de CLI da nuvem](../dev-tools/cloud-cli-overview.md#git-commands).

**Para clonar um ambiente `master` do projeto**:

1. Faça logon na estação de trabalho local com uma conta do [proprietário do sistema de arquivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=pt-BR).

1. Altere para o diretório do servidor Web ou host virtual _docroot_.

1. Faça logon usando a CLI do `magento-cloud`.

   ```bash
   magento-cloud login
   ```

1. Liste seus projetos.

   ```bash
   magento-cloud project:list
   ```

1. Clonar um projeto.

   ```bash
   magento-cloud project:get <project-ID>
   ```

   Quando solicitado, forneça um nome de diretório.

1. Mude para o diretório `magento2`.

1. Listar ambientes disponíveis para o projeto.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >O comando `magento-cloud environment:list` exibe hierarquias de ambiente, enquanto o comando `git branch` não.

1. Busque as ramificações remotas.

   ```bash
   git fetch origin
   ```

1. Obter código atualizado.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Consulte [Integrações](../integrations/overview.md) para obter informações sobre como usar serviços de hospedagem baseados em Git com o Adobe Commerce na infraestrutura em nuvem.

## Criar uma ramificação para desenvolvimento

Depois de clonar o projeto e atualizar a configuração da conta de administrador do Adobe Commerce, você pode ramificar para desenvolvimento. Como dito anteriormente, você deve criar um ambiente usando o comando `magento-cloud environment:branch <branch-name>` ou o [!DNL Cloud Console] para que o ambiente se torne _ativo_.

- Para [Início](../architecture/starter-develop-deploy-workflow.md#clone-and-branch), considere criar uma ramificação para `staging` e, em seguida, crie uma ramificação de desenvolvimento com base na ramificação `staging`.
- Para [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow), crie ramificações de desenvolvimento com base na ramificação `Integration`.

**Para criar uma ramificação de desenvolvimento**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Crie um ambiente com base na ramificação recomendada para o fluxo de trabalho do projeto.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. Atualizar dependências.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_opcional_] Crie um [backup](../storage/snapshots.md) do ambiente.

### Mesclar uma ramificação

Após concluir o desenvolvimento, mescle esta ramificação com a principal:

1. Confirmar e enviar alterações de código:

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Mesclar com o ambiente pai:

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### Excluir um ambiente

Exclua um ambiente somente se tiver certeza de que ele não é mais necessário. Não é possível recuperar um ambiente depois de excluí-lo.

>[!WARNING]
>
>Você não pode excluir a ramificação `master` de nenhum projeto.

Você precisa ser um administrador de projeto, um administrador de ambiente ou um Proprietário da conta para executar esta tarefa. Consulte [Gerenciar acesso de usuário a projetos na nuvem](../project/user-access.md).

Ao excluir um ambiente, ele é definido como _inativo_. O código ainda está disponível na ramificação Git, mas não contém mais os serviços ou o banco de dados. Para excluir o ambiente completamente, você também deve excluir a ramificação Git remota correspondente.

**Para excluir um ambiente**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Buscar atualizações do servidor remoto.

   ```bash
   git fetch
   ```

1. Exclua a ramificação do ambiente.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   Como opção, é possível excluir mais de um ambiente de cada vez, adicionando várias IDs de ambiente ao comando de exclusão.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. Responda às solicitações para excluir o ambiente local e o ambiente remoto correspondente.

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   A exclusão do ambiente o coloca em um estado _inativo_.

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   Excluir a ramificação Git remota remove o ambiente do projeto.

1. Aguarde a exclusão do ambiente.

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>Para ativar um ambiente inativo, use o comando `magento-cloud environment:activate`.

## Interagir com ambientes remotos

Depois de [configurar chaves SSH](../development/secure-connections.md), você pode [se conectar do espaço de trabalho local a um ambiente remoto](../development/secure-connections.md#connect-to-a-remote-environment) e interagir com os serviços do projeto e modificar as configurações.
