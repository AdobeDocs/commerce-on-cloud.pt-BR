---
title: Criar variáveis
description: Consulte a lista de variáveis de ambiente que controlam ações na fase de criação da infraestrutura do Adobe Commerce na nuvem.
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# Criar variáveis

As variáveis _build_ a seguir controlam ações na fase de compilação e podem herdar e substituir valores das [Variáveis globais](variables-global.md). Insira estas variáveis no estágio `build` do arquivo `.magento.env.yaml`:

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

Para obter mais informações sobre como personalizar o processo de criação e implantação:

- [Configuração de implantação](configure-env-yaml.md)
- [Processo de implantação](../deploy/process.md)

As seguintes variáveis foram removidas na v2.2:

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **Padrão**—`1`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Defina o nível de aninhamento de diretórios para salvar arquivos de relatório de erros, a fim de evitar o preenchimento do diretório de relatório com dezenas de milhares de arquivos, o que pode dificultar o gerenciamento e a revisão dos dados. O padrão para esta configuração é `1`. Normalmente, você não precisa alterar o valor padrão, a menos que tenha problemas para gerenciar arquivos de relatório de erros no diretório `<magento_root>/var/report/`.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Especifique uma lista de patches de qualidade do Adobe Commerce a serem aplicados durante a implantação.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

O exemplo a seguir especifica três patches a serem aplicados durante a implantação.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

Consulte [Aplicar patches](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **Padrão**—`6`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Especifica qual nível de compactação [gzip](https://www.gnu.org/software/gzip) (`0` a `9`) usar ao compactar conteúdo estático; `0` desabilita a compactação.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **Padrão**—`600`
- **Versão** — Adobe Commerce 2.1.4 e posterior

Quando o tempo necessário para compactar os ativos estáticos excede o tempo limite de compactação, ele interrompe o processo de implantação. Defina o tempo máximo de execução, em segundos, para o comando static content compression.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **Padrão**—`false`
- **Versão** — Adobe Commerce 2.4.2 e posterior

Defina como `true` para impedir a geração de conteúdo estático para temas pai durante a fase de compilação.

Defina `SCD_NO_PARENT: false` durante a fase de compilação para que a geração de conteúdo estático para os temas principais não afete a implantação do site ou cause tempo de inatividade desnecessário. Consulte [Implantação de conteúdo estático](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Você pode configurar vários locais por tema. Essa personalização ajuda a acelerar o processo de criação, reduzindo o número de arquivos de tema desnecessários. Por exemplo, você pode criar o tema _magento/backend_ em inglês e um tema personalizado em outros idiomas.

O exemplo a seguir cria o tema `Magento/backend` com três localidades:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

O exemplo a seguir cria três temas com três localidades:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Ou você pode optar por _não_ implantar um tema:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.2.0 e posterior

Permite aumentar o tempo de execução máximo esperado para implantação de conteúdo estático.

Por padrão, o Adobe Commerce na infraestrutura em nuvem define a execução máxima esperada para 900 segundos, mas em alguns cenários pode ser necessário mais tempo para concluir a implantação de conteúdo estático para um projeto em nuvem.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **Padrão**—`quick`
- **Versão** — Adobe Commerce 2.2.0 e posterior

Personalize a [estratégia de implantação](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=pt-BR) para conteúdo estático. Consulte [Implantar arquivos de exibição estáticos](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=pt-BR).

Use estas opções _somente_ se você tiver mais de uma localidade:

- `standard`—implanta todos os arquivos de exibição estática para todos os pacotes.
- `quick`—(_padrão_) minimiza o tempo de implantação.
- `compact` — preserva o espaço em disco no servidor. No Adobe Commerce versão 2.2.4 e anterior, essa configuração substitui o valor de `scd_threads` por um valor de `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Padrão**—Automático
- **Versão** — Adobe Commerce 2.1.4 e posterior

Define o número de threads para implantação de conteúdo estático. O valor padrão é definido com base na contagem de threads do CPU detectada e não excede um valor 4. Aumentar o número de threads acelera a implantação de conteúdo estático; diminuir o número de threads o torna mais lento. Você pode definir o valor da thread, por exemplo:

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Para reduzir ainda mais o tempo de implantação, use o [Gerenciamento de Configuração](../store/store-settings.md) com o comando `scd-dump` para mover a implantação estática para a fase de compilação.

## `SCD_USE_BALER`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.3.0 e posterior

O [Baler](https://github.com/magento/baler) verifica seu código JavaScript gerado e cria um pacote JavaScript otimizado. Implantar o pacote otimizado em seu site pode reduzir o número de solicitações de rede ao carregar seu site e melhorar o tempo de carregamento da página.

Defina como `true` para executar o Baler após executar a implantação de conteúdo estático.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Como Baler está na versão alfa, não é aconselhável usá-lo em ambientes de Produção.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Padrão**— _Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Defina como `true` para ignorar o comando `composer dump-autoload` durante a instalação do Cloud Docker. Essa variável só é relevante para contêineres do Cloud Docker com sistemas de arquivos graváveis. Nesses casos, ignorar o comando evita erros de outros comandos que tentam acessar o código do diretório `generated` excluído.

Quando o Adobe Commerce executa o `composer dump-autoload`, ele cria arquivos de carregamento automático com links para classes geradas na pasta `generated`, o que não é um problema em ambientes de produção com sistemas de arquivos somente leitura. No entanto, para instalações do Cloud Docker com sistemas de arquivos graváveis (criados apenas para teste e desenvolvimento usando o `./vendor/bin/ece-docker build:compose --with-test`), é possível executar o comando `bin/magento -n setup:upgrade` sem a opção `--keep-generated`, que exclui o diretório `generated`. Se o diretório for excluído, o comando `composer dump-autoload` falhará porque o carregamento automático contém links para arquivos no diretório excluído.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Padrão**— _Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Defina como `true` para ignorar a implantação de conteúdo estático durante a fase de compilação.

Se você já implantar conteúdo estático durante a fase de compilação com o [Gerenciamento de Configuração](../store/store-settings.md), ignore a implantação de conteúdo estático para um teste de compilação rápido.

Na fase de compilação, defina `SKIP_SCD: false` de forma que a compilação de conteúdo estático ocorra durante a fase de compilação, em que o processo não afete a implantação do site ou cause tempo de inatividade desnecessário. Consulte [Implantação de conteúdo estático](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Padrão**—_Não definido_
- **Versão** — Adobe Commerce 2.1.4 e posterior

Habilite ou desabilite o nível de detalhamento de depuração [Symfony](https://symfony.com/doc/current/console/verbosity.html) para comandos CLI `bin/magento` executados durante a fase de implantação.

>[!NOTE]
>
>Para usar VERBOSE_COMMANDS para controlar os detalhes na saída do comando para comandos CLI `bin/magento` com êxito e com falha, você deve definir [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Escolha o nível de detalhes fornecido nos logs:

- `-v`= saída normal
- `-vv`= mais saída detalhada
- `-vvv` = saída detalhada ideal para depuração

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
