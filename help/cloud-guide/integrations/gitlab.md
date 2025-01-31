---
title: Integração com GitLab
description: Saiba como integrar seu projeto Adobe Commerce na infraestrutura em nuvem com o GitLab.
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Integração com GitLab

Você pode configurar um repositório GitLab para criar e implantar automaticamente um ambiente quando enviar alterações de código. Essa integração sincroniza seu repositório GitLab com sua conta do Adobe Commerce na infraestrutura em nuvem.

{{private-repository}}

Essa integração permite:

- Criar um ambiente ao criar uma ramificação
- Reimplante o ambiente ao mesclar uma solicitação de pull
- Excluir o ambiente ao excluir a ramificação

Você deve obter um token do GitLab e um webhook para continuar o processo.

## Pré-requisitos

- Acesso de administrador ao projeto de infraestrutura em nuvem do Adobe Commerce
- Ferramenta [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) em seu ambiente local
- Uma conta do GitLab
- Um token de acesso pessoal do GitLab com acesso de gravação ao repositório do GitLab, os escopos selecionados devem ser pelo menos: `api` e `read_repository`.

## Preparar seu repositório

Clonar seu projeto Adobe Commerce na infraestrutura em nuvem de um ambiente existente e migrar as ramificações do projeto para um novo repositório GitLab vazio, preservando os mesmos nomes de ramificação. É **crítico** manter uma árvore Git idêntica, para que você não perca ambientes ou ramificações existentes no seu projeto do Adobe Commerce na infraestrutura em nuvem.

1. No terminal, faça logon no projeto Adobe Commerce na infraestrutura da nuvem.

   ```bash
   magento-cloud login
   ```

1. Liste os projetos e copie a ID do projeto.

   ```bash
   magento-cloud project:list
   ```

1. Clona o projeto em seu ambiente local.

   ```bash
   magento-cloud project:get <project-id>
   ```

1. Adicione seu repositório GitLab como remoto (supondo que o GitLab seja usado em sua versão SaaS).

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   O nome padrão da conexão remota pode ser `origin` ou `magento`. Se `origin` existir, você pode escolher um nome diferente ou pode renomear ou excluir a referência existente. Consulte a [documentação git-remote](https://git-scm.com/docs/git-remote).

1. Verifique se você adicionou o remoto do GitLab corretamente.

   ```bash
   git remote -v
   ```

   Resposta esperada:

   ```
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. Envie os arquivos do projeto para o novo repositório do GitLab. Lembre-se de manter todos os nomes de ramificação iguais.

   ```bash
   git push -u origin master
   ```

   Se você estiver começando com um novo repositório GitLab, talvez precise usar a opção `-f`, pois o repositório remoto não corresponde à sua cópia local.

1. Verifique se o repositório do GitLab contém todos os arquivos de projeto.

## Habilitar a integração do GitLab

Use o comando `magento-cloud integration` para habilitar a integração do GitLab e obter o URL de carga para o webhook do GitLab para enviar atualizações do GitLab para o projeto de infraestrutura do Adobe Commerce na nuvem.

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| Opção | Descrição |
| ------ | ----------- |
| `<project-ID>` | Sua ID de projeto do Adobe Commerce na infraestrutura em nuvem |
| `<your-GitLab-token>` | O token de acesso pessoal gerado para o GitLab |
| `--base-url` | URL do GitLab (`https://gitlab.com/` se o GitLab for usado em sua versão SaaS) |
| `--server-project` | Nome do projeto no GitLab (parte após o URL base) |
| `--build-merge-requests` | Um parâmetro _opcional_ que instrui o Adobe Commerce na infraestrutura em nuvem a criar um novo ambiente para cada solicitação de mesclagem (`true` por padrão) |
| `--merge-requests-clone-parent-data` | Um parâmetro _opcional_ que instrui o Adobe Commerce na infraestrutura em nuvem a clonar os dados do ambiente pai para solicitações de mesclagem (`true` por padrão) |
| `--fetch-branches` | Um parâmetro _opcional_ que faz com que o Adobe Commerce na infraestrutura de nuvem busque todas as ramificações do remoto (como ambientes inativos) (`true` por padrão) |
| `--prune-branches` | Um parâmetro _opcional_ que instrui o Adobe Commerce na infraestrutura em nuvem a excluir ramificações que não existem no local remoto (`true` por padrão) |

>[!WARNING]
>
>O comando `magento-cloud integration` substitui o código _all_ no projeto de infraestrutura do Adobe Commerce na nuvem pelo código do repositório do GitLab. Isso inclui todas as ramificações, incluindo a ramificação `production`. Essa ação acontece instantaneamente e não pode ser desfeita. Como prática recomendada, é importante clonar todas as ramificações do projeto Adobe Commerce na infraestrutura em nuvem e enviá-las para o repositório do GitLab antes de adicionar a integração do GitLab.

**Para habilitar a integração do GitLab**:

1. No terminal, adicione a integração do GitLab ao projeto de infraestrutura Adobe Commerce na nuvem:

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. Quando solicitado, insira `y` para adicionar a integração.

   ```
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. Copie a **URL do gancho** exibida pela saída de retorno.

   ```
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### Adicionar o webhook no GitLab

Para comunicar eventos, como solicitações de push ou mesclagem, ao servidor Git da Nuvem, você deve [criar um webhook](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) para o repositório GitLab

1. No repositório do GitLab, clique na guia **Configurações**.

1. Na barra de navegação esquerda, clique em **Webhooks**.

1. No formulário _Webhooks_, edite os seguintes campos:

   - **URL**: insira o `Hook URL` retornado quando você habilitou a integração com o GitLab.
   - **Token de Segredo**: insira um segredo de verificação, se necessário.
   - **Acionador**: verifique `Merge request events` e/ou `Push events` conforme suas necessidades.
   - **Habilitar verificação SSL**: você deve selecionar esta opção.

1. Clique em **Adicionar webhook**.

### Testar a integração

Depois de configurar a integração do GitLab, você pode verificar se a integração está operacional usando a CLI do `magento-cloud`:

```bash
magento-cloud integration:validate
```

Ou você pode testá-lo enviando uma simples alteração ao seu repositório GitLab.

1. Criar um arquivo de teste.

   ```bash
   touch test.md
   ```

1. Confirme e envie a alteração para seu repositório GitLab.

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. Faça logon no [[!DNL Cloud Console]](../project/overview.md) e verifique se a mensagem de confirmação é exibida e se o projeto está sendo implantado.

## Criar uma ramificação de nuvem

Use o comando `environment:push` da CLI do `magento-cloud` para criar e ativar um novo ambiente. Consulte [Criar uma ramificação de nuvem](bitbucket.md#create-a-cloud-branch).

## Remover a integração

Use o comando `integration:delete` da CLI do `magento-cloud` para remover a integração. Consulte [Remover a integração](bitbucket.md#remove-the-integration).
