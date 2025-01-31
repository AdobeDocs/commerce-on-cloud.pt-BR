---
title: Estrutura de projeto
description: Saiba mais sobre a estrutura de arquivos e os modelos de projeto do Adobe Commerce na infraestrutura em nuvem.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Estrutura de projeto

Um projeto de infraestrutura do Adobe Commerce na nuvem inclui arquivos essenciais para credenciais e configuração de aplicativo. Esses arquivos estão disponíveis no como modelo de acordo com a versão do Adobe Commerce. Consulte os modelos de nuvem com base na versão do Adobe Commerce no [`magento/magento-cloud` repositório do GitHub](https://github.com/magento/magento-cloud).

A tabela a seguir descreve os arquivos incluídos em um projeto na nuvem:

| Arquivo | Descrição |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Arquivo de configuração que redireciona `www` para o domínio apex e o aplicativo `php` para servir HTTP. Consulte [Configurar rotas](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | Um arquivo de configuração que define uma instância do MySQL (MariaDB), Redis e OpenSearch ou Elasticsearch. Consulte [Configurar serviços](../services/services-yaml.md). |
| `/app` | A pasta `code` é usada para módulos personalizados. A pasta `design` é usada para [temas personalizados](../store/custom-theme.md). A pasta `etc` contém arquivos de configuração para o aplicativo. |
| `/m2-hotfixes` | Usado para patches personalizados. |
| `/update` | Uma pasta de serviço usada pelo módulo de suporte. |
| `.gitignore` | Especifique quais arquivos e diretórios ignorar. Consulte [`.gitignore` referência](#ignoring-files). |
| `.magento.app.yaml` | Um arquivo de configuração que define as propriedades para criar seu aplicativo. Consulte [Configurar aplicativo](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Arquivo de configuração das fases de criação, implantação e pós-implantação. O pacote `ece-tools` inclui uma amostra desse arquivo. Consulte [Configurar ambientes](../environment/configure-env-yaml.md). |
| `composer.json` | Busca o Adobe Commerce e os scripts de configuração para preparar seu aplicativo. Consulte [Pacotes necessários](../development/overview.md#required-packages). |
| `composer.lock` | Armazena dependências de versão para cada pacote. Consulte [Pacotes necessários](../development/overview.md#required-packages). |
| `magento-vars.php` | Usado para definir [vários armazenamentos](../store/multiple-sites.md) e sites usando variáveis. |

{style="table-layout:auto"}

>[!NOTE]
>
>Quando você envia suas alterações locais para o servidor remoto, o script de implantação usa os valores definidos pelos arquivos de configuração no diretório `.magento` e, em seguida, o script exclui o diretório e seu conteúdo. O ambiente de desenvolvimento local não é afetado.

## Diretório raiz do aplicativo

O local do diretório raiz do aplicativo depende do ambiente.

- **Integração do Starter e do Pro**: `/app`
- **Produção Inicial**: `/<project-ID>`
- **Estágios Profissionais**: `/<project-ID>_stg`
- **Produção Profissional**: `/<project-ID>`

### Diretórios graváveis

Os ambientes remotos de integração, preparo e produção são somente leitura. Os seguintes diretórios são *apenas* diretórios graváveis por motivos de segurança:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>Em ambientes de produção e de preparo, cada nó no cluster de três nós tem um diretório `/tmp` que não é compartilhado com os outros nós.

## Ignorar arquivos

Há um arquivo `.gitignore` básico com o repositório do projeto Adobe Commerce na infraestrutura em nuvem. Consulte o arquivo [.gitignore mais recente no repositório magento-cloud](https://github.com/magento/magento-cloud/blob/master/.gitignore). Para adicionar um arquivo que esteja na lista `.gitignore`, você pode usar a opção `-f` (forçar) ao preparar uma confirmação:

```bash
git add <path/filename> -f
```

## Alterar modelo base

Você pode usar as seguintes etapas para alterar a estrutura de um projeto existente para refletir o modelo base mais recente do Adobe Commerce na infraestrutura em nuvem.

1. Clonar o projeto em uma estação de trabalho local.

1. Atualize o arquivo `composer.json` com os seguintes valores para a seção `extra`.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Adicione o arquivo `.gitignore` criado para o modelo base. Por exemplo, se você precisar do arquivo `.gitignore` para a versão 2.2.6 do modelo, use o arquivo [.gitignore para 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) como referência.

1. Limpe o cache do Git.

   ```bash
   git rm -r --cached .
   ```

1. Adicionar e confirmar alterações.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
