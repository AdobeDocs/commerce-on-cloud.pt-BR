---
title: Introdução a trechos de VCL personalizados
description: Saiba mais sobre como usar trechos de código da Linguagem de controle do Vernish para personalizar a configuração do serviço Fastly para o Adobe Commerce.
feature: Cloud, Configuration, Services
exl-id: 90f0bea6-4365-4657-94e9-92a0fd1145fd
source-git-commit: a51946f65ccd606cde6fbb4278f625a49ae42dad
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 0%

---

# Introdução ao VCL personalizado

O Fastly oferece suporte a uma versão personalizada do Varnish Configuration Language (VCL) para adaptar a configuração do serviço Fastly às suas necessidades.

Trechos de VCL personalizados são blocos de lógica de VCL adicionados à versão ativa do VCL carregada no site do Adobe Commerce. Um trecho de VCL personalizado modifica como os serviços de cache do Fastly respondem ao tráfego de solicitação. Por exemplo, você pode adicionar um trecho de VCL personalizado para permitir o tráfego de solicitação somente de endereços IP de clientes especificados. Ou crie um trecho para bloquear o tráfego de sites conhecidos por enviar spam de referência para seus sites do Adobe Commerce.

Os snippets de VCL personalizados — gerados, compilados e transmitidos para todos os caches do Fastly — são carregados e ativados sem tempo de inatividade do servidor.

>[!NOTE]
>
>Antes de adicionar código VCL personalizado, dicionários de borda e ACLs à configuração do módulo Fastly, verifique se o serviço de cache do Fastly funciona com a configuração padrão. Consulte [Configurar serviços do Fastly](fastly-configuration.md).

O Fastly suporta dois tipos de snippets de VCL personalizados:

- [Trechos regulares](https://docs.fastly.com/en/guides/about-vcl-snippets) — Os trechos regulares personalizados de VCL são codificados para versões específicas de VCL. Você pode criar, modificar e implantar trechos de VCL regulares a partir da API de Administração ou do Fastly.

- [Trechos dinâmicos](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) — trechos de VCL criados usando a API Fastly. Você pode modificar e implantar trechos dinâmicos sem precisar atualizar a versão do Fastly VCL para o seu serviço.

Recomendamos o uso de trechos de VCL personalizados com Dicionários Edge e Listas de Controle de Acesso (ACL) para armazenar dados usados em seu código personalizado.

- [**Dicionário Edge**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries) — Armazena dados como pares de valores-chave em um contêiner de dicionário que pode ser referenciado de trechos de VCL personalizados

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) — Armazena os dados do endereço IP do cliente que definem a lista de controle de acesso para regras de bloqueio ou permissão implementadas usando trechos VCL personalizados

O dicionário e os dados de ACL são implantados nos nós do Fastly Edge acessíveis em regiões de rede. Além disso, os dados podem ser atualizados dinamicamente pela rede sem exigir a reimplantação do código de VCL para seu ambiente de preparo ou produção.

>[!NOTE]
>
>Você só poderá adicionar trechos de VCL personalizados a um ambiente de Preparo ou Produção se tiver [serviços do Fastly](fastly-configuration.md) configurados para esse ambiente.

## Tutorial

Este tutorial e exemplos demonstram o uso de trechos de VCL personalizados regulares com Dicionários do Edge e ACLs do Edge para personalizar a configuração do serviço Fastly para o Adobe Commerce. Para obter informações mais detalhadas, consulte a documentação do Fastly:

- [Guia do Fastly VCL](https://docs.fastly.com/guides/vcl/guide-to-vcl) — Informações sobre a implementação do Fastly Varnish, extensões do Fastly VCL e recursos para saber mais sobre o Varnish e o VCL.
- [Referência de VCL do Fastly](https://docs.fastly.com/guides/vcl/) — Referência de programação detalhada para desenvolver e solucionar problemas de VCL personalizado do Fastly e trechos de VCL personalizados.

Você pode criar e gerenciar trechos de VCL personalizados no Administrador do Adobe Commerce ou usando a API do Fastly:

- [Administrador do Adobe Commerce](#manage-custom-vcl-from-admin) — Recomendamos usar o Administrador do Adobe Commerce para gerenciar trechos de VCL personalizados porque ele automatiza o processo para validar, carregar e aplicar as alterações de VCL à configuração do serviço Fastly. Além disso, você pode visualizar e editar os trechos de VCL personalizados adicionados à configuração do serviço Fastly no Admin.

- [API Fastly](#manage-vcl-using-the-api) — Se você não conseguir acessar o Administrador, use a API Fastly para gerenciar trechos de VCL personalizados. Por exemplo, use a API para solucionar problemas de configuração do serviço Fastly quando o site estiver inativo ou para adicionar um trecho de VCL personalizado. Além disso, algumas operações só podem ser concluídas usando a API. Por exemplo, você deve usar a API para reativar uma versão de VCL mais antiga ou para exibir todos os trechos de VCL incluídos em uma versão de VCL especificada. Consulte [Referência rápida da API para trechos de VCL](#api-quick-reference-for-vcl-snippets).

### Exemplo de código de trecho VCL

O exemplo a seguir mostra o trecho de VCL personalizado (formato JSON) que filtra o tráfego por endereço IP do cliente:

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>Neste exemplo, o código do VCL é formatado como uma carga JSON que pode ser salva em um arquivo e enviada em uma solicitação da API Fastly. Para evitar erros de validação de JSON ao enviar o trecho como JSON para uma solicitação de API, use uma barra invertida para escapar caracteres especiais no código. Consulte [Uso de trechos de VCL dinâmicos](https://docs.fastly.com/vcl/vcl-snippets/) na documentação do Fastly VCL. Se você enviar o trecho de VCL do Admin, não será necessário escapar caracteres especiais.

A lógica do VCL no campo `content` executa as seguintes ações:

- Verifica o endereço IP de entrada, `client.ip` em cada solicitação

- Bloqueia qualquer solicitação com um endereço IP incluído na ACL de borda do *ACLNAME*, retornando um erro `403 Forbidden`

A tabela a seguir fornece detalhes sobre os dados principais para trechos de VCL personalizados. Para obter uma referência mais detalhada, consulte a referência [trechos de VCL](https://docs.fastly.com/api/config#api-section-snippet) na documentação do Fastly.

| Valor | Descrição |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | A chave de API para acessar a conta do Fastly. Consulte [Obter credenciais](fastly-configuration.md). |
| `active` | Status ativo do trecho ou da versão. Retorna `true` ou `false`. Se verdadeiro, o trecho ou a versão está em uso. Clonar um trecho ativo usando seu número de versão. |
| `content` | O trecho de código VCL a ser executado. O Fastly não é compatível com todos os recursos de linguagem de VCL. Além disso, o Fastly fornece extensões com funcionalidade personalizada. Para obter detalhes sobre os recursos com suporte, consulte a [Referência de programação do Fastly VCL](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | Status dinâmico de um trecho. Retorna `false` para [trechos regulares](https://docs.fastly.com/en/guides/about-vcl-snippets) incluídos no VCL com versão para a configuração do serviço Fastly. Retorna `true` para um [trecho dinâmico](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) que pode ser modificado e implantado sem a necessidade de uma nova versão de VCL. |
| `number` | Número da versão do VCL em que o trecho está incluído. O Fastly usa a *Versão Editável nº* em seus valores de exemplo. Se você adicionar trechos personalizados da API, inclua o número da versão na solicitação da API. Se você adicionar um VCL personalizado do Administrador, a versão será fornecida para você. |
| `priority` | Valor numérico de `1` a `100` que especifica quando o código de trecho de VCL personalizado é executado. Os trechos com valores de prioridade mais baixos são executados primeiro. Se não for especificado, o valor de `priority` assumirá `100` como padrão.<p>Incluir na lista de permissões Qualquer trecho de VCL personalizado com valor de prioridade `5` é executado imediatamente, o que é melhor para o código de VCL que implementa o roteamento de solicitações (bloqueia e alerta e redireciona). A prioridade `100` é melhor para substituir o código de trecho de VCL padrão.<p>Todos os [trechos de VCL padrão](fastly-configuration.md#upload-vcl-snippets) incluídos no módulo Magento-Fastly têm `priority=50`.<ul><li>Atribua uma prioridade alta como `100` para executar o código VCL personalizado após todas as outras funções VCL e substituir o código VCL padrão.</li></ul> |
| `service_id` | A ID de serviço do Fastly para um ambiente específico de Preparo ou Produção. Essa ID é atribuída quando seu projeto é adicionado à conta de serviço do Adobe Commerce na infraestrutura em nuvem [Fastly](fastly.md#fastly-service-account-and-credentials). |
| `type` | Especifica o local para inserir o trecho gerado, como `init` (acima das sub-rotinas) e `recv` (dentro das sub-rotinas). Para obter detalhes, consulte a referência do Fastly [VCL snippets](https://docs.fastly.com/api/config#api-section-snippet). |

## Gerenciar VCL personalizado do Administrador

Você pode [adicionar trechos de VCL personalizados](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) da seção *Configuração do Fastly* > *Trechos de VCL Personalizados* no Administrador.

![Gerenciar trechos de VCL personalizados](../../assets/cdn/fastly-edit-snippets.png)

A exibição *Fragmentos de VCL personalizados* mostra somente fragmentos que foram adicionados por meio do Administrador. Se fragmentos forem adicionados usando a API Fastly, use a API para [gerenciá-los](#manage-vcl-using-the-api).

Os seguintes exemplos mostram como criar e gerenciar trechos de VCL personalizados no Admin e usar módulos do Fastly Edge e dicionários do Edge:

- [Redirecionar solicitações para um back-end do CMS](fastly-vcl-wordpress.md)
- [Bloquear spam de referência](fastly-vcl-badreferer.md)
- [Bloquear spam de referência](fastly-vcl-badreferer.md)
- [VCL Personalizado para inclui na lista de permissões de IP](fastly-vcl-allowlist.md)
- [VCL Personalizado para inclui na lista de bloqueios de IP](fastly-vcl-blocking.md)
- [Ignorar cache Fastly](fastly-vcl-bypass-to-origin.md)

## Trechos que não podem ser visualizados/modificados pelo administrador do Commerce

Não é possível visualizar ou modificar alguns trechos diretamente no Administrador do Commerce. Por exemplo, [trechos dinâmicos](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets). Na seção Trechos de VCL Personalizados, você não verá trechos que foram adicionados pela equipe de Suporte na Nuvem diretamente ao [Painel de gerenciamento do Fastly](fastly.md#fastly-service-account-and-credentials).


**Para observar os trechos adicionados pela equipe de Suporte na Nuvem:**

1. Vá para a seção **Ferramentas**.

1. Clique em **Listar todas as versões** ao lado de _Histórico de Versões_.

1. Clique no ícone de olho ao lado da Versão VCL aplicável para exibir os trechos existentes.


## Gerenciar VCL usando a API

A apresentação a seguir mostra como criar arquivos de trecho VCL comuns e adicioná-los à configuração do serviço Fastly usando a API Fastly. Você pode criar e gerenciar os trechos do aplicativo *terminal*. Você não precisa de uma conexão SSH em um ambiente específico.

**Pré-requisitos:**

- Configure seu ambiente Adobe Commerce na infraestrutura em nuvem para os serviços Fastly. Consulte [Configurar Fastly](fastly-configuration.md).

- [Obter credenciais da API Fastly](fastly-configuration.md) para autenticar solicitações na API Fastly. Obtenha as credenciais para o ambiente correto: Preparo ou Produção.

- Salve as credenciais de serviço do Fastly como variáveis de ambiente bash que podem ser usadas nos comandos cURL:

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  As variáveis de ambiente exportadas estão disponíveis somente na sessão bash atual e são perdidas quando você fecha o terminal. É possível redefinir variáveis exportando um novo valor. Para exibir a lista de variáveis exportadas relacionadas ao Fastly:

  ```bash
  export | grep FASTLY
  ```

## Adicionar trechos de VCL

Este tutorial fornece as etapas básicas para adicionar trechos personalizados usando a API do Fastly.

>[!NOTE]
>
>Para saber como gerenciar trechos de VCL personalizados do Administrador do Adobe Commerce, consulte [Gerenciar VCL com o Administrador do Adobe Commerce](#manage-custom-vcl-from-admin).


**Pré-requisitos**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### Etapa 1: Localizar a versão ativa do VCL

Use a operação [get version](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) da API do Fastly para obter o número de versão do VCL ativo:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

Na resposta JSON, observe o número de versão do VCL ativo retornado na chave `number`, por exemplo `"number": 99`. Você precisa do número da versão ao clonar o VCL para edição.

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Salve o número da versão ativa em uma variável de ambiente bash para uso em solicitações de API subsequentes:

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### Etapa 2: clonar a versão do VCL ativo e todos os trechos

Antes de adicionar ou modificar trechos de VCL personalizados, você deve criar uma cópia da versão de VCL ativa para edição. Use a operação [clone](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) da API do Fastly:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

Na resposta JSON, o número da versão é incrementado, e o valor da chave *ative* é `false`. Você pode modificar a nova versão do VCL inativa localmente.

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Salve o novo número de versão em uma variável de ambiente bash para uso em comandos subsequentes:

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### Etapa 3: Criar um trecho de VCL personalizado

Crie e salve seu código VCL personalizado em um arquivo JSON com o seguinte conteúdo e formato:

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

Os valores incluem:

- `name` — Nome do trecho VCL.

- `dynamic` — Indica se este é um [trecho regular](https://docs.fastly.com/en/guides/about-vcl-snippets) ou um [trecho dinâmico](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets).

- `type` — Especifica o local para inserção do trecho gerado, como `init` (acima das sub-rotinas) e `recv` (dentro das sub-rotinas). Consulte [Valores de objeto de trecho de VCL do Fastly](https://docs.fastly.com/api/config#snippet) para obter informações sobre esses valores.

- `priority` — Um valor de `1` a `100` que determina quando o código de trecho de VCL personalizado é executado. Os trechos de VCL personalizados com valores mais baixos são executados primeiro.

  Todo o código VCL padrão do módulo Fastly VCL tem um `priority` de `50`. Se desejar que uma ação ocorra por último ou para substituir o código VCL padrão, use um número maior, como `100`. Para executar o código de trecho de VCL personalizado imediatamente, defina a prioridade como um valor mais baixo, como `5`.

- `content` — O trecho de código VCL a ser executado em uma linha, sem quebras de linha. Consulte [Exemplo de trecho de VCL personalizado](#example-vcl-snippet-code).

### Etapa 4: adicionar o trecho VCL à configuração do Fastly

Use a operação [criar trecho](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) da API do Fastly para adicionar o trecho VCL personalizado à versão do VCL.

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

O `<filename.json>` é o nome do arquivo que você preparou na etapa anterior. Repita esse comando para cada trecho de VCL.

Se você receber uma resposta `500 Internal Server Error` do serviço Fastly, verifique a sintaxe do arquivo JSON para verificar se você está carregando um arquivo válido.

### Etapa 5: Validar e ativar trechos de VCL personalizados

Depois de adicionar um trecho de VCL personalizado, o Fastly insere o trecho na versão do VCL que você está editando. Para aplicar alterações, conclua as etapas a seguir para validar o código do trecho de VCL e ativar a versão do VCL.

1. Use a operação [validar VCL versão](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) da API do Fastly para verificar o código VCL atualizado.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Se a API Fastly retornar um erro, corrija o problema e valide a versão atualizada do VCL novamente.

1. Use a operação [ativar](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) da API do Fastly para ativar a nova versão do VCL.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```


## Referência rápida da API para trechos de VCL

Esses exemplos de solicitação de API usam variáveis de ambiente exportadas para fornecer as credenciais para autenticação com o Fastly. Para obter detalhes sobre esses comandos, consulte a [Referência da API do Fastly](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>Use esses comandos para gerenciar trechos adicionados usando a API do Fastly. Se você tiver adicionado trechos do Administrador, consulte [Gerenciar trechos de VCL usando o Administrador](#manage-vcl-using-the-api).

- **Obter número de versão do VCL ativo**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **Listar todos os trechos de VCL regulares anexados a um serviço**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **Revisar um trecho individual**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  O `<snippet_name>` é o nome de um trecho, como `my_regular_snippet`.

- **Atualizar um trecho**

  Modifique o [arquivo JSON preparado](#step-3-create-a-custom-vcl-snippet) e envie a seguinte solicitação:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **Excluir um trecho de VCL individual**

  Obtenha uma lista de trechos e use o seguinte comando `curl` com o nome do trecho específico a ser excluído:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **Substituir valores no [código padrão de VCL do Fastly](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  Crie um trecho com valores atualizados e atribua uma prioridade de `100`.
