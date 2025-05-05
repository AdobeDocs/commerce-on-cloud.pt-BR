---
title: Variáveis globais
description: Consulte a lista de variáveis de ambiente que controlam ações no processo de implantação do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Variáveis globais

As variáveis globais controlam ações em cada fase do processo de implantação do [!DNL Commerce]: compilação, implantação e pós-implantação. Como as variáveis globais afetam cada fase, você deve defini-las no estágio `global` do arquivo `.magento.env.yaml`:

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

Para obter mais informações sobre como personalizar o processo de criação e implantação:

- [Configuração de implantação](configure-env-yaml.md)
- [Processo de implantação](../deploy/process.md)

## `ENABLE_EVENTING`

- **Padrão**-_Não definido_
- **Versão** — Adobe Commerce 2.4.5 e posterior

Quando definido como `true`, habilita o cron para executar consumidores da fila de mensagens. O Adobe I/O Events para Adobe Commerce usa filas de mensagens para acelerar a entrega de eventos críticos.

A Adobe recomenda que você também adicione a variável [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) ao estágio `deploy` do arquivo `.magento.env.yaml` com `cron_run` definido como `true`.

O exemplo a seguir mostra uma variável `ENABLE_EVENTING` totalmente configurada.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **Padrão**-_Não definido_
- **Versão** — Adobe Commerce 2.4.4 e posterior

Quando definido como `true`, habilita webhooks do Commerce. O webhook é executado em um endpoint externo, como uma ação de tempo de execução do App Builder ou um sistema de gerenciamento de inventário de terceiros. O [_Guia de Webhooks_](https://developer.adobe.com/commerce/extensibility/webhooks) descreve esse recurso detalhadamente.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Substitui o nível mínimo de log para todos os fluxos de saída sem alterar o código, o que ajuda a solucionar problemas com a implantação. Por exemplo, se a implantação falhar, você pode usar essa variável para aumentar a granularidade do registro globalmente. Consulte [Níveis de log](log-handlers.md#log-levels). O valor `min_level` nos manipuladores de log substitui essa configuração.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>A configuração da variável `MIN_LOGGING_LEVEL` não altera a configuração do nível de log do manipulador de arquivos, que é definido como `debug` por padrão.

## `SCD_ON_DEMAND`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Ative a geração de conteúdo estático quando solicitado por um usuário (SCD). O conteúdo estático sob demanda é ideal para o fluxo de trabalho de desenvolvimento e teste, pois diminui o tempo de implantação.

O pré-carregamento do cache com o [`post_deploy` gancho](../application/hooks-property.md) reduz o tempo de inatividade do site. O aquecimento do cache está disponível somente para projetos Pro que contêm ambientes de Preparo e Produção no [!DNL Cloud Console] e para projetos Starter. Adicione a variável de ambiente `SCD_ON_DEMAND` ao estágio `global` no arquivo `.magento.env.yaml`:

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

A variável `SCD_ON_DEMAND` ignora o SCD em ambas as fases (compilação e implantação), limpa as pastas `pub/static` e `var/view_preprocessed` e grava o seguinte no arquivo `app/etc/env.php`:

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.2.0 e posterior

Permite aumentar o tempo de execução máximo esperado para implantação de conteúdo estático.

Por padrão, o Adobe Commerce define a execução máxima esperada para 900 segundos, mas em alguns cenários, pode ser necessário mais tempo para concluir a implantação do conteúdo estático para um projeto na nuvem.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.4.2 e posterior

Defina como `true` para impedir a geração de conteúdo estático para temas pai durante as fases de compilação e implantação. Quando essa opção é definida como `true`, menos conteúdo estático é gerado, o que melhora os tempos gerais de compilação e implantação.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.3.0 e posterior

[Baler](https://github.com/magento/baler) é um módulo que verifica o código JavaScript gerado e cria um pacote JavaScript otimizado. Implantar o pacote otimizado em seu site pode reduzir o número de solicitações de rede ao carregar seu site e melhorar o tempo de carregamento da página.

Defina como `true` para executar o Baler após executar a implantação de conteúdo estático.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Instale e configure o módulo Baler antes de usar esse recurso. Como o Baler está na versão alfa, ative essa opção somente em ambientes de preparo.

## `SKIP_HTML_MINIFICATION`

- **Padrão**:
   - `true`—para `ece-tools` 2002.0.13 e posterior
   - `false`—para versões anteriores do `ece-tools`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Habilita ou desabilita a cópia de arquivos de exibição estáticos para o diretório `<magento_root>/init/` no final do estágio de compilação. Se definido como `true`, os arquivos não serão copiados e a minificação de HTML estará disponível mediante solicitação. Defina esse valor como `true` para reduzir o tempo de inatividade ao implantar em ambientes de Preparo e Produção.

- **`false`**—Copia o diretório `view_preprocessed` para o diretório `<magento_root>/init/` no final da fase de compilação e restaura o diretório no diretório `<magento_root>/var` no início da fase de implantação.
- **`true`** — Habilita a minificação de HTML sob demanda; _não_ copia `<magento_root>var/view_preprocessed` para o diretório `<magento_root>/init/` no final da fase de compilação.

Adicione a variável de ambiente `SKIP_HTML_MINIFICATION` ao estágio `global` no arquivo `.magento.env.yaml`:

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Use a variável `X_FRAME_CONFIGURATION` para alterar a configuração do cabeçalho [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html?lang=pt-BR) do site do Adobe Commerce. Esta configuração controla como o navegador renderiza uma página em um `<frame>`, `<iframe>` ou `<object>`. Use uma das seguintes opções:

- `DENY`—A página não pode ser exibida em um quadro.
- `SAMEORIGIN`—(A configuração padrão do Adobe Commerce.) A página pode ser exibida somente em um quadro na mesma origem da própria página.

>[!WARNING]
>
>A opção `ALLOW-FROM <uri>` foi descontinuada porque os navegadores com suporte para Adobe Commerce não oferecem mais suporte a ela. Consulte [Compatibilidade do navegador](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility).

Adicione a variável de ambiente `X_FRAME_CONFIGURATION` ao estágio `global` no arquivo `.magento.env.yaml`:

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
