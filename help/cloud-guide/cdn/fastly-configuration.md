---
title: Configurar os serviços do Fastly
description: Saiba como configurar os serviços do Fastly para seu projeto no Adobe Commerce.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: f9ce1e8b-4e9f-488e-8a4d-f866567c41d8
source-git-commit: 867abffd6cbed6e026c20b646ff641cc6ab40580
workflow-type: tm+mt
source-wordcount: '2063'
ht-degree: 0%

---

# Configurar os serviços do Fastly

O Fastly é necessário para o Adobe Commerce em ambientes de Preparo e Produção de infraestrutura em nuvem.

O Fastly trabalha com o Varnish para fornecer recursos rápidos de armazenamento em cache e uma Rede de entrega de conteúdo (CDN) para ativos estáticos. O Fastly também fornece um Firewall de Aplicativo Web (WAF) para proteger seu site e a infraestrutura da Nuvem. Para proteger seu site e sua infraestrutura em nuvem contra tráfego mal-intencionado e ataques, roteie todo o tráfego de entrada do site pelo Fastly.

>[!NOTE]
>
>O Fastly não está disponível em ambientes de integração.

Conclua as etapas a seguir para habilitar, configurar e testar o Fastly no início do processo de desenvolvimento do site para habilitar o acesso seguro ao site.

- Obter credenciais do Fastly para ambientes de preparo e produção
- Habilitar o cache Fastly CDN
- Carregar trechos de VCL do Fastly
- Atualizar a configuração do DNS para rotear o tráfego para o serviço Fastly
- Teste do armazenamento em cache Fastly

>[!NOTE]
>
>Depois de ativar e verificar a configuração inicial do Fastly, você pode personalizar a configuração. Por exemplo, você pode ativar opções adicionais, como otimização de imagem, módulos de borda e código de VCL personalizado. Consulte [Personalizar a configuração do cache](fastly-custom-cache-configuration.md).

Durante o provisionamento do projeto, a Adobe adiciona seu projeto à [conta de serviço do Fastly](fastly.md#fastly-service-account-and-credentials) para o Adobe Commerce na infraestrutura na nuvem e cria as credenciais da conta do Fastly para os ambientes de Início `master` e Preparo e Produção Profissional. Cada ambiente tem credenciais exclusivas.

Você precisa das credenciais do Fastly para configurar os serviços do Fastly CDN com o Administrador do Adobe Commerce e enviar as solicitações da API do Fastly.

## Acesso rápido ao Painel de administração

Com o Adobe Commerce na infraestrutura em nuvem, não é possível acessar diretamente o Painel de administração do Fastly.

Você deve usar o administrador do Adobe Commerce para revisar e atualizar a configuração do Fastly em seus ambientes. Se você não conseguir resolver um problema usando os recursos do Fastly no Admin, envie um [tíquete de Suporte do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket).

## Obter credenciais do Fastly

Use os métodos a seguir para localizar e salvar a ID de serviço e o token da API do Fastly no seu ambiente:

**Para exibir suas credenciais do Fastly**:

>[!NOTE]
>
>Não compartilhe seu token de API em tíquetes de suporte, fóruns públicos ou qualquer local público. Além disso, nunca confirme tokens de API em repositórios de código — os repositórios devem conter somente arquivos imutáveis sem informações confidenciais.
>
>O Suporte da Adobe Commerce já tem acesso às chaves necessárias, portanto, não é necessário fornecer o token da API ao buscar assistência.
>
>Se o token da API for compartilhado publicamente ou anexado a um tíquete de suporte, ele será considerado comprometido. Nesses casos, o Adobe será solicitado a gerar um novo token para você.
>
>Relacionado: [Erro ao validar as credenciais do Fastly](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials#solution)

O método de exibição de credenciais é diferente para projetos Pro e Starter.

- Diretório compartilhado montado em IaaS — Em projetos Pro, use SSH para se conectar ao servidor e obter as credenciais do Fastly no arquivo `/mnt/shared/fastly_tokens.txt`. Os ambientes de preparo e produção têm credenciais exclusivas. Você deve obter as credenciais para cada ambiente.

- Espaço de trabalho local — Na linha de comando, use a CLI do `magento-cloud` para [listar e revisar](../environment/variables-cloud.md#viewing-environment-variables) variáveis de ambiente do Fastly.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console] — Verifique as variáveis de ambiente a seguir na [Configuração do ambiente](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>Se não conseguir encontrar as credenciais do Fastly para os ambientes de Preparo ou Produção, entre em contato com o Consultor Técnico do Cliente Adobe (CTA).

## Habilitar cache Fastly

Você precisa dos seguintes componentes para habilitar e configurar os serviços do Fastly:

- Versão mais recente da [CDN Fastly para o módulo Magento 2](fastly.md#fastly-cdn-module-for-magento-2) instalada nos ambientes de Preparo e Produção. Consulte [Atualizar Rapidamente](#upgrade-the-fastly-module).

- [Credenciais rápidas](#get-fastly-credentials) para Adobe Commerce em ambientes de preparo e produção da infraestrutura em nuvem

**Para habilitar o cache do Fastly CDN no Armazenamento em Preparo e na Produção**:

{{admin-login-step}}

1. Clique em **Lojas** > Configurações > **Configuração** > **Avançado** > **Sistema** e expanda **Cache de Página Inteira**.

   ![Expanda para selecionar Fastly](../../assets/cdn/fastly-menu.png)

1. Na seção _Aplicativo de Cache_, remova a seleção de **Usar valor do sistema** e selecione **Fastly CDN** na lista suspensa.

   ![Escolher rapidamente](../../assets/cdn/fastly-enable-admin.png)

1. Expanda **Fastly Configuration** e [escolha as opções de cache](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module).

1. Depois de configurar as opções de armazenamento em cache, clique em **Salvar configuração** na parte superior da página.

1. Limpe o cache de acordo com a notificação.

1. Continue a configurar o Fastly voltando às **Lojas** > **Configurações** > **Configuração** > **Avançadas** > **Sistema** > **Configuração do Fastly**.

### Credenciais do Test Fastly

1. No Administrador, navegue até **Lojas** > Configurações > **Configuração** > **Avançado** > **Sistema** > **Configuração do Fastly**.

1. Se necessário, adicione os valores de **Fastly service ID** e **API token** para o ambiente do seu projeto.

   ![Fastly credentials Administrador](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >Não selecione o link para criar o token da API do Fastly. Em vez disso, use as [credenciais do Fastly (ID do serviço e token da API) fornecidas pelo Adobe](#get-fastly-credentials).

1. Clique em **Testar credenciais**.

1. Se o teste for bem-sucedido, clique em **Salvar configuração** e limpe o cache.

   Se o teste falhar, verifique se os valores corretos da ID de serviço e do token da API correspondem às credenciais do ambiente atual.

   Se o teste falhar novamente, envie um tíquete de suporte do Adobe Commerce ou entre em contato com o representante de conta da Adobe. Para projetos Pro, inclua os URLs para seus sites de Produção e Preparo. Para projetos iniciais, inclua as URLs para seu `Master` e site de Preparo.

>[!NOTE]
>
>Para obter instruções sobre como alterar as credenciais do token da API do Fastly para um ambiente de Preparo ou Produção, consulte [Alterar credenciais do Fastly](fastly.md#change-fastly-api-token).

### Carregar VCL para Fastly

Depois de habilitar o módulo Fastly, carregue o [código VCL](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets) padrão para os servidores Fastly. Este código fornece uma série de trechos de VCL que especificam as definições de configuração para habilitar o armazenamento em cache e outros serviços do Fastly CDN para sua infraestrutura do Adobe Commerce na nuvem.

>[!NOTE]
>
>Os serviços de cache do Fastly não funcionam até que você conclua o upload inicial do código do Fastly VCL para os sites de Preparo e Produção do Adobe Commerce.

**Para carregar o VCL** do Fastly:

1. Na seção _Configuração do Fastly_, clique em **Carregar VCL para Fastly**, conforme mostrado na figura a seguir.

   ![Carregar um Magento VCL para o Fastly](../../assets/cdn/fastly-upload-vcl-admin.png)

1. Depois que o upload for concluído, atualize o cache de acordo com a notificação na parte superior da página.

## Provisionar certificados SSL/TLS

O Adobe fornece um certificado SSL/TLS validado por domínio para fornecer tráfego HTTPS seguro do Fastly. A Adobe fornece um certificado para cada ambiente de Produção Pro, Preparo e Produção de Início para proteger todos os domínios nesse ambiente. Para obter informações detalhadas sobre o certificado fornecido, consulte [certificados SSL (TLS) da Adobe para Adobe Commerce na infraestrutura em nuvem](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html?lang=pt-BR).

>[!NOTE]
>
>Você pode fornecer seu próprio certificado TLS ou SSL em vez de usar o certificado Let&#39;s Encrypt fornecido pela Adobe. No entanto, esse processo requer trabalho adicional para configurar e manter. Para escolher essa opção, envie um tíquete de suporte do Adobe Commerce ou trabalhe com a Adobe para adicionar certificados personalizados e hospedados à sua Adobe Commerce em ambientes de infraestrutura em nuvem.

Para ativar os certificados SSL/TLS para ambientes Adobe Commerce, a automação do Adobe conclui as seguintes etapas:

- Valida a propriedade do domínio
- Provisiona um certificado Let&#39;s Encrypt SSL/TLS que cobre subdomínios e níveis superiores especificados para suas lojas
- Carrega o certificado para o ambiente de nuvem quando o site está ativo

Essa automação exige que você atualize a configuração de DNS do site para fornecer informações de validação de domínio. Use **um** dos seguintes métodos:

- **Validação de DNS** - Para sites dinâmicos, atualize sua configuração de DNS com registros CNAME que apontem para o serviço Fastly
- **Registros CNAME de desafio do ACME**-Atualize sua configuração de DNS com registros CNAME de desafio do ACME fornecidos pela Adobe para cada domínio em seu ambiente

>[!TIP]
>
>Se você tiver um domínio de Produção que não esteja ativo, use os registros CNAME do desafio ACME para validação de domínio. Adicionar os registros à configuração de DNS antecipadamente permite que a Adobe provisione o certificado SSL/TLS com os domínios corretos antes da inicialização do site. Antes de iniciar a produção, você deve substituir esses registros de espaço reservado pelos registros CNAME fornecidos pelo Adobe.

Quando a validação do domínio for concluída, a Adobe provisionará o certificado Let&#39;s Encrypt TLS/SSL e o fará upload para os ambientes de preparo ou produção ativos. Esse processo pode levar até 12 horas. Recomendamos que você conclua as atualizações de configuração de DNS com vários dias de antecedência para evitar atrasos no desenvolvimento e na inicialização do site.

## Atualizar a configuração DNS com configurações de desenvolvimento

Durante o processo inicial de configuração do Fastly, você pode usar os seguintes URLs para configurar e testar o armazenamento em cache do Fastly nos ambientes de Preparo e Produção:

- Para preparo e produção profissionais:

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- Somente para produção inicial:

   - `mcprod.<your-domain>.com`

Esses URLs de pré-produção padrão ficam disponíveis após o provisionamento do projeto. O valor de `"your-domain"` é o nome de domínio especificado durante o processo de integração.

>[!NOTE]
>
>Não é possível especificar um domínio personalizado para um ambiente de não produção nos projetos iniciais.

Para rotear o tráfego dos URLs de armazenamento para o serviço Fastly, atualize a configuração do DNS. Ao atualizar a configuração, o Adobe provisiona automaticamente os certificados SSL/TLS necessários e faz o upload deles para os seus ambientes de nuvem. Esse provisionamento pode levar até 12 horas.

>[!NOTE]
>
>Quando estiver pronto para iniciar seu site de Produção, você deverá atualizar a configuração do DNS novamente para apontar seus domínios de produção para o serviço Fastly e concluir tarefas de configuração adicionais. Consulte [Lista de verificação de inicialização](../launch/checklist.md).

**Pré-requisitos:**

- Ative o módulo Fastly.
- Carregue o código padrão Fastly VCL.
- Forneça uma lista de subdomínios e níveis superiores de cada ambiente para a Adobe ou envie um tíquete de suporte da Adobe Commerce.
- Aguarde a confirmação de que os domínios especificados foram adicionados aos ambientes da Nuvem.
- Em projetos iniciais, adicione os domínios à configuração do serviço Fastly. Consulte [Gerenciar domínios](fastly-custom-cache-configuration.md#manage-domains).
- Para obter informações sobre como atualizar a configuração de DNS, verifique com o [registrador de DNS](https://lookup.icann.org/) o método correto para o serviço de domínio.

**Para atualizar a configuração DNS para desenvolvimento**:

1. Aponte URLs de pré-produção para o serviço Fastly adicionando registros CNAME: `prod.magentocloud.map.fastly.net`, por exemplo:

   | Domínio ou Subdomínio | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   Quando os registros CNAME estão ativos, o Adobe provisiona certificados e faz upload dos certificados SSL/TLS.

   >[!NOTE]
   >
   >Se você planeja usar domínios apex (`your-domain.com`) para seu site de Produção, é necessário configurar registros de endereço DNS (registros A) para apontar para os endereços IP do servidor Fastly. Consulte [Atualizar configuração de DNS com configurações de produção](../launch/checklist.md#to-update-dns-configuration-for-site-launch).


1. Adicione registros CNAME de desafio do ACME para validação de domínio e pré-provisionamento de certificados SSL/TLS de produção, por exemplo:

   | Domínio ou Subdomínio | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >Os registros de desafio ACME neste exemplo são marcadores de posição que não têm como objetivo provisionar seus sites de preparo e produção do Adobe Commerce. Obtenha as informações corretas de registros de desafios do ACME para seu projeto entrando em contato com a Adobe.

   Depois de adicionar os registros CNAME, o Adobe valida os domínios e provisiona o certificado SSL/TLS para o ambiente. Ao atualizar a configuração do DNS para rotear o tráfego desses domínios para o serviço Fastly, o Adobe carrega o certificado para o ambiente.

1. Atualize o URL básico do Adobe Commerce.

   - Use o SSH para fazer logon no ambiente de Produção do.

     ```bash
     magento-cloud ssh
     ```

   - Use a CLI da nuvem para alterar o URL de base da sua loja.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >Como alternativa ao uso da CLI da nuvem, você pode atualizar a URL base do [Admin](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=pt-BR)

1. Reinicie o navegador da Web.

1. Teste seu site.

## Teste do armazenamento em cache Fastly

Após concluir as alterações de configuração de DNS, use a ferramenta de linha de comando [cURL](https://curl.se/) para verificar se o cache do Fastly está funcionando.

**Para verificar os cabeçalhos de resposta**:

1. Em um terminal, use o seguinte comando `curl` para testar a URL do site ativo:

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   Se você não tiver definido uma rota estática ou concluído a configuração de DNS para os domínios no site ativo, use o sinalizador `--resolve`, que ignora a resolução de nome DNS.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. Na resposta, verifique os [cabeçalhos](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) para garantir que o Fastly esteja funcionando. Você deve ver os seguintes cabeçalhos exclusivos na resposta:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Se os cabeçalhos não tiverem os valores corretos, consulte [Resolver erros encontrados nos cabeçalhos de resposta](fastly-troubleshooting.md#curl) para obter ajuda sobre solução de problemas.

## Atualização do módulo Fastly

O Fastly atualiza o módulo CDN para Magento 2 para resolver problemas, aumentar o desempenho e fornecer novos recursos.
Recomendamos que você atualize o módulo Fastly nos ambientes de Preparo e Produção para a [versão mais recente](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

Depois de atualizar o módulo, você deve fazer upload do código do VCL para aplicar as alterações na configuração do serviço Fastly.

>[!WARNING]
>
> Se você personalizou o código padrão do Fastly VCL com uma versão personalizada, a atualização do módulo Fastly substitui suas alterações. Se você tiver adicionado trechos de VCL personalizados com nomes exclusivos, essas alterações serão preservadas durante o processo de atualização. Como prática recomendada, atualize o ambiente de preparo e valide as alterações antes de aplicá-las ao ambiente de produção.

**Para verificar a versão do módulo CDN Fastly para Magento 2**:

1. Altere para o diretório raiz do seu ambiente de nuvem.

1. Use o Composer para verificar a versão instalada.

   ```bash
   composer show *fastly*
   ```

1. Se a [última versão](https://github.com/fastly/fastly-magento2/releases) não estiver instalada, conclua as etapas para atualizar o módulo Fastly.

**Para atualizar o módulo Fastly**:

1. No ambiente de integração local, use as seguintes informações do módulo para [atualizar o módulo Fastly](../store/extensions.md#upgrade-an-extension).

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. Envie suas atualizações para o ambiente de preparo.

1. Faça logon no Administrador para que seu ambiente de Preparo [carregue o código VCL](#upload-vcl-to-fastly).

1. [Verifique os serviços do Fastly](fastly-troubleshooting.md#verify-or-debug-fastly-services) no site de Preparo do Adobe Commerce.

Depois de verificar os serviços do Fastly no site de Preparo, repita o processo de atualização no ambiente de Produção.

>[!TIP]
>
> Se você tiver problemas com os serviços do Fastly em seus ambientes do Adobe Commerce, consulte a [Solução de problemas do Adobe Commerce Fastly](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html?lang=pt-BR).
