---
source-git-commit: 9166b44ae53e8cfc6b8022730a6b91406ba696c0
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# magento-cloud (Adobe Commerce na infraestrutura em nuvem)

<!-- The template to render with above values -->

**Versão**: 1.46.1

Esta referência contém 119 comandos disponíveis através da ferramenta de linha de comando `magento-cloud`.
A lista inicial é gerada automaticamente usando o comando `magento-cloud list` no Adobe Commerce na infraestrutura em nuvem.

## Geral

Essa referência é gerada a partir da base de código do aplicativo. Para alterar o conteúdo, você pode atualizar o código-fonte para a implementação de comando correspondente no repositório [codebase](https://github.com/magento/magento-cloud-cli) e enviar suas alterações para revisão. Outra maneira é _Fornecer comentários_ (localizar o link no canto superior direito). Para obter as diretrizes de contribuição, consulte [Contribuições de código](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Opções globais

#### `--help`, `-h`

Exibir esta mensagem de ajuda

- Padrão: `false`
- Não aceita um valor

#### `--verbose`, `-v|-vv|-vvv`

Aumentar a verbosidade das mensagens

- Padrão: `false`
- Não aceita um valor

#### `--version`, `-V`

Exibir esta versão do aplicativo

- Padrão: `false`
- Não aceita um valor

#### `--yes`, `-y`

Responda &quot;sim&quot; às perguntas de confirmação; aceite o valor padrão para outras perguntas; desative a interação

- Padrão: `false`
- Não aceita um valor

#### `--no-interaction`

Não faça perguntas interativas; aceite valores padrão. Equivalente ao uso da variável de ambiente: MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- Padrão: `false`
- Não aceita um valor


## `clear-cache`

```bash
magento-cloud cc
```

Limpe o cache da CLI

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `decode`

```bash
magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Decodificar uma string codificada, como MAGENTO_CLOUD_VARIABLES

### Argumentos

#### `value`

O valor da variável a ser decodificado

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade a ser exibida na variável

- Requer um valor


## `docs`

```bash
magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

Abrir a documentação online

### Argumentos

#### `search`

Pesquisar termo(s)

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--browser`

O navegador a ser usado para abrir o URL. Defina 0 para nenhum.

- Requer um valor

#### `--pipe`

Transfira o URL para o stdout.

- Padrão: `false`
- Não aceita um valor


## `help`

```bash
magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

Exibe a ajuda para um comando

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### Argumentos

#### `command_name`

O nome do comando

- Padrão: `help`

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída (txt, json ou md)

- Padrão: `txt`
- Requer um valor

#### `--raw`

Para gerar a ajuda do comando raw

- Padrão: `false`
- Não aceita um valor


## `list`

```bash
magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

Comandos de Listas

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### Argumentos

#### `command`

O comando a ser executado

- Obrigatório


#### `namespace`

O nome do namespace

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--raw`

Para gerar a lista de comandos raw

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída (txt, xml, json ou md)

- Padrão: `txt`
- Requer um valor

#### `--all`

Mostrar todos os comandos, incluindo os ocultos

- Padrão: `false`
- Não aceita um valor


## `multi`

```bash
magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

Executar um comando em vários projetos

### Argumentos

#### `cmd`

O comando a ser executado

- Padrão: `[]`
- Obrigatório

- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--projects`, `-p`

Uma lista de IDs de projeto, separadas por vírgulas e/ou espaço em branco

- Requer um valor

#### `--continue`

Continuar executando comandos mesmo se uma exceção for encontrada

- Padrão: `false`
- Não aceita um valor

#### `--sort`

Uma propriedade pela qual classificar a lista de opções do projeto

- Padrão: `title`
- Requer um valor

#### `--reverse`

Inverter a ordem das opções do projeto

- Padrão: `false`
- Não aceita um valor


## `web`

```bash
magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Abrir o projeto na interface do usuário da Web

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--browser`

O navegador a ser usado para abrir o URL. Defina 0 para nenhum.

- Requer um valor

#### `--pipe`

Transfira o URL para o stdout.

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `activity:cancel`

```bash
magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Cancelar uma atividade

### Argumentos

#### `id`

A ID da atividade. O padrão é a atividade cancelável mais recente.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--type`, `-t`

Filtrar por tipo (ao selecionar uma atividade padrão). Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para o tipo, por exemplo, &#39;%var%&#39; para selecionar atividades relacionadas a variáveis.

- Padrão: `[]`
- Requer um valor

#### `--exclude-type`, `-x`

Excluir por tipo (ao selecionar uma atividade padrão). Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para excluir tipos.

- Padrão: `[]`
- Requer um valor

#### `--all`, `-a`

Verificar atividades recentes em todos os ambientes (ao selecionar uma atividade padrão)

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `activity:get`

```bash
magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

Exibir informações detalhadas sobre uma única atividade

### Argumentos

#### `id`

A ID da atividade. O padrão é a atividade mais recente.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade a ser exibida

- Requer um valor

#### `--type`, `-t`

Filtrar por tipo (ao selecionar uma atividade padrão). Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para o tipo, por exemplo, &#39;%var%&#39; para selecionar atividades relacionadas a variáveis.

- Padrão: `[]`
- Requer um valor

#### `--exclude-type`, `-x`

Excluir por tipo (ao selecionar uma atividade padrão). Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para excluir tipos.

- Padrão: `[]`
- Requer um valor

#### `--state`

Filtrar por estado (ao selecionar uma atividade padrão): in_progress, pending, complete ou canceled. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--result`

Filtrar por resultado (ao selecionar uma atividade padrão): sucesso ou falha

- Requer um valor

#### `--incomplete`, `-i`

Incluir somente atividades incompletas (ao selecionar uma atividade padrão). Esta é uma abreviação de — state=in_progress,pending

- Padrão: `false`
- Não aceita um valor

#### `--all`, `-a`

Verificar atividades recentes em todos os ambientes (ao selecionar uma atividade padrão)

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `activity:list`

```bash
magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obter uma lista de atividades para um ambiente ou projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--type`, `-t`

Filtrar atividades por tipo Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. A primeira parte do nome da atividade pode ser omitida. Por exemplo, &quot;cron&quot; pode selecionar atividades &quot;environment.cron&quot;. Os caracteres % ou * podem ser usados como curinga, por exemplo, &#39;%var%&#39; para selecionar atividades relacionadas a variáveis.

- Padrão: `[]`
- Requer um valor

#### `--exclude-type`, `-x`

Excluir atividades por tipo. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. A primeira parte do nome da atividade pode ser omitida. Por exemplo, &quot;cron&quot; pode excluir atividades &quot;environment.cron&quot;. Os caracteres % ou * podem ser usados como curinga para excluir tipos.

- Padrão: `[]`
- Requer um valor

#### `--limit`

Limitar o número de resultados exibidos

- Padrão: `10`
- Requer um valor

#### `--start`

Somente atividades criadas antes desta data serão listadas

- Requer um valor

#### `--state`

Filtrar atividades por estado: in_progress, pending, complete ou canceled. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--result`

Filtrar atividades por resultado: sucesso ou falha

- Requer um valor

#### `--incomplete`, `-i`

Listar apenas atividades incompletas

- Padrão: `false`
- Não aceita um valor

#### `--all`, `-a`

Listar atividades em todos os ambientes

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: id*, created*, description*, progress*, state*, result*, completed, ambientes, type (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `activity:log`

```bash
magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Exibir o log de uma atividade

### Argumentos

#### `id`

A ID da atividade. O padrão é a atividade mais recente.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Intervalo de atualização da atividade (segundos). Defina como 0 para desativar a atualização.

- Padrão: `3`
- Requer um valor

#### `--timestamps`, `-t`

Exibir um carimbo de data e hora ao lado de cada mensagem

- Padrão: `false`
- Não aceita um valor

#### `--type`

Filtrar por tipo (ao selecionar uma atividade padrão). Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para o tipo, por exemplo, &#39;%var%&#39; para selecionar atividades relacionadas a variáveis.

- Padrão: `[]`
- Requer um valor

#### `--exclude-type`, `-x`

Excluir por tipo (ao selecionar uma atividade padrão). Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para excluir tipos.

- Padrão: `[]`
- Requer um valor

#### `--state`

Filtrar por estado (ao selecionar uma atividade padrão): in_progress, pending, complete ou canceled. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--result`

Filtrar por resultado (ao selecionar uma atividade padrão): sucesso ou falha

- Requer um valor

#### `--incomplete`, `-i`

Incluir somente atividades incompletas (ao selecionar uma atividade padrão). Esta é uma abreviação de — state=in_progress,pending

- Padrão: `false`
- Não aceita um valor

#### `--all`, `-a`

Verificar atividades recentes em todos os ambientes (ao selecionar uma atividade padrão)

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `app:config-get`

```bash
magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Exibir a configuração de um aplicativo

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade de configuração a ser exibida

- Requer um valor

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--identity-file`, `-i`

[Opção obsoleta, não é mais usada]

- Requer um valor


## `app:list`

```bash
magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Listar aplicativos no projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--pipe`

Gerar uma lista somente de nomes de aplicativos

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: nome*, tipo*, disco, caminho, tamanho (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `auth:api-token-login`

```bash
magento-cloud auth:api-token-login
```

Faça logon na Magento Cloud usando um token de API

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `auth:browser-login`

```bash
magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

Fazer logon na Magento Cloud por meio de um navegador

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--force`, `-f`

Fazer logon novamente, mesmo que já tenha feito logon

- Padrão: `false`
- Não aceita um valor

#### `--browser`

O navegador a ser usado para abrir o URL. Defina 0 para nenhum.

- Requer um valor

#### `--pipe`

Transfira o URL para o stdout.

- Padrão: `false`
- Não aceita um valor


## `auth:info`

```bash
magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

Exibir as informações da sua conta

### Argumentos

#### `property`

A propriedade da conta a ser exibida

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--no-auto-login`

Ignora o logon automático. Nada será gerado se não estiver conectado e o código de saída será 0, assumindo que não há outros erros.

- Padrão: `false`
- Não aceita um valor

#### `--property`, `-P`

A propriedade da conta a ser exibida (sintaxe alternativa)

- Requer um valor

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `auth:logout`

```bash
magento-cloud logout [-a|--all] [--other]
```

Fazer logoff da Magento Cloud

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--all`, `-a`

Fazer logoff de todas as sessões locais

- Padrão: `false`
- Não aceita um valor

#### `--other`

Fazer logoff de outras sessões locais

- Padrão: `false`
- Não aceita um valor


## `blackfire:setup`

```bash
magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Configurar a integração do Blackfire.io para o projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--server_id`

A ID do servidor

- Requer um valor

#### `--server_token`

O token do servidor

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `certificate:add`

```bash
magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Adicionar um certificado SSL ao projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--cert`

O caminho para o arquivo de certificado

- Requer um valor

#### `--key`

O caminho para o arquivo de chave privada do certificado

- Requer um valor

#### `--chain`

O caminho para o arquivo da cadeia de certificados

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `certificate:delete`

```bash
magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

Excluir um certificado do projeto

### Argumentos

#### `id`

A ID do certificado (ou o início dela)

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `certificate:get`

```bash
magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

Exibir um certificado

### Argumentos

#### `id`

A ID do certificado (ou o início dela)

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade do certificado a ser exibida

- Requer um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `certificate:list`

```bash
magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Listar certificados de projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--domain`

Filtrar por nome de domínio (pesquisa que não diferencia maiúsculas de minúsculas)

- Requer um valor

#### `--exclude-domain`

Excluir certificados, correspondendo pelo nome de domínio (pesquisa que não diferencia maiúsculas de minúsculas)

- Requer um valor

#### `--issuer`

Filtrar por emissor

- Requer um valor

#### `--only-auto`

Mostrar apenas certificados autoprovisionados

- Padrão: `false`
- Não aceita um valor

#### `--no-auto`

Mostrar apenas certificados adicionados manualmente

- Padrão: `false`
- Não aceita um valor

#### `--ignore-expiry`

Mostrar certificados expirados e não expirados

- Padrão: `false`
- Não aceita um valor

#### `--only-expired`

Mostrar apenas certificados expirados

- Padrão: `false`
- Não aceita um valor

#### `--no-expired`

Mostrar apenas certificados não expirados (padrão)

- Padrão: `false`
- Não aceita um valor

#### `--pipe-domains`

Retornar apenas uma lista de nomes de domínio cobertos pelos certificados

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: criado, domínios, expires, id, emissor. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `commit:get`

```bash
magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

Mostrar detalhes da confirmação

### Argumentos

#### `commit`

O SHA de confirmação. Isso também pode aceitar os sufixos &quot;HEAD&quot; e acento circunflexo (^) ou til (~) para confirmações principais.

- Padrão: `HEAD`

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade de confirmação a ser exibida.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `commit:list`

```bash
magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

Listar confirmações

### Argumentos

#### `commit`

O SHA de Git commit inicial. Isso também pode aceitar os sufixos &quot;HEAD&quot; e acento circunflexo (^) ou til (~) para confirmações principais.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--limit`

O número de confirmações a serem exibidas.

- Padrão: `10`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: author, date, sha, summary. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `db:dump`

```bash
magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Criar um dump local do banco de dados remoto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--schema`

O esquema a ser descarregado. Omitir o uso do schema padrão (geralmente &quot;principal&quot;).

- Requer um valor

#### `--file`, `-f`

Um nome de arquivo personalizado para o despejo

- Requer um valor

#### `--directory`, `-d`

Um diretório personalizado para o despejo

- Requer um valor

#### `--gzip`, `-z`

Compactar o despejo usando gzip

- Padrão: `false`
- Não aceita um valor

#### `--timestamp`, `-t`

Adicionar um carimbo de data e hora ao nome do arquivo de despejo

- Padrão: `false`
- Não aceita um valor

#### `--stdout`, `-o`

Saída para STDOUT em vez de um arquivo

- Padrão: `false`
- Não aceita um valor

#### `--table`

Tabela(s) a serem incluídas

- Padrão: `[]`
- Requer um valor

#### `--exclude-table`

Tabela(s) a ser excluída(s)

- Padrão: `[]`
- Requer um valor

#### `--schema-only`

Despejar apenas esquemas, sem dados

- Padrão: `false`
- Não aceita um valor

#### `--charset`

A codificação do conjunto de caracteres para o despejo

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `db:size`

```bash
magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

Estimar o uso do disco de um banco de dados

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--bytes`, `-B`

Mostrar tamanhos em bytes.

- Padrão: `false`
- Não aceita um valor

#### `--cleanup`, `-C`

Verificar se as tabelas podem ser limpas e mostrar recomendações (somente InnoDb).

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: max, percent_used, used. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `db:sql`

```bash
magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

Executar SQL no banco de dados remoto

### Argumentos

#### `query`

Uma instrução SQL a ser executada

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--raw`

Produzir saída bruta, não tabular

- Padrão: `false`
- Não aceita um valor

#### `--schema`

O esquema a ser usado. Omitir o uso do schema padrão (geralmente &quot;principal&quot;). Transmita uma string vazia para não usar nenhum esquema.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `domain:add`

```bash
magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Adicionar um novo domínio ao projeto

### Argumentos

#### `name`

O nome do domínio

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--cert`

O caminho para o arquivo de certificado deste domínio

- Requer um valor

#### `--key`

O caminho para o arquivo de chave privada do certificado fornecido.

- Requer um valor

#### `--chain`

O caminho para o(s) arquivo(s) da cadeia de certificados do certificado fornecido

- Padrão: `[]`
- Requer um valor

#### `--attach`

O domínio de produção que este substitui nas rotas do ambiente. Necessário para domínios de ambiente de não produção.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `domain:delete`

```bash
magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Excluir um domínio do projeto

### Argumentos

#### `name`

O nome do domínio

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `domain:get`

```bash
magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

Mostrar informações detalhadas de um domínio

### Argumentos

#### `name`

O nome do domínio

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade de domínio a ser exibida

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `domain:list`

```bash
magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obter uma lista de todos os domínios

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: name*, ssl*, created_at*, registered_name, replacement_for, type, updated_at (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `domain:update`

```bash
magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Atualizar um domínio

### Argumentos

#### `name`

O nome do domínio

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--cert`

O caminho para o arquivo de certificado deste domínio

- Requer um valor

#### `--key`

O caminho para o arquivo de chave privada do certificado fornecido.

- Requer um valor

#### `--chain`

O caminho para o(s) arquivo(s) da cadeia de certificados do certificado fornecido

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:activate`

```bash
magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Ativar um ambiente

### Argumentos

#### `environment`

Os ambientes a serem ativados

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--parent`

Definir um novo ambiente principal antes da ativação

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:branch`

```bash
magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

Ramificar um ambiente

### Argumentos

#### `id`

A ID (nome da ramificação) do novo ambiente


#### `parent`

O pai do novo ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--title`

O título do novo ambiente

- Requer um valor

#### `--type`

O tipo do novo ambiente

- Requer um valor

#### `--no-clone-parent`

Não clonar os dados do ambiente pai

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:checkout`

```bash
magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

Confira um ambiente

### Argumentos

#### `id`

A ID do ambiente a ser verificado. Por exemplo: &quot;sprint2&quot;

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `environment:delete`

```bash
magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Excluir um ou mais ambientes

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### Argumentos

#### `environment`

Os ambientes a serem excluídos. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--delete-branch`

Excluir ramificações Git para ambientes inativos, sem confirmação

- Padrão: `false`
- Não aceita um valor

#### `--no-delete-branch`

Não excluir ramificações Git (ambientes inativos)

- Padrão: `false`
- Não aceita um valor

#### `--type`

Excluir todos os ambientes de um tipo (adicionando a qualquer outro selecionado) Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--only-type`, `-t`

Somente os ambientes de exclusão de um tipo específico de Valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--exclude`

Ambiente(s) a não excluir. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--exclude-type`

Os tipos de ambiente dos quais não excluir valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--inactive`

Excluir todos os ambientes inativos (adicionando a quaisquer outros selecionados)

- Padrão: `false`
- Não aceita um valor

#### `--merged`

Excluir todos os ambientes mesclados (adicionando a qualquer outro selecionado)

- Padrão: `false`
- Não aceita um valor

#### `--allow-delete-parent`

Permitir que ambientes com filhos sejam excluídos

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:http-access`

```bash
magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Atualizar configurações de acesso HTTP para um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--access`

Restrição de acesso no formato &quot;permissão:endereço&quot;. Use 0 para limpar todos os endereços.

- Padrão: `[]`
- Requer um valor

#### `--auth`

Credenciais de autenticação Básicas do HTTP no formato &quot;nome de usuário:senha&quot;. Use 0 para limpar todas as credenciais.

- Padrão: `[]`
- Requer um valor

#### `--enabled`

Se o controle de acesso deve ser ativado: 1 para ativar, 0 para desativar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:info`

```bash
magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Ler ou definir propriedades de um ambiente

### Argumentos

#### `property`

O nome da propriedade


#### `value`

Definir um novo valor para a propriedade

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:init`

```bash
magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

Inicializar um ambiente a partir de um repositório Git público

### Argumentos

#### `url`

Um URL para um repositório Git

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--profile`

O nome do perfil

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:list`

```bash
magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Obter uma lista de ambientes

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--no-inactive`, `-I`

Não mostrar ambientes inativos

- Padrão: `false`
- Não aceita um valor

#### `--pipe`

Gerar uma lista simples de IDs de ambiente.

- Padrão: `false`
- Não aceita um valor

#### `--refresh`

Se a lista deve ser atualizada.

- Padrão: `1`
- Requer um valor

#### `--sort`

Uma propriedade para classificar por

- Padrão: `title`
- Requer um valor

#### `--reverse`

Classificar em ordem inversa (descendente)

- Padrão: `false`
- Não aceita um valor

#### `--type`

Filtrar a lista por tipos de ambiente. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: id*, title*, status*, type*, created, machine_name, updated (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `environment:logs`

```bash
magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

Ler logs de um ambiente

### Argumentos

#### `type`

O tipo de log, por exemplo, &quot;acesso&quot; ou &quot;erro&quot;

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--lines`

O número de linhas a serem exibidas

- Padrão: `100`
- Requer um valor

#### `--tail`

Continuamente rastrear o log

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor


## `environment:merge`

```bash
magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Mesclar um ambiente

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### Argumentos

#### `environment`

O ambiente a ser mesclado

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:pause`

```bash
magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Pausar um ambiente

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:push`

```bash
magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

Enviar código para um ambiente

### Argumentos

#### `source`

A referência de origem: um nome de ramificação ou hash de confirmação

- Padrão: `HEAD`

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--target`

O nome da ramificação de destino. O padrão é a ramificação atual.

- Requer um valor

#### `--force`, `-f`

Permitir atualizações não rápidas

- Padrão: `false`
- Não aceita um valor

#### `--force-with-lease`

Permitir atualizações não rápidas se a ramificação de rastreamento remoto estiver atualizada

- Padrão: `false`
- Não aceita um valor

#### `--set-upstream`, `-u`

Defina o ambiente de destino como upstream para a ramificação de origem. Isso também definirá o projeto de destino como remoto para o repositório local.

- Padrão: `false`
- Não aceita um valor

#### `--activate`

Ativar o ambiente antes de enviar

- Padrão: `false`
- Não aceita um valor

#### `--parent`

Definir o novo ambiente principal (usado somente com — ativate)

- Requer um valor

#### `--type`

Definir o tipo de ambiente (usado somente com —ativate )

- Requer um valor

#### `--no-clone-parent`

Não clonar os dados da ramificação pai (usados somente com — ativate)

- Padrão: `false`
- Não aceita um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `environment:redeploy`

```bash
magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Reimplantação de um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:relationships`

```bash
magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

Mostrar relações de um ambiente

### Argumentos

#### `environment`

O ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade de relacionamento a ser exibida

- Requer um valor

#### `--refresh`

Se os relacionamentos devem ser atualizados

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `environment:resume`

```bash
magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Retomar um ambiente pausado

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:scp`

```bash
magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

Copiar arquivos de e para um ambiente usando scp

### Argumentos

#### `files`

Arquivos para copiar. Use o prefixo remote: para definir locais remotos.

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--recursive`, `-r`

Copiar recursivamente diretórios inteiros

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `environment:ssh`

```bash
magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

SSH para o ambiente atual

### Argumentos

#### `cmd`

Um comando a ser executado no ambiente.

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--pipe`

Saída somente do URL SSH.

- Padrão: `false`
- Não aceita um valor

#### `--all`

Saída de todos os URLs SSH (para cada aplicativo).

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `environment:synchronize`

```bash
magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

Sincronizar o código e/ou os dados de um ambiente a partir de seu pai

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### Argumentos

#### `synchronize`

O que sincronizar: &quot;código&quot;, &quot;dados&quot; ou ambos

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--rebase`

Sincronizar código rebaseando em vez de mesclar

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `environment:url`

```bash
magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obter os URLs públicos de um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--primary`, `-1`

Retornar apenas o URL da rota principal

- Padrão: `false`
- Não aceita um valor

#### `--browser`

O navegador a ser usado para abrir o URL. Defina 0 para nenhum.

- Requer um valor

#### `--pipe`

Transfira o URL para o stdout.

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `environment:xdebug`

```bash
magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Abrir um túnel para o Xdebug no ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--port`

A porta local

- Padrão: `9000`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `integration:activity:get`

```bash
magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

Exibir informações detalhadas sobre uma única atividade de integração

### Argumentos

#### `integration`

Uma ID de integração. Deixe em branco para escolher em uma lista.


#### `activity`

A ID da atividade. O padrão é a atividade de integração mais recente.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade a ser exibida

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

[Opção obsoleta, não usada]

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `integration:activity:list`

```bash
magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Obter uma lista de atividades para uma integração

### Argumentos

#### `id`

Uma ID de integração. Deixe em branco para escolher em uma lista.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--type`

Filtrar atividades por tipo. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--exclude-type`, `-x`

Excluir atividades por tipo. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco. Os caracteres % ou * podem ser usados como curinga para excluir tipos.

- Padrão: `[]`
- Requer um valor

#### `--limit`

Limitar o número de resultados exibidos

- Padrão: `10`
- Requer um valor

#### `--start`

Somente atividades criadas antes desta data serão listadas

- Requer um valor

#### `--state`

Filtrar atividades por estado. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--result`

Filtrar atividades por resultado

- Requer um valor

#### `--incomplete`, `-i`

Listar apenas atividades incompletas

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: id*, created*, description*, type*, state*, result*, completed (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

[Opção obsoleta, não usada]

- Requer um valor


## `integration:activity:log`

```bash
magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

Exibir o log de uma atividade de integração

### Argumentos

#### `integration`

Uma ID de integração. Deixe em branco para escolher em uma lista.


#### `activity`

A ID da atividade. O padrão é a atividade de integração mais recente.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--timestamps`, `-t`

Exibir um carimbo de data e hora ao lado de cada mensagem

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

[Opção obsoleta, não usada]

- Requer um valor


## `integration:add`

```bash
magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Adicionar uma integração ao projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--type`

O tipo de integração (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerDuty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- Requer um valor

#### `--base-url`

O URL de base da instalação do servidor

- Requer um valor

#### `--bitbucket-url`

O URL base da instalação do Servidor Bitbucket

- Requer um valor

#### `--username`

O nome de usuário do servidor Bitbucket

- Requer um valor

#### `--token`

Um token de autenticação ou acesso para a integração

- Requer um valor

#### `--key`

Uma chave de consumidor OAuth de Bitbucket

- Requer um valor

#### `--secret`

Um segredo do consumidor OAuth do Bitbucket

- Requer um valor

#### `--license-key`

A chave de licença dos logs do New Relic

- Requer um valor

#### `--server-project`

O projeto (por exemplo, &quot;namespace/repo&quot;)

- Requer um valor

#### `--repository`

O repositório a ser rastreado (por exemplo, &quot;proprietário/repositório&quot;)

- Requer um valor

#### `--build-merge-requests`

GitLab: criar solicitações de mesclagem como ambientes

- Padrão: `true`
- Requer um valor

#### `--build-pull-requests`

Criar cada solicitação de pull como um ambiente

- Padrão: `true`
- Requer um valor

#### `--build-draft-pull-requests`

Criar solicitações de pull de rascunho

- Padrão: `true`
- Requer um valor

#### `--build-pull-requests-post-merge`

Criar solicitações pull com base em seu estado pós-mesclagem

- Padrão: `false`
- Requer um valor

#### `--build-wip-merge-requests`

GitLab: criar solicitações de mesclagem WIP

- Padrão: `true`
- Requer um valor

#### `--merge-requests-clone-parent-data`

GitLab: clonar dados para solicitações de mesclagem

- Padrão: `true`
- Requer um valor

#### `--pull-requests-clone-parent-data`

Clonar os dados do ambiente pai para solicitações de pull

- Padrão: `true`
- Requer um valor

#### `--resync-pull-requests`

Sincronizar novamente os dados do ambiente de solicitação de pull em cada build

- Padrão: `false`
- Requer um valor

#### `--fetch-branches`

Buscar todas as ramificações remotas (como ambientes inativos)

- Padrão: `true`
- Requer um valor

#### `--prune-branches`

Excluir ramificações que não existem no local remoto

- Padrão: `true`
- Requer um valor

#### `--resources-init`

Os recursos a serem usados ao inicializar um novo serviço (&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- Requer um valor

#### `--url`

O endpoint do URL ou da API para a integração

- Requer um valor

#### `--shared-key`

Webhook: a chave secreta compartilhada JWS

- Requer um valor

#### `--file`

O nome de um arquivo local que contém o script a ser carregado

- Requer um valor

#### `--events`

Uma lista de eventos nos quais agir, por exemplo, environment.push

- Padrão: `*`
- Requer um valor

#### `--states`

Uma lista de estados nos quais agir; por exemplo, pendente, em_andamento, concluído

- Padrão: `complete`
- Requer um valor

#### `--environments`

As IDs de ambiente a serem incluídas

- Padrão: `*`
- Requer um valor

#### `--excluded-environments`

As IDs de ambiente a serem excluídos

- Padrão: `[]`
- Requer um valor

#### `--from-address`

[Opcional] Personalizar do endereço para emails de alerta

- Requer um valor

#### `--recipients`

Os endereços de email dos destinatários

- Padrão: `[]`
- Requer um valor

#### `--channel`

O canal do Slack

- Requer um valor

#### `--routing-key`

A chave de roteamento PagerDuty

- Requer um valor

#### `--category`

A categoria Lógica Sumo, usada para filtragem

- Requer um valor

#### `--index`

O índice do Splunk

- Requer um valor

#### `--sourcetype`

O tipo de origem do evento Splunk

- Requer um valor

#### `--protocol`

Protocolo de transporte do Syslog (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Padrão: `tls`
- Requer um valor

#### `--syslog-host`

Host Syslog relay/collector

- Requer um valor

#### `--syslog-port`

Porta de relé/coletor do Syslog

- Requer um valor

#### `--facility`

Instalação do Syslog

- Padrão: `1`
- Requer um valor

#### `--message-format`

Formato de mensagem do Syslog (&#39;rfc3164&#39; ou &#39;rfc5424&#39;)

- Padrão: `rfc5424`
- Requer um valor

#### `--auth-mode`

Modo de autenticação (&quot;prefix&quot; ou &quot;structured_data&quot;)

- Padrão: `prefix`
- Requer um valor

#### `--auth-token`

Token de autenticação

- Requer um valor

#### `--verify-tls`

Se a verificação do certificado HTTPS deve ser habilitada (recomendado)

- Padrão: `true`
- Requer um valor

#### `--header`

Cabeçalho(s) HTTP para usar em solicitações POST. Separe os nomes e os valores com dois pontos (:).

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `integration:delete`

```bash
magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Excluir uma integração de um projeto

### Argumentos

#### `id`

A ID de integração. Deixe em branco para escolher em uma lista.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `integration:get`

```bash
magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

Exibir detalhes de uma integração

### Argumentos

#### `id`

Uma ID de integração. Deixe em branco para escolher em uma lista.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade de integração a ser exibida

- Aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `integration:list`

```bash
magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Exibir uma lista de integrações de projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: id, resumo, tipo. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `integration:update`

```bash
magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Atualizar uma integração

### Argumentos

#### `id`

A ID da integração a ser atualizada

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--type`

O tipo de integração (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerDuty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- Requer um valor

#### `--base-url`

O URL de base da instalação do servidor

- Requer um valor

#### `--bitbucket-url`

O URL base da instalação do Servidor Bitbucket

- Requer um valor

#### `--username`

O nome de usuário do servidor Bitbucket

- Requer um valor

#### `--token`

Um token de autenticação ou acesso para a integração

- Requer um valor

#### `--key`

Uma chave de consumidor OAuth de Bitbucket

- Requer um valor

#### `--secret`

Um segredo do consumidor OAuth do Bitbucket

- Requer um valor

#### `--license-key`

A chave de licença dos logs do New Relic

- Requer um valor

#### `--server-project`

O projeto (por exemplo, &quot;namespace/repo&quot;)

- Requer um valor

#### `--repository`

O repositório a ser rastreado (por exemplo, &quot;proprietário/repositório&quot;)

- Requer um valor

#### `--build-merge-requests`

GitLab: criar solicitações de mesclagem como ambientes

- Padrão: `true`
- Requer um valor

#### `--build-pull-requests`

Criar cada solicitação de pull como um ambiente

- Padrão: `true`
- Requer um valor

#### `--build-draft-pull-requests`

Criar solicitações de pull de rascunho

- Padrão: `true`
- Requer um valor

#### `--build-pull-requests-post-merge`

Criar solicitações pull com base em seu estado pós-mesclagem

- Padrão: `false`
- Requer um valor

#### `--build-wip-merge-requests`

GitLab: criar solicitações de mesclagem WIP

- Padrão: `true`
- Requer um valor

#### `--merge-requests-clone-parent-data`

GitLab: clonar dados para solicitações de mesclagem

- Padrão: `true`
- Requer um valor

#### `--pull-requests-clone-parent-data`

Clonar os dados do ambiente pai para solicitações de pull

- Padrão: `true`
- Requer um valor

#### `--resync-pull-requests`

Sincronizar novamente os dados do ambiente de solicitação de pull em cada build

- Padrão: `false`
- Requer um valor

#### `--fetch-branches`

Buscar todas as ramificações remotas (como ambientes inativos)

- Padrão: `true`
- Requer um valor

#### `--prune-branches`

Excluir ramificações que não existem no local remoto

- Padrão: `true`
- Requer um valor

#### `--resources-init`

Os recursos a serem usados ao inicializar um novo serviço (&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- Requer um valor

#### `--url`

O endpoint do URL ou da API para a integração

- Requer um valor

#### `--shared-key`

Webhook: a chave secreta compartilhada JWS

- Requer um valor

#### `--file`

O nome de um arquivo local que contém o script a ser carregado

- Requer um valor

#### `--events`

Uma lista de eventos nos quais agir, por exemplo, environment.push

- Padrão: `*`
- Requer um valor

#### `--states`

Uma lista de estados nos quais agir; por exemplo, pendente, em_andamento, concluído

- Padrão: `complete`
- Requer um valor

#### `--environments`

As IDs de ambiente a serem incluídas

- Padrão: `*`
- Requer um valor

#### `--excluded-environments`

As IDs de ambiente a serem excluídos

- Padrão: `[]`
- Requer um valor

#### `--from-address`

[Opcional] Personalizar do endereço para emails de alerta

- Requer um valor

#### `--recipients`

Os endereços de email dos destinatários

- Padrão: `[]`
- Requer um valor

#### `--channel`

O canal do Slack

- Requer um valor

#### `--routing-key`

A chave de roteamento PagerDuty

- Requer um valor

#### `--category`

A categoria Lógica Sumo, usada para filtragem

- Requer um valor

#### `--index`

O índice do Splunk

- Requer um valor

#### `--sourcetype`

O tipo de origem do evento Splunk

- Requer um valor

#### `--protocol`

Protocolo de transporte do Syslog (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Padrão: `tls`
- Requer um valor

#### `--syslog-host`

Host Syslog relay/collector

- Requer um valor

#### `--syslog-port`

Porta de relé/coletor do Syslog

- Requer um valor

#### `--facility`

Instalação do Syslog

- Padrão: `1`
- Requer um valor

#### `--message-format`

Formato de mensagem do Syslog (&#39;rfc3164&#39; ou &#39;rfc5424&#39;)

- Padrão: `rfc5424`
- Requer um valor

#### `--auth-mode`

Modo de autenticação (&quot;prefix&quot; ou &quot;structured_data&quot;)

- Padrão: `prefix`
- Requer um valor

#### `--auth-token`

Token de autenticação

- Requer um valor

#### `--verify-tls`

Se a verificação do certificado HTTPS deve ser habilitada (recomendado)

- Padrão: `true`
- Requer um valor

#### `--header`

Cabeçalho(s) HTTP para usar em solicitações POST. Separe os nomes e os valores com dois pontos (:).

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `integration:validate`

```bash
magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

Validar uma integração existente

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### Argumentos

#### `id`

Uma ID de integração. Deixe em branco para escolher em uma lista.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `local:build`

```bash
magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

Criar o projeto atual localmente

### Argumentos

#### `app`

Especificar aplicativos a serem compilados

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--abslinks`, `-a`

Usar links absolutos

- Padrão: `false`
- Não aceita um valor

#### `--source`, `-s`

O diretório de origem. O padrão é a raiz do projeto atual.

- Requer um valor

#### `--destination`, `-d`

O destino, ao qual a raiz da Web de cada aplicativo será vinculada. Padrão: _www

- Requer um valor

#### `--copy`, `-c`

Copie para um diretório de build, em vez de criar symlinking a partir da origem

- Padrão: `false`
- Não aceita um valor

#### `--clone`

Usar o Git para clonar o HEAD atual no diretório de compilação

- Padrão: `false`
- Não aceita um valor

#### `--run-deploy-hooks`

Executar ganchos deploy e/ou post_deploy

- Padrão: `false`
- Não aceita um valor

#### `--no-clean`

Não remover compilações antigas

- Padrão: `false`
- Não aceita um valor

#### `--no-archive`

Não criar ou usar um arquivo morto de compilação

- Padrão: `false`
- Não aceita um valor

#### `--no-backup`

Não fazer backup da build anterior

- Padrão: `false`
- Não aceita um valor

#### `--no-cache`

Desativar armazenamento em cache

- Padrão: `false`
- Não aceita um valor

#### `--no-build-hooks`

Não executar ganchos pós-compilação

- Padrão: `false`
- Não aceita um valor

#### `--no-deps`

Não instalar as dependências de compilação localmente

- Padrão: `false`
- Não aceita um valor

#### `--working-copy`

Drush: use o Git para clonar um repositório de cada módulo Drupal em vez de simplesmente baixar uma versão

- Padrão: `false`
- Não aceita um valor

#### `--concurrency`

Drush: defina o número de projetos simultâneos que serão processados ao mesmo tempo

- Padrão: `4`
- Requer um valor

#### `--lock`

Drush: criar ou atualizar um arquivo de bloqueio (disponível somente com a versão 7+ do Drush)

- Padrão: `false`
- Não aceita um valor


## `local:dir`

```bash
magento-cloud dir [<subdir>]
```

Encontrar a raiz do projeto local

### Argumentos

#### `subdir`

O subdiretório a ser localizado (&quot;local&quot;, &quot;web&quot; ou &quot;compartilhado&quot;)

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `metrics:all`

```bash
magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Mostrar métricas de CPU, disco e memória para um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--bytes`, `-B`

Mostrar tamanhos em bytes

- Padrão: `false`
- Não aceita um valor

#### `--range`, `-r`

O intervalo de tempo. As métricas serão carregadas por essa duração até a hora de término (—para). Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo de 5m, máximo de 8h ou mais (dependendo do projeto), padrão de 10m.

- Requer um valor

#### `--interval`, `-i`

O intervalo. O padrão é uma divisão do intervalo. Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo 1 m.

- Requer um valor

#### `--to`

A hora final. O padrão é agora.

- Requer um valor

#### `--latest`, `-1`

Mostrar apenas o último ponto de dados único

- Padrão: `false`
- Não aceita um valor

#### `--service`, `-s`

Filtrar por nome de serviço ou aplicativo Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--type`

Filtrar por tipo de serviço (se — service não for fornecido). A versão não é necessária. Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: timestamp*, serviço*, cpu_percent*, mem_percent*, disk_percent*, tmp_disk_percent*, cpu_limit, cpu_used, disk_limit, disk_used, inodes_limit, inodes_percent, inodes_used, mem_limit, mem_used, tmp_disk_limit, tmp_disk_used, tmp_inodes_limit, tmp_inodes_percent, tmp_inodes_used, type (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `metrics:cpu`

```bash
magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Mostrar o uso de um ambiente do CPU

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--range`, `-r`

O intervalo de tempo. As métricas serão carregadas por essa duração até a hora de término (—para). Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo de 5m, máximo de 8h ou mais (dependendo do projeto), padrão de 10m.

- Requer um valor

#### `--interval`, `-i`

O intervalo. O padrão é uma divisão do intervalo. Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo 1 m.

- Requer um valor

#### `--to`

A hora final. O padrão é agora.

- Requer um valor

#### `--latest`, `-1`

Mostrar apenas o último ponto de dados único

- Padrão: `false`
- Não aceita um valor

#### `--service`, `-s`

Filtrar por nome de serviço ou aplicativo Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--type`

Filtrar por tipo de serviço (se — service não for fornecido). A versão não é necessária. Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: carimbo de data e hora*, serviço*, usado*, limite*, porcentagem*, tipo (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `metrics:disk-usage`

```bash
magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Mostrar o uso do disco de um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--bytes`, `-B`

Mostrar tamanhos em bytes

- Padrão: `false`
- Não aceita um valor

#### `--range`, `-r`

O intervalo de tempo. As métricas serão carregadas por essa duração até a hora de término (—para). Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo de 5m, máximo de 8h ou mais (dependendo do projeto), padrão de 10m.

- Requer um valor

#### `--interval`, `-i`

O intervalo. O padrão é uma divisão do intervalo. Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo 1 m.

- Requer um valor

#### `--to`

A hora final. O padrão é agora.

- Requer um valor

#### `--latest`, `-1`

Mostrar apenas o último ponto de dados único

- Padrão: `false`
- Não aceita um valor

#### `--service`, `-s`

Filtrar por nome de serviço ou aplicativo Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--type`

Filtrar por tipo de serviço (se — service não for fornecido). A versão não é necessária. Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--tmp`

Relatar uso de disco temporário (mostra colunas: timestamp, serviço, tmp_used, tmp_limit, tmp_percent, tmp_ipercent)

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: timestamp*, service*, used*, limit*, percent*, ipercent*, tmp_percent*, ilimit, iused, tmp_ilimit, tmp_ipercent, tmp_iused, tmp_limit, tmp_used, type (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `metrics:memory`

```bash
magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Mostra o uso de memória de um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--bytes`, `-B`

Mostrar tamanhos em bytes

- Padrão: `false`
- Não aceita um valor

#### `--range`, `-r`

O intervalo de tempo. As métricas serão carregadas por essa duração até a hora de término (—para). Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo de 5m, máximo de 8h ou mais (dependendo do projeto), padrão de 10m.

- Requer um valor

#### `--interval`, `-i`

O intervalo. O padrão é uma divisão do intervalo. Você pode especificar unidades: horas (h), minutos (m) ou segundos (s). Mínimo 1 m.

- Requer um valor

#### `--to`

A hora final. O padrão é agora.

- Requer um valor

#### `--latest`, `-1`

Mostrar apenas o último ponto de dados único

- Padrão: `false`
- Não aceita um valor

#### `--service`, `-s`

Filtrar por nome de serviço ou aplicativo Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--type`

Filtrar por tipo de serviço (se — service não for fornecido). A versão não é necessária. Os caracteres % ou * podem ser usados como curinga.

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: carimbo de data e hora*, serviço*, usado*, limite*, porcentagem*, tipo (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `mount:download`

```bash
magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Baixar arquivos de uma montagem, usando o &#39;rsync&#39;

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--all`, `-a`

Baixar de todas as montagens

- Padrão: `false`
- Não aceita um valor

#### `--mount`, `-m`

A montagem (como um caminho relativo ao aplicativo)

- Requer um valor

#### `--target`

O diretório no qual os arquivos serão baixados. Se — all for usado, o caminho de montagem será anexado

- Requer um valor

#### `--source-path`

Use o caminho de origem da montagem (em vez do caminho de montagem) como um subdiretório do destino, quando — all for usado

- Padrão: `false`
- Não aceita um valor

#### `--delete`

Se os arquivos irrelevantes devem ser excluídos no diretório de destino

- Padrão: `false`
- Não aceita um valor

#### `--exclude`

Arquivos a serem excluídos do download (padrão)

- Padrão: `[]`
- Requer um valor

#### `--include`

Arquivo(s) não a excluir (padrão)

- Padrão: `[]`
- Requer um valor

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `mount:list`

```bash
magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Obter uma lista de montagens

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--paths`

Saída somente dos caminhos de montagem (um por linha)

- Padrão: `false`
- Não aceita um valor

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: definição, caminho. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor


## `mount:size`

```bash
magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Verificar o uso de montagens no disco

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--bytes`, `-B`

Mostrar tamanhos em bytes

- Padrão: `false`
- Não aceita um valor

#### `--refresh`

Atualizar o cache

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: disponível, máx, montagens, percent_used, tamanhos, usados. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor


## `mount:upload`

```bash
magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Carregar arquivos para uma montagem, usando o &#39;rsync&#39;

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--source`

Um diretório contendo arquivos para fazer upload

- Requer um valor

#### `--mount`, `-m`

A montagem (como um caminho relativo ao aplicativo)

- Requer um valor

#### `--delete`

Se os arquivos irrelevantes devem ser excluídos na montagem

- Padrão: `false`
- Não aceita um valor

#### `--exclude`

Arquivos a serem excluídos do upload (padrão)

- Padrão: `[]`
- Requer um valor

#### `--include`

Arquivo(s) não a excluir (padrão)

- Padrão: `[]`
- Requer um valor

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--instance`, `-I`

Uma ID de instância

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `operation:list`

```bash
magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Operações de tempo de execução de lista do Beta em um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--full`

Não limite o comprimento do comando a ser exibido. O limite padrão é de 24 linhas.

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: service*, name*, start*, role, stop (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `operation:run`

```bash
magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

BETA Execute uma operação no ambiente

### Argumentos

#### `operation`

O nome da operação

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--worker`

Um nome de trabalhador

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `project:clear-build-cache`

```bash
magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

Limpar o cache de criação de um projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `project:get`

```bash
magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

Clonar um projeto localmente

### Argumentos

#### `project`

A ID do projeto


#### `directory`

O diretório para o qual clonar. O padrão é o título do projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--environment`, `-e`

A ID de ambiente a ser clonada. O padrão é o padrão do projeto ou o primeiro ambiente disponível

- Requer um valor

#### `--depth`

Criar um clone superficial: limite o número de confirmações no histórico

- Requer um valor

#### `--build`

Criar o projeto após a clonagem

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `project:info`

```bash
magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Ler ou definir propriedades de um projeto

### Argumentos

#### `property`

O nome da propriedade


#### `value`

Definir um novo valor para a propriedade

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `project:list`

```bash
magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Obter uma lista de todos os projetos ativos

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--pipe`

Gerar uma lista simples de IDs de projeto. Desativa a paginação.

- Padrão: `false`
- Não aceita um valor

#### `--region`

Filtrar por região (correspondência exata)

- Requer um valor

#### `--title`

Filtrar por título (pesquisa que não diferencia maiúsculas de minúsculas)

- Requer um valor

#### `--my`

Exibir somente seus projetos

- Padrão: `false`
- Não aceita um valor

#### `--refresh`

Atualizar ou não a lista

- Padrão: `1`
- Requer um valor

#### `--sort`

Uma propriedade para classificar por

- Padrão: `title`
- Requer um valor

#### `--reverse`

Classificar em ordem inversa (descendente)

- Padrão: `false`
- Não aceita um valor

#### `--page`

Número da página. Isto ativa a paginação, apesar da configuração ou — count. Ignorado se o — pipe for indicado.

- Requer um valor

#### `--count`, `-c`

O número de projetos a serem exibidos por página. Use 0 para desativar a paginação. Ignorado se o — page for especificado.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`

Colunas a serem exibidas. Colunas disponíveis: id*, título*, região*, created_at, organization_id, organization_label, organization_name, status (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `project:set-remote`

```bash
magento-cloud set-remote [<project>]
```

Definir o projeto remoto para o repositório Git atual

### Argumentos

#### `project`

A ID do projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `repo:cat`

```bash
magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

Ler um arquivo no repositório do projeto

### Argumentos

#### `path`

O caminho para o arquivo

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--commit`, `-c`

O SHA de confirmação. Isso também pode aceitar os sufixos &quot;HEAD&quot; e acento circunflexo (^) ou til (~) para confirmações principais.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `repo:ls`

```bash
magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Listar arquivos no repositório do projeto

### Argumentos

#### `path`

O caminho para um subdiretório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--directories`, `-d`

Mostrar somente diretórios

- Padrão: `false`
- Não aceita um valor

#### `--files`, `-f`

Mostrar somente arquivos

- Padrão: `false`
- Não aceita um valor

#### `--git-style`

Saída de estilo semelhante a &quot;git ls-tree&quot;

- Padrão: `false`
- Não aceita um valor

#### `--commit`, `-c`

O SHA de confirmação. Isso também pode aceitar os sufixos &quot;HEAD&quot; e acento circunflexo (^) ou til (~) para confirmações principais.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `repo:read`

```bash
magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Ler um diretório ou arquivo no repositório do projeto

### Argumentos

#### `path`

O caminho para o diretório ou arquivo

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--commit`, `-c`

O SHA de confirmação. Isso também pode aceitar os sufixos &quot;HEAD&quot; e acento circunflexo (^) ou til (~) para confirmações principais.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `route:get`

```bash
magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

Exibir informações detalhadas sobre um roteiro

### Argumentos

#### `route`

O URL original da rota

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--id`

Uma ID de rota para selecionar

- Requer um valor

#### `--primary`, `-1`

Selecionar a rota principal

- Padrão: `false`
- Não aceita um valor

#### `--property`, `-P`

A propriedade a ser exibida

- Requer um valor

#### `--refresh`

Ignorar o cache de rotas

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

[Opção obsoleta, não é mais usada]

- Requer um valor

#### `--identity-file`, `-i`

[Opção obsoleta, não é mais usada]

- Requer um valor


## `route:list`

```bash
magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

Listar todas as rotas de um ambiente

### Argumentos

#### `environment`

A ID do ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Ignorar o cache de rotas

- Padrão: `false`
- Não aceita um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: route*, type*, to*, url (* = default columns). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `self:install`

```bash
magento-cloud self:install [--shell-type SHELL-TYPE]
```

Instalar ou atualizar arquivos de configuração da CLI

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--shell-type`

O tipo de shell para preenchimento automático (bash ou zsh)

- Requer um valor


## `self:update`

```bash
magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

Atualize a CLI para a versão mais recente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--no-major`

Atualizar somente entre versões secundárias ou de patch

- Padrão: `false`
- Não aceita um valor

#### `--unstable`

Atualização para uma nova versão instável, se disponível

- Padrão: `false`
- Não aceita um valor

#### `--manifest`

Substituir o local do arquivo de manifesto

- Requer um valor

#### `--current-version`

Substituir a versão atual

- Requer um valor

#### `--timeout`

Um tempo limite para a verificação da versão

- Padrão: `30`
- Requer um valor


## `service:list`

```bash
magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Listar serviços no projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--pipe`

Gerar uma lista apenas de nomes de serviço

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: disco, nome, tamanho, tipo. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `service:mongo:dump`

```bash
magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Criar um despejo de arquivo binário de dados do MongoDB

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--collection`, `-c`

A coleção para despejo

- Requer um valor

#### `--gzip`, `-z`

Compactar o despejo usando gzip

- Padrão: `false`
- Não aceita um valor

#### `--stdout`, `-o`

Saída para STDOUT em vez de um arquivo

- Padrão: `false`
- Não aceita um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `service:mongo:export`

```bash
magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Exportar dados do MongoDB

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--collection`, `-c`

A coleção a ser exportada

- Requer um valor

#### `--jsonArray`

Exportar dados como uma única matriz JSON

- Padrão: `false`
- Não aceita um valor

#### `--type`

O tipo de exportação, por exemplo, &quot;csv&quot;

- Requer um valor

#### `--fields`, `-f`

Os campos a serem exportados

- Padrão: `[]`
- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `service:mongo:restore`

```bash
magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Restaurar um despejo de arquivo binário de dados no MongoDB

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--collection`, `-c`

A coleção a restaurar

- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `service:mongo:shell`

```bash
magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Usar o shell MongoDB

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--eval`

Passar um fragmento do JavaScript para o shell

- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `service:redis-cli`

```bash
magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

Acesse a CLI Redis

### Argumentos

#### `args`

Argumentos a serem adicionados ao comando Redis

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `snapshot:create`

```bash
magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Criar um instantâneo de um ambiente

### Argumentos

#### `environment`

O ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--live`

Live backup: não interrompa o ambiente. Se definido, o ambiente estará em execução e aberto a conexões durante o backup. Isso reduz o tempo de inatividade, correndo o risco de fazer backup dos dados em um estado inconsistente.

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `snapshot:delete`

```bash
magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

Excluir um instantâneo do ambiente

### Argumentos

#### `id`

A ID do instantâneo. Necessário no modo não interativo.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `snapshot:get`

```bash
magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

Exibir um instantâneo do ambiente

### Argumentos

#### `id`

A ID do instantâneo. O padrão é o mais recente.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade a ser exibida.

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor


## `snapshot:list`

```bash
magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Listar instantâneos disponíveis de um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `snapshot:restore`

```bash
magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

Restaurar um instantâneo do ambiente

### Argumentos

#### `snapshot`

O nome do snapshot. O padrão é o mais recente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--target`

O ambiente para o qual restaurar. Define como padrão o ambiente atual do snapshot

- Requer um valor

#### `--branch-from`

Se o — target ainda não existir, isto especifica o pai do novo ambiente

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `source-operation:list`

```bash
magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Listar operações de origem em um ambiente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--full`

Não limite o comprimento do comando a ser exibido. O limite padrão é de 24 linhas.

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: aplicativo, comando, operação. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `source-operation:run`

```bash
magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

Executar uma operação de origem

### Argumentos

#### `operation`

O nome da operação

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--variable`

Uma variável a ser definida durante a operação, no formato type:name=value

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `ssh-cert:load`

```bash
magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

Gerar um certificado SSH

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh-only`

Somente atualize o certificado, se necessário (não grave a configuração SSH)

- Padrão: `false`
- Não aceita um valor

#### `--new`

Forçar a atualização do certificado

- Padrão: `false`
- Não aceita um valor

#### `--new-key`

[Obsoleto] Use — new

- Padrão: `false`
- Não aceita um valor


## `ssh-key:add`

```bash
magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

Adicionar uma nova chave SSH

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argumentos

#### `path`

O caminho para uma chave pública SSH existente

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--name`

Um nome para identificar a chave

- Requer um valor


## `ssh-key:delete`

```bash
magento-cloud ssh-key:delete [<id>]
```

Excluir uma chave SSH

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argumentos

#### `id`

A ID da chave SSH a ser excluída

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `ssh-key:list`

```bash
magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Obter uma lista de chaves SSH em sua conta

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: id*, title*, path*, impressão digital (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `subscription:info`

```bash
magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

Ler ou modificar propriedades de assinatura

### Argumentos

#### `property`

O nome da propriedade


#### `value`

Definir um novo valor para a propriedade

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--id`, `-s`

A ID da assinatura

- Requer um valor

#### `--date-fmt`

O formato de data (como uma string de formato de data do PHP)

- Padrão: `c`
- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `tunnel:close`

```bash
magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Fechar túneis SSH

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--all`, `-a`

Fechar todos os túneis

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `tunnel:info`

```bash
magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Exibir informações de relacionamento para túneis SSH

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

A propriedade de relacionamento a ser exibida

- Requer um valor

#### `--encode`, `-c`

Saída como JSON codificado em base64

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor


## `tunnel:list`

```bash
magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Listar túneis SSH

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--all`, `-a`

Exibir todos os túneis

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: porta*, projeto*, ambiente*, aplicativo*, relacionamento*, url (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor


## `tunnel:open`

```bash
magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Abrir túneis SSH para os relacionamentos de um aplicativo

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--gateway-ports`, `-g`

Permitir que hosts remotos se conectem a portas encaminhadas locais

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `tunnel:single`

```bash
magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Abrir um único túnel SSH para uma relação de aplicativo

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--port`

A porta local

- Requer um valor

#### `--gateway-ports`, `-g`

Permitir que hosts remotos se conectem a portas encaminhadas locais

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--app`, `-A`

O nome do aplicativo remoto

- Requer um valor

#### `--relationship`, `-r`

O relacionamento de serviço a ser usado

- Requer um valor

#### `--identity-file`, `-i`

Uma identidade SSH (chave privada) para usar

- Requer um valor


## `user:add`

```bash
magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Adicionar um usuário ao projeto

### Argumentos

#### `email`

O endereço de email do usuário

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--role`, `-r`

A função do projeto do usuário (&quot;administrador&quot; ou &quot;visualizador&quot;) ou a função do tipo de ambiente (por exemplo, &quot;preparo:colaborador&quot; ou &quot;produção:visualizador&quot;). Para remover um usuário de um tipo de ambiente, defina a função como &quot;nenhum&quot;. Os caracteres % ou * podem ser usados como curinga para o tipo de ambiente, por exemplo, &#39;%:viewer&#39; para conceder ao usuário a função &#39;viewer&#39; em todos os tipos. A função pode ser abreviada. Por exemplo, &quot;produção:v&quot;.

- Padrão: `[]`
- Requer um valor

#### `--force-invite`

Enviar um convite, mesmo que um já tenha sido enviado

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `user:delete`

```bash
magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

Excluir um usuário do projeto

### Argumentos

#### `email`

O endereço de email do usuário

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `user:get`

```bash
magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

Exibir as funções de um usuário

### Argumentos

#### `email`

O endereço de email do usuário

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--level`, `-l`

O nível de função (&quot;projeto&quot; ou &quot;ambiente&quot;)

- Requer um valor

#### `--pipe`

Transmitir a função para stdout (após fazer quaisquer alterações)

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor

#### `--role`, `-r`

[Obsoleto: use user:update para alterar a(s) função(ões) de um usuário]

- Requer um valor


## `user:list`

```bash
magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Listar usuários do projeto

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: email*, nome*, função*, id*, granted_at, updated_at (* = colunas padrão). O caractere &quot;+&quot; pode ser usado como um espaço reservado para as colunas padrão. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor


## `user:update`

```bash
magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Atualizar funções de usuário em um projeto

### Argumentos

#### `email`

O endereço de email do usuário

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--role`, `-r`

A função do projeto do usuário (&quot;administrador&quot; ou &quot;visualizador&quot;) ou a função do tipo de ambiente (por exemplo, &quot;preparo:colaborador&quot; ou &quot;produção:visualizador&quot;). Para remover um usuário de um tipo de ambiente, defina a função como &quot;nenhum&quot;. Os caracteres % ou * podem ser usados como curinga para o tipo de ambiente, por exemplo, &#39;%:viewer&#39; para conceder ao usuário a função &#39;viewer&#39; em todos os tipos. A função pode ser abreviada. Por exemplo, &quot;produção:v&quot;.

- Padrão: `[]`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `variable:create`

```bash
magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

Criar uma variável

### Argumentos

#### `name`

O nome da variável

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--update`, `-u`

Atualizar a variável se ela já existir

- Padrão: `false`
- Não aceita um valor

#### `--level`, `-l`

O nível no qual a variável será definida (&quot;projeto&quot; ou &quot;ambiente&quot;)

- Requer um valor

#### `--name`

O nome da variável

- Requer um valor

#### `--value`

O valor da variável

- Requer um valor

#### `--json`

Se o valor da variável é formatado em JSON

- Padrão: `false`
- Requer um valor

#### `--sensitive`

Se o valor da variável é sensível

- Padrão: `false`
- Requer um valor

#### `--prefix`

O prefixo do nome da variável que pode determinar seu tipo, por exemplo, &quot;env&quot;. Aplicável somente se o nome ainda não contiver um prefixo. (por exemplo, &quot;nenhum&quot; ou &quot;env:&quot;)

- Padrão: `none`
- Requer um valor

#### `--enabled`

Se a variável deve ser ativada no ambiente

- Padrão: `true`
- Requer um valor

#### `--inheritable`

Se a variável é herdável por ambientes secundários

- Padrão: `true`
- Requer um valor

#### `--visible-build`

Se a variável deve estar visível no momento da criação

- Requer um valor

#### `--visible-runtime`

Se a variável deve estar visível no tempo de execução

- Padrão: `true`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `variable:delete`

```bash
magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Excluir uma variável

### Argumentos

#### `name`

O nome da variável

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--level`, `-l`

O nível da variável (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `variable:get`

```bash
magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

Exibir uma variável

### Argumentos

#### `name`

O nome da variável

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--property`, `-P`

Exibir uma única propriedade de variável

- Requer um valor

#### `--level`, `-l`

O nível da variável (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--pipe`

[Opção obsoleta] Gera apenas o valor da variável

- Padrão: `false`
- Não aceita um valor


## `variable:list`

```bash
magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Variáveis da lista

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--level`, `-l`

O nível da variável (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: is_enabled, level, name, value. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor


## `variable:update`

```bash
magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Atualizar uma variável

### Argumentos

#### `name`

O nome da variável

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--allow-no-change`

Retorno com êxito (código de saída zero) se nenhuma alteração for fornecida

- Padrão: `false`
- Não aceita um valor

#### `--level`, `-l`

O nível da variável (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requer um valor

#### `--value`

O valor da variável

- Requer um valor

#### `--json`

Se o valor da variável é formatado em JSON

- Padrão: `false`
- Requer um valor

#### `--sensitive`

Se o valor da variável é sensível

- Padrão: `false`
- Requer um valor

#### `--enabled`

Se a variável deve ser ativada no ambiente

- Padrão: `true`
- Requer um valor

#### `--inheritable`

Se a variável é herdável por ambientes secundários

- Padrão: `true`
- Requer um valor

#### `--visible-build`

Se a variável deve estar visível no momento da criação

- Requer um valor

#### `--visible-runtime`

Se a variável deve estar visível no tempo de execução

- Padrão: `true`
- Requer um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--no-wait`, `-W`

Não aguarde a conclusão da operação

- Padrão: `false`
- Não aceita um valor

#### `--wait`

Aguardar a conclusão da operação (padrão)

- Padrão: `false`
- Não aceita um valor


## `worker:list`

```bash
magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Obter uma lista de todos os trabalhadores implantados

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--refresh`

Se o cache deve ser atualizado

- Padrão: `false`
- Não aceita um valor

#### `--pipe`

Gerar uma lista somente de nomes de trabalhadores

- Padrão: `false`
- Não aceita um valor

#### `--project`, `-p`

A ID ou URL do projeto

- Requer um valor

#### `--environment`, `-e`

A ID do ambiente. Use &quot;.&quot; para selecionar o ambiente padrão do projeto.

- Requer um valor

#### `--format`

O formato de saída: table, csv, tsv ou plain

- Padrão: `table`
- Requer um valor

#### `--columns`, `-c`

Colunas a serem exibidas. Colunas disponíveis: comandos, nome, tipo. Os caracteres % ou * podem ser usados como curinga. Os valores podem ser divididos por vírgulas (por exemplo, &quot;a,b,c&quot;) e/ou espaços em branco.

- Padrão: `[]`
- Requer um valor

#### `--no-header`

Não gerar saída do cabeçalho da tabela

- Padrão: `false`
- Não aceita um valor
