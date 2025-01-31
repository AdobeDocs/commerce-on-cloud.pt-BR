---
title: propriedade da Web
description: Veja exemplos de como configurar a propriedade da Web no arquivo de configuração do aplicativo  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# propriedade da Web

A propriedade `web` define como o aplicativo é exposto à Web (em HTTP), determina como o aplicativo Web fornece conteúdo e controla como o contêiner de aplicativo responde às solicitações recebidas definindo regras em cada local _block_. Um bloco representa um caminho absoluto precedente a uma barra (`/`).

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

Você pode ajustar a configuração do `locations` usando os seguintes valores de chave para cada bloco `locations`:

| Atributo | Descrição |
| :--- | :--- |
| `allow` | Servir arquivos que não correspondem às &quot;regras&quot;. Valor padrão = `true` |
| `expires` | Defina o número de segundos para armazenar o conteúdo em cache no navegador. Esta chave habilita os cabeçalhos `cache-control` e `expires` para conteúdo estático. Se esse valor não for definido, a diretiva `expires` e os cabeçalhos resultantes não serão incluídos ao veicular arquivos de conteúdo estático. Um valor negativo 1 (`-1`) resulta em nenhum armazenamento em cache e é o valor padrão. Você pode expressar o valor de tempo com as seguintes unidades: `ms` (milissegundos), `s` (segundos), `m` (minutos), `h` (horas), `d` (dias), `w` (semanas), `M` (meses, 30d) ou `y` (anos, 365d) |
| `headers` | Defina cabeçalhos personalizados, como `X-Frame-Options`, para o conteúdo estático veiculado a partir deste local. |
| `index` | Liste os arquivos estáticos para servir seu aplicativo, como o arquivo `index.html`. Esta chave espera uma coleção. Isso só funciona se o acesso ao arquivo ou arquivos for &quot;permitido&quot; pela chave `allow` ou `rules` para esse local. |
| `rules` | Especifique substituições para um local. Use uma expressão regular para corresponder a uma solicitação. Se uma solicitação recebida corresponder à regra, o manuseio regular da solicitação será substituído pelas chaves usadas na regra. |
| `passthru` | Defina o URL usado caso um arquivo estático ou PHP não possa ser encontrado. Normalmente, esta URL é o controlador frontal para seus aplicativos, como o `/index.php` ou o `/app.php`. |
| `root` | Defina o caminho relativo à raiz do aplicativo que está exposto na Web. O diretório público (local &quot;/&quot;) de um projeto na nuvem é definido como &quot;pub&quot; por padrão. |
| `scripts` | Permitir o carregamento de scripts neste local. Defina o valor como `true` para permitir scripts. |

A configuração padrão permite o seguinte:

- No caminho raiz (`/`), somente a Web e a mídia podem ser acessadas
- Nos caminhos `~/pub/static` e `~/pub/media`, qualquer arquivo pode ser acessado

O exemplo a seguir mostra a configuração padrão no arquivo `.magento.app.yaml` para um conjunto de locais acessíveis pela Web associados a uma entrada na propriedade [`mounts` ](properties.md#mounts):

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>Este exemplo mostra a configuração da Web padrão para um projeto na nuvem configurado para oferecer suporte a um único domínio. Para um projeto que requer suporte para vários sites ou lojas, a configuração `web` deve ser definida para dar suporte a domínios compartilhados. Consulte [Configurar locais para domínios compartilhados](../store/multiple-sites.md#configure-locations-for-shared-domains).
