---
title: Implantar para preparo e produção
description: Saiba como implantar seu Adobe Commerce no código de infraestrutura em nuvem nos ambientes de preparo e produção para testes adicionais.
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 1cfeb472-c6ec-44ff-9b32-516ffa1b30d2
source-git-commit: fe634412c6de8325faa36c07e9769cde0eb76c48
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%

---

# Implantar para preparo e produção

O processo de implantação e ativação começa com o desenvolvimento, continua com o armazenamento temporário e termina com a ativação na produção. A Adobe fornece uma solução completa de ambiente para garantir configurações consistentes. Todo ambiente suporta acesso direto de URL à loja e acesso de Admin e SSH para comandos da CLI.

Quando estiver pronto para implantar seu armazenamento, você deve concluir a implantação e o teste no ambiente de preparo antes de implantar na produção. Esta seção fornece instruções e informações detalhadas sobre o processo de criação e implantação, migração de dados e conteúdo e testes.

>[!TIP]
>
>A Adobe recomenda criar um [backup](../storage/snapshots.md) do ambiente antes das implantações.

Além disso, você pode habilitar o [Rastrear implantações com o New Relic](../monitor/track-deployments.md) para monitorar eventos de implantação e ajudar a analisar o desempenho entre implantações.

## Fluxo de implantação inicial

A Adobe recomenda criar uma ramificação `staging` a partir da ramificação `master` para oferecer melhor suporte ao desenvolvimento e à implantação do seu plano inicial. Você tem dois dos seus quatro ambientes ativos prontos: `master` para produção e `staging` para preparo.

Para obter informações detalhadas do processo, consulte [Iniciar Desenvolvimento e Implantação de Fluxo de Trabalho](../architecture/starter-develop-deploy-workflow.md).

## Fluxo de implantação Pro

O Pro vem com um grande ambiente de integração com duas ramificações ativas, uma ramificação global `master`, ramificações de Preparo e Produção. Quando você cria seu projeto, o código está pronto para ramificar, desenvolver e enviar para a criação e implantação do site. Embora o ambiente de integração possa ter muitas ramificações, o ambiente de preparo e produção têm apenas uma ramificação para cada ambiente.

Para obter informações detalhadas do processo, consulte [Desenvolver e implantar fluxo de trabalho](../architecture/pro-develop-deploy-workflow.md).

## Implantar código para preparo

O ambiente de preparo fornece um ambiente de quase produção que inclui um banco de dados, um servidor da Web e todos os serviços, inclusive o Fastly e o New Relic. Você pode enviar, mesclar e implantar totalmente por meio dos [[!DNL Cloud Console]](../project/overview.md) ou dos [comandos da CLI da nuvem](../dev-tools/cloud-cli-overview.md) por meio de um aplicativo de terminal.

### Implantar código com o [!DNL Cloud Console]

O [!DNL Cloud Console] fornece recursos para criar, gerenciar e implantar código em ambientes de Integração, Preparo e Produção para planos Starter e Pro.

**Para projetos Pro, implante a ramificação de integração no preparo**:

1. [Faça logon](https://accounts.magento.cloud) no seu projeto.
1. Selecione a ramificação `integration`.
1. Selecione a opção **Mesclar** para implantar em Preparo.

   ![Mesclar](../../assets/button-merge.png){width="150"}

1. Conclua todos os [testes](../test/staging-and-production.md) no ambiente de preparo.
1. Quando o Preparo estiver pronto, selecione a opção **Mesclar** para implantar na Produção.

**Para Iniciante, implante a ramificação de desenvolvimento no preparo**:

1. [Faça logon](https://accounts.magento.cloud) no seu projeto.
1. Selecione a ramificação do código preparado.
1. Selecione a opção **Mesclar** para implantar em Preparo.

   ![Mesclar](../../assets/button-merge.png){width="150"}

1. Conclua todos os [testes](../test/staging-and-production.md) no ambiente de preparo.
1. Quando o Preparo estiver pronto, selecione a opção **Mesclar** para implantar na Produção (`master`).

### Implantar código com a linha de comando

A CLI da nuvem fornece comandos para implantar código. Você precisa de acesso SSH e Git para o seu projeto.

#### Etapa 1: implantar e testar o ambiente de integração

1. Depois de fazer logon no projeto, verifique o ambiente de integração:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Sincronizar o ambiente de integração local com o ambiente remoto:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Crie um instantâneo do ambiente como um backup:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Atualize o código na sua ramificação local, conforme necessário.

1. Adicionar, confirmar e enviar alterações para o ambiente.

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. Concluir o teste no local.

#### Etapa 2: mesclar alterações no armazenamento temporário e implantar

1. Confira o Ambiente de preparo:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Sincronizar o ambiente de preparo local com o ambiente remoto:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Crie um instantâneo do ambiente como um backup:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Mescle o ambiente de integração com o ambiente de preparo para implantar:

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. Concluir o teste no local.

#### Etapa 3: implantar para produção

1. Confira, sincronize e crie um instantâneo do seu ambiente de produção local.

1. Mescle o ambiente de preparo com a produção para implantar:

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. Concluir o teste no local.

## Migrar arquivos estáticos

[Arquivos estáticos](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/implementation-playbook/glossary) são armazenados em `mounts`. Há dois métodos para migrar arquivos de um local de montagem de origem, como o ambiente local, para um local de montagem de destino. Ambos os métodos usam o utilitário `rsync`, mas a Adobe recomenda usar a CLI `magento-cloud` para mover arquivos entre os ambientes local e remoto. E a Adobe recomenda usar o método `rsync` ao mover arquivos de uma origem remota para um local remoto diferente.

### Migrar arquivos usando a CLI

Você pode usar os comandos da CLI do `mount:upload` e do `mount:download` para migrar arquivos entre os ambientes local e remoto. Ambos os comandos usam o utilitário `rsync`, mas os comandos da CLI fornecem opções e prompts personalizados para o ambiente da Adobe Commerce na infraestrutura em nuvem. Por exemplo, se você usar o comando simples sem opções, a CLI solicitará que você selecione a montagem ou as montagens para upload ou download.

```bash
magento-cloud mount:download
```

Exemplo de resposta:

```
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**Para carregar arquivos de uma pasta local `pub/media/` para a pasta remota `pub/media/` do ambiente atual**:

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

Exemplo de resposta:

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

Use a opção `--help` para os comandos `mount:upload` e `mount:download` para ver mais opções. Por exemplo, há uma opção `--delete` para remover arquivos irrelevantes durante a migração.

### Migrar arquivos usando rsync

Como alternativa, você pode usar o utilitário `rsync` para migrar arquivos.

```bash
rsync -azvP <source> <destination>
```

Esse comando usa as seguintes opções:

- `a`-arquivo
- `z` - compactar arquivos durante a migração
- `v`-detalhado
- `P` progresso parcial

Consulte a ajuda do [rsync](https://linux.die.net/man/1/rsync).

>[!NOTE]
>
>Para transferir mídia diretamente de ambientes remotos para remotos, você deve habilitar o encaminhamento do agente SSH, consulte [orientação do GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Para migrar arquivos estáticos de ambientes remotos para remotos diretamente (abordagem rápida)**:

1. Use SSH para fazer logon no ambiente de origem. Não use a CLI do `magento-cloud`. O uso da opção `-A` é importante porque ela habilita o encaminhamento da conexão do agente de autenticação.

   >[!TIP]
   >
   >Para localizar o link **Acesso ao SSH** em seu [!DNL Cloud Console], selecione o ambiente e clique em **Acessar Site**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Use o comando `rsync` para copiar o diretório `pub/media` do seu ambiente de origem para um ambiente remoto diferente.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Faça logon em outro ambiente remoto para verificar se os arquivos foram migrados com êxito.

## Migrar o banco de dados

>[!WARNING]
>
>As operações de importação e exportação de banco de dados podem demorar muito, o que pode afetar o desempenho e a disponibilidade do site. Agende operações de importação e exportação fora do horário de pico para evitar desempenho lento ou paralisações nos sites de produção.

>[!BEGINSHADEBOX]

**Pré-requisito:** Um despejo de banco de dados (veja a Etapa 3) deve incluir disparadores de banco de dados. Para despejá-los, confirme se você tem o [privilégio de ACIONADOR](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger).

>[!IMPORTANT]
>
>O banco de dados do ambiente de integração é estritamente para teste de desenvolvimento e pode incluir dados que você não deseja migrar para Preparo e Produção.

Para implantações de integração contínua, a Adobe **não recomenda** migrar dados da Integração para o Preparo e a Produção. Você pode passar nos dados de teste ou substituir dados importantes. Todas as configurações vitais são passadas usando o [arquivo de configuração](../store/store-settings.md) e o comando `setup:upgrade` durante a compilação e a implantação.

>[!ENDSHADEBOX]

A Adobe **recomenda** migrar dados da Produção para o Armazenamento temporário para testar totalmente seu site e armazená-lo em um ambiente de quase produção com todos os serviços e configurações.

>[!NOTE]
>
>Para transferir mídia de ambientes remotos para remotos diretamente, você deve habilitar o encaminhamento do agente ssh, consulte [orientação do GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Fazer backup do banco de dados

É uma prática recomendada criar um backup do banco de dados. O procedimento a seguir usa a orientação de [Fazer backup do banco de dados](../storage/database-dump.md).

**Para despejar o banco de dados**:

1. [Use SSH para fazer logon no ambiente remoto](../development/secure-connections.md#use-an-ssh-command) que contém o banco de dados a ser copiado.

1. Liste os relacionamentos de ambiente e observe as informações de logon do banco de dados.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Para Preparo e Produção Profissionais, o nome do banco de dados está na variável `MAGENTO_CLOUD_RELATIONSHIPS` (normalmente o mesmo que o nome do aplicativo e do usuário).

1. Crie um backup do banco de dados. Para escolher um diretório de destino para o despejo do banco de dados, use a opção `--dump-directory`.

   Para ambientes Starter e ambientes de integração Pro, use `main` como o nome do banco de dados:

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Opções de despejo:
   - `--dump-directory=<dir>` — Escolha um diretório de destino para o despejo de banco de dados
   - `--remove-definers`—Remover instruções DEFINER do despejo de banco de dados

1. Embora o método ECE-Tools seja preferido, outro método é criar um arquivo de despejo de banco de dados usando MySQL nativo no formato GZIP.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Se você tiver configurado a autenticação de dois fatores no ambiente de destino, é melhor excluir tabelas 2FA relacionadas para evitar a reconfiguração após a migração do banco de dados:

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Digite `logout` para encerrar a conexão SSH.

### Remover e recriar o banco de dados

Ao importar dados, você deve eliminar e criar um banco de dados.

**Para remover e recriar o banco de dados**:

1. Estabeleça um [túnel SSH](../development/secure-connections.md#ssh-tunneling) para o ambiente remoto.

1. Conectar ao serviço de banco de dados.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. No prompt `MariaDB [main]>`, descarte o banco de dados.

   Para integração Starter e Pro:

   ```shell
   drop database main;
   ```

   Para ambientes de produção e de preparo:

   ```shell
   drop database <database_name>;
   ```

1. Recrie o banco de dados.

   Para integração Starter e Pro:

   ```shell
   create database main;
   ```

1. Importe o banco de dados.

   Importar para produção:

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Importar para Preparo:

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Esses comandos descompactam o arquivo de despejo do banco de dados, removem as instruções `DEFINER` e importam o banco de dados usando as credenciais especificadas.
