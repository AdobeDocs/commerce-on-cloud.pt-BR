---
title: Gerenciamento de logs do New Relic
description: Saiba como usar o log do New Relic
feature: Cloud, Logs, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Gerenciamento de logs do New Relic

Todos os projetos de infraestrutura em nuvem incluem o [gerenciamento de logs do New Relic](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). O serviço é pré-configurado para agregar todos os dados de log de seus ambientes de preparo e produção e exibi-los em um painel de gerenciamento de log centralizado.

Os dados agregados incluem informações dos seguintes logs:

- Todos `ece-tools` e logs de aplicativo do diretório `~/var/log`
- Logs para serviços em nuvem do diretório `var/log/platform/<project-ID>`
- Fastly CDN e WAF

Quando seu projeto está conectado ao New Relic, você pode usar o serviço Logs da New Relic para concluir tarefas como as seguintes:

- Usar queries do New Relic para pesquisar dados de log agregados
- Visualizar dados de log por meio do aplicativo New Relic Logs
- Criar gráficos, painéis e alertas personalizados
- Solucionar problemas de desempenho a partir de um único painel

## Exibir e analisar dados de log

Use o aplicativo de Logs do New Relic para pesquisar os dados de log agregados e solucionar problemas de erros de aplicativo, infraestrutura, CDN e WAF. Você pode criar gráficos, painéis e alertas usando dados de registro coletados dos serviços de infraestrutura e APM da New Relic.

**Para usar o aplicativo de Logs do New Relic**:

1. Faça logon em sua [conta do New Relic](https://login.newrelic.com/login).

1. Selecione **Logs** no menu de navegação do Explorer.

1. Verifique se sua Conta está selecionada na parte superior da exibição _Todos os logs_.

1. Selecione um intervalo de tempo para a consulta de Logs.

1. Para revisar os dados do log de infraestrutura dos serviços em nuvem (logs de `~/var/log/`), insira a cadeia de caracteres de consulta `has: "filePath"` no campo _Pesquisar Logs_. Em seguida, clique em **[!UICONTROL Query logs]**.

   Os nomes dos arquivos de log são armazenados na coluna `filePath`, com caminhos completos para o arquivo de log.

   ![Dados de log do serviço New Relic do projeto na nuvem](../../assets/new-relic/var-log-query.png)

1. Para examinar os dados de log do Fastly, insira a cadeia de caracteres de consulta `has: "client_ip"` no campo _Pesquisar Logs_. Em seguida, clique em **[!UICONTROL Query logs]**.

1. Para filtrar os resultados do log do Fastly por código de país, clique em **[!UICONTROL Add column]** e selecione **[!UICONTROL geo_country_code]**.

   ![Filtro de atributo de log da New Relic CDN do projeto na nuvem](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>Você pode salvar a exibição de consulta na lista suspensa _Exibições salvas_. Clique em **[!UICONTROL Create new]**, forneça um nome, selecione opções e clique em **[!UICONTROL Save view]**.
>
>Consulte [Introdução ao gerenciamento de logs](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) e [Introdução ao idioma de consulta do New Relic](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) no site _New Relic Docs_.
