---
title: VCL personalizado para solicitações de bloqueio
description: Bloqueie solicitações recebidas pelo endereço IP usando uma lista de controle de acesso (ACL) do Edge com um trecho de VCL personalizado.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# VCL personalizado para solicitações de bloqueio

Você pode usar o módulo Fastly CDN para o Magento 2 a fim de criar uma ACL do Edge com uma lista de endereços IP que você deseja bloquear. Em seguida, você pode usar essa lista com um trecho VCL para bloquear solicitações recebidas. O código verifica o endereço IP da solicitação recebida. Se ele corresponder a um endereço IP incluído na lista de ACL, o Fastly bloqueará o acesso ao site e retornará um `403 Forbidden error`. Todos os outros endereços IP de clientes têm acesso permitido.

**Pré-requisitos:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Lista de endereços IP de clientes a serem bloqueados

## Criar Edge ACL para bloquear endereços IP de clientes

Você cria uma ACL do Edge para definir a lista de endereços IP a serem bloqueados. Depois de criar a ACL, você pode usá-la em um trecho de VCL personalizado para gerenciar o acesso ao site de Preparo ou Produção.

Gerencie o acesso para sites de Preparo e Produção criando a ACL do Edge com o mesmo nome em ambos os ambientes. O código de trecho VCL se aplica a ambos os ambientes.

1. Faça logon no Administrador.
1. Navegue até **Lojas** > Configurações > **Configuração** > **Avançado** > **Sistema** > **Cache de Página Inteira** > **Configuração Rápida**.
1. Expanda a seção **Edge ACL**.
1. Clique em **Adicionar ACL** para criar uma lista. Para este exemplo, nomeie a lista &quot;incluir na lista de bloqueios arquivo&quot;.
1. Insira os valores do endereço IP na lista. Todos os endereços IP de cliente adicionados a esta lista estão bloqueados e não podem acessar o site.
1. Opcionalmente, marque a caixa de seleção **Negado**, se necessário.

Você faz referência à ACL do Edge pelo nome em seu código de trecho de VCL.

## Incluir na lista de bloqueios Criar o VCL personalizado para o arquivo de pesquisa

>[!NOTE]
>
>Este exemplo mostra aos usuários avançados como criar um trecho de código VCL para configurar regras de bloqueio personalizadas para fazer upload no serviço Fastly. Você pode configurar um incluir na lista de bloqueios incluo na lista de permissões de classificação ou de classificação com base no país do administrador da Adobe Commerce usando o recurso [Bloqueio](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) disponível no módulo Fastly CDN for Magento 2.

Depois de definir a ACL do Edge, você pode usá-la para criar o trecho VCL para bloquear o acesso aos endereços IP especificados na ACL. Você pode usar o mesmo trecho de VCL em ambientes de Preparo e Produção, mas deve fazer upload do trecho para cada ambiente separadamente.

O seguinte código de trecho de VCL personalizado (formato JSON) mostra a lógica para bloquear solicitações recebidas com um endereço IP de cliente que corresponda a um endereço na ACL de inclui na lista de bloqueios.

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

Antes de criar um trecho com base neste exemplo, revise os valores para determinar se você precisa fazer alterações:

- `name`: Nome do trecho VCL. Neste exemplo, usamos o nome `blocklist`.

- `priority`: Determina quando o trecho VCL é executado. A prioridade é `5` para executar imediatamente e verificar se uma solicitação de administrador vem de um endereço IP permitido. O trecho é executado antes de qualquer um dos trechos de VCL de Magento padrão (`magentomodule_*`) ter uma prioridade 50. Defina a prioridade para cada trecho personalizado acima ou abaixo de 50, dependendo de quando você deseja que seu trecho seja executado. Os trechos com números de prioridade mais baixa são executados primeiro.

- `type`: Especifica o tipo de trecho de VCL que determina o local do trecho no código de VCL gerado. Neste exemplo, usamos `recv`, que insere o código de VCL na sub-rotina `vcl_recv`, abaixo do VCL padrão e acima de qualquer objeto. Consulte a [Referência de trecho Fastly VCL](https://docs.fastly.com/api/config#api-section-snippet) para obter a lista de tipos de trecho.

- `content`: o trecho de código VCL a ser executado, que verifica o endereço IP do cliente. Se o IP estiver na ACL do Edge, o acesso será bloqueado com um erro `403 Forbidden` para todo o site. Todos os outros endereços IP de clientes têm acesso permitido.

Depois de revisar e atualizar o código do seu ambiente, use um dos métodos a seguir para adicionar o trecho de VCL personalizado à configuração do serviço Fastly:

- [Adicionar o trecho de VCL personalizado do Administrador](#add-the-custom-vcl-snippet). Esse método é recomendado se você puder acessar o Administrador. (Exige o [Fastly versão 1.2.58](fastly-configuration.md#upgrade-fastly-module) ou posterior.)

- Salve o exemplo de código JSON em um arquivo (por exemplo, `blocklist.json`) e [carregue-o usando a API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Use esse método se não conseguir acessar o Administrador.

## Adicionar o trecho de VCL personalizado

{{admin-login-step}}

1. Clique em **Lojas** > Configurações > **Configuração** > **Avançado** > **Sistema**.

1. Expanda **Cache de Página Inteira** > **Configuração Rápida** > **Trechos de VCL Personalizados**.

1. Clique em **Criar trecho personalizado**.

1. Adicione os valores do trecho de VCL:

   - **Nome** — `blocklist`

   - **Tipo** — `recv`

   - **Prioridade** — `5`

   - Adicionar o conteúdo do trecho **VCL**:

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. Clique em **Criar** para gerar o arquivo de trecho VCL com o padrão de nome `type_priority_name.vcl`, por exemplo `recv_5_blocklist.vcl`

1. Depois que a página for recarregada, clique em **Fazer upload do VCL para o Fastly** na seção *Configuração do Fastly* para adicionar o arquivo à configuração do serviço Fastly.

1. Após os uploads, atualize o cache de acordo com a notificação na parte superior da página.

O Fastly valida a versão atualizada do código do VCL durante o processo de upload. Se a validação falhar, edite o trecho de VCL personalizado para corrigir o problema. Em seguida, carregue o VCL novamente.

## Exemplos adicionais de VCL para solicitações de bloqueio

Os exemplos a seguir mostram como bloquear solicitações usando instruções condition em vez de uma lista ACL.

>[!WARNING]
>
>Nesses exemplos, o código do VCL é formatado como uma carga JSON que pode ser salva em um arquivo e enviada em uma solicitação da API Fastly. Você pode enviar o trecho [VCL de Admin](#add-the-custom-vcl-snippet) ou como uma sequência de caracteres JSON usando a API Fastly. Para evitar erros de validação ao usar a API Fastly com uma string JSON, é necessário usar uma barra invertida para escapar caracteres especiais.

>[!NOTE]
>Se você estiver enviando o trecho de VCL do Administrador, extraia os valores individuais do código de VCL de amostra e insira-os nos campos correspondentes. Por exemplo:
>- Nome: `<name of the VCL>`
>- Dinâmico: `<0/1>`
>- Tipo: `<type>`
>- Prioridade: `<priority>`
>- Conteúdo: `<content>`

Consulte [Uso de trechos de VCL dinâmicos](https://docs.fastly.com/vcl/vcl-snippets/) na documentação do Fastly VCL.

### Amostra de código VCL: Bloco por código de país

Este exemplo usa o código de país ISO 3166-1 de dois caracteres para o país associado ao endereço IP.

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>Em vez de usar um trecho de VCL personalizado, você pode usar o recurso [Bloqueio](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) do Fastly no Admin do Adobe Commerce na infraestrutura da nuvem para configurar o bloqueio por código de país ou por uma lista de códigos de país.

### Amostra de código VCL: cabeçalho de solicitação Bloquear por Agente do Usuário HTTP

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
