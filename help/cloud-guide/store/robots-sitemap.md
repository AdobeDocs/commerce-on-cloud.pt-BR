---
title: Adicionar mapa do site e robôs de mecanismo de pesquisa
description: Saiba como adicionar o mapa do site e os robôs do mecanismo de pesquisa ao Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1d52481fb6874f3a9ba14b0ff4fe39dc7d564938
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# Adicionar mapa do site e robôs de mecanismo de pesquisa

Uma tentativa de gerar e gravar o arquivo `sitemap.xml` no diretório raiz resulta no seguinte erro:

```
Please make sure that "/" is writable by the web-server.
```

Com o Adobe Commerce na infraestrutura de nuvem, você só pode gravar em diretórios específicos, como `var`, `pub/media`, `pub/static` ou `app/etc`. Ao gerar o arquivo `sitemap.xml` usando o painel Administrador, você deve especificar o caminho `/media/`.

Você não precisa gerar um arquivo `robots.txt` porque ele gera o conteúdo `robots.txt` sob demanda e o armazena no banco de dados. Você pode exibir o conteúdo em seu navegador com o link `<domain.your.project>/robots.txt` ou `<domain.your.project>/robots`.

Isso requer o ECE-Tools versão 2002.0.12 e posterior com um arquivo `.magento.app.yaml` atualizado. Veja um exemplo dessas regras no [repositório magento-cloud](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Para gerar um arquivo `sitemap.xml` na versão 2.2 e posterior**:

1. Acesse o Admin.
1. No menu _Marketing_, clique em **Mapa do Site** na seção _SEO e Pesquisa_.
1. Na exibição _Mapa do Site_, clique em **Adicionar Mapa do Site**.
1. Na exibição _Novo Mapa de Site_, insira os seguintes valores:

   - **Nome do arquivo**:`sitemap.xml`
   - **Caminho**:`/media/`

1. Clique em **Salvar e gerar**. O novo mapa do site fica disponível na grade _Mapa do Site_.
1. Clique no caminho na coluna _Link for Google_.

**Para adicionar conteúdo ao `robots.txt` arquivo**:

1. Acesse o Admin.
1. No menu _Conteúdo_, clique em **Configuração** na seção _Design_.
1. No modo de exibição _Configuração de Design_, clique em **Editar** para o site na coluna _Ação_.
1. Na exibição _Site Principal_, clique em **Robôs do Mecanismo de Pesquisa**.
1. Atualize a **Editar instrução personalizada do campo robots.txt**.
1. Clique em **Salvar configuração**.
1. Verifique o arquivo `<domain.your.project>/robots.txt` ou a URL `<domain.your.project>/robots` no navegador.

>[!NOTE]
>
>Se o arquivo `<domain.your.project>/robots.txt` gerar um `404 error`, [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) para remover o redirecionamento de `/robots.txt` para `/media/robots.txt`.

## Regravar usando o trecho Fastly VCL

Se você tiver domínios diferentes e precisar de mapas de site separados, poderá criar uma VCL para rotear para o mapa de site apropriado. Gere o arquivo `sitemap.xml` no painel Admin como descrito acima e crie um trecho Fastly VCL personalizado para gerenciar o redirecionamento. Consulte [Fragmentos de VCL personalizados](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Você pode fazer upload de trechos de VCL personalizados na interface do usuário do Administrador ou usando a API do Fastly. Consulte [Exemplos e tutoriais de trechos de VCL personalizados](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Use um trecho Fastly VCL para redirecionar

Crie um trecho de VCL personalizado para reescrever o caminho de `sitemap.xml` para `/media/sitemap.xml` usando os pares de valores-chave `type` e `content`.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

O exemplo a seguir demonstra como reescrever o caminho de `robots.txt` e `sitemap.xml` para `/media/robots.txt` e `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Para usar um trecho de VCL do Fastly para um redirecionamento de domínio específico**:

Crie um arquivo `pub/media/domain_robots.txt`, onde o domínio seja `domain.com`, e use o próximo trecho VCL:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

O trecho VCL roteia `http://domain.com/robots.txt` e apresenta o arquivo `pub/media/domain_robots.txt`.

Para configurar um redirecionamento para `robots.txt` e `sitemap.xml` em um único trecho, crie arquivos `pub/media/domain_robots.txt` e `pub/media/domain_sitemap.xml`, em que o domínio seja `domain.com`, e use o próximo trecho VCL:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

Na configuração do administrador `sitemap`, você deve especificar o local do arquivo usando `pub/media/` em vez de `/`.

### Configurar indexação por mecanismo de pesquisa

Para ativar as personalizações do `robots.txt` na Produção, habilite a indexação pelos mecanismos de pesquisa para a opção `<environment-name>` nas configurações do projeto no Cloud Console:

- Console da Nuvem Herdado — o URL segue o padrão `https://<region-id>.magento.cloud/projects/<project_id>`

  Alterne a configuração [!UICONTROL Indexing by search engines] (Console Herdado) [!UICONTROL Hide from search engines] (Console Adobe) para **Ligado**.

  ![Usar o [!DNL Cloud Console] para gerenciar ambientes](../../assets/robots-indexing-by-search-engine.png)

- Adobe Cloud Console — o URL segue o padrão ``https://console.adobecommerce.com/<username>/<project_id>``

  Desmarque a configuração [!UICONTROL Hide from search engines].

- Você também pode usar a Magento-Cloud CLI para atualizar esta configuração:

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- A indexação por mecanismos de pesquisa só pode ser ativada na Produção, mas não em nenhum dos ambientes inferiores.
>
>- Se você estiver usando o PWA Studio e não puder acessar o arquivo `robots.txt` configurado, adicione `robots.txt` ao [Incluo na lista de permissões de Nome da Frente](https://github.com/magento/magento2-upward-connector#front-name-allowlist) em **Lojas** > Configuração > **Geral** > **Web** > Configuração ASCENDENTE do PWA.

