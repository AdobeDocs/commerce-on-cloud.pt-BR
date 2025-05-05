---
title: CLI da nuvem
description: Saiba mais sobre a CLI da magento-cloud e como ela ajuda você a gerenciar ambientes de desenvolvimento locais para seu projeto de infraestrutura do Adobe Commerce na nuvem.
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---


# CLI da nuvem

A ferramenta da CLI do `magento-cloud` permite que desenvolvedores e administradores de sistema gerenciem projetos e ambientes na nuvem, executem rotinas e executem tarefas de automação localmente. A CLI do `magento-cloud` estende os recursos e a funcionalidade do [[!DNL Cloud Console]](../../get-started/cloud-console.md). Depois de instalar a CLI do `magento-cloud` na estação de trabalho local, você poderá usá-la para gerenciar os ambientes de integração do Adobe Commerce na infraestrutura em nuvem Starter e Pro.

>[!NOTE]
>
>Essa é uma ferramenta local e não pode ser instalada no ambiente de nuvem (que é somente leitura) usando esse método. Você só pode instalar módulos no ambiente de Nuvem por meio do **fluxo de trabalho de implantação**
>- [Fluxo de trabalho de implantação profissional](https://experienceleague.adobe.com/pt-br/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [Fluxo de trabalho de implantação inicial](https://experienceleague.adobe.com/pt-br/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**Para instalar a `magento-cloud` CLI**:

1. Na sua _estação de trabalho local_, altere para o diretório onde você pretende clonar o projeto na Nuvem e onde o [proprietário do sistema de arquivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=pt-BR) tem acesso de _gravação_.

1. Instale a CLI do `magento-cloud`.

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. Adicione a CLI do `magento-cloud` ao perfil bash.

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. Recarregue o perfil bash atualizado.

   ```bash
   . ~/.bash_profile
   ```

1. Para iniciar a CLI, chame `magento-cloud` e insira suas credenciais de conta da Nuvem quando solicitado.

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. Verifique se o comando `magento-cloud` está no caminho. O exemplo a seguir lista os comandos disponíveis.

   ```bash
   magento-cloud list
   ```

## Comandos comuns

O Adobe projetou esses comandos para gerenciar ambientes de integração na nuvem e recomenda que você execute a CLI do `magento-cloud` de um diretório de projeto para poder omitir o parâmetro `-p <project-ID>`.

A lista de comandos da CLI `magento-cloud` comumente usados inclui apenas as opções necessárias. Você pode usar a opção `--help` com qualquer comando para ver mais informações.

| Comando | Descrição |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | Faça logon no projeto. |
| `magento-cloud list` | Liste os comandos disponíveis para a ferramenta CLI. |
| `magento-cloud environment:list` | Listar os ambientes no projeto atual. |
| `magento-cloud environment:checkout` | Confira um ambiente existente. |
| `magento-cloud environment:merge -e` | Mesclar alterações neste ambiente com seu pai. |
| `magento-cloud variables` | Variáveis de lista neste ambiente. |
| `magento-cloud ssh` | Use o SSH para se conectar ao ambiente remoto. |
| `magento-cloud url` | Abra a loja do Adobe Commerce em um navegador. |
| `magento-cloud web` | Abra o [!DNL Cloud Console]. |

## Comandos de ambiente

O ambiente _name_ é diferente do ambiente _ID_ somente se você usar espaços ou letras maiúsculas no nome do ambiente. Uma ID de ambiente consiste em todas as letras minúsculas, números e símbolos permitidos. Letras maiúsculas em um nome de ambiente são convertidas em minúsculas na ID; espaços em um nome de ambiente são convertidos em traços.

Um nome de ambiente _não pode_ incluir caracteres reservados para o shell do Linux ou para expressões regulares. Os caracteres proibidos incluem chaves (`{ }`), parênteses, asterisco (`*`), colchetes (`< >`), E comercial (`&`), porcentagem (`%`) e outros caracteres.

O comando `magento-cloud environment:list` exibe hierarquias de ambiente, enquanto `git branch` não exibe. Se você tiver ambientes aninhados, use o seguinte:

```bash
magento-cloud environment:list
```

### Reimplantação do ambiente

Acione uma reimplantação sem usar um push. Verifique e confirme o ambiente para reimplantação. Não use a reimplantação se houver uma build em um estado pendente.

```bash
magento-cloud environment:redeploy
```

Exemplo de resposta:

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Comandos do Git

Você pode notar que alguns desses comandos são semelhantes aos comandos do Git. Os comandos `magento-cloud` se conectam diretamente ao projeto da Nuvem baseada em Git com recursos adicionais. Se você criar uma ramificação sem usar a CLI do `magento-cloud`, ela não será &quot;ativada&quot; e não será criada automaticamente quando você enviar as alterações para o ambiente remoto. O comando da CLI `magento-cloud` inclui ativação.

Para criar uma ramificação, use o comando `magento-cloud` para que a ramificação seja ativada.

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

Para status da filial:

- Use o comando `magento-cloud env` para exibir uma lista das ramificações de ambiente e seus status: ativo ou inativo.
- Use o comando `magento-cloud environment:activate` para ativar uma ramificação de ambiente.

Envie uma confirmação do Git vazia para acionar uma implantação. Por exemplo:

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

Algumas ações, como adicionar um usuário, não resultam na implantação.

### Criar uma ramificação de ambiente

As etapas a seguir demonstram o uso dos comandos CLI e Git alternadamente para gerenciar o ambiente local:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Alternar para o [proprietário do sistema de arquivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=pt-BR).

1. Faça logon no projeto.

   ```bash
   magento-cloud login
   ```

1. Liste seus projetos.

   ```bash
   magento-cloud project:list
   ```

1. Listar ambientes no projeto. Todos os ambientes incluem uma ramificação Git ativa que contém seu código, banco de dados, variáveis de ambiente, configurações e serviços.

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >É importante usar o comando `magento-cloud environment:list` porque ele exibe hierarquias de ambiente, enquanto o comando `git branch` não.

1. Busque ramificações de origem para obter o código mais recente.

   ```bash
   git fetch origin
   ```

1. Fazer check-out ou alternar para uma ramificação e um ambiente específicos.

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Os comandos do Git só verificam a ramificação Git. O comando `magento-cloud checkout` verifica a ramificação e alterna para o ambiente ativo.

   >[!TIP]
   >
   >Você pode criar uma ramificação de ambiente usando a sintaxe de comando `magento-cloud environment:branch <environment-name> <parent-environment-ID>`. Pode levar algum tempo adicional para criar e ativar uma ramificação de ambiente.

1. Use a ID de ambiente para enviar qualquer código atualizado para o local. Isso não será necessário se a ramificação do ambiente for nova.

   ```bash
   git pull origin <environment-ID>
   ```

1. (_Opcional_) Crie um [instantâneo](../storage/snapshots.md) do ambiente como backup.

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## Atualizar a CLI

A CLI do `magento-cloud` verifica as atualizações disponíveis quando você faz logon, mas é possível verificar atualizações usando o comando `self:update`. Se houver uma atualização disponível, siga as instruções para atualizar a CLI.

Se sua CLI do `magento-cloud` estiver atualizada, você verá a seguinte resposta:

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
