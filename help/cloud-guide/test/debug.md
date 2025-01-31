---
title: Configurar [!DNL Xdebug]
description: Saiba como configurar a extensão Xdebug para depurar o desenvolvimento de projetos do Adobe Commerce na infraestrutura em nuvem.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1920'
ht-degree: 0%

---

# Configurar Xdebug

[!DNL Xdebug] é uma extensão para depurar seu PHP. Embora você possa usar um IDE de sua escolha, o seguinte explica como configurar o [!DNL Xdebug] e o [!DNL PhpStorm] para depurar em seu ambiente local.

>[!NOTE]
>
>Você pode configurar o [!DNL Xdebug] para ser executado no ambiente do Cloud Docker para depuração local sem alterar a configuração do projeto do Adobe Commerce na infraestrutura em nuvem. Consulte [Configurar Xdebug para Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).

Para habilitar [!DNL Xdebug], você deve configurar um arquivo no repositório Git, configurar o IDE e configurar o encaminhamento de portas. Você pode definir algumas configurações no arquivo `magento.app.yaml`. Após a edição, envie as alterações do Git por push em todos os ambientes de Iniciação e integração Pro para habilitar o [!DNL Xdebug]. O [!DNL Xdebug] já está disponível em ambientes de Preparo e Produção Profissionais.

Depois de configurado, você pode depurar comandos CLI, solicitações da Web e código. Lembre-se de que todos os ambientes de infraestrutura em nuvem são somente leitura. Clonar o código no ambiente de desenvolvimento local para executar a depuração. Para ambientes de Produção e Preparo Pro, consulte [instruções adicionais](#debug-for-pro-staging-and-production) para [!DNL Xdebug].

## Requisitos

Para executar e usar o [!DNL Xdebug], você precisa da URL SSH do ambiente. Você pode localizar as informações por meio do [[!DNL Cloud Console]](../project/overview.md) ou do seu [!DNL Cloud Onboarding UI].

## Configurar Xdebug

Para configurar o [!DNL Xdebug], siga estas etapas:

- [Trabalhe em uma ramificação para enviar atualizações de arquivo](#get-started-with-a-branch)
- [Habilitar [!DNL Xdebug] para ambientes](#enable-xdebug-in-your-environment)
- [Configurar o servidor PHPStorm](#configure-phpstorm-server)
- [Configurar encaminhamento de porta](#set-up-port-forwarding)

### Introdução a uma ramificação

Para adicionar [!DNL Xdebug], o Adobe recomenda trabalhar em [uma ramificação de desenvolvimento](../dev-tools/cloud-cli-overview.md#create-an-environment-branch).

### Ativar o Xdebug no seu ambiente

Você pode habilitar o [!DNL Xdebug] diretamente para todos os ambientes de inicialização e integração Pro. Essa etapa de configuração não é necessária para ambientes de produção e preparo profissionais. Consulte [Depuração para Preparo e Produção Profissionais](#debug-for-pro-staging-and-production).

>[!VIDEO](https://video.tv.adobe.com/v/3437407?learn=on)

Para habilitar [!DNL Xdebug] para o seu projeto, adicione `xdebug` à seção `runtime:extensions` do arquivo `.magento.app.yaml`.

**Para habilitar Xdebug**:

1. No terminal local, abra o arquivo `.magento.app.yaml` em um editor de texto.

1. Na seção `runtime`, em `extensions`, adicione `xdebug`. Por exemplo:

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. Salve as alterações no arquivo `.magento.app.yaml` e saia do editor de texto.

1. Adicione, confirme e envie as alterações para reimplantar o ambiente.

   ```bash
   git add .magento.app.yaml
   ```

   ```bash
   git commit -m "add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

Quando implantado em ambientes iniciais e ambientes de integração Pro, o [!DNL Xdebug] agora está disponível. Continue configurando seu IDE. Para PhpStorm, consulte [Configurar PhpStorm](#configure-phpstorm).

### Configurar o servidor PhpStorm

>[!VIDEO](https://video.tv.adobe.com/v/3437409?learn=on)

O IDE [PhpStorm](https://www.jetbrains.com/phpstorm/) deve ser configurado para funcionar corretamente com [!DNL Xdebug].

**Para configurar o PhpStorm para funcionar com o Xdebug**:

1. No projeto PhpStorm, abra o painel **Configurações**.

   - _macOS_—Selecione **PhpStorm** > **Configurações**.
   - _Windows/Linux_—Selecione **Arquivo** > **Configurações**.

1. No painel _Configurações_, expanda a seção **PHP** e clique em **Servidores**.

1. Clique em **+** para adicionar uma configuração de servidor. O nome do projeto está em cinza na parte superior.

1. [Opcional] Defina as seguintes configurações para a nova configuração do servidor. Consulte [Nenhum servidor de depuração configurado](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured) na documentação do _PHPStorm_.

   - **Nome** — Insira o mesmo que o nome de host. Este valor deve corresponder ao valor da variável `PHP_IDE_CONFIG` em [Comandos CLI de Depuração](#debug-cli-commands) para usar a CLI para depuração.
   - **Host** — Digite o nome do host.
   - **Porta** — Digite `443`.
   - **Depurador**—Selecione `Xdebug`.

1. Selecione **Usar mapeamentos de caminho**. No painel _Arquivo/Diretório_, a raiz do projeto para `serverName` é exibida.

1. Na coluna **Caminho absoluto no servidor**, clique no ícone **Editar** e adicione uma configuração com base no ambiente.

   - Para todos os ambientes Starter e Pro integration, o caminho remoto é `/app`.
   - Para ambientes de preparo e produção profissionais:

      - Produção: `/app/<project_code>/`
      - Estágios: `/app/<project_code>_stg/`

1. Altere a porta [!DNL Xdebug] para `9000,9003` ou você pode limitá-la a apenas `9000` no painel **PHP** > **Depurar** > **Xdebug** > **Porta de Depuração**.

1. Clique em **Aplicar**.

### Crie a configuração de execução/depuração do PHPStorm

Isso permite que o aplicativo tenha as configurações de depuração corretas para lidar com a solicitação do aplicativo do Adobe Commerce.

>[!VIDEO](https://video.tv.adobe.com/v/3437426?learn=on)

1. Abra o aplicativo PHPStorm e clique em **[!UICONTROL Add Configuration]** no lado superior direito da tela.

1. Clique em **[!UICONTROL Add new run configuration]**.

1. Selecione a opção **[!UICONTROL PHP Remote Debug]**.

   - Insira um nome exclusivo, mas reconhecível.
   - Marque a caixa de seleção [!UICONTROL Filter debug connection by IDE key]**.
   - Selecione o servidor criado na [seção anterior](#configure-phpstorm-server). Se você ainda não o criou, crie um agora, mas consulte essa parte do guia de configuração.
   - No campo de texto **[!UICONTROL IDE key(session id)]**, digite `PHPSTORM` em letras maiúsculas. Usaremos isso em outras partes da configuração, portanto, é importante manter isso inalterado. Se você escolher outra string, deverá se lembrar de usá-la em outro lugar no processo de instalação e configuração.

1. Clique em **[!UICONTROL Apply]** > **[!UICONTROL OK]**.

### Configurar encaminhamento de porta

>[!VIDEO](https://video.tv.adobe.com/v/3437410?learn=on)

Mapeie a conexão `XDEBUG` do servidor para o sistema local. Para fazer qualquer tipo de depuração, você deve encaminhar a porta 9000 do Adobe Commerce no servidor de infraestrutura em nuvem para o computador local. Consulte uma das seguintes seções:

- [Encaminhamento de portas no Mac ou UNIX](#port-forwarding-on-mac-or-unix)
- [Encaminhamento de porta no Windows](#port-forwarding-on-windows)

#### Encaminhamento de portas no Mac ou UNIX®

**Para configurar o encaminhamento de portas em um ambiente Mac ou UNIX®**:

1. Abra um terminal.

1. Use SSH para estabelecer a conexão.

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   Use a opção `-v` (detalhada) para que sempre que um soquete for conectado à porta que está sendo encaminhada ele seja exibido no terminal.

   Se um erro &quot;não é possível conectar&quot; ou &quot;não foi possível escutar a porta no remoto&quot; for exibido, pode haver outra sessão SSH ativa persistindo no servidor que está ocupando a porta 9000. Se essa conexão não estiver sendo usada, você poderá encerrá-la.

**Para solucionar problemas de conexão**:

1. Use o SSH para fazer logon no ambiente de integração remota, preparo ou produção.

1. Exibir uma lista de sessões SSH: `who`

1. Visualizar sessões SSH existentes por usuário. Tenha cuidado para não afetar um usuário que não seja você mesmo!

   - integração: usuários com nomes semelhantes a `dd2q5ct7mhgus`
   - Estágios: os nomes de usuários são semelhantes a `dd2q5ct7mhgus_stg`
   - Produção: nomes de usuários semelhantes a `dd2q5ct7mhgus`

1. Para uma sessão de usuário mais antiga que a sua, localize o valor do pseudo-terminal (PTS), como `pts/0`.

1. Elimine a ID de processo (PID) correspondente ao valor PTS.

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   Exemplo de resposta:

   ```
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   Para encerrar a conexão, digite um comando kill com o ID do processo (PID).

   ```bash
   kill 3664
   ```

#### Encaminhamento de porta no Windows

Para configurar o encaminhamento de portas (encapsulamento SSH) no Windows, você deve configurar o aplicativo de terminal do Windows. Este exemplo passa pela criação de um túnel SSH usando [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Você pode usar outros aplicativos, como o Cygwin. Para obter mais informações sobre outros aplicativos, consulte a documentação do fornecedor fornecida com esses aplicativos.

**Para configurar um túnel SSH no Windows usando Putty**:

1. Se ainda não tiver feito isso, baixe [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

1. Comece o Putty.

1. No painel Categoria, clique em **Sessão**.

1. Insira as seguintes informações:

   - Campo **Nome do host (ou endereço IP)**: insira a [URL do SSH](../development/secure-connections.md#connect-to-a-remote-environment) para o seu servidor na nuvem
   - Campo **Porta**: Digite `22`

   ![Configurar Putty](../../assets/xdebug/putty-session.png)

1. No painel _Categoria_, clique em **Conexão** > **SSH** > **Túneis**.

1. Insira as seguintes informações:

   - Campo **Porta do Source**: Digite `9000`
   - Campo **Destino**: Digite `127.0.0.1:9000`
   - Clique em **Remoto**

1. Clique em **Adicionar**.

   ![Criar um túnel SSH no Putty](../../assets/xdebug/putty-tunnels.png)

1. No painel _Categoria_, clique em **Sessão**.

1. No campo **Sessões Salvas**, digite um nome para esse túnel SSH.

1. Clique em **Salvar**.

   ![Salve seu túnel SSH](../../assets/xdebug/putty-session-save.png)

1. Para testar o túnel SSH, clique em **Carregar** e em **Abrir**.

   Se um erro &quot;não é possível conectar&quot; for exibido, verifique o seguinte:

   - Todas as configurações de Putty estão corretas
   - Você está executando o Putty na máquina em que suas chaves SSH privadas do Adobe Commerce na infraestrutura em nuvem estão localizadas

## Acesso SSH a ambientes Xdebug

Para iniciar a depuração, executar a configuração e muito mais, você precisa dos comandos SSH para acessar os ambientes. Você pode obter essas informações por meio do [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) e da planilha do seu projeto.

Para ambientes Starter e ambientes de integração Pro, você pode usar o seguinte comando da CLI `magento-cloud` para SSH nesses ambientes:

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

Para usar [!DNL Xdebug], SSH conecta o ambiente da seguinte maneira:

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

Por exemplo,

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## Depuração para preparo e produção profissionais

>[!NOTE]
>
>Em ambientes de Preparo e Produção Profissionais, o [!DNL Xdebug] está sempre disponível, pois esses ambientes têm uma configuração especial para o [!DNL Xdebug]. Todas as requisições normais da web são roteadas para um processo PHP dedicado que não tem [!DNL Xdebug]. Portanto, essas solicitações são processadas normalmente e não estão sujeitas à degradação de desempenho quando [!DNL Xdebug] é carregado. Quando uma solicitação da Web enviada com a chave [!DNL Xdebug] é roteada para um processo PHP separado que tem [!DNL Xdebug] carregado.

Para usar [!DNL Xdebug] especificamente no ambiente de preparo e produção do plano Pro, você cria um túnel SSH separado e uma sessão da Web somente para os quais tem acesso. Esse uso difere do acesso típico, fornecendo acesso apenas a você e não a todos os usuários.

Você precisa do seguinte:

- Comandos SSH para acessar os ambientes. Você pode obter essas informações por meio do [[!DNL Cloud Console]](../project/overview.md) ou do seu [!DNL Cloud Onboarding UI].
- O valor `xdebug_key` definido ao configurar os ambientes de Preparo e Pro.

  O `xdebug_key` pode ser encontrado usando SSH para fazer logon no nó primário e executando:

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**Para configurar um túnel SSH para um ambiente de Preparo ou de Produção**:

1. Abra um terminal.

1. Limpe todas as sessões SSH para cada nó da Web do cluster.

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. Configure o túnel SSH para o Xdebug para cada nó da Web do cluster.

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

>[!NOTE]
>
>Para obter o valor correto para `USERNAME@CLUSTER.ent.magento.cloud`:
>- Método 1: Magento-Cloud CLI: `magento-cloud ssh --all`
>- Método 2: Console Commerce: https://CONSOLE-URL/ENVIRONMENT, clique na lista suspensa `SSH v`

**Para iniciar a depuração usando a URL de ambiente**:

Esta é uma demonstração das configurações usadas, bem como uma demonstração do parâmetro GET para iniciar uma sessão de depuração remota.

>[!VIDEO](https://video.tv.adobe.com/v/3437417?learn=on)

1. Habilitar depuração remota; visite o site no navegador e anexe o seguinte à URL em que `KEY` é o valor de `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   Esta etapa define o cookie que envia solicitações do navegador para o disparador [!DNL Xdebug].

1. Conclua sua depuração com [!DNL Xdebug].

1. Quando estiver pronto para encerrar a sessão, use o seguinte comando para remover o cookie e finalizar a depuração pelo navegador, onde `KEY` é o valor de `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >Não há suporte para o `XDEBUG_SESSION_START` passado por `POST` solicitações.

## Comandos CLI de depuração

Esta seção aborda os comandos CLI de depuração.

Para depurar comandos da CLI:

1. SSH no servidor que você deseja depurar usando comandos CLI.

1. Crie as seguintes variáveis de ambiente:

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   Essas variáveis são removidas quando a sessão SSH termina.

1. Iniciar depuração

   Em ambientes Starter e de integração Pro, execute o comando da CLI para depurar.
Você pode adicionar opções de tempo de execução, por exemplo:

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   Em ambientes de preparo e produção Pro, você deve especificar o caminho para o arquivo de configuração PHP [!DNL Xdebug] ao depurar comandos CLI, por exemplo:

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## Depurar solicitações da Web

As etapas a seguir ajudam a depurar solicitações da Web.

1. No menu _Extensão_, clique em **Depurar** para habilitar.

1. Clique com o botão direito do mouse, selecione o menu de opções e defina a chave IDE como **PHPSTORM**.

1. Instale o cliente [!DNL Xdebug] no navegador. Configure e ative-o.

### Exemplo: configuração do Chrome

Esta seção discute como usar o [!DNL Xdebug] no Chrome usando a extensão Auxiliar do [!DNL Xdebug]. Para obter informações sobre as ferramentas do [!DNL Xdebug] para outros navegadores, consulte a documentação do navegador.

**Para usar o Auxiliar Xdebug com o Chrome**:

1. Crie um [túnel SSH](#ssh-access-to-xdebug-environments) para o servidor na nuvem.

1. Instale a [Extensão auxiliar do Xdebug](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc) do armazenamento do Chrome.

1. Ative a extensão no Chrome como mostrado na figura a seguir.

   ![Habilitar a extensão Xdebug no Chrome](../../assets/xdebug/enable-chrome-ext.png)

1. No Chrome, clique com o botão direito do mouse no ícone verde auxiliar na barra de ferramentas do Chrome.

1. No menu suspenso, clique em **Opções**.

1. Na lista _Chave IDE_, clique em **PhpStorm**.

1. Clique em **Salvar**.

   ![Opções do Auxiliar de Depuração](../../assets/xdebug/helper-options.png)

1. Abra o projeto PhpStorm.

1. Na barra de navegação superior, clique no ícone **Iniciar escuta**.

   Se a barra de navegação não for exibida, clique em **Exibir** > **Barra de Navegação**.

1. No painel de navegação do PhpStorm, clique duas vezes no arquivo PHP para testar.

## Depurar código local

Devido aos ambientes somente leitura, você deve extrair código para a estação de trabalho local de um ambiente ou ramificação Git específica para executar a depuração.

O método que você escolher depende de você. Você tem as seguintes opções:

- Confira o código do Git e execute `composer install`

  Este método funciona a menos que `composer.json` faça referência a pacotes em repositórios privados aos quais você não tem acesso. Este método resulta na obtenção de toda a base de código do Adobe Commerce.

- Copiar os diretórios `vendor`, `app`, `pub`, `lib` e `setup`

  Esse método resulta em ter todos os códigos que você pode testar. Dependendo de quantos ativos estáticos você tem, isso pode resultar em uma longa transferência com um grande volume de arquivos.

- Copiar somente o diretório `vendor`

  Como a maioria do código está no diretório `vendor`, esse método provavelmente resultará em um bom teste, embora não esteja testando toda a base de código.

**Para compactar arquivos e copiá-los no computador local**:

1. Use o SSH para fazer logon no ambiente remoto.

1. Compacte os arquivos.

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   Por exemplo, para compactar somente o diretório `vendor`:

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. Em seu ambiente local, use o PhpStorm para compactar os arquivos.

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
