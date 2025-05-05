---
title: Serviço PrivateLink
description: Saiba como usar o serviço PrivateLink para estabelecer uma conexão segura entre uma nuvem privada e a plataforma de nuvem da Adobe Commerce na mesma região.
feature: Cloud, Iaas, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# Serviço PrivateLink

O Adobe Commerce na infraestrutura de nuvem oferece suporte à integração com o serviço [AWS PrivateLink](https://aws.amazon.com/privatelink/) ou [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/). Você pode usar o PrivateLink para estabelecer comunicação segura e privada entre o Adobe Commerce em ambientes de infraestrutura em nuvem com serviços e aplicativos hospedados em sistemas externos. O aplicativo do Adobe Commerce e os sistemas externos devem ser acessíveis por meio de endpoints da Nuvem privada virtual (VPC) configurados na mesma plataforma de nuvem (AWS ou Azure) na mesma região de nuvem.

>[!TIP]
>
>O PrivateLink é melhor usado para proteger conexões para integrações não HTTP(S), como transferências de banco de dados ou arquivos. Se você planeja integrar seu aplicativo com as APIs do Adobe Commerce, veja como criar uma [Malha de API Adobe](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/) no _Malha de API para o Adobe Developer App Builder_.

## Recursos e suporte

A integração do serviço PrivateLink para projetos de infraestrutura em nuvem do Adobe Commerce inclui os seguintes recursos e suporte:

- Uma conexão segura entre a Nuvem privada virtual (VPC) do cliente e o Adobe VPC na mesma plataforma de nuvem (AWS ou Azure) na mesma região da Nuvem.
- Suporte para comunicação unidirecional ou bidirecional entre os serviços de endpoint disponíveis no Adobe e VPCs do cliente.
- Ativação de serviço:

   - Abra as portas necessárias no ambiente Adobe Commerce na infraestrutura em nuvem
   - Estabeleça a conexão inicial entre o cliente e os VPCs Adobe
   - Solução de problemas de conexão durante a ativação

## Limitação

- O suporte para PrivateLink está disponível somente em ambientes de produção e preparo profissionais. Não está disponível em ambientes locais ou de integração, nem em projetos iniciais.
- Não é possível estabelecer conexões SSH usando PrivateLink. Consulte [Habilitar chaves SSH](secure-connections.md).
- O suporte da Adobe Commerce não abrange a solução de problemas do AWS PrivateLink além da ativação inicial.
- Os clientes são responsáveis pelos custos associados ao gerenciamento de sua própria VPC.
- Você não pode usar o protocolo HTTPS (porta 443) para se conectar ao Adobe Commerce na infraestrutura de nuvem através do Link Privado do Azure devido a [Encobrimento rápido da origem](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html?lang=pt-BR). Essa limitação não se aplica ao AWS PrivateLink.
- PrivateDNS não está disponível.

## Tipos de conexão PrivateLink

Há dois tipos de conexão PrivateLink disponíveis, mostrados no diagrama de rede a seguir, para estabelecer uma comunicação segura entre sua loja e sistemas externos hospedados fora do ambiente da nuvem.

![Diagrama de rede PrivateLink](../../assets/privatelink-architecture-diagram.png)

Escolha um dos tipos de conexão PrivateLink mais adequados para seus ambientes do Adobe Commerce na infraestrutura em nuvem:

- **PrivateLink Unidirecional**-Escolha esta configuração para recuperar dados com segurança de um Adobe Commerce no repositório de infraestrutura da nuvem.
- **PrivateLink bidirecional**-Escolha esta configuração para estabelecer conexões seguras de e para sistemas fora da Adobe Commerce no ambiente de infraestrutura em nuvem. A opção bidirecional requer duas conexões:

   - Uma conexão entre o VPC do cliente e o Adobe VPC
   - Uma conexão entre o Adobe VPC e o VPC do cliente

>[!TIP]
>
>Trabalhe com o administrador de rede ou o provedor da plataforma na nuvem para obter ajuda sobre como selecionar o tipo de conexão PrivateLink ou ajuda sobre configuração e administração do VPC. Consulte a documentação do PrivateLink da plataforma de nuvem: [AWS PrivateLink](https://aws.amazon.com/privatelink/) ou [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/).

## Solicitar ativação do PrivateLink

>[!WARNING]
>
>A habilitação do PrivateLink pode levar até _cinco_ dias úteis. O fornecimento de informações incompletas ou imprecisas pode atrasar o processo.

### Pré-requisitos

![marque](../../assets/fix.svg) uma conta da nuvem (AWS ou Azure) na mesma região da instância do Adobe Commerce na infraestrutura em nuvem.

![marque](../../assets/fix.svg) uma VPC no ambiente do cliente que hospeda os serviços para se conectar por meio do PrivateLink. Consulte a documentação do AWS ou do Azure para obter ajuda com a configuração do VPC ou entre em contato com o administrador de rede.

![verificar](../../assets/fix.svg) Para conexões PrivateLink bidirecionais, você deve criar a configuração do serviço de ponto de extremidade para seu aplicativo ou serviço e criar um ponto de extremidade em seu ambiente VPC antes de solicitar a habilitação do PrivateLink. Consulte [Configurar para conexões PrivateLink bidirecionais](#set-up-for-bidirectional-privatelink-connections).

Obtenha os seguintes dados necessários para a ativação do PrivateLink:

- **Número da conta da Nuvem do Cliente** (AWS ou Azure) — deve estar na mesma região que a Adobe Commerce na instância da infraestrutura em nuvem
- **Região da nuvem**—Forneça a região da nuvem onde a conta está hospedada para fins de verificação
- **Portas de serviços e comunicação**—o Adobe deve abrir portas para habilitar a comunicação de serviço entre VPCs, por exemplo, porta SQL 3306, porta SFTP 2222
- **ID do Projeto** — Forneça a ID do projeto do Adobe Commerce na infraestrutura em nuvem Pro. Você pode obter a ID do Projeto e outras informações do projeto usando o seguinte comando [CLI da Nuvem](../dev-tools/cloud-cli-overview.md): `magento-cloud project:info`
- **Tipo de conexão** — especifique unidirecional ou bidirecional para o tipo de conexão
- **Serviço de ponto de extremidade**—Para conexões PrivateLink bidirecionais, forneça a URL DNS para o serviço de ponto de extremidade do VPC ao qual o Adobe deve se conectar, por exemplo: `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **Acesso ao serviço de ponto de extremidade concedido**—Para se conectar ao serviço externo, permita o acesso ao serviço de ponto de extremidade à seguinte entidade de conta da AWS: `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >Se o acesso ao serviço de ponto de extremidade não for fornecido, a conexão PrivateLink bidirecional com o serviço no VPC será **não** adicionada, o que atrasa a configuração.

#### Pré-requisitos adicionais específicos da ativação do Azure Private Link

- Forneça a ID do cluster; usando SSH, faça logon no controle remoto e use o comando: `cat /etc/platform_cluster`
- Para que um serviço externo se conecte ao cluster Adobe Commerce Pro, é necessário:

   - Uma lista de portas no cluster Pro para expor ao novo Ponto de Extremidade Privado externo
   - Uma lista de IDs de assinatura do Azure para as conexões de Ponto de Extremidade Privado

- Para conectar seu cluster Adobe Commerce Pro a um serviço externo, você precisa:

   - Uma lista de IDs de recursos para os serviços de destino. As IDs de serviço de Link privado externo são semelhantes ao seguinte:

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### Fluxo de trabalho de ativação

O fluxo de trabalho a seguir descreve o processo de ativação da integração do PrivateLink com o Adobe Commerce na infraestrutura em nuvem.

1. **O cliente** envia um tíquete de suporte solicitando a habilitação do PrivateLink com a linha de assunto `PrivateLink support for <company>`. Inclua os [dados necessários para a habilitação](#prerequisites) no tíquete. O Adobe usa o tíquete Suporte para coordenar a comunicação durante o processo de ativação.

1. **Adobe** habilita o acesso da conta do cliente ao serviço de ponto de extremidade no Adobe VPC.

   - Atualize a configuração do serviço de ponto de extremidade Adobe para aceitar solicitações iniciadas da conta do AWS ou do Azure do cliente.
   - Atualize o tíquete Suporte para fornecer o nome de serviço do ponto de extremidade do Adobe VPC ao qual se conectar, por exemplo `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`.

1. O **Cliente** adiciona o serviço de ponto de extremidade Adobe à sua conta da Nuvem (AWS ou Azure), o que aciona uma solicitação de conexão com o Adobe. Consulte a documentação da plataforma na nuvem para obter instruções:

   - Para o AWS, consulte [Aceitar e rejeitar solicitações de conexão de ponto de extremidade de interface].
   - Para o Azure, consulte [Gerenciar solicitações de conexão].

1. **Adobe** aprova a solicitação de conexão.

1. Após a aprovação da solicitação de conexão, o **cliente** [verifica a conexão](#test-vpc-endpoint-service-connection) entre a VPC e o Adobe VPC.

1. Etapas adicionais para ativar conexões bidirecionais:

   - **Adobe** fornece a entidade de conta de Adobe (usuário raiz para a conta do AWS ou do Azure) e solicita acesso ao serviço de ponto de extremidade do VPC do cliente.
   - O **Cliente** habilita o acesso Adobe ao serviço de ponto de extremidade no VPC do cliente. Isso pressupõe que a entidade de conta Adobe tenha acesso a `arn:aws:iam::402592597372:root`, conforme descrito anteriormente no pré-requisito **acesso ao serviço de Ponto de Extremidade concedido**.

      - Atualize a configuração do serviço de ponto de extremidade do cliente para aceitar solicitações iniciadas da conta Adobe. Consulte a documentação da plataforma na nuvem para obter instruções:

         - Para o AWS, consulte [Adicionando e removendo permissões para o serviço de ponto de extremidade].
         - Para o Azure, consulte [Gerenciar uma conexão de Ponto de Extremidade Privado]

      - Forneça ao Adobe o nome do serviço de ponto de extremidade do VPC do cliente.

   - **Adobe** adiciona o serviço de ponto de extremidade do cliente à conta da plataforma Adobe (AWS ou Azure), o que aciona uma solicitação de conexão com o cliente VPC.
   - O **Cliente** aprova a solicitação de conexão do Adobe para concluir a instalação.
   - O **Cliente** [verifica a conexão](#test-vpc-endpoint-service-connection) do Adobe VPC.

## Testar conexão do serviço de ponto de extremidade do VPC

Você pode usar o aplicativo Telnet para testar a conexão com o serviço de ponto de extremidade do VPC.

**Para testar a conexão com o serviço de ponto de extremidade do VPC**:

1. No diretório raiz do projeto, **confira** o ambiente de Preparo ou Produção configurado para acessar o serviço de ponto de extremidade do PrivateLink.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Execute o seguinte comando CURL:

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   Exemplo:

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   Exemplo de resposta bem-sucedida:

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   Exemplo de resposta com falha:

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. Verifique se o serviço está escutando na VM.

   ```bash
   netstat -na | grep <port>
   ```

1. Verifique o fluxo de pacotes.

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   Verifique as seguintes configurações internas para garantir que a configuração seja válida:

   - Configurações de ponto de extremidade e serviços de ponto de extremidade
   - Configurações do Balanceador de Carga de Rede (NLB)
   - Os grupos de destino no NLB e verificar se estão íntegros
   - O URL do ponto de extremidade netcat/curl de cada VM (listado acima)

   Consulte os seguintes artigos para obter ajuda com a solução de problemas de conexão:

   - [AWS: Solucionando problemas de conexões de serviço de ponto de extremidade]
   - [Amazon: Solucionando problemas de conectividade do Azure Private Link]

   Se não conseguir resolver os erros, atualize o tíquete de Suporte da Adobe Commerce para solicitar ajuda para estabelecer a conexão.

## Alterar configuração do PrivateLink

[Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) para alterar uma configuração existente do PrivateLink. Por exemplo, você pode solicitar alterações como as seguintes:

- Remova a conexão PrivateLink do ambiente de produção ou preparo do Adobe Commerce na infraestrutura em nuvem Pro.
- Altere o número da conta da plataforma Customer Cloud para acessar o serviço de ponto de extremidade Adobe.
- Adicionar ou remover conexões PrivateLink do Adobe VPC para outros serviços de endpoint disponíveis no ambiente VPC do cliente.

## Configurar conexões bidirecionais do PrivateLink

O VPC do cliente deve ter os seguintes recursos disponíveis para oferecer suporte a conexões bidirecionais de PrivateLink:

- Um NLB (Balanceador de Carga de Rede)
- Uma configuração de serviço de endpoint que permite o acesso a um aplicativo ou serviço pelo VPC do cliente
- Um [ponto de extremidade de interface] (AWS) ou [ponto de extremidade privado] (Azure) que permite que o Adobe se conecte aos serviços de ponto de extremidade hospedados em sua VPC

Se esses recursos não estiverem disponíveis na VPC do cliente, você deverá fazer logon na conta da plataforma em nuvem para adicionar a configuração.

- Console do Amazon VPC - `https://console.aws.amazon.com/vpc/`
- Portal do Azure- `https://portal.azure.com`

Consulte a documentação da sua plataforma na nuvem para obter instruções de configuração do PrivateLink:

- **Documentação do AWS PrivateLink**
   - [Criar um Balanceador de Carga de Rede]
   - [Criar uma configuração de serviço de ponto de extremidade]
   - [Criar um ponto de extremidade de interface]
   - [Ciclo de vida do ponto de extremidade da interface]

- **Documentação do Azure PrivateLink**
   - [Criar um Balanceador de Carga]
   - [Fluxo de trabalho do Link Privado do Azure]

<!--Link definitions-->

[Aceitar e rejeitar solicitações de conexão de ponto de extremidade de interface]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[Adicionar e remover permissões do serviço de ponto de extremidade]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon: solução de problemas de conectividade do Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS: solução de problemas de conexões de serviço de endpoint]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Fluxo de trabalho do Link privado do Azure]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[Criar um balanceador de carga]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[Criar um Balanceador de Carga de Rede]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[Criar uma configuração de serviço de ponto de extremidade]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[Criar um ponto de extremidade de interface]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[endpoint da interface]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[Gerenciar uma conexão de Ponto de Extremidade Privado]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[Gerenciar solicitações de conexão]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[terminal privado]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
