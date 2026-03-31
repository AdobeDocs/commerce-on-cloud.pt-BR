---
title: Variáveis de ADMINISTRAÇÃO
description: Consulte uma lista de variáveis de ambiente usadas ao instalar o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: ac1b2001294ba72304fc7ad3c760872dbd73e44f
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# Variáveis do administrador

Os usuários que têm acesso administrativo ao projeto Adobe Commerce na infraestrutura em nuvem podem usar as seguintes variáveis de ambiente de projeto para substituir as definições de configuração da conta de usuário administrativo para acessar a interface do usuário administrador.

## Credenciais de administrador

Você pode substituir as credenciais de usuário Admin durante a instalação do Commerce pelas variáveis ADMIN na tabela a seguir.

Se quiser alterar os valores após a instalação, conecte-se ao seu ambiente usando SSH e use o comando [`admin:user` da Adobe Commerce CLI ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) para criar ou editar as credenciais de usuário do Administrador.

| Variável | Padrão | Descrição |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Nome de usuário do Proprietário da licença | Um nome de usuário para o usuário administrativo com a capacidade de criar outros usuários, incluindo usuários administrativos. |
| `ADMIN_FIRSTNAME` | Nome do proprietário da licença | O nome do usuário administrativo. |
| `ADMIN_LASTNAME` | Sobrenome do proprietário da licença | O sobrenome do usuário administrativo. |
| `ADMIN_EMAIL` | Email do proprietário da licença | Endereço de email do usuário administrativo. Esse endereço é usado para enviar notificações de redefinição de senha. |
| `ADMIN_PASSWORD` | Senha do proprietário da licença | Senha do usuário administrativo. Quando o projeto é criado, uma senha aleatória é gerada e um email é enviado ao Proprietário da licença. Durante a criação do projeto, o Proprietário da licença já deve ter alterado a senha. Entre em contato com o Proprietário da licença para obter a senha atualizada. |
| `ADMIN_LOCALE` | `en_US` | A localidade padrão usada pelo Administrador. |

## URL do administrador

Use a variável de ambiente a seguir para proteger o acesso à interface do usuário do administrador. Se especificado, esse valor substitui o URL padrão durante a instalação. No [!DNL Adobe Commerce] na infraestrutura em nuvem, você deve definir ou alterar a URL do Administrador usando a variável `ADMIN_URL` no ([!DNL Cloud Console] ou [!DNL Cloud CLI]). A modificação da configuração de [!DNL Admin] é aplicável somente para instalações locais.

`ADMIN_URL` — O URL relativo para acessar a interface do usuário do Administrador. A URL padrão é `/admin`.

### Alterar o URL do administrador

Por padrão, a URL do [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html) está definida como *&lt;domain_name>/admin*. Por motivos de segurança, a Adobe recomenda alterá-lo para um URL de administrador exclusivo e personalizado, o que não é fácil de adivinhar.

**Em [!DNL Adobe Commerce] na infraestrutura de nuvem**, você deve alterar a URL do Administrador usando a variável de ambiente `ADMIN_URL` em ([!DNL Cloud Console] ou [!DNL Cloud CLI]). A modificação da configuração de [!DNL Admin] é aplicável somente para instalações locais. Para instalações locais, siga [usar uma URL de administrador personalizada](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html#use-a-custom-admin-url).

A Adobe recomenda alterar a variável de nível de ambiente para o URL do administrador após a instalação. Defina esta configuração por motivos de segurança antes de ramificar a partir do ambiente `master` clonado. Todas as ramificações criadas a partir da ramificação `master` herdam as variáveis de nível de ambiente e seus valores, a menos que você defina a herança como false.

Use [!DNL Cloud Console] ou [!DNL Cloud CLI] para definir ou atualizar `ADMIN_URL`.

#### Opção A: Alterar a URL do Administrador usando o [!DNL Cloud Console]

##### Ambiente de integração

No [Cloud Console](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html), adicione uma nova variável com:

- **Nome:** `ADMIN_URL`
- **Valor:** sua nova URL de Administrador (por exemplo, `magento_A8v10`)

- Para obter etapas detalhadas, consulte [adicionar variáveis de ambiente](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment) ou [variáveis de ambiente](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html) na documentação do desenvolvedor.

##### Definir a URL do Administrador no [!DNL Cloud Console]

1. Faça logon no [Cloud Console](https://console.adobecommerce.com/).
2. Selecione um projeto na lista **[!UICONTROL All projects]**.
3. Na visão geral do projeto, selecione o ambiente e clique no ícone de configuração.
4. Selecione a guia **[!UICONTROL Variables]**.
5. Clique em **[!UICONTROL Create Variable]** (ou edite a variável `ADMIN_URL` existente, se presente).
6. Insira o seguinte:
   - **Nome da variável:** `ADMIN_URL`
   - **Valor:** seu novo caminho de Administrador (por exemplo, `magento_A8v10`).

   Por padrão, **[!UICONTROL Available during runtime]** e **[!UICONTROL Make inheritable]** estão selecionados. Para impedir que ambientes filhos herdem esse valor, desmarque **[!UICONTROL Make inheritable]** para essa variável.
7. Clique em **[!UICONTROL Create variable]** (ou **[!UICONTROL Save]**) e aguarde até que a implantação seja concluída. O botão só estará visível quando os campos obrigatórios contiverem valores.

##### Quando Preparo e Produção não estão disponíveis no [!DNL Cloud Console]

[Envie um tíquete de suporte](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) solicitando a adição da variável `ADMIN_URL` para o ambiente de preparo ou produção. Se o ambiente de Preparo e Produção estiver acessível no [!DNL Cloud Console], adicione a variável conforme descrito em [Ambiente de integração](#integration-environment).

#### Opção B: Alterar a URL do Administrador usando o [!DNL Cloud CLI]

Use o comando `magento-cloud variable:update` para atualizar a variável. (O comando `variable:set` foi descontinuado e não está disponível.)

O exemplo a seguir atualiza o ambiente `master` `ADMIN_URL` para `newAdmin_A8v10` e impede que ambientes filho herdem o valor:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **Reimplantação:** alterar a variável `ADMIN_URL` em [!DNL Cloud CLI] aciona uma reimplantação do ambiente.
- **Herança:** As variáveis são herdáveis por padrão. Para impedir que o valor seja herdado por ambientes filhos, use a opção `--inheritable false` como mostrado. Para obter mais detalhes, consulte [visibilidade do nível de variável](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html#visibility).

>[!NOTE]
>
>O valor `ADMIN_URL` aceita letras (a-z, A-Z), números (0-9) e o caractere de sublinhado (_). Espaços ou outros caracteres não são aceitos.
