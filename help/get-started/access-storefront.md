---
title: Acessar o painel de administração do Commerce
description: Saiba como acessar o painel de administração do Commerce.
recommendations: noDisplay, catalog
exl-id: 827417b0-9048-44d8-8c82-07befba476c7
TQID: https://experienceleague.adobe.com/V3BXuCc9aqT5YuyIS8WAZgUdPAYNhQunAgg2i2FCaOs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 361
ht-degree: 0%

---

# Acessar o painel de administração do Commerce

Os usuários que têm acesso administrativo ao painel Administrador do Commerce podem adicionar usuários, configurar serviços da loja, concluir o trabalho de configuração e personalização da loja e muito mais.

Para um novo projeto, a primeira etapa depois de receber o email de boas-vindas é proteger o acesso de Administrador ao projeto, alterando a senha na conta de Proprietário da licença. O nome de usuário padrão dessa conta é o endereço de email do Proprietário da licença.

Você pode submeter uma solicitação de alteração de senha usando um dos seguintes métodos:

- Localize o email de boas-vindas enviado ao endereço de email do Proprietário da licença, siga o link e altere sua senha.

- Copie a URL de armazenamento de [[!DNL Cloud Console]](../cloud-guide/project/overview.md) em um navegador. Em seguida, anexe `/admin` ao final da URL para abrir a página de entrada. Clique no **Esqueceu a senha?** para enviar uma solicitação de alteração de senha ao endereço de email do Proprietário da licença.

Depois de enviar a solicitação de alteração de senha, verifique se há notificação de redefinição de senha no seu email. Se você não receber o email, verifique sua pasta de spam.

>[!TIP]
>
>Se a redefinição de senha falhar ou você não conseguir fazer logon no painel de administração, um usuário com acesso de administrador poderá se conectar ao projeto usando SSH e adicionar um usuário administrador usando o comando da CLI `admin:user:create`. Consulte [Criar, editar ou desbloquear uma conta de administrador](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) no _Guia de instalação_.

## Monitorar integridade do site

A [Ferramenta de Análise do Site](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro) é uma ferramenta de autoatendimento proativa e um repositório central que inclui insights e recomendações de sistema detalhados para garantir a segurança e a operabilidade da instalação do Adobe Commerce. Ele fornece monitoramento de desempenho em tempo real, relatórios e conselhos 24 horas por dia, 7 dias por semana, para identificar possíveis problemas e melhor visibilidade das configurações de integridade, segurança e aplicativos do site. Ele ajuda a reduzir o tempo de resolução e a melhorar a estabilidade e o desempenho do site. Você pode acessar a Ferramenta de Análise do Site diretamente do [Painel de administração](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
