---
title: Chaves de autenticação
description: Saiba como aplicar chaves de autenticação a um projeto de desenvolvimento no Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Chaves de autenticação

Você deve ter uma chave de autenticação para acessar o repositório do Adobe Commerce e habilitar os comandos instalar e atualizar para o projeto do Adobe Commerce na infraestrutura em nuvem. Há dois métodos para especificar credenciais de autorização do Composer.

- **arquivo de autenticação** — Um arquivo que contém suas [credenciais de autorização](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html?lang=pt-BR) do Adobe Commerce no diretório raiz da infraestrutura em nuvem do Adobe Commerce.
- **Variável de ambiente** — Uma variável de ambiente para configurar chaves de autenticação no projeto Adobe Commerce na infraestrutura em nuvem para evitar exposição acidental.

>[!BEGINSHADEBOX]

**Nota de segurança**

A Adobe recomenda usar o método [variável de ambiente](#composer-auth-environment-variable) com seu projeto de nuvem para evitar a exposição acidental de suas credenciais de autorização.

O método de arquivo de autenticação é ideal ao usar o Cloud Docker para Commerce como uma ferramenta de desenvolvimento local, mas tenha cuidado para não carregar o arquivo `auth.json` para um repositório público baseado em Git. Você pode adicionar o arquivo `auth.json` ao arquivo [`.gitignore` &#x200B;](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## Arquivo de autenticação

**Para criar um `auth.json` arquivo**:

1. Se você não tiver um arquivo `auth.json` no diretório raiz do projeto, crie um.

   - Usando um editor de texto, crie um arquivo `auth.json` no diretório raiz do projeto.
   - Copie o conteúdo da [amostra `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) para o novo arquivo `auth.json`.

1. Substitua `<public-key>` e `<private-key>` pelas credenciais de autenticação da Adobe Commerce.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Salve as alterações e saia do editor de texto.

## Variável de ambiente de autenticação do Composer

O método a seguir é a melhor maneira de evitar a exposição acidental de credenciais confidenciais em um repositório público baseado em Git.

**Para adicionar chaves de autenticação usando uma variável de ambiente**:

1. No _[!DNL Cloud Console]_, clique no ícone de configuração no lado direito da navegação do projeto.

   ![Configurar projeto](../../assets/icon-configure.png){width="36"}

1. Na lista _Configurações do projeto_, clique em **[!UICONTROL Variables]**.

1. Clique em **[!UICONTROL Create variable]**.

1. No campo **[!UICONTROL Variable name]**, digite `env:COMPOSER_AUTH`.

1. No campo _Valor_, adicione o seguinte e substitua `<public-key>` e `<private-key>` pelas credenciais de autenticação da Adobe Commerce:

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Selecione **[!UICONTROL Available during buildtime]** e desmarque **[!UICONTROL Available during runtime]**.

1. Clique em **[!UICONTROL Create variable]**.

1. Remova o arquivo `auth.json` de cada ambiente.
