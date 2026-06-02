---
title: Inclusões do lado do servidor
description: Saiba como usar inclusões de servidor com o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# Inclusões do lado do servidor

[Inclusões de servidor](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) são diretivas em páginas do HTML que são avaliadas no servidor enquanto as páginas são renderizadas. O SSI permite adicionar conteúdo gerado dinamicamente a uma página existente do HTML sem veicular a página inteira.

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

O SSI permite incluir em suas diretivas de resposta do HTML que fazem com que o servidor preencha partes do HTML, respeitando qualquer [configuração de cache](caching.md) existente.

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
