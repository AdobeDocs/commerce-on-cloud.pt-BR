---
title: Propriedade de ganchos
description: Veja exemplos de como configurar a propriedade de ganchos no arquivo de configuração do aplicativo  [!DNL Commerce] .
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Propriedade de ganchos

Use a seção `hooks` para executar comandos de shell durante as fases de compilação, implantação e pós-implantação:

- **`build`**—Execute comandos _antes_ empacotando seu aplicativo. Serviços, como o banco de dados ou Redis, não estão disponíveis, pois o aplicativo ainda não foi implantado. Adicione comandos personalizados _antes_ do comando `php ./vendor/bin/ece-tools` padrão para que o conteúdo gerado de forma personalizada continue na fase de implantação.

- **`deploy`**—Execute comandos _após_ empacotando e implantando seu aplicativo. Você pode acessar outros serviços neste ponto. Como o comando `php ./vendor/bin/ece-tools` padrão copia o diretório `app/etc` para o local correto, você deve adicionar comandos personalizados _após_ o comando de implantação para evitar a falha de comandos personalizados.

- **`post_deploy`**—Execute os comandos _após_ a implantação do aplicativo e _após_ o contêiner começa a aceitar conexões. O gancho `post_deploy` limpa o cache e pré-carrega (aquece) o cache. Você pode personalizar a lista de páginas usando a variável `WARM_UP_PAGES` na [Fase de pós-implantação](../environment/variables-post-deploy.md). Embora não seja obrigatório, isso funciona em conjunto com a variável de ambiente `SCD_ON_DEMAND`.

O exemplo a seguir mostra a configuração padrão no arquivo `.magento.app.yaml`. Adicione comandos CLI nas seções `build`, `deploy` ou `post_deploy` _antes_ do comando `ece-tools`:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

Além disso, é possível personalizar ainda mais a fase de compilação usando os comandos `generate` e `transfer` para executar ações adicionais ao compilar ou mover arquivos especificamente.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` — faz com que os ganchos falhem no primeiro comando com falha, em vez do comando final com falha.
- `build:generate` — aplica patches, valida a configuração, gera ID e gera conteúdo estático se o SCD estiver habilitado para a fase de compilação.
- `build:transfer` — transfere o código gerado e o conteúdo estático para o destino final.

Os comandos são executados do diretório do aplicativo (`/app`). Você pode usar o comando `cd` para alterar o diretório. Os ganchos falharão se o comando final neles falhar. Para fazer com que falhem no primeiro comando com falha, adicione `set -e` ao início do gancho.

**Para compilar arquivos Sass usando grunt**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Compilar arquivos Sass usando `grunt` antes da implantação de conteúdo estático, o que acontece durante a compilação. Coloque o comando `grunt` antes do comando `build`.

{{scenarios}}
