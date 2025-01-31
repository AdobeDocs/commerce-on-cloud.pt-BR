---
title: Propriedades
description: Use a lista de propriedades como referência ao configurar o aplicativo  [!DNL Commerce]  para compilação e implantação na infraestrutura de nuvem.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Propriedades para configuração de aplicativo

O arquivo `.magento.app.yaml` usa propriedades para gerenciar o suporte ao ambiente para o aplicativo [!DNL Commerce].

| Nome | Descrição | Padrão | Obrigatório |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | Personalizar funções de usuário | — | Não |
| [`crons`](crons-property.md) | Atualizar especificações e agendar trabalhos cron | — | Não |
| [`dependencies`](#dependencies) | Habilitar dependências adicionais | `php:composer/composer: '2.2.4'` | Não |
| [`disk`](#disk) | Definir o tamanho do disco persistente | `5120` | Sim |
| [`firewall`](firewall-property.md) | (Somente iniciador) Controlar tráfego de saída | — | Não |
| [`hooks`](hooks-property.md) | Personalizar comandos do shell para as fases de criação, implantação e pós-implantação | — | Não |
| [`mounts`](#mounts) | Definir caminhos | Caminhos:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | Não |
| [`name`](#name) | Definir o nome do aplicativo | `mymagento` | Sim |
| [`relationships`](#relationships) | Serviços de mapa | Serviços:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | Não |
| [`runtime`](#runtime) | A propriedade de tempo de execução inclui extensões exigidas pelo aplicativo [!DNL Commerce]. | Extensões:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | Sim |
| [`type`](#type-and-build) | Definir a imagem base do contêiner | `php:8.3` | Sim |
| [`variables`](variables-property.md) | Aplicação de uma variável de ambiente para uma versão específica do Commerce | — | Não |
| [`web`](web-property.md) | Lidar com solicitações externas | — | Sim |
| [`workers`](workers-property.md) | Lidar com solicitações externas | — | Sim, se não estiver usando a propriedade da Web |

{style="table-layout:auto"}

## `name`

A propriedade `name` fornece o nome do aplicativo usado no arquivo [`routes.yaml`](../routes/routes-yaml.md) para definir o upstream de HTTP (por padrão, `mymagento:http`). Por exemplo, se o valor de `name` for `app`, você deverá usar `app:http` no campo de upstream.

>[!WARNING]
>
>Não altere o nome do aplicativo após sua implantação. Isso resulta em perda de dados.

## `type` e `build`

As propriedades `type` e `build` fornecem informações sobre a imagem de contêiner base para compilar e executar o projeto.

A linguagem `type` com suporte é PHP. Especifique a versão do PHP da seguinte maneira:

```yaml
type: php:<version>
```

A propriedade `build` determina o que acontece por padrão ao compilar o projeto. O `flavor` especifica um conjunto padrão de tarefas de compilação a serem executadas. O exemplo a seguir mostra a configuração padrão para `type` e `build` de `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.3
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Instalação e uso do Composer 2

A propriedade `build: flavor:` não é usada para o Composer 2.x; portanto, você deve instalar o Composer manualmente durante a fase de compilação. Para instalar e usar o Composer 2.x em seus projetos Starter e Pro, você deve fazer três alterações na configuração do `.magento.app.yaml`:

1. Remover `composer` como `build: flavor:` e adicionar `none`. Essa alteração impede que a Cloud use a versão 1.x padrão do Composer para executar tarefas de build.
1. Adicione `composer/composer: '^2.0'` como uma dependência `php` para instalar o Composer 2.x.
1. Adicione as tarefas de compilação `composer` a um gancho `build` para executar as tarefas de compilação usando o Composer 2.x.

Usar os seguintes fragmentos de configuração na sua própria configuração do `.magento.app.yaml`:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Consulte [Pacotes obrigatórios](../development/overview.md#required-packages) para obter mais informações sobre o Composer.

## `dependencies`

Especifique as dependências que seu aplicativo pode precisar durante o processo de compilação.

O Adobe Commerce oferece suporte a dependências nos seguintes idiomas:

- PHP
- Ruby
- Node.js

Essas dependências são independentes das eventuais dependências do aplicativo e estão disponíveis no `PATH`, durante o processo de compilação e no ambiente de tempo de execução do aplicativo.

Você pode especificar essas dependências da seguinte maneira:

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

Use para modificar a configuração do PHP no tempo de execução, como habilitar extensões. As seguintes extensões são necessárias:

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

Consulte [configurações do PHP](php-settings.md) para obter detalhes sobre como habilitar extensões.

## `disk`

Define o tamanho de disco persistente do aplicativo em MB.

```yaml
disk: 5120
```

O tamanho de disco mínimo recomendado é de 256 MB. Se você vir o erro `UserError: Error building the project: Disk size may not be smaller than 128MB`, aumente o tamanho para 256 MB.

>[!NOTE]
>
>Para ambientes de Preparo e Produção Profissionais, você deve [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para atualizar a configuração `mounts` e `disk` para o seu aplicativo. Ao enviar o tíquete, indique as alterações de configuração necessárias e inclua uma versão atualizada do arquivo `.magento.app.yaml`.
>
>Não é possível aumentar temporariamente o armazenamento em disco no armazenamento temporário ou na produção; esse processo não é reversível.

## `relationships`

Define o mapeamento de serviços no aplicativo.

A relação `name` está disponível para o aplicativo na variável de ambiente `MAGENTO_CLOUD_RELATIONSHIPS`. A relação `<service-name>:<endpoint-name>` mapeia para os valores name e type definidos no arquivo `.magento/services.yaml`.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

Veja a seguir um exemplo dos relacionamentos padrão:

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

Consulte [Serviços](../services/services-yaml.md) para obter uma lista completa dos tipos de serviços e pontos de extremidade atualmente com suporte.

## `mounts`

Um objeto cujas chaves são caminhos relativos à raiz do aplicativo. A montagem é uma área gravável no disco para arquivos. Esta é uma lista padrão de montagens configuradas no arquivo `magento.app.yaml` usando a sintaxe `volume_id[/subpath]`:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

O formato para adicionar sua montagem a esta lista é o seguinte:

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared` — Compartilha um volume entre seus aplicativos dentro de um ambiente.
- `disk` — Define o tamanho disponível para o volume compartilhado.

>[!NOTE]
>
>Para ambientes de Preparo e Produção Profissionais, você deve [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para atualizar a configuração `mounts` e `disk` para o seu aplicativo. Ao enviar o tíquete, indique as alterações de configuração necessárias e inclua uma versão atualizada do arquivo `.magento.app.yaml`.

Você pode tornar a montagem da Web acessível adicionando-a ao bloco de locais [`web`](web-property.md).

>[!WARNING]
>
>Depois que o site tiver dados, não altere a parte `subpath` do nome de montagem. Este valor é o identificador exclusivo da área `files`. Se você alterar esse nome, perderá todos os dados do site armazenados no local antigo.

## `access`

A propriedade `access` indica um nível mínimo de função de usuário que tem acesso SSH permitido aos ambientes. As funções de usuário disponíveis são:

- `admin` — Pode alterar configurações e executar ações no ambiente; tem direitos de _colaborador_ e _visualizador_.
- `contributor` — Pode enviar código para este ambiente e ramificar do ambiente; tem direitos de _visualizador_.
- `viewer` — Pode exibir somente o ambiente.

A função de usuário padrão é `contributor`, que restringe o acesso SSH de usuários com apenas direitos de _visualizador_. Você pode alterar a função de usuário para `viewer` para permitir acesso SSH a usuários com direitos de apenas _visualizador_:

```yaml
access:
    ssh: viewer
```
