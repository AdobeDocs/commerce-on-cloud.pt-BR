---
title: Propriedade Crons
description: Veja exemplos de como configurar a propriedade "crons" no arquivo de configuração do aplicativo  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Propriedade Crons

O Adobe Commerce usa a propriedade `crons` para agendar atividades repetitivas. É ideal para agendar uma tarefa específica para execução em determinados horários do dia. Somente um trabalho cron pode ser executado de cada vez na instância da Web para projetos do Adobe Commerce na infraestrutura em nuvem devido à natureza dos ambientes somente leitura. É uma prática recomendada dividir tarefas de longa duração em tarefas menores em fila. Como alternativa, você pode criar uma [instância do trabalhador](workers-property.md).

A Adobe recomenda que você execute `crons` como o [proprietário do sistema de arquivos](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html). _não_ execute `crons` como `root` ou como o usuário do servidor Web.

Essa configuração é diferente das implantações locais do Adobe Commerce, que têm vários trabalhos cron padrão. Consulte [Configurar trabalhos cron](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html) no _Guia de configuração_.

## Configurar trabalhos cron

A propriedade `crons` descreve processos que são acionados de acordo com um agendamento. Cada tarefa requer um nome e as seguintes opções:

- `spec` — A expressão CRON usada para agendamento.
- `cmd` — O comando a ser executado em `start` e `stop`.
- `shutdown_timeout`—(_Opcional_) Se um trabalho cron for cancelado, esse será o número de segundos após o qual um sinal SIGKILL será enviado para interromper o trabalho ou processo. O padrão é 10 segundos.
- `timeout`—(_Opcional_) A quantidade máxima de tempo que um trabalho cron pode ser executado antes do tempo limite. O padrão é o valor máximo permitido de 86.400 segundos (24 horas).

Por padrão, cada projeto de nuvem do Commerce tem a seguinte configuração `crons` padrão no arquivo `.magento.app.yaml`:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Se o seu projeto requer trabalhos cron personalizados, você pode adicioná-los à configuração padrão `crons`. Consulte [Criar um trabalho cron](#build-a-cron-job).

### `crontab`

A Adobe Commerce adicionou uma opção de configuração de autocronagem somente a projetos Pro para oferecer suporte à configuração de autoatendimento `crons` nos ambientes de Preparo e Produção. Se esta opção estiver habilitada, você poderá usar `crontab` para revisar a configuração do cron. Isto _não_ está disponível com projetos iniciais.

Embora você possa usar o `crontab` para revisar a configuração em projetos Pro, a Adobe Commerce não usa o `crontab` para executar trabalhos cron para sites implantados na infraestrutura em nuvem.

**Para revisar a configuração do cron em ambientes Pro**:

1. Use o [SSH](../development/secure-connections.md#use-an-ssh-command) para fazer logon no ambiente remoto.

1. Liste os processos cron programados.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Se o comando `crontab -l` retornar um erro `Command not found` (somente em ambientes de Preparo e Produção Pro), você deverá [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para habilitar a opção de configuração de autoatendimento de cronograma automático em seu projeto.

O exemplo a seguir mostra a saída `crontab` para um ambiente que tem apenas a configuração `crons` padrão:

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Criar um trabalho cron

Um trabalho cron inclui a especificação de cronograma e tempo e o comando a ser executado no horário agendado. Para ambientes Starter e Pro `integration`, o intervalo mínimo é uma vez a cada cinco minutos. Para ambientes de preparo e produção profissionais, o intervalo mínimo é de uma vez por minuto. No Adobe Commerce na infraestrutura em nuvem, você adiciona trabalhos cron personalizados ao arquivo `.magento.app.yaml` na seção `crons`. O formato geral é `spec` para agendamento e `cmd` para especificar o comando ou script personalizado a ser executado.

### Especificação

O Adobe Commerce usa uma expressão de cinco valores para uma especificação `crons` (spec): `* * * * *`

1. Minuto (0 a 59) Para todos os ambientes Starter e Pro, a frequência mínima com suporte para trabalhos cron é de cinco minutos. Talvez seja necessário definir as configurações em seu Administrador.
2. Hora (0 a 23)
3. Dia do mês (1 a 31)
4. Mês (1 a 12)
5. Dia da semana (0 a 6) (de domingo a sábado; 7 também é domingo em alguns sistemas)

Alguns exemplos:

- O `00 */3 * * *` é executado a cada três horas no primeiro minuto (12h, 3h, 6h)
- O `20 */8 * * *` é executado a cada 8 horas aos minutos 20 (12:20, 8:20, 16:20)
- `00 00 * * *` é executado uma vez por dia à meia-noite
- O `00 * * * 1` é executado uma vez por semana, às segundas-feiras à meia-noite.

>[!NOTE]
>
>A hora `crons` especificada no arquivo `.magento.app.yaml` é baseada no fuso horário do servidor, não no fuso horário especificado nos valores de configuração de repositório no banco de dados.

Ao determinar o agendamento, considere o tempo necessário para concluir a tarefa. Por exemplo, se você executar um job a cada três horas e a tarefa levar 40 minutos para ser concluída, poderá considerar alterar o tempo agendado.

### Comando

O `cmd` especifica o comando ou script personalizado a ser executado. O formato do script de comando pode incluir o seguinte:

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Por exemplo:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

Neste exemplo, `<path-to-php-binary>` é `/usr/bin/php`. O diretório de instalação, que inclui a ID do projeto é `/app/abc123edf890/bin/magento`, e a ação do script é `export:start catalog_category_product`.

### Adicionar trabalhos cron personalizados ao seu projeto

Na plataforma Adobe Commerce na infraestrutura em nuvem, é possível adicionar personalizações à seção `crons` do arquivo [`.magento.app.yaml`](../application/configure-app-yaml.md).

>[!NOTE]
>
>Para ambientes Starter e Pro `integration`, o intervalo mínimo é uma vez a cada cinco minutos. Para ambientes de preparo e produção profissionais, o intervalo mínimo é de uma vez por minuto. Você não pode configurar intervalos mais frequentes do que os mínimos padrão.

Em projetos do Adobe Commerce Pro, o [recurso de autcrons](#set-up-cron-jobs) deve estar habilitado no seu projeto antes que você possa adicionar trabalhos cron personalizados a ambientes de Preparo e Produção usando o arquivo `.magento.app.yaml`. Se este recurso não estiver habilitado, [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para habilitar o autocronismo.

**Para adicionar trabalhos cron personalizados**:

1. No ambiente de desenvolvimento local, edite o arquivo `.magento.app.yaml` no diretório `/app` do Adobe Commerce.

1. Na seção `crons`, adicione sua personalização usando o seguinte formato:

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   No exemplo a seguir, o trabalho `productcatalog` exporta o catálogo de produtos a cada oito horas, 20 minutos após a hora.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Atualizar trabalhos cron

Para adicionar, remover ou atualizar um trabalho personalizado, altere a configuração na seção `crons` do arquivo `.magento.app.yaml`. Em seguida, teste as atualizações no ambiente remoto `integration` antes de enviar as alterações para os ambientes de Preparo e Produção.

## Desabilitar trabalhos cron

Você pode desabilitar manualmente os trabalhos cron antes de concluir as tarefas de manutenção, como reindexação ou limpeza do cache para evitar problemas de desempenho. Você pode usar o comando `cron:disable` da CLI `ece-tools` para desabilitar todos os trabalhos cron e parar quaisquer processos cron ativos.

**Para desabilitar os trabalhos cron**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Desative as tarefas cron e interrompa os processos cron ativos.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. Após concluir todas as tarefas de manutenção necessárias, certifique-se de habilitar os trabalhos cron novamente.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Solução de problemas de trabalhos cron

o Adobe atualizou o pacote Adobe Commerce na infraestrutura em nuvem para otimizar o processamento de cron na plataforma Adobe Commerce na infraestrutura em nuvem e para corrigir problemas relacionados ao cron. Se você encontrar problemas com o processamento CRON, verifique se seu projeto está usando a versão mais recente do pacote `ece-tools`. Consulte [Atualizar ECE-Tools](../dev-tools/update-package.md).

Você pode revisar as informações de processamento do CRON nos arquivos de log no nível do aplicativo para cada ambiente. Consulte [Logs do aplicativo](../test/log-locations.md#application-logs).

Consulte os seguintes artigos de suporte da Adobe Commerce para obter ajuda com a solução de problemas relacionados ao cron:

- [Tarefas do Cron bloqueiam tarefas de outros grupos](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [Redefinir trabalhos cron travados manualmente na nuvem](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
