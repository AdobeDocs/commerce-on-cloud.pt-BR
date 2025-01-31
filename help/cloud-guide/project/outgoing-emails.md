---
title: Configurar emails de saída
description: Saiba como habilitar emails de saída para o Adobe Commerce na infraestrutura em nuvem.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Configurar emails de saída

Você pode habilitar e desabilitar emails de saída para ambientes de integração (e de preparo somente para o Iniciador) do [!DNL Cloud Console] ou da linha de comando. Ative os emails de saída para enviar autenticação de dois fatores ou redefinir emails de senha para usuários do projeto na nuvem.

Por padrão, os emails de saída são ativados nos ambientes de Produção e Preparo (somente Pro). No entanto, a configuração **[!UICONTROL Enable outgoing emails]** pode parecer desabilitada nas configurações do ambiente independentemente do status até que você defina a propriedade `enable_smtp` por meio da [linha de comando](#enable-emails-in-the-cli) ou do [Console da Nuvem](outgoing-emails.md#enable-emails-in-the-cloud-console).

A atualização do valor da propriedade `enable_smtp` pela [linha de comando](#enable-emails-in-the-cli) também altera o valor da configuração [!UICONTROL Enable outgoing emails] desse ambiente no Console da Nuvem.

>[!NOTE]
>
>Habilitar/desabilitar a configuração **[!UICONTROL Enable outgoing emails]** não habilitará/desabilitará emails nos ambientes de Preparo Profissional ou Produção.

{{redeploy-warning}}

## Ativar emails no Cloud Console

Use o botão **[!UICONTROL Outgoing emails]** no modo de exibição _Configurar ambiente_ para habilitar ou desabilitar o suporte por email.

Se os emails de saída precisarem ser desativados ou reativados nos ambientes de Produção Pro ou de Preparo, você poderá enviar um [tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>O status do email de saída pode não ser refletido para ambientes de preparo profissional ou de produção no Cloud Console.

**Para gerenciar o suporte de email do[!DNL Cloud Console]**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Selecione um projeto na lista _Todos os projetos_.
1. No painel Projeto, clique no ícone de configuração no canto superior direito.
1. Clique em **[!UICONTROL Environments]** e selecione um ambiente específico na lista (exceto Preparo e Produção para Pro).
1. Para habilitar ou desabilitar emails de saída, alterne _Habilitar emails de saída_ **Ativado** ou **Desativado**.

   ![Habilitar configuração de email de saída](../../assets/outgoing-emails.png)

Depois de alterar a configuração, o ambiente é criado e implantado com a nova configuração.

## Habilitar emails na CLI

Você pode alterar a configuração de email de um ambiente ativo usando o comando `environment:info` da CLI `magento-cloud` para definir a propriedade `enable_smtp`. Habilitar o SMTP atualiza a variável de ambiente `MAGENTO_CLOUD_SMTP_HOST` com o endereço IP do host SMTP para enviar emails.

**Para gerenciar suporte a email na linha de comando**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Verifique a configuração de email de saída do ambiente.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Altere a configuração de suporte de email definindo a variável de ambiente `enable_smtp` como `true` ou `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Aguarde a criação e a implantação do ambiente.

1. Use um SSH para fazer logon no ambiente remoto.

1. Verifique se o email funciona; envie um email de teste para um endereço que você possa verificar.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. Verifique se o email foi selecionado pelo SendGrid.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
