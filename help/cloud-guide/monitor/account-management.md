---
title: Gerenciamento de conta da New Relic
description: Saiba como acessar sua conta do New Relic e gerenciar o acesso, as integrações e o uso de ferramentas para seu projeto do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 5b633108f4113b26f6487073c1ccedebb632b111
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# Gerenciamento de conta do New Relic

Quando a Adobe provisiona seu projeto de infraestrutura em nuvem, o Proprietário da licença recebe um email da New Relic com credenciais e instruções para acessar a conta da New Relic. Se você não recebeu o email, use o endereço de email do Proprietário da licença para redefinir a senha do New Relic.

Se o Proprietário da Licença tiver sido alterado e o novo Proprietário da Licença não tiver acesso à New Relic no momento, [envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket).

## Gerenciar acesso de usuário (função de Administrador)

>[!NOTE]
>
>Conceda acesso total somente a usuários que exijam estritamente acesso ao conjunto completo de recursos.

**Para acessar o Gerenciamento de usuários no New Relic**:

1. Faça logon em sua [conta do New Relic](https://login.newrelic.com/login).

1. Selecione seu nome de usuário na navegação inferior esquerda.

1. Clique em **[!UICONTROL Administration]** e selecione uma das opções a seguir na lista:

   - **[!UICONTROL User management]** para adicionar um usuário e gerenciar usuários ativos e convites pendentes.

   - **[!UICONTROL Access management]** para gerenciar grupos de usuários, funções e contas.

Consulte [Gerenciamento de usuários](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) na documentação do _New Relic_.

## Configurar o New Relic para o ambiente inicial

>[!NOTE]
>
>**Ambientes profissionais** são pré-configurados para usar os serviços da New Relic e podem ignorar as instruções de habilitação e conexão. Se o New Relic APM não estiver instalado nos ambientes de Preparo e Produção ou o New Relic Infrastructure não estiver disponível no ambiente Produção, [envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) para solicitar a instalação.

Para ambientes Iniciantes, você deve verificar o arquivo `.magento.app.yaml` para verificar se a seção `runtime` inclui a extensão do New Relic. Se a extensão não tiver sido configurada, adicione o seguinte:

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Aplicar chave de licença

Para conectar um ambiente em nuvem ao New Relic, adicione a chave de licença do New Relic ao ambiente.

- Para **projetos Pro**, a Adobe adiciona a chave de licença aos seus ambientes de Produção e Preparo durante o processo de provisionamento. Você pode fazer logon em sua [conta do New Relic](https://login.newrelic.com/login) para verificar a conectividade entre o site do Adobe Commerce na infraestrutura em nuvem e o New Relic.

- Para **Projetos iniciais**, você tem uma chave de licença do New Relic que suporta até _três_ ambientes. Você deve adicionar a chave manualmente às configurações do ambiente. Os ambientes iniciais não são pré-provisionados para usar o serviço do New Relic.

Para ambientes Iniciantes, habilite a integração do New Relic adicionando a chave de licença do New Relic à configuração do ambiente. Adicione a chave aos ambientes de armazenamento temporário e produção e a um outro ambiente de sua escolha. Somente a chave de licença do New Relic é necessária para a configuração. Você pode encontrar informações sobre opções de configuração adicionais no tópico [Relatórios do New Relic](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html?lang=pt-BR) no _Guia do Usuário do Adobe Commerce_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Credenciais de logon para a página da conta da Adobe Commerce ou para a licença da New Relic associada ao projeto
>- [Acesso de nível administrativo](../project/user-access.md) aos ambientes iniciais para configurar
>- Credenciais para acessar o [Administrador](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html?lang=pt-BR) do ambiente

**Para configurar o New Relic para ambientes iniciais**:

1. Encontre sua chave de licença do New Relic no [!DNL Cloud Console] ou na Cloud CLI.

   **[!DNL Cloud Console]método**:

   - Abra a [página da conta](https://accounts.magento.cloud/user) do projeto na nuvem.

   - Na guia _Projetos_, localize o projeto.

   - Clique em **Exibir Detalhes** para obter informações sobre a infraestrutura do projeto.

   - Expanda a seção **Serviço New Relic** para exibir a chave de licença.

   - Copie a chave de licença.

   **Método CLI da nuvem**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Adicione a chave de licença do New Relic a um ambiente usando a CLI do `magento-cloud`.

   - Altere para o ambiente que precisa da chave de licença.
   - Atualize o valor da variável usando o seguinte comando da CLI `magento-cloud`:

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Opcionalmente, você pode adicioná-lo a partir do [Administrador do Commerce](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html?lang=pt-BR#step-3%3A-configure-your-store).

1. Faça logon em sua [conta do New Relic](https://login.newrelic.com/login) para verificar se você pode visualizar dados do ambiente do Adobe Commerce. Consulte [Investigar desempenho](investigate-performance.md).

### Remover chave de licença

Você só pode usar sua chave de licença do New Relic em três ambientes ativos. Se a chave estiver em uso em três ambientes, remova-a de um dos ambientes antes de adicioná-la a um ambiente diferente.

**Para remover uma chave de licença de um ambiente**:

1. Listar variáveis de ambiente.

   ```bash
   magento-cloud variable:list
   ```

   Exemplo de resposta:

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Se você adicionou a chave de licença como uma variável do _projeto_, remova essa variável de nível de projeto. Uma variável de projeto adiciona a licença à _a cada_ ramificação de ambiente criada, que pode consumir ou exceder o limite de licença. Para listar variáveis de projeto: `magento-cloud variable:list --level project`

1. Exclua a variável de licença.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
