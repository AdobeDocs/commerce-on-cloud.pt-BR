---
title: Variáveis de ADMINISTRAÇÃO
description: Consulte uma lista de variáveis de ambiente usadas ao instalar o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Variáveis do administrador

Os usuários que têm acesso administrativo ao projeto Adobe Commerce na infraestrutura em nuvem podem usar as seguintes variáveis de ambiente de projeto para substituir as definições de configuração da conta de usuário administrativo para acessar a interface do usuário administrador.

## Credenciais de administrador

Você pode substituir as credenciais de usuário Admin durante a instalação do Commerce pelas variáveis ADMIN na tabela a seguir.

Se quiser alterar os valores após a instalação, conecte-se ao seu ambiente usando SSH e use o comando [`admin:user` da Adobe Commerce CLI ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) para criar ou editar as credenciais de usuário do Administrador.

| Variável | Padrão | Descrição |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Endereço de email do proprietário da licença | Um nome de usuário para o usuário administrativo com a capacidade de criar outros usuários, incluindo usuários administrativos. |
| `ADMIN_EMAIL` |                             | Endereço de email do usuário administrativo. Esse endereço é usado para enviar notificações de redefinição de senha. |
| `ADMIN_PASSWORD` |                             | Senha do usuário administrativo. Quando o projeto é criado, uma senha aleatória é gerada e um email é enviado ao Proprietário da licença. Durante a criação do projeto, o Proprietário da licença já deve ter alterado a senha. Entre em contato com o Proprietário da licença para obter a senha atualizada. |
| `ADMIN_LOCALE` | `en_US` | A localidade padrão usada pelo Administrador. |

## URL do administrador

Use a variável de ambiente a seguir para proteger o acesso à interface do usuário do administrador. Se especificado, esse valor substitui o URL padrão durante a instalação.

`ADMIN_URL` — O URL relativo para acessar a interface do usuário do Administrador. A URL padrão é `/admin`. Por motivos de segurança, a Adobe recomenda alterar o padrão para um URL de administrador exclusivo e personalizado, o que não é fácil de adivinhar.

### Alterar o URL do administrador

O Adobe recomenda alterar a variável de nível de ambiente para o URL do administrador após a instalação. Defina esta configuração por motivos de segurança antes de ramificar a partir do ambiente `master` clonado. Todas as ramificações criadas a partir da ramificação `master` herdam as variáveis de nível de ambiente e seus valores.

Use o comando `magento-cloud variable:update` para atualizar o valor da variável. (O comando `variable:set` foi descontinuado e não está disponível.) O exemplo a seguir atualiza o ADMIN_URL para `newAdmin_A8v10`:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>O valor `ADMIN_URL` aceita letras (a-z ou A-Z), números (0-9) e o caractere de sublinhado (_) para um caminho de administrador personalizado. Espaços ou outros caracteres **não** aceitos.

**Para alterar a URL usando o[!DNL Cloud Console]**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Na visão geral do projeto, selecione o ambiente e clique no ícone de configuração.

   ![Configuração do projeto](../../assets/icon-configure.png){width="36"}

1. Selecione a guia **Variáveis**.

1. Clique em **Criar Variável**.

1. Insira o seguinte:

   - **Nome da variável** = `ADMIN_URL`
   - **value** = Nova URL. Por exemplo, defina a URL do Administrador como `magento_A8v10`.

   Por padrão, `Available during runtime` e `Make inheritable` estão selecionados.

1. Clique em **Criar variável** e aguarde até que a implantação seja concluída. Esse botão só estará visível quando os campos obrigatórios contiverem valores.
