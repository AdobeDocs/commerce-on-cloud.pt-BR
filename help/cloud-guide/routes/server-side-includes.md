---
title: Inclusões do lado do servidor
description: Saiba como usar inclusões de servidor com o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Inclusões do lado do servidor

[Inclusões do lado do servidor](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) são diretivas em páginas de HTML que são avaliadas no servidor enquanto as páginas são renderizadas. O SSI permite adicionar conteúdo gerado dinamicamente a uma página de HTML existente, sem veicular a página inteira.

Você pode ativar ou desativar o SSI por rota no `.magento/routes.yaml`; por exemplo:

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

O SSI permite incluir em suas diretivas de resposta HTML que fazem com que o servidor preencha partes do HTML, respeitando qualquer [configuração de cache](caching.md) existente.

O exemplo a seguir mostra como inserir um controle de data dinâmico na parte superior de uma página e outro controle de data na parte inferior, que é atualizado a cada 600 segundos:

Adicione o seguinte a qualquer página, como `/index.php`:

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

Adicionar o seguinte a `time.php`:

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

Navegue até a página em que você adicionou o controle. Atualize a página várias vezes e observe que a hora na parte superior da página muda, mas a hora na parte inferior muda somente a cada 600 segundos.
