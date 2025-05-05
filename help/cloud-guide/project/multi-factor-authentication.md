---
title: Habilitar autenticação multifator para acesso SSH
description: Saiba como gerenciar requisitos de autenticação para acesso SSH ao Adobe Commerce em ambientes de infraestrutura em nuvem.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Habilitar autenticação multifator para acesso SSH

Para maior segurança, o Adobe Commerce na infraestrutura em nuvem fornece imposição de autenticação de vários fatores (MFA) para gerenciar requisitos de autenticação para acesso SSH a ambientes em nuvem.

Quando o MFA é ativado em um projeto, todas as contas de usuário com acesso SSH exigem um código de autenticação de dois fatores (TFA) ou um token de API e certificado SSH para acessar o ambiente.

>[!NOTE]
>
>O MFA não está habilitado em projetos na nuvem por padrão. O proprietário da conta do projeto Adobe Commerce na infraestrutura em nuvem deve [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) para habilitá-lo. Quando o MFA é habilitado, todos os usuários devem ter a autenticação de dois fatores (TFA) habilitada em sua conta do Adobe Commerce na infraestrutura em nuvem para obter acesso SSH aos ambientes do projeto.

## Certificados para acesso SSH

O MFA permite que os usuários troquem um token de acesso OAUTH com um certificado SSH de curta duração gerado pela API do Adobe Cloud Certifier. Se o usuário tiver a função de Administrador ou Colaborador, uma chave SSH válida e um código TFA ou token de API válido, o Adobe Commerce na infraestrutura em nuvem usará essas credenciais para gerar o certificado SSH temporário. A expiração do certificado é definida como uma hora, mas é atualizada automaticamente durante a sessão atual.

Depois de fazer logon em um projeto com MFA, os usuários devem usar a CLI do `magento-cloud` para gerar o certificado SSH:

```bash
magento-cloud ssh-cert:load
```

O comando `ssh-cert:load` gera o certificado SSH e o instala no agente SSH do usuário local.

### Gerar certificado automaticamente ao fazer logon

Você pode configurar seu ambiente local para gerar o certificado SSH automaticamente ao autenticar na CLI do `magento-cloud`.

**Para adicionar a geração automática do certificado SSH à sua configuração da CLI `magento-cloud`**:

1. Na estação de trabalho local, crie um arquivo chamado `config.yaml` na pasta `.magento-cloud` do diretório base, se ele não existir.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Adicione a configuração a seguir ao arquivo `config.yaml`.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. Use a CLI do `magento-cloud` para autenticar novamente:

   >Fazer logoff:

   ```bash
   magento-cloud logout
   ```

   >Fazer logon:

   ```bash
   magento-cloud login
   ```

   >Siga a resposta:

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## Conectar-se a um ambiente usando SSH com TFA

Quando o MFA é habilitado em um projeto, você deve ter o TFA habilitado em sua conta antes de poder se conectar a um ambiente remoto usando um SSH. Consulte [Habilitar TFA](user-access.md#enable-tfa-for-cloud-accounts).

>[!BEGINSHADEBOX]

**Pré-requisitos:**

Para projetos habilitados com imposição de MFA, o acesso SSH requer as seguintes permissões e configurações de conta:

- [Acesso do administrador ou colaborador ao ambiente](user-access.md)
- [Chave de acesso SSH configurada na conta](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA ativado por conta](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**Para se conectar usando SSH com as credenciais da conta de usuário do TFA**:

1. Faça logon em [sua conta](https://console.adobecommerce.com).

1. Na estação de trabalho local, use a CLI do `magento-cloud` para gerar o certificado SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Exemplo de resposta:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Use um SSH para se conectar ao ambiente remoto.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## Gerenciar código-fonte usando SSH com TFA

Ao gerenciar o código-fonte para o Adobe Commerce em projetos de infraestrutura em nuvem, use o SSH para autenticar no repositório Git do projeto. Se o projeto tiver a imposição de MFA habilitada, gere um certificado SSH antes de executar operações de linha de comando usando o repositório Git.

**Para se conectar usando SSH com as credenciais da conta de usuário do TFA**:

1. Faça logon em [sua conta](https://console.adobecommerce.com) e autentique usando o TFA.

   >[!NOTE]
   >
   >Se você não tiver o TFA ativado em sua conta, é necessário ativá-lo. Consulte [Habilitar o TFA em contas na nuvem](user-access.md#enable-tfa-for-cloud-accounts).

1. Na estação de trabalho local, use a CLI do `magento-cloud` para gerar o certificado SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Exemplo de resposta:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Clonar o repositório Git para o ambiente do projeto:

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > Exemplo de resposta:

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## Conectar-se a um ambiente usando SSH com um token de API

Quando o MFA é ativado em um projeto, os processos automatizados que exigem acesso SSH a um ambiente em nuvem exigem um token de API. Você pode gerar o token de uma conta do Adobe Commerce na infraestrutura em nuvem com acesso de Administrador ou Colaborador no projeto.

A autenticação com um token de API ainda requer a geração de um certificado SSH. Os processos automatizados também devem automatizar a geração de um certificado SSH.

>[!BEGINSHADEBOX]

**Pré-requisitos:**

- [Acesso de administrador ou colaborador à Adobe Commerce no ambiente de infraestrutura em nuvem](user-access.md)
- [Token de API válido disponível por conta](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**Para se conectar usando SSH com uma credencial de token de API**:

1. Faça logon no projeto na nuvem usando a autenticação de chave de API.

   ```bash
   magento-cloud auth:api-token
   ```

1. No prompt, insira o valor de um token de API válido.

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### Exemplo: script SSH automatizado

Há duas opções para armazenar o token da API.

>[!NOTE]
>
>Se um token de API for armazenado, a CLI do `magento-cloud` será autenticada automaticamente e não haverá necessidade de executar o comando `magento-cloud login`.

**Opção 1**: criar uma variável de ambiente para armazenar o token de API

Escreva o token no seu bash_profile

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**Opção 2**: adicionar o token ao arquivo `config.yaml`

1. Na estação de trabalho local, crie um arquivo chamado `config.yaml` na pasta `.magento-cloud` do diretório base, se ele não existir.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Adicione a configuração a seguir ao arquivo `config.yaml`.

   ```yaml
   api:
      token: <your api token>
   ```

>Exemplo de script bash

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## Solução de problemas

Use as informações a seguir para resolver falhas de solicitações de conexão SSH devido a erros de autenticação como `access requires MFA` ou `permission denied`.

### Sua solicitação não fornece um certificado válido

Se sua solicitação não fornecer um certificado válido, uma mensagem semelhante à seguinte será exibida:

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

Tente executar os seguintes procedimentos de solução de problemas para resolver o problema de conexão:

- Verifique a configuração do TFA da conta
- Autentique novamente e recarregue o certificado

**Para verificar a configuração e a autenticação do TFA**:

1. Faça logon em [sua conta](https://console.adobecommerce.com).

1. No menu de conta superior direito, clique em **[!UICONTROL My Profile]**.

1. Na página _Meu perfil_, clique na guia **[!UICONTROL Security]**.

   Se o TFA estiver ativado, a seção Segurança fornece opções para gerenciar a configuração do TFA.

1. Se o TFA não estiver configurado, clique em **[!UICONTROL Set up application]** e siga as instruções para habilitá-lo. Consulte [Habilitar TFA](user-access.md#enable-tfa-for-cloud-accounts).

1. Se o TFA estiver configurado, tente autenticar novamente.

**Para autenticar e recarregar o certificado SSH**:

1. Use a CLI do `magento-cloud` para autenticar novamente:

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. Recarregue o certificado SSH:

   ```bash
   magento-cloud ssh-cert:load
   ```

### Permissão negada

Se a chave SSH estiver ausente ou for inválida, a solicitação de conexão SSH retornará um erro `Permission denied (publickey)`.

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

Para corrigir o problema, adicione a chave SSH à sua sessão atual ou atualize o arquivo de configuração SSH para carregar suas chaves SSH automaticamente. Consulte [Adicionar uma chave SSH pública](../development/secure-connections.md#add-an-ssh-public-key-to-your-account).

### Não é possível acessar projetos sem MFA

Se você autenticar em um projeto com a autenticação multifator (MFA) habilitada, poderá receber o seguinte erro ao se conectar a outros projetos que não exigem a MFA:

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

Exemplo de resposta:

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

Durante a geração do certificado SSH, a CLI do `magento-cloud` adiciona uma chave SSH adicional ao ambiente local. Essa chave é usada por padrão se a configuração SSH local não incluir a chave SSH para acesso ao projeto.

**Para adicionar sua chave SSH à configuração local**:

1. Crie o arquivo `config` se ele não existir.

   ```bash
   touch ~/.ssh/config
   ```

1. Adicionar uma configuração `IdentityFile`.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>Você pode especificar várias chaves SSH adicionando várias entradas `IdentityFile` à sua configuração.
