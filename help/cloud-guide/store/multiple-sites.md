---
title: Configurar vários sites ou lojas
description: Saiba como configurar vários sites ou lojas para o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Configurar vários sites ou lojas

Você pode configurar o Adobe Commerce para ter vários sites ou lojas, como uma loja em inglês, uma loja em francês e uma loja em alemão. Consulte [Noções básicas sobre sites, lojas e exibições de loja](best-practices.md#store-views).

>[!WARNING]
>
>Os dados do catálogo se expandem à medida que você aumenta o número de sites e lojas. Dependendo da arquitetura do projeto, os armazenamentos adicionais podem levar a um processo de indexação mais longo e tempos de resposta mais lentos para páginas de catálogo não armazenadas em cache. A Adobe recomenda que você monitore o desempenho do site de perto.

O processo para configurar vários armazenamentos depende da sua escolha entre usar domínios exclusivos ou compartilhados.

Vários armazenamentos com domínios exclusivos:

```
https://first.store.com/
https://second.store.com/
```

Vários armazenamentos com o mesmo domínio:

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Para adicionar uma visualização de loja à URL base do site, não é necessário criar vários diretórios. Consulte [Adicionar o código de armazenamento à URL de base](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=pt-BR) no _Guia de Configuração_.

## Adicionar domínios

Domínios personalizados podem ser adicionados ao Pro Staging e a qualquer ambiente de produção; eles não podem ser adicionados a ambientes de integração.

O processo para adicionar um domínio depende do tipo de conta da Cloud:

- Para preparo e produção profissionais

  Adicione o novo domínio ao Fastly, consulte [Gerenciar domínios](../cdn/fastly-custom-cache-configuration.md#manage-domains) ou abra um tíquete de suporte para solicitar assistência. Além disso, você deve [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) para solicitar que novos domínios sejam adicionados a um cluster.

- Somente para produção inicial

  Adicione o novo domínio ao Fastly, consulte [Gerenciar domínios](../cdn/fastly-custom-cache-configuration.md#manage-domains) ou [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) para solicitar assistência. Além disso, você deve adicionar o novo domínio à guia **Domínios** em [!DNL Cloud Console]: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Configurar instalação local

Para configurar sua instalação local para usar várias lojas, consulte [Vários sites ou lojas](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html?lang=pt-BR) no _Guia de Configuração_.

Depois de criar e testar com êxito a instalação local para usar várias lojas, você deve preparar seu ambiente de integração:

1. **Configurar rotas ou locais**—especifique como as URLs de entrada são tratadas pela Adobe Commerce

   - [Rotas para domínios separados](#configure-routes-for-separate-domains)
   - [Locais para domínios compartilhados](#configure-locations-for-shared-domains)

1. **Configurar sites, lojas e visualizações de loja**—configure usando a interface do Administrador do Adobe Commerce
1. **Modificar variáveis** — especifique os valores das variáveis `MAGE_RUN_TYPE` e `MAGE_RUN_CODE` no arquivo `magento-vars.php`
1. **Implantar e testar ambientes**—implantar e testar a ramificação `integration`

>[!TIP]
>
>Você pode usar um ambiente local para configurar vários sites ou lojas. Consulte as instruções do Cloud Docker para [Configurar vários sites ou lojas](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites).

### Atualizações de configuração para ambientes Pro

{{pro-self-service-warning}}

### Configurar rotas para domínios separados

As rotas definem como processar URLs de entrada. Vários armazenamentos com domínios exclusivos exigem que você defina cada domínio no arquivo `routes.yaml`. A maneira como você configura as rotas depende de como você deseja que o site funcione.

**Para configurar rotas em um ambiente de integração**:

1. Na estação de trabalho local, abra o arquivo `.magento/routes.yaml` em um editor de texto.

1. Defina o domínio e os subdomínios. O valor upstream `mymagento` é o mesmo valor que a propriedade de nome no arquivo `.magento.app.yaml`.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Salve as alterações no arquivo `routes.yaml`.

1. Continue para [Configurar sites, lojas e exibições de loja](#set-up-websites-stores-and-store-views).

### Configurar locais para domínios compartilhados

Onde a configuração de rotas define como as URLs são processadas, a propriedade `web` no arquivo `.magento.app.yaml` define como o aplicativo é exposto à Web. Os _locais_ da Web permitem mais granularidade para solicitações recebidas. Por exemplo, se o seu domínio for `store.com`, você poderá usar `/first` (site padrão) e `/second` para solicitações a dois armazenamentos diferentes que compartilham um domínio.

**Para configurar um novo local da Web**:

1. Crie um alias para a raiz (`/`). Neste exemplo, o alias é `&app` na linha 3.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Crie uma passagem para o site (`/website`) e faça referência à raiz usando o alias da etapa anterior.

   O alias permite que `website` acesse valores do local raiz. Neste exemplo, o site `passthru` está na linha 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**Para configurar um local com um diretório diferente**:

1. Crie um alias para os locais raiz (`/`) e estático (`/static`).

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Crie um subdiretório para o site no diretório `pub`: `pub/<website>`

1. Copie o arquivo `pub/index.php` no diretório `pub/<website>` e atualize o caminho `bootstrap` (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Crie uma passagem para o arquivo `index.php`.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Confirme e envie por push os arquivos alterados.

   - `pub/<website>/index.php` (Se este arquivo estiver em `.gitignore`, talvez o push exija a opção force.)
   - `.magento.app.yaml`

### Configurar sites, lojas e visualizações de loja

Na _Interface do Administrador_, configure os **Sites**, as **Lojas** e as **Exibições da Loja** do Adobe Commerce. Consulte [Configurar vários sites, lojas e exibições de loja no Administrador](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=pt-BR) no _Guia de Configuração_.

É importante usar o mesmo nome e código de seus sites, lojas e exibições de loja do Administrador ao configurar a instalação local. Você precisa desses valores ao atualizar o arquivo `magento-vars.php`.

### Modificar variáveis

Em vez de configurar um host virtual NGINX, passe as variáveis `MAGE_RUN_CODE` e `MAGE_RUN_TYPE` usando o arquivo `magento-vars.php` no diretório raiz do projeto.

**Para transmitir variáveis usando o `magento-vars.php` arquivo**:

1. Abra o arquivo `magento-vars.php` em um editor de texto.

   O [arquivo `magento-vars.php` padrão](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) deve ser semelhante ao seguinte:

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Mover o bloco `if` comentado para que fique _depois_ do bloco `function` e não seja mais comentado.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Substitua os seguintes valores no bloco `if (isHttpHost("example.com"))`:
   - `example.com`—com a URL de base do seu _site_
   - `default`—com o CÓDIGO exclusivo do seu _site_ ou _exibição de loja_
   - `store`—com um dos seguintes valores:
      - `website`—carrega o _site_ na vitrine
      - `store`—carregar uma _exibição de loja_ na loja

   Para vários sites usando domínios exclusivos:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   Para vários sites com o mesmo domínio, você precisa verificar o _host_ e o _URI_:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Salve as alterações no arquivo `magento-vars.php`.

### Implantar e testar no servidor de integração

Envie suas alterações para o ambiente de integração do Adobe Commerce na infraestrutura em nuvem e teste seu site.

1. Adicionar, confirmar e enviar alterações de código à ramificação remota.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Aguarde a conclusão da implantação.

1. Após a implantação, abra o URL da loja em um navegador da web.

   Com um domínio exclusivo, use o formato: `http://<magento-run-code>.<site-URL>`

   Por exemplo, `http://french.master-name-projectID.us.magentosite.cloud/`

   Com um domínio compartilhado, use o formato: `http://<site-URL>/<magento-run-code>`

   Por exemplo, `http://master-name-projectID.us.magentosite.cloud/french/`

1. Teste seu site completamente e mescle o código à ramificação `integration` para implantação adicional.

## Implantar para preparo e produção

Siga o processo de implantação para [implantação em Preparo e Produção](../deploy/staging-production.md). Para ambientes Starter e Pro, você usa o [!DNL Cloud Console] para enviar código por push entre ambientes.

A Adobe recomenda fazer testes completos no ambiente de preparo antes de enviá-los para o ambiente de produção. Faça alterações de código no ambiente de integração e comece o processo de implantação nos ambientes novamente.

