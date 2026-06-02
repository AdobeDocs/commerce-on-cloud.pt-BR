---
title: VCL personalizado para ignorar o cache do Fastly
description: Solucione problemas de tráfego de solicitação para o servidor de origem criando um trecho de VCL personalizado para ignorar o cache do Fastly.
feature: Cloud, Configuration, Cache
exl-id: 4e19d6d4-b5a1-4623-b0be-804ddc81ff3d
TQID: https://experienceleague.adobe.com/67LdlbG62T-cBEgNTwQ5p5MvvYQwNuHUYVQhrVBSn1A
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 303
ht-degree: 0%

---

# VCL personalizado para ignorar o cache do Fastly

Você pode criar um trecho de VCL personalizado para ignorar o cache do Fastly e solucionar problemas no tráfego de solicitação para o servidor de origem. Por exemplo, você pode criar um trecho para determinar se os problemas do site são causados por cache ou para solucionar problemas de cabeçalhos.

Você pode configurar o trecho para ignorar o cache rápido de solicitações de um endereço IP ou URL específico.

>[!NOTE]
>
>Antes de mesclar a configuração de VCL personalizada em um ambiente de produção, certifique-se de testar o código no ambiente de preparo.

**Pré-requisitos:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Para ignorar o cache do Fastly com base no endereço IP ou na URL**:

{{admin-login-step}}

1. Clique em **Lojas** > Configurações > **Configuração** > **Avançado** > **Sistema**.

1. Expanda **Cache de Página Inteira** > **Configuração Rápida** > **Trechos de VCL Personalizados**.

1. Clique em **Criar trecho personalizado**.

1. Adicione os valores do trecho de VCL:

   - **Nome** — `bypass_fastly`

   - **Tipo** — `recv`

   - **Prioridade** — `5`

   - **VCL** conteúdo do trecho —

     O exemplo a seguir ignora o Fastly para um endereço IP específico:

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     O exemplo a seguir ignora o Fastly para um padrão de URL específico:

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     Para uma correspondência exata de URL, use o operador `==` em vez do operador `~`. Consulte a [Referência de VCL do Fastly] para obter detalhes.

1. Clique em **Criar**.

   ![Criar/Ignorar Fastly no trecho do VCL](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. Depois que a página for recarregada, clique em **Carregar VCL para Fastly** na seção *Configuração Fastly*.

1. Depois que o upload for concluído, atualize o cache de acordo com a notificação na parte superior da página.

   O Fastly valida a versão atualizada do VCL durante o processo de upload. Se a validação falhar, edite o trecho de VCL personalizado para corrigir os problemas. Em seguida, carregue o VCL novamente.

Depois de adicionar o trecho VCL, você pode usar comandos cURL para enviar solicitações ao servidor de origem a partir do endereço IP ou URL especificado, conforme mostrado no exemplo a seguir:

```bash
curl -svo /dev/null www.example.com/index.html
```

Em seguida, inspecione a resposta para solucionar problemas com o conteúdo não armazenado em cache.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Referência do Fastly VCL]: https://docs.fastly.com/vcl/

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
