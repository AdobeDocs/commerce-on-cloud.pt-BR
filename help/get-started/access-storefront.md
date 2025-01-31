---
title: Acessar o painel de administração do Commerce
description: Saiba como acessar o painel de administração do Commerce.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Acessar o painel de administração do Commerce

Os usuários que têm acesso administrativo ao painel Administrador do Commerce podem adicionar usuários, configurar serviços da loja, concluir o trabalho de configuração e personalização da loja e muito mais.

Para um novo projeto, a primeira etapa depois de receber o email de boas-vindas é proteger o acesso de Administrador ao projeto, alterando a senha na conta de Proprietário da licença. O nome de usuário padrão dessa conta é o endereço de email do Proprietário da licença.

Você pode submeter uma solicitação de alteração de senha usando um dos seguintes métodos:

- Localize o email de boas-vindas enviado ao endereço de email do Proprietário da licença, siga o link e altere sua senha.

- Copie a URL de armazenamento de [[!DNL Cloud Console]](../cloud-guide/project/overview.md) em um navegador. Em seguida, anexe `/admin` ao final da URL para abrir a página de entrada. Clique no link **Esqueceu a senha?Link** para enviar uma solicitação de alteração de senha ao endereço de email do Proprietário da Licença.

Depois de enviar a solicitação de alteração de senha, verifique se há notificação de redefinição de senha no seu email. Se você não receber o email, verifique sua pasta de spam.

>[!TIP]
>
>Se a redefinição de senha falhar ou você não conseguir fazer logon no painel de administração, um usuário com acesso de administrador poderá se conectar ao projeto usando SSH e adicionar um usuário administrador usando o comando da CLI `admin:user:create`. Consulte [Criar, editar ou desbloquear uma conta de administrador](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) no _Guia de instalação_.

## Monitorar integridade do site

A [Ferramenta de Análise do Site](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro) é uma ferramenta de autoatendimento proativa e um repositório central que inclui insights e recomendações de sistema detalhados para garantir a segurança e a operabilidade da instalação do Adobe Commerce. Ele fornece monitoramento de desempenho em tempo real, relatórios e conselhos 24 horas por dia, 7 dias por semana, para identificar possíveis problemas e melhor visibilidade das configurações de integridade, segurança e aplicativos do site. Ele ajuda a reduzir o tempo de resolução e a melhorar a estabilidade e o desempenho do site. Você pode acessar a Ferramenta de Análise do Site diretamente do [Painel de administração](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
