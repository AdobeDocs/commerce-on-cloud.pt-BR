---
source-git-commit: 69b764a03e6c272498e57645dca40d29c4e79626
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 3%

---
# ece-tools

**Versão**: 2002.2.7

Esta referência contém 34 comandos disponíveis através da ferramenta de linha de comando `ece-tools`.
A lista inicial é gerada automaticamente usando o comando `ece-tools list` no Adobe Commerce na infraestrutura em nuvem.

## Geral

Essa referência é gerada a partir da base de código do aplicativo. Para alterar o conteúdo, _Dê-nos seu feedback_ (localize o link no canto superior direito). Para obter as diretrizes de contribuição, consulte [Contribuições de código](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Opções globais

#### `--help`, `-h`

Exibe a ajuda para o comando fornecido. Quando nenhum comando é fornecido, exibir ajuda para o comando de lista

- Padrão: `false`
- Não aceita um valor

#### `--silent`

Não enviar nenhuma mensagem

- Padrão: `false`
- Não aceita um valor

#### `--quiet`, `-q`

Somente erros são exibidos. Todas as outras saídas são suprimidas

- Padrão: `false`
- Não aceita um valor

#### `--verbose`, `-v|-vv|-vvv`

Aumentar a verbosidade das mensagens: 1 para saída normal, 2 para saída mais detalhada e 3 para depuração

- Padrão: `false`
- Não aceita um valor

#### `--version`, `-V`

Exibir esta versão do aplicativo

- Padrão: `false`
- Não aceita um valor

#### `--ansi`

Forçar (ou desativar — no- ansi) a saída ANSI

- Não aceita um valor

#### `--no-ansi`

Negar a opção &quot;—ansi&quot;

- Não aceita um valor

#### `--no-interaction`, `-n`

Não faça nenhuma pergunta interativa

- Padrão: `false`
- Não aceita um valor


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

Comando interno para fornecer sugestões de conclusão de shell

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--shell`, `-s`

O tipo de concha (&quot;bash&quot;, &quot;fish&quot;, &quot;zsh&quot;)

- Requer um valor

#### `--input`, `-i`

Uma matriz de tokens de entrada (por exemplo, COMP_WORDS ou argv)

- Padrão: `[]`
- Requer um valor

#### `--current`, `-c`

O índice da matriz &quot;input&quot; na qual o cursor está (por exemplo, COMP_CWORD)

- Requer um valor

#### `--api-version`, `-a`

A versão da API do script de conclusão

- Requer um valor

#### `--symfony`, `-S`

obsoleto

- Requer um valor


## `build`

```bash
ece-tools build
```

Compila o aplicativo.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

Despejar o script de conclusão do shell

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### Argumentos

#### `shell`

O tipo de shell (por exemplo, &quot;bash&quot;), o valor do var env &quot;$SHELL&quot; será usado se isso não for fornecido

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--debug`

Analisar o log de depuração da conclusão

- Padrão: `false`
- Não aceita um valor


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

Cria backups do banco de dados.

### Argumentos

#### `databases`

Bancos de dados para backup. Valores Disponíveis: [vendas da cotação principal]. Se o valor do argumento não for especificado, os backups do banco de dados serão criados usando as credenciais armazenadas na variável de ambiente `MAGENTO_CLOUD_RELATIONSHIP` ou/e a propriedade `stage.deploy.DATABASE_CONFIGURATION` do arquivo de configuração .magento.env.yaml.

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--remove-definers`, `-d`

Remover definidores do dump do banco de dados

- Padrão: `false`
- Não aceita um valor

#### `--dump-directory`, `-a`

Usar diretório alternativo para salvar o despejo

- Requer um valor


## `deploy`

```bash
ece-tools deploy
```

Implanta o aplicativo.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

Exibir a ajuda de um comando

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### Argumentos

#### `command_name`

O nome do comando

- Padrão: `help`

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--format`

O formato de saída (txt, xml, json ou md)

- Padrão: `txt`
- Requer um valor

#### `--raw`

Para gerar a ajuda do comando raw

- Padrão: `false`
- Não aceita um valor


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

Listar comandos

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### Argumentos

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

#### `--short`

Para ignorar a descrição dos argumentos dos comandos

- Padrão: `false`
- Não aceita um valor


## `patch`

```bash
ece-tools patch
```

Aplica patches personalizados.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `post-deploy`

```bash
ece-tools post-deploy
```

Executa operações após a implantação.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `run`

```bash
ece-tools run <scenario>...
```

Executar cenário(s).

### Argumentos

#### `scenario`

Cenário(s)

- Padrão: `[]`
- Obrigatório

- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `backup:list`

```bash
ece-tools backup:list
```

Mostra a lista de arquivos de backup.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

Restaure arquivos de configuração importantes. Execute o backup :list para mostrar a lista de arquivos de backup.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--force`, `-f`

Substituir arquivos existentes durante a restauração de um backup

- Padrão: `false`
- Não aceita um valor

#### `--file`

Um caminho de recuperação de arquivo específico

- Aceita um valor


## `build:generate`

```bash
ece-tools build:generate
```

Gera todos os arquivos necessários para o estágio de criação.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `build:transfer`

```bash
ece-tools build:transfer
```

Transfere os arquivos gerados para o diretório init.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

Cria um arquivo `.magento.env.yaml` com a configuração de variável de compilação, implantação e pós-implantação especificada. Substitui qualquer arquivo `.magento.env.yaml` existente.

### Argumentos

#### `configuration`

Configuração no formato JSON

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

Atualiza o arquivo `.magento.env.yaml` existente com a configuração especificada. Cria o arquivo `.magento.env.yaml` se ele não existir.

### Argumentos

#### `configuration`

Configuração no formato JSON

- Obrigatório

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

Valida o arquivo de configuração `.magento.env.yaml`

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `config:dump`

```bash
ece-tools config:dumpdump
```

Configuração de despejo para implantação de conteúdo estático.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cron:disable`

```bash
ece-tools cron:disable
```

Desative todos os processos cron do Magento e encerre todos os processos em execução.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cron:enable`

```bash
ece-tools cron:enable
```

Habilita os processos cron do Magento.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cron:kill`

```bash
ece-tools cron:kill
```

Encerra todos os processos cron do Magento.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

Desbloqueie trabalhos cron que ficaram no estado &quot;em execução&quot;.

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--job-code`

Código do trabalho do Cron a ser desbloqueado.

- Padrão: `[]`
- Aceita vários valores


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

Gera o arquivo dist/error-codes.md do arquivo schema.error.yaml.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Atualiza o compositor para implantação do Git.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

Exibir variáveis de ambiente de configuração de nuvem codificadas.

### Argumentos

#### `variable`

Variáveis de ambiente a serem exibidas, opções possíveis: serviços, rotas, variáveis

- Padrão: `[]`
- Matriz

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

Exibe informações sobre o erro por id de erro ou informações sobre todos os erros da última implantação.

### Argumentos

#### `error-code`

Código de erro, se não for passado o comando exibir informações sobre todos os erros da última implantação

### Opções

Para opções globais, consulte [Opções globais](#global-options).

#### `--json`, `-j`

Usado para obter resultado no formato JSON

- Padrão: `false`
- Não aceita um valor


## `module:refresh`

```bash
ece-tools module:refresh
```

Atualiza a configuração para ativar módulos recém-adicionados.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `schema:generate`

```bash
ece-tools schema:generate
```

Gera o arquivo *.dist do esquema.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

Verifica o estado ideal de configuração.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

Verifica a configuração do master-slave.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

Verifica o SCD na configuração da build.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

Verifica a configuração de SCD por demanda.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

Verifica o SCD na configuração de implantação.

### Opções

Para opções globais, consulte [Opções globais](#global-options).


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

Verifica a capacidade de dividir o banco de dados e se o banco de dados já foi dividido ou não.

### Opções

Para opções globais, consulte [Opções globais](#global-options).
