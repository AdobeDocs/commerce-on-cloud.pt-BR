---
title: Configurar o serviço MySQL
description: Saiba como gerenciar o serviço MySQL para armazenamento de dados persistente com o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# Configurar o serviço MySQL

O serviço `mysql` fornece armazenamento de dados persistente com base nas versões 10.2 a 10.4 do [MariaDB](https://mariadb.com/), com suporte para o mecanismo de armazenamento [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) e reimplementou os recursos do MySQL 5.6 e 5.7.

A reindexação no MariaDB 10.4 leva mais tempo em comparação com outras versões do MariaDB ou MySQL. Consulte [Indexadores](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) no guia de _Práticas recomendadas de desempenho_.

>[!WARNING]
>
>Tenha cuidado ao atualizar o MariaDB da versão 10.1 para 10.2. O MariaDB 10.1 é a última versão que oferece suporte ao _XtraDB_ como mecanismo de armazenamento. MariaDB 10.2 usa _InnoDB_ para o mecanismo de armazenamento. Depois de atualizar da versão 10.1 para a 10.2, não é possível reverter a alteração. O Adobe Commerce é compatível com ambos os mecanismos de armazenamento; no entanto, você deve verificar as extensões e outros sistemas usados pelo seu projeto para garantir que eles sejam compatíveis com o MariaDB 10.2. Consulte [Alterações Incompatíveis Entre 10.1 e 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Para habilitar MySQL**:

1. Adicione o nome, o tipo e o valor de disco necessário (em MB) ao arquivo `.magento/services.yaml`.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >Erros do MySQL, como `PDO Exception: MySQL server has gone away`, podem ocorrer como resultado de espaço em disco insuficiente. Verifique se você alocou espaço em disco suficiente para o serviço no arquivo [`.magento/services.yaml`](services-yaml.md#disk).

1. Configure as relações no arquivo `.magento.app.yaml`.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Adicionar, confirmar e enviar por push as alterações de código.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Verifique as relações de serviço](services-yaml.md#service-relationships).

{{service-change-tip}}

## Configurar banco de dados MySQL

Você tem as seguintes opções ao configurar o banco de dados MySQL:

- **`schemas`** — Um esquema define um banco de dados. O esquema padrão é o banco de dados `main`.
- **`endpoints`** — Cada ponto de extremidade representa uma credencial com privilégios específicos. O ponto de extremidade padrão é `mysql`, que tem acesso de `admin` ao banco de dados `main`.
- **`properties`** — As propriedades são usadas para definir configurações adicionais de banco de dados.

Este é um exemplo básico de configuração no arquivo `.magento/services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

O `properties` no exemplo acima modifica as configurações padrão `optimizer` como [recomendado no Guia de Práticas Recomendadas de Desempenho](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).

**Opções de configuração do MariaDB**:

| Opções | Descrição | Valor padrão |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | O conjunto de caracteres padrão. | utf8mb4 |
| `default_collation` | O agrupamento padrão. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Tamanho máximo para pacotes, em MB. Intervalo `1` a `100`. | 16 |
| `optimizer_switch` | Defina valores para o otimizador de consultas. Consulte a [documentação do MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Selecione as estatísticas usadas pelo otimizador. Intervalo `1` a `5`. Consulte a [documentação do MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 para 10.4 e posterior |

### Configurar vários usuários do banco de dados

Como opção, você pode configurar vários usuários com permissões diferentes para acessar o banco de dados `main`.

Por padrão, há um ponto de extremidade chamado `mysql` que tem acesso de administrador ao banco de dados. Para configurar vários usuários do banco de dados, você deve definir vários pontos de extremidade no arquivo `services.yaml` e declarar as relações no arquivo `.magento.app.yaml`. Para ambientes de Preparo e Produção Profissionais, [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar o usuário adicional.

Use uma matriz aninhada para definir os endpoints de acesso do usuário específico. Cada endpoint pode designar acesso a um ou mais schemas (bancos de dados) e diferentes níveis de permissão em cada um.

Os níveis de permissão válidos são:

- `ro`: somente consultas SELECT são permitidas.
- `rw`: consultas SELECT e consultas INSERT, UPDATE e DELETE são permitidas.
- `admin`: Todas as consultas são permitidas, incluindo consultas DDL (CREATE TABLE, DROP TABLE e muito mais).

Por exemplo:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

No exemplo anterior, o ponto de extremidade `admin` fornece acesso de nível administrativo ao banco de dados `main`, o ponto de extremidade `reporter` fornece acesso somente leitura e o ponto de extremidade `importer` fornece acesso somente leitura, o que significa:

- O usuário `admin` tem controle total do banco de dados.
- O usuário `reporter` tem somente privilégios SELECT.
- O usuário `importer` tem privilégios SELECT, INSERT, UPDATE e DELETE.

Adicione os pontos de extremidade definidos no exemplo acima à propriedade `relationships` do arquivo `.magento.app.yaml`. Por exemplo:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Se você configurar um usuário MySQL, não poderá usar o mecanismo de controle de acesso [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) para procedimentos e exibições armazenados.

## Conectar ao banco de dados

O acesso direto ao banco de dados do MariaDB requer que você use um SSH para fazer logon no ambiente remoto da nuvem e se conectar ao banco de dados.

1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recupere as credenciais de logon do MySQL das propriedades `database` e `type` na variável [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships).

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Na resposta, localize as informações do MySQL. Por exemplo:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Conectar ao banco de dados.

   - Para Iniciante, use o seguinte comando:

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Para Pro, use o seguinte comando com nome de host, número da porta, nome de usuário e senha recuperados da variável `$MAGENTO_CLOUD_RELATIONSHIPS`.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>Você pode usar o comando `magento-cloud db:sql` para se conectar ao banco de dados remoto e executar comandos SQL.

## Conectar ao banco de dados secundário

>[!IMPORTANT]
>
>Esse recurso está disponível somente nos clusters Pro Production e Staging.

Às vezes, você precisa se conectar ao banco de dados secundário para melhorar o desempenho do banco de dados ou resolver problemas de bloqueio do banco de dados. Se esta configuração for necessária, use `"port" : 3304` para estabelecer a conexão. Consulte o tópico [Prática recomendada para configurar a conexão subordinada do MySQL](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) no guia _Práticas recomendadas de implementação_.

## Solução de problemas

Consulte os seguintes artigos de suporte da Adobe Commerce para obter ajuda com a solução de problemas do MySQL:

- [Verificando consultas e processos lentos MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [Criar despejo de banco de dados na Nuvem](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [Solução de problemas da Ferramenta de Migração de Dados](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Atualização do Adobe Commerce: compacta para tabelas dinâmicas 2.2.x, 2.3.x para 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
