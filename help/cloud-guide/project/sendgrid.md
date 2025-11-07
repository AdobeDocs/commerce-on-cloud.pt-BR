---
title: Serviço de email SendGrid
description: Saiba mais sobre o serviço de email SendGrid para Adobe Commerce na infraestrutura em nuvem e como você pode testar sua configuração de DNS.
exl-id: 06236068-df32-468f-99ec-c379984be136
source-git-commit: 0cb86dd5e4fe627b198ac3c1a6b14607f377a9a3
workflow-type: tm+mt
source-wordcount: '1403'
ht-degree: 0%

---

# Serviço de email SendGrid

O serviço de proxy SendGrid SMTP (Simple Mail Transfer Protocol) fornece autenticação de email de saída e serviços de monitoramento de reputação, incluindo suporte para:

* Todos os emails transacionais de saída
* Endereços IP dedicados
* Registro de domínio, assinaturas do DomainKeys Identified Mail (DKIM) para validação de domínio de email (somente para ambientes de preparo e produção profissionais)
* Registro de domínio personalizado (somente para Pro)
* Integração automatizada para ambientes de integração Starter e Pro. Os ambientes de produção e de preparo profissionais exigem provisionamento e configuração manuais durante o processo de provisionamento de hardware do IaaS (Infrastructure as a Service, infraestrutura como serviço)

O proxy SMTP SendGrid não se destina ao uso como um servidor de email de uso geral para receber emails de entrada ou para uso com campanhas de marketing por email. Se você planeja importar e enviar emails de boas-vindas para um grande número de clientes (por exemplo, 10.000 ou mais), divida a importação e o envio de email em vários dias. Essa prática ajuda a permanecer dentro dos limites diários de envio e protege a reputação do remetente.

>[!TIP]
>
>Verifique se você configurou os endereços de email de armazenamento apropriados no Admin acessando Lojas > Configuração > Geral para evitar problemas com a capacidade de entrega e a verificação de domínio. Você deve desmarcar **[!UICONTROL Use Default]** e substituir os valores padrão por um domínio que você possua. Os serviços de email de domínio público/compartilhado, como gmail.com e outlook.com, não devem ser configurados como o endereço de email do remetente ao enviar emails pelo Sendgrid.

## Ativar ou desativar email

Você pode ativar ou desativar os emails de saída para cada ambiente do Cloud Console ou da linha de comando.

Por padrão, os emails de saída são ativados em ambientes de Produção e Preparo profissionais. No entanto, [!UICONTROL Outgoing emails] pode parecer desabilitado nas configurações do ambiente até que você defina a propriedade `enable_smtp` por meio da [linha de comando](outgoing-emails.md#enable-emails-in-the-cli) ou do [Console da Nuvem](outgoing-emails.md#enable-emails-in-the-cloud-console). Você pode permitir que emails de saída para ambientes de integração e de preparo enviem emails de autenticação de dois fatores ou redefinam senhas para usuários do projeto na nuvem. Consulte [Configurar emails para teste](outgoing-emails.md).

Se os emails de saída precisarem ser desativados ou reativados nos ambientes de Produção Pro ou de Preparo, você poderá enviar um [tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>A atualização do valor da propriedade [!UICONTROL enable_smtp] pela [linha de comando](outgoing-emails.md#enable-emails-in-the-cli) também altera o valor da configuração [!UICONTROL Enable outgoing emails] deste ambiente no [Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console).

## Painel SendGrid

Todos os projetos na nuvem são gerenciados em uma conta central, portanto, somente o Suporte tem acesso ao painel SendGrid. O SendGrid não fornece recursos de restrição de subconta.

Para examinar os logs de atividades quanto ao status da entrega ou uma lista de endereços de email rejeitados, rejeitados ou bloqueados, [envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). A equipe de Suporte **não pode** recuperar logs de atividades com mais de 30 dias.

Se possível, inclua as seguintes informações na solicitação:

* o endereço ou endereços de email afetados
* o período em questão (somente nos últimos 30 dias)
* o assunto do email

## Email identificado do DomainKeys (DKIM)

O DKIM é uma tecnologia de autenticação de email que permite aos Provedores de Serviços de Internet (ISPs) identificar endereços de remetente legítimos e falsos, uma técnica comumente usada em phishing e fraudes por email. O DKIM depende de um proprietário de domínio que gerencia os registros DNS. Ao usar o DKIM, o servidor do remetente usa uma chave privada para assinar as mensagens. Além disso, o proprietário do domínio adiciona um registro DKIM, que é um registro `TXT` modificado, aos registros DNS do domínio do remetente. Este registro `TXT` contém uma chave pública que os servidores de email do destinatário usam para verificar a assinatura de uma mensagem. O procedimento de criptografia de chave pública do DKIM permite que os recipients verifiquem a autenticidade de um remetente. Consulte [Explicação dos Registros do DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>O suporte às assinaturas do SendGrid DKIM e à autenticação de domínio só está disponível nos ambientes de Produção e Preparo para projetos Pro, mas não para todos os ambientes de Início. Como resultado, os emails transacionais de saída provavelmente serão sinalizados por filtros de spam. Usar o DKIM melhora a taxa de entrega como um remetente de email autenticado. Para melhorar a taxa de delivery de mensagens, você pode atualizar do Starter para o Pro ou usar seu próprio servidor SMTP ou provedor de serviços de delivery de email. Consulte [Configurar conexões de email](https://experienceleague.adobe.com/pt-br/docs/commerce-admin/systems/communications/email-communications) no _guia de Sistemas de Administração_.

### Autenticação de remetente e domínio

Para que o SendGrid envie emails transacionais em seu nome de ambientes de Produção Pro ou de Preparo, você deve definir as configurações de DNS para incluir as três entradas de DNS do subdomínio SendGrid. Cada conta SendGrid recebe um registro `TXT` exclusivo, usado para autenticar emails de saída.

>[!TIP]
>
>Certifique-se de configurar os **[!UICONTROL SEndereços de email de armazenamento]** com o domínio adequado em **[!UICONTROL Stores > Configuration > General > Store Email Addresses]**. A autenticação de domínio é executada no endereço de email do remetente. Se a configuração padrão (`example.com`) estiver definida, os emails de `example.com` serão bloqueados pelo Sendgrid.

**Para habilitar a autenticação de domínio**:

1. Envie um [tíquete de suporte](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar a habilitação do DKIM para um domínio específico (**Ambientes de preparo e produção profissionais somente**).
1. Atualize sua configuração de DNS com os registros `TXT` e `CNAME` fornecidos a você no tíquete de suporte.

**Exemplo de registro `TXT` com ID de conta**:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Exemplo `CNAME` registros**:

| Domínio | Aponta para | Tipo de registro |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1_domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2_domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### Assinaturas do DKIM e segurança automatizada

Você pode escolher entre segurança automática e manual ao configurar um domínio autenticado. Se você escolher a segurança automatizada, o SendGrid gerenciará automaticamente seus registros DKIM e SPF. Quando você adiciona um novo endereço IP de envio dedicado à sua conta, o SendGrid atualiza as configurações de DNS e a assinatura do DKIM imediatamente. Se você desativar a segurança automática, será responsável por atualizar sua assinatura DKIM sempre que alterar seu domínio de envio.

**Exemplo de segurança automatizada habilitada**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Exemplo de segurança automatizada desabilitada**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

Após a configuração da autenticação de domínio, o SendGrid manipula automaticamente os registros SPF (Estrutura de Política de Segurança) e DKIM para você. Depois que SendGrid fornecer os registros `CNAME` para serem adicionados aos seus registros DNS, você poderá adicionar endereços IP dedicados e fazer outras atualizações de conta sem precisar gerenciar os registros SPF manualmente. Consulte [Segurança automatizada e sua assinatura do DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Para testar a configuração do DNS:

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Limite de email transacional

O limite de email transacional se refere ao número de mensagens de email transacionais que você pode enviar de ambientes Pro em um período específico, como 12.000 emails por mês de ambientes não relacionados à produção. O limite foi projetado para proteger contra o envio de spam e possíveis danos à reputação do email.

Não há limites rígidos para o número de emails que podem ser enviados no ambiente de Produção, desde que a pontuação da Reputação do remetente seja superior a 95%. A reputação é afetada pelo número de emails devolvidos ou rejeitados e se os registros de spam baseados em DNS sinalizaram seu domínio como uma possível fonte de spam. Consulte [Emails não enviados quando os créditos do SendGrid forem excedidos no Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) na _Base de Dados de Conhecimento de Suporte da Commerce_.

**Para verificar se o máximo de créditos foi excedido**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Verifique `/var/log/mail.log` por `authentication failed : Maxium credits exceeded` entradas.

   Se você vir quaisquer entradas de log `authentication failed` e a **Idoneidade de envio de email** for de no mínimo 95, você pode [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar um aumento de alocação de crédito.

>[!NOTE]
>
>O arquivo `var/log/mail.log` é um *log em execução*. À medida que novas entradas são adicionadas, as entradas mais antigas são removidas do arquivo ao longo do tempo. Somente a atividade de log mais recente está disponível no log. As entradas de log mais antigas não são arquivadas ou retidas depois de removidas do mail.log.

### Reputação de envio de email

Uma reputação de envio de email é uma pontuação atribuída por um provedor de serviços de Internet (ISP) a uma empresa que envia mensagens de email. Quanto maior a pontuação, mais provável um ISP entregará mensagens na caixa de entrada de um recipient. Se a pontuação cair abaixo de um determinado nível, o ISP poderá rotear mensagens para a pasta de spam dos recipients ou até mesmo rejeitar mensagens completamente. A pontuação de reputação é determinada por vários fatores, como uma média de 30 dias de classificação de seus endereços IP em relação a outros endereços IP e taxa de reclamação de spam. Consulte [8 Maneiras de Verificar a Reputação do Envio de Emails](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).

### Listas de supressão de email

Uma lista de supressão de email é uma lista de recipients para os quais os emails não devem ser enviados se isso prejudicar sua reputação de envio e as taxas de delivery. Ela é exigida pela CAN-SPAM Act para garantir que os remetentes de email tenham um método de recusa de recipients que cancelaram a inscrição ou marcaram o email como spam. A lista de supressão também coleta emails que são rejeitados, bloqueados ou inválidos.

Para evitar que emails sejam enviados para a pasta de spam, siga o artigo de práticas recomendadas do Sendgrid, [Por que Meus Emails Estão Indo para o Spam?](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder).

Se alguns destinatários não estiverem recebendo seus emails, você poderá [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) para solicitar uma revisão das listas de supressão e remover os destinatários, se necessário.

Para obter mais detalhes, consulte [O que é uma Lista de Supressão?](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
