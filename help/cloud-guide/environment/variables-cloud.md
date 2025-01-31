---
title: Variáveis específicas da nuvem
description: Consulte uma lista de variáveis de ambiente específicas do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Variáveis específicas da nuvem

As variáveis de ambiente específicas do Adobe Commerce na infraestrutura em nuvem usam o prefixo `MAGENTO_CLOUD_*`:

| Variável | Descrição |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | O caminho absoluto para o diretório do aplicativo. |
| `MAGENTO_CLOUD_APPLICATION` | Um objeto JSON codificado na base64 que descreve o aplicativo. Ele mapeia para o conteúdo do arquivo `.magento.app.yaml` e tem subchaves. |
| `MAGENTO_CLOUD_APPLICATION_NAME` | O nome do aplicativo configurado no arquivo `.magento.app.yaml`. |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | O caminho absoluto para a raiz do documento web, se aplicável. |
| `MAGENTO_CLOUD_ENVIRONMENT` | O nome da ramificação do ambiente. |
| `MAGENTO_CLOUD_PROJECT` | A ID do projeto. |
| `MAGENTO_CLOUD_RELATIONSHIPS` | Um objeto JSON codificado em base64 que representa a definição do ponto de extremidade da chave (nome do relacionamento) e do valor (matrizes de pares de relacionamento). Cada definição de endpoint de relacionamento é uma forma decomposta de um URL. Ele tem um `scheme`, um `host`, um `port` e _opcionalmente_ um `username`, `password`, `path`, e algumas informações adicionais em `query`. |
| `MAGENTO_CLOUD_ROUTES` | Descreva as rotas definidas no arquivo de ambiente `.magento/routes.yaml`. |
| `MAGENTO_CLOUD_TREE_ID` | A ID da árvore do aplicativo, que corresponde ao SHA da árvore no Git. |
| `MAGENTO_CLOUD_VARIABLES` | Um objeto JSON codificado em base64 com pares de valores chave, como `"key":"value"`. |
| `MAGENTO_CLOUD_LOCKS_DIR` | Fornece o caminho para o ponto de montagem do provedor de bloqueio na infraestrutura em nuvem. O provedor de bloqueio impede a inicialização de trabalhos cron duplicados e grupos cron. |

>[!WARNING]
>
>Para adicionar variáveis de ambiente a [substituir definições de configuração](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) usando [[!DNL Cloud Console]](../project/overview.md), você deve anexar o nome da variável a `env:` como no exemplo a seguir:
>
>![Exemplo de variável de ambiente](../../assets/set-env-variable-ui.png)

Como os valores podem mudar com o tempo, é melhor inspecionar a variável no tempo de execução e usá-la para configurar seu aplicativo. Por exemplo, use a variável `MAGENTO_CLOUD_RELATIONSHIPS` para recuperar as relações relacionadas ao ambiente da seguinte maneira:

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## Exibição de variáveis de ambiente

Você pode usar o comando `env:config:show` do [pacote `ece-tools`](../dev-tools/package-overview.md) para mostrar uma lista de variáveis do ambiente atual.

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

Exemplo de saída para a opção `variables`:

```
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
