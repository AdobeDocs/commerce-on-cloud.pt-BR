---
title: Exibir e gerenciar logs
description: Entenda os tipos de arquivos de log disponíveis na infraestrutura da nuvem e onde encontrá-los.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: 731cc36816afdb5374269e871d337e056a71c050
workflow-type: tm+mt
source-wordcount: '1205'
ht-degree: 0%

---

# Exibir e gerenciar logs

Os logs do Adobe Commerce em projetos de infraestrutura em nuvem são úteis para solucionar problemas relacionados à [criação e implantação de ganchos](../application/hooks-property.md), aos serviços em nuvem e ao aplicativo do Adobe Commerce.

Você pode exibir os logs do sistema de arquivos, do [!DNL Cloud Console] e da CLI do `magento-cloud`.

- **Sistema de arquivos** — O diretório de sistema `/var/log` contém logs para todos os ambientes. O diretório `var/log/` contém logs específicos do aplicativo exclusivos a um ambiente específico. Esses diretórios não são compartilhados entre nós em um cluster. Em ambientes de produção e preparo profissionais, você deve verificar os registros em cada nó.

- **[!DNL Cloud Console]** — Você pode ver informações de log de compilação, implantação e pós-implantação na lista de _mensagens_ do ambiente.

- **CLI da Nuvem**—Você pode exibir logs de ambiente local usando o comando `magento-cloud log` ou logs de ambiente remoto usando o comando `magento-cloud ssh`.

## Locais de log

Os registros do sistema são armazenados nos seguintes locais:

- Integração: `/var/log/<log-name>.log`
- Estágios Profissionais: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Produção Pro: `/var/log/platform/<project-ID>/<log-name>.log`

O valor de `<project-ID>` depende do projeto e se o ambiente é de Preparo ou de Produção. Por exemplo, com a ID de projeto `yw1unoukjcawe`, o usuário do ambiente de Preparo é `yw1unoukjcawe_stg` e o usuário do ambiente de Produção é `yw1unoukjcawe`.

Usando esse exemplo, o log de implantação é: `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Exibir logs de ambiente remoto

A maioria dos registros inclui eventos que ocorrem no ambiente remoto. Para o Pro, há vários nós e cada nó tem registros exclusivos. Use o seguinte para ver uma lista de todos os hosts:

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Exemplo de resposta:

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**Para exibir uma lista de logs de ambientes remotos**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Exemplo para Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**Para exibir um log remoto**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Exemplo para Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Para ambientes Pro Staging e Pro Production, a rotação, compactação e remoção automáticas do registro são ativadas para arquivos de registro com um nome de arquivo fixo. Cada tipo de arquivo de log tem um padrão rotativo e uma duração.
>>Detalhes completos sobre a rotação de logs e a duração de logs compactados do ambiente podem ser encontrados em: `/etc/logrotate.conf` e `/etc/logrotate.d/<various>`.
>>Para ambientes de Pro Staging e Pro Production, você deve [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar alterações na configuração de rotação do log.

>[!TIP]
>
>A rotação de logs não pode ser configurada em ambientes Pro Integration.
>>Para a Integração Pro, você deve implementar uma solução/script personalizado e [configurar seu cron](../application/crons-property.md) para executar o script conforme necessário.

>[!NOTE]
>
>Os ambientes do Projeto inicial não têm rotação de log.

## Criar e implantar logs

Depois de enviar as alterações para o ambiente, você pode revisar o log de cada gancho no arquivo `var/log/cloud.log`. O log contém mensagens de início e parada para cada gancho. No exemplo a seguir, as mensagens são &quot;`Starting post-deploy.`&quot; e &quot;`Post-deploy is complete.`&quot;

Verifique os carimbos de data e hora nas entradas do log e localize os logs para uma implantação específica. Este é um exemplo condensado de saída de log que você pode usar para solução de problemas:

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Ao configurar seu ambiente de nuvem, você pode configurar [notificações por email e Slack baseadas em log](../environment/set-up-notifications.md) para criar e implantar ações.

Os seguintes logs têm um local comum para todos os projetos na nuvem:

- **Log de implantação**: `var/log/cloud.log`
- **Último log de erros de implantação**: `var/log/cloud.error.log`
- **Log de depuração**: `var/log/debug.log`
- **Log de exceções**: `var/log/exception.log`
- **Log do sistema**: `var/log/system.log`
- **Log de suporte**: `var/log/support_report.log`
- **Relatórios**: `var/report/`

Embora o arquivo `cloud.log` contenha comentários de cada estágio do processo de implantação, os logs criados pelo gancho de implantação são exclusivos de cada ambiente. O log de implantação específico do ambiente está nos seguintes diretórios:

- **Integração Starter e Pro**: `/var/log/deploy.log`
- **Estágios Profissionais**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Produção Profissional**: `/var/log/platform/<project-ID>/deploy.log`

### Implantar log

O log de cada implantação se concatena com o arquivo `deploy.log` específico. O exemplo a seguir imprime o log de implantação do ambiente atual no terminal:

```bash
magento-cloud log -e <environment-ID> deploy
```

Exemplo de resposta:

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Log de erros

Mensagens de erro e aviso geradas durante o processo de implantação são gravadas nos arquivos `var/log/cloud.log` e `var/log/cloud.error.log`. O arquivo de log de erros da nuvem contém apenas erros e avisos da implantação mais recente. Um arquivo vazio indica uma implantação bem-sucedida sem erros.

Você pode exibir o arquivo de log usando a [CLI de nuvem SSH](#view-remote-environment-logs) ou pode usar as ECE-Tools para mostrar os erros com sugestões:

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Exemplo de resposta:

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

A maioria das mensagens de erro contém uma descrição e uma ação sugerida. Use a [Referência da mensagem de erro para ECE-Tools](../dev-tools/error-reference.md) para consultar o código de erro para obter mais orientações. Para obter mais orientações, use a [Solução de problemas de implantação do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html).

## Logs do aplicativo

Assim como os logs de implantação, os logs de aplicativos são exclusivos para cada ambiente:

| Arquivo de log | Integração do Starter e do Pro | Descrição |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Log de implantação** | `/var/log/deploy.log` | Atividade do [gancho de implantação](../application/hooks-property.md). |
| **Log pós-implantação** | `/var/log/post_deploy.log` | Atividade do [gancho pós-implantação](../application/hooks-property.md). |
| **Log do Cron** | `/var/log/cron.log` | Saída de trabalhos cron. |
| **Log de acesso do Nginx** | `/var/log/access.log` | Na inicialização do Nginx, erros de HTTP para diretórios ausentes e tipos de arquivos excluídos. |
| **Log de erros do Nginx** | `/var/log/error.log` | Mensagens de inicialização úteis para depurar erros de configuração associados ao Nginx. |
| **log de acesso ao PHP** | `/var/log/php.access.log` | Solicitações para o serviço PHP. |
| **log do FPM do PHP** | `/var/log/app.log` | |

Para ambientes de preparo e produção Pro, os logs de implantação, pós-implantação e cron estão disponíveis somente no primeiro nó do cluster:

| Arquivo de log | Estágio Pro | Produção Pro |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Log de implantação** | Somente o primeiro nó:<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | Somente o primeiro nó:<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Log pós-implantação** | Somente o primeiro nó:<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | Somente o primeiro nó:<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Log do Cron** | Somente o primeiro nó:<br>`/var/log/platform/<project-ID>_stg*/cron.log` | Somente o primeiro nó:<br>`/var/log/platform/<project-ID>/cron.log` |
| **Log de acesso do Nginx** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Log de erros do Nginx** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **log de acesso ao PHP** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **log do FPM do PHP** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### Arquivos de log arquivados

Os logs do aplicativo são compactados e arquivados uma vez por dia e mantidos por **365 dias** por padrão (para clusters Pro Staging e de Produção). A rotação de logs não está disponível em todos os ambientes de integração/Iniciador. Os logs compactados são nomeados usando uma ID exclusiva que corresponde ao `Number of Days Ago + 1`. Por exemplo, em ambientes de produção Pro, um log de acesso do PHP para 21 dias no passado é armazenado e nomeado da seguinte maneira:

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

Os arquivos de log arquivados são sempre armazenados no diretório onde o arquivo original estava localizado antes da compactação.

Você pode [enviar um tíquete de suporte](https://experienceleague.adobe.com/home?support-tab=home#support) para solicitar alterações no período de retenção do log ou na configuração de logrotate. Você pode aumentar o período de retenção até um máximo de 365 dias, reduzi-lo para conservar a cota de armazenamento ou adicionar outros caminhos de log à configuração de logrotate. Essas alterações estão disponíveis para clusters Pro de armazenamento temporário e produção.

Por exemplo, se você criar um caminho personalizado para armazenar logs no diretório `var/log/mymodule`, poderá solicitar a rotação de logs para esse caminho. No entanto, a infraestrutura atual requer nomes de arquivos consistentes para que o Adobe configure a rotação de logs adequadamente. A Adobe recomenda manter os nomes de log consistentes para evitar problemas de configuração.

>[!NOTE]
>
>Os arquivos de log de **Implantação** e **Pós-implantação** não são girados e arquivados. Todo o histórico de implantação é gravado nesses arquivos de log.

## Logs de serviço

Como cada serviço é executado em um contêiner separado, os logs do serviço não estão disponíveis no ambiente de integração. A infraestrutura do Adobe Commerce na nuvem fornece acesso ao container do servidor Web somente no ambiente de integração. Os seguintes locais de registro de serviço são para os ambientes de produção e preparo profissionais:

- **Log Redis**: `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **log do Elasticsearch**: `/var/log/elasticsearch/elasticsearch.log`
- **Log de coleta de lixo Java**: `/var/log/elasticsearch/gc.log`
- **Log de email**: `/var/log/mail.log`
- **Log de erros do MySQL**: `/var/log/mysql/mysql-error.log`
- **Log lento do MySQL**: `/var/log/mysql/mysql-slow.log`
- **Log de RabbitMQ**: `/var/log/rabbitmq/rabbit@host1.log`

Os logs de serviço são arquivados e salvos por períodos diferentes, dependendo do tipo de log. Por exemplo, os registros MySQL têm o tempo de vida mais curto, removido após sete dias.

>[!TIP]
>
>Os locais do arquivo de log na arquitetura dimensionada dependem do tipo de nó. Consulte o tópico [Locais de log na arquitetura em escala](../architecture/scaled-architecture.md#log-locations).

## Dados de log para produção e preparo profissionais

Em ambientes de Produção e Preparo Pro, use o [gerenciamento de logs do New Relic](../monitor/log-management.md) integrado ao seu projeto para gerenciar dados de logs agregados de todos os logs associados ao seu projeto de infraestrutura do Adobe Commerce na nuvem.

O aplicativo de logs do New Relic fornece um painel de gerenciamento de log centralizado para solucionar problemas e monitorar o Adobe Commerce em ambientes de produção e preparo de infraestrutura em nuvem. O painel também fornece acesso aos dados de log para os serviços Fastly CDN, Image Otimization e Web application firewall (WAF). Consulte [serviços da New Relic](../monitor/new-relic-service.md).
