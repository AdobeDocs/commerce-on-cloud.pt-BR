---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Incluir arquivo para modificar o VCL personalizado para o Fastly

## Modificar o trecho de VCL personalizado

1. [Faça logon](/help/get-started/onboarding.md#access-your-admin-panel) no Administrador.

1. Clique em **Lojas** > **Configurações** > **Configuração** > **Avançadas** > **Sistema**.

1. Expanda **Cache de Página Inteira** > **Configuração Rápida** > **Trechos de VCL Personalizados**.

   ![Gerenciar trechos de VCL personalizados](/help/assets/cdn/fastly-manage-snippets.png)

1. Na coluna _Ação_, clique no ícone de configurações ao lado do trecho a ser editado.

1. Depois que a página for recarregada, clique em **Carregar VCL para Fastly** na seção _Configuração Fastly_.

1. Depois que o upload for concluído, atualize o cache de acordo com a notificação na parte superior da página.

>[!WARNING]
>
>A opção _Custom VCL snippets_ UI mostra apenas os trechos adicionados pelo Administrador do Adobe Commerce. Se você adicionar trechos usando a API Fastly, use a API para [gerenciá-los](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
