---
title: Conexões seguras
description: Saiba como aplicar chaves SSH ao projeto Adobe Commerce na infraestrutura em nuvem e fazer logon em ambientes remotos.
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: 9c0b4bea11abb2ce5644556ab3dadd361f8ff449
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 0%

---

# Conexões seguras com ambientes remotos

O Secure Shell (SSH) é um protocolo comum usado para fazer logon com segurança em servidores e sistemas remotos. Você pode usar o SSH para acessar seus ambientes remotos para gerenciar o aplicativo do Adobe Commerce e acessar logs de ambientes remotos. O Adobe só oferece suporte a conexões FTP seguras (sFTP) usando sua chave pública SSH. Conexões FTP não são suportadas.

## Gerar um par de chaves SSH

Crie um par de chaves SSH em cada máquina e espaço de trabalho que exija acesso ao código-fonte e aos ambientes do projeto. A chave SSH permite que você se conecte ao GitHub para gerenciar o código-fonte e se conectar aos servidores da nuvem sem precisar fornecer constantemente seu nome de usuário e senha. Consulte [Conectando ao GitHub com SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) para obter mais instruções sobre como criar um par de chaves SSH.

- A _chave pública_ é segura para fornecer acesso a um site, SSH e sFTP.
- A _chave privada_ permanece privada na estação de trabalho.

>[!CAUTION]
>
>**Nunca compartilhe sua chave privada.** Não adicione-o a um tíquete, copie-o em um chat ou anexe-o a emails.

## Adicionar uma chave pública SSH à sua conta

Depois de adicionar ou atualizar sua chave pública SSH para sua conta do Adobe Commerce na infraestrutura em nuvem, [reimplante todos os ambientes ativos](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy) em sua conta para instalar a chave.

Você pode adicionar chaves SSH à sua conta usando um dos seguintes métodos: Cloud CLI ou [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### Adicione sua chave SSH usando a CLI da nuvem

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Faça logon no projeto:

   ```bash
   magento-cloud login
   ```

1. Adicione a chave pública.

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Você pode listar e excluir chaves SSH usando os comandos CLI da Nuvem `ssh-key:list` e `ssh-key:delete`.

>[!TAB Console]

### Adicione sua chave SSH usando o [!DNL Cloud Console]

**Para adicionar uma chave SSH a um novo projeto**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Clique em **[!UICONTROL No SSH key]**. Esse ícone fica à direita do campo de comando e é visível quando o projeto não contém uma chave SSH.

1. Copie e cole o conteúdo da sua chave SSH pública no campo **Chave pública**.

1. Siga os prompts restantes.

**Para adicionar uma chave SSH ao seu perfil da nuvem**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. No menu de conta superior direito, clique em **Meu perfil**.

1. Na exibição _Chaves SSH_, clique em **Adicionar chave pública**.

1. No formulário _Adicionar uma chave SSH_, dê à sua chave um **Título** e cole a chave SSH pública no campo **Chave**.

1. Clique em **Salvar**.

>[!ENDTABS]

## Conectar-se a um ambiente remoto

Você pode se conectar a um ambiente remoto usando a CLI do `magento-cloud` ou um comando SSH. Os comandos da CLI do `magento-cloud` só podem ser usados em ambientes de integração Starter e Pro.

### Usar a CLI da nuvem

**Para fazer logon em um ambiente de integração remota**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Listar os ambientes nesse projeto.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### Usar um comando SSH

O [!DNL Cloud Console] inclui uma lista de comandos de acesso à Web e SSH para cada ambiente.

**Para copiar o comando SSH**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selecione um projeto na lista _Todos os projetos_.

1. Selecione um ambiente.

1. Clique em **[!UICONTROL SSH]**.

1. Na guia _SSH_, clique no botão Copiar para copiar o comando SSH completo para a área de transferência.

1. Abra um terminal e cole o comando SSH para criar uma conexão.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Para ambientes de preparo e produção Pro, o comando SSH pode ser semelhante a:
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

A infraestrutura do Adobe Commerce na nuvem é compatível com o acesso a seus ambientes usando sFTP (FTP seguro) com autenticação SSH. Use um cliente compatível com autenticação de chave SSH para sFTP e use sua chave SSH pública. Sua chave SSH pública deve ser adicionada ao ambiente de destino. Para ambientes Starter e ambientes de integração Pro, você pode [adicioná-lo por meio do [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

Conexões sFTP somente leitura _não_ suportadas; o acesso sFTP é fornecido com permissão de _gravação_ por padrão.

Ao configurar o sFTP, use as informações do comando do ambiente de acesso SSH: `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **Nome de usuário**: todo o conteúdo antes de `@` no seu destino de acesso SSH.
- **Senha**: você não precisa de uma senha para sFTP. O acesso sFTP usa a autenticação de chave SSH.
- **Host**: todo o conteúdo após `@` no seu acesso SSH.
- **Porta**: 22, que é a porta SSH padrão.
- **SSH** Chave privada: se necessário, forneça a localização da sua chave privada para o cliente sFTP. Por padrão, chaves privadas são armazenadas no diretório `~/.ssh`.

Dependendo do cliente, opções adicionais podem ser necessárias para concluir a autenticação SSH para sFTP. Revise a documentação do cliente selecionado.

Para **ambientes iniciais e ambientes de integração Pro**, você também pode considerar [adicionar um `mount`](../application/properties.md#mounts) para acessar um diretório específico. Você adicionaria a montagem ao arquivo `.magento.app.yaml`. Para obter uma lista de diretórios graváveis, consulte [Estrutura de projeto](../project/file-structure.md). Esse ponto de montagem só funciona nesses ambientes.

Para **ambientes de Preparo e Produção Pro**, se você não tiver acesso SSH ao ambiente, deverá [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para solicitar acesso sFTP e um ponto de montagem para acesso à pasta específica, por exemplo, `pub/media`.

>[!NOTE]
>Para Preparo e Produção Profissionais, se a conexão sFTP for para um usuário _genérico_ que **não** precise ser [adicionado ao projeto na nuvem](../project/user-access.md), você deve [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) com a chave **pública** anexada. **Nunca forneça sua chave SSH privada.**

## Tunelamento SSH

Você pode usar o tunelamento SSH para se conectar a um serviço do ambiente de desenvolvimento local como se o serviço fosse local. Antes do túnel, configure o [SSH](#add-an-ssh-public-key-to-your-account).

Use um aplicativo de terminal para fazer logon e emitir comandos.

```bash
magento-cloud login
```

Verifique se algum túnel está aberto usando.

```bash
magento-cloud tunnel:list
```

Para criar um túnel, você deve saber o [nome do aplicativo](../application/properties.md#name). Você pode verificar o nome do aplicativo usando a CLI:

```bash
magento-cloud apps
```

### Configurar o túnel SSH

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

Por exemplo, para abrir um túnel para a ramificação `sprint5` em um projeto com um aplicativo chamado `mymagento`, digite

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

Exemplo de resposta:

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**Para exibir informações sobre o túnel**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### Conectar-se a serviços

Depois de estabelecer um túnel SSH, você pode se conectar a serviços como se estivessem sendo executados localmente. Por exemplo, para se conectar ao banco de dados, use o seguinte comando:

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```

#### Obter credenciais do MySQL

Recupere as credenciais de logon do MySQL das propriedades `database` na variável de ambiente `$MAGENTO_CLOUD_RELATIONSHIPS`. Para obter instruções sobre como recuperar as informações em um ambiente local ou remoto, consulte [Relações de serviço](../services/services-yaml.md#service-relationships).
