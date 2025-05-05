---
title: Armazenamento em cache
description: Saiba como habilitar o armazenamento em cache de seu Adobe Commerce em ambientes de infraestrutura em nuvem.
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
source-git-commit: a1ed2818cbaf5adf8b673df0ee9b9218e6f700a2
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Armazenamento em cache

Você pode ativar o armazenamento em cache no ambiente do projeto de infraestrutura em nuvem. Se você desativar o armazenamento em cache, o Adobe Commerce fornecerá os arquivos diretamente.

{{route-placeholder}}

## Configurar armazenamento em cache

Habilite o cache para o aplicativo configurando as regras de cache no arquivo `.magento/routes.yaml` da seguinte maneira:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## Armazenamento em cache com base em rotas

Habilite o cache refinado configurando regras de cache para várias rotas separadamente, como mostra o exemplo a seguir:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

O exemplo anterior armazena em cache as seguintes rotas:

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

E as seguintes rotas estão **não** armazenadas em cache:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Expressões regulares em rotas **não** são suportadas.

## Duração do cache

A duração do cache é determinada pelo valor do cabeçalho de resposta `Cache-Control`. Se nenhum cabeçalho `Cache-Control` estiver na resposta, a chave `default_ttl` será usada.

## Chave do cache

Para decidir como armazenar uma resposta em cache, o Adobe Commerce cria uma chave de cache que depende de vários fatores e armazena a resposta associada a essa chave. Quando uma solicitação vem com a mesma chave de cache, a resposta é reutilizada. Sua finalidade é semelhante ao cabeçalho HTTP [`Vary`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

As chaves de parâmetros `headers` e `cookies` permitem alterar essa chave de cache.

O valor padrão para essas chaves é o seguinte:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Atributos de cache

### `enabled`

Quando definido como `true`, habilite o cache para esta rota. Quando definido como `false`, desabilite o cache para esta rota.

### `headers`

Define de quais valores a chave do cache deve depender.

Por exemplo, se a chave `headers` for a seguinte:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

Em seguida, o Adobe Commerce armazena em cache uma resposta diferente para cada valor do cabeçalho HTTP `Accept`.

### `cookies`

A chave `cookies` define de quais valores a chave de cache deve depender.

Por exemplo:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

A chave do cache depende do valor do cookie `value` na solicitação.

Existe um caso especial se a chave `cookies` tiver o valor `["*"]`. Esse valor significa que qualquer solicitação com um cookie ignora o cache. Este é o valor padrão.

>[!NOTE]
>
>Não é possível usar curingas no nome do cookie. Use um nome de cookie preciso ou faça a correspondência de todos os cookies com um asterisco (`*`). Por exemplo, `SESS*` ou `~SESS` atualmente são **valores inválidos**.

Os cookies têm as seguintes restrições:

- Há um máximo definido de **50 cookies** no sistema. Caso contrário, o aplicativo acionará uma exceção `Unable to send the cookie. Maximum number of cookies would be exceeded`. Para aumentar o número de cookies para 200, aplique o [patch MDVA-12304](https://experienceleague.adobe.com/docs/commerce-operations/tools/quality-patches-tool/release-notes.html?lang=pt-BR) usando a [Ferramenta de Patches de Qualidade](https://experienceleague.adobe.com/pt-br/docs/commerce-learn/tutorials/tools/quality-patch-tool).
- Um tamanho máximo de cookie é de **4096 bytes**. Caso contrário, o aplicativo acionará uma exceção `Unable to send the cookie. Size of '%name' is %size bytes`.

### `default_ttl`

Se a resposta não tiver um cabeçalho `Cache-Control`, a chave `default_ttl` será usada para definir a duração do cache, em segundos. O valor padrão é `0`, o que significa que nada está armazenado em cache.
