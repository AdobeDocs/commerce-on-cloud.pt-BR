---
title: Configurar rotas
description: Saiba como definir as rotas para solicitações HTTPS recebidas para o Adobe Commerce em ambientes de infraestrutura em nuvem.
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Configurar rotas

O arquivo `routes.yaml` no diretório `.magento/routes.yaml` define as rotas do Adobe Commerce nos ambientes de Integração, Preparo e Produção da infraestrutura em nuvem. As rotas determinam como o aplicativo processa solicitações HTTP e HTTPS recebidas.

O arquivo padrão `routes.yaml` especifica os modelos de rota para processar solicitações HTTP como HTTPS em projetos que têm um único domínio padrão e em projetos configurados para vários domínios:

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

Use a CLI `magento-cloud` para exibir uma lista das rotas configuradas:

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Atualizações de configuração para ambientes Pro

{{pro-self-service-warning}}

## Modelos de roteiro

O arquivo `routes.yaml` é uma lista de rotas com modelos e suas configurações. Você pode usar os seguintes espaços reservados em modelos de rota:

- O espaço reservado `{default}` representa o nome de domínio qualificado configurado como padrão para o projeto.

  Por exemplo, um projeto com o domínio padrão `example.com` e os seguintes modelos de rota:

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  Esses modelos resolvem para os seguintes URLs em um ambiente de produção:

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- O espaço reservado `{all}` representa todos os nomes de domínio configurados para o projeto.

  Por exemplo, um projeto com domínios `example.com` e `example1.com` com os seguintes modelos de rota:

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  Esses modelos resolvem as seguintes rotas para todos os domínios no projeto:

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  O espaço reservado `{all}` é útil para projetos configurados para vários domínios. Em uma ramificação de não produção, `{all}` é substituído pela ID do projeto e pela ID do ambiente para cada domínio.

  Se um projeto não tiver domínios configurados, o que é comum durante o desenvolvimento, o espaço reservado `{all}` se comporta da mesma forma que o espaço reservado `{default}`.

O Adobe Commerce também gera rotas para cada ambiente de integração ativo. Para ambientes de integração, o espaço reservado `{default}` é substituído pelo seguinte nome de domínio:

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

Por exemplo, a ramificação `refactorcss` do projeto `mswy7hzcuhcjw` hospedado no cluster `us` tem o seguinte domínio:

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Se o projeto na nuvem tiver suporte para vários armazenamentos, siga as instruções de configuração de rota para [vários sites ou armazenamentos](../store/multiple-sites.md).

### Barra à direita

As definições de rota contêm uma barra à direita para indicar uma pasta ou diretório; no entanto, o mesmo conteúdo pode ser distribuído com ou sem uma barra à direita. As seguintes URLs resolvem o mesmo, mas podem ser interpretadas como _duas URLs_ diferentes:

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>É uma prática recomendada usar uma barra à direita para diretórios, mas seja qual for o método escolhido, é importante **permanecer consistente** para evitar a geração de dois locais.

## Protocolos de rota

Todos os ambientes são compatíveis com HTTP e HTTPS automaticamente.

- Se a configuração especificar apenas a rota HTTP, as rotas HTTPS serão criadas automaticamente, permitindo que o site seja distribuído a partir de HTTP e HTTPS sem exigir redirecionamentos.

  Por exemplo, um projeto com o domínio padrão `example.com` e o seguinte modelo de rota:

  ```text
  http://{default}/
  ```

  Esse modelo resolve os seguintes URLs:

  ```text
  http://example.com/
  
  https://example.com/
  ```

- Se a configuração especificar apenas a rota HTTPS, todas as solicitações HTTP serão redirecionadas para HTTPS.

  Por exemplo, um projeto com o domínio padrão `example.com` com o seguinte modelo de rota:

  ```text
  https://{default}/
  ```

  Esse modelo resolve para o seguinte URL:

  ```text
  https://example.com/
  ```

  Ele também processa o seguinte redirecionamento:

  `http://example.com/` ==> `https://example.com/`

Servir todas as páginas por TLS. Para essa configuração, você deve configurar redirecionamentos para todas as solicitações não criptografadas para o equivalente TLS usando um dos seguintes métodos:

- Altere o protocolo para HTTPS no arquivo `routes.yaml`.

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- Para ambientes de Preparo e Produção, habilite a opção [Forçar TLS no Fastly](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html) na interface do Administrador. Quando você usa essa opção, o Fastly lida com o redirecionamento para HTTPS, para que você não precise atualizar a configuração `routes.yaml`.

## Opções de roteiro

Configure cada rota separadamente usando as seguintes propriedades:

| Propriedade | Descrição |
| ---------------- | ----------- |
| `type: upstream` | Serve um aplicativo. Além disso, tem uma propriedade `upstream` que especifica o nome do aplicativo (conforme definido em `.magento.app.yaml`) seguido pelo ponto de extremidade `:http`. |
| `type: redirect` | Redireciona para outra rota. Ela é seguida pela propriedade `to`, que é um redirecionamento HTTP para outra rota identificada por seu modelo. |
| `cache:` | Controla o [armazenamento em cache para a rota](caching.md). |
| `redirects:` | Controla [regras de redirecionamento](redirects.md). |
| `ssi:` | Habilitação de controles de [Server Side Includes](server-side-includes.md). |

## Rotas simples

Nos exemplos a seguir, a configuração de rota roteia o domínio apex e o subdomínio `www` para o aplicativo `mymagento`. Essa rota não redireciona solicitações HTTPS.

**Exemplo 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

Neste exemplo, o roteamento de solicitações segue estas regras:

- O servidor responde diretamente às solicitações com o seguinte padrão de URL:

  ```text
  http://example.com/path
  ```

- O servidor emite um _redirecionamento_ de 301 para solicitações com o seguinte padrão de URL:

  ```text
  http://www.example.com/mypath
  ```

  Essas solicitações redirecionam para o domínio apex, por exemplo:

  ```text
  http://example.com/mypath
  ```

No exemplo a seguir, a configuração de rota não redireciona URLs do domínio www para o domínio apex. Em vez disso, as solicitações são atendidas nos domínios www e apex.

**Exemplo 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## Rotas curingas

O Adobe Commerce na infraestrutura em nuvem oferece suporte a rotas curingas, para que você possa mapear vários subdomínios para o mesmo aplicativo. Isso funciona para rotas de redirecionamento e upstream. Você coloca um asterisco (\*) como prefixo na rota. Por exemplo, as seguintes rotas para o mesmo aplicativo:

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

Isso funciona como um domínio abrangente em um ambiente ativo.

### Roteamento de um domínio não mapeado

Você pode rotear para um sistema que não esteja mapeado para um domínio usando um ponto (`.`) para separar o subdomínio.

**Exemplo:**

Um projeto com uma ramificação `add-theme` é roteado para a seguinte URL:

```text
http://add-theme-projectID.us.magento.com/
```

Se você definir o seguinte modelo de roteiro:

```text
http://www.{default}/
```

A rota é resolvida para o seguinte URL:

```text
http://www.add-theme-projectID.us.magento.com/
```

Você pode inserir qualquer subdomínio antes do ponto e da rota ser resolvida.

**Exemplo:**

Defina o seguinte modelo de roteiro:

```text
http://*.{default}/
```

Esse modelo resolve ambos os URLs a seguir:

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

Você pode visualizar o padrão de rota para domínios não mapeados estabelecendo uma conexão SSH com o ambiente e usando a CLI `magento-cloud` para listar as rotas:

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## Redirecionamentos e armazenamento em cache

Conforme discutido com mais detalhes em [Redirecionamentos](redirects.md), você pode gerenciar regras de redirecionamento complexas, como _redirecionamentos parciais_, e especificar regras para o [armazenamento em cache](caching.md) baseado em rota:

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
