---
title: Lançamento do site
description: Saiba como iniciar a preparação para a inicialização do site.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---

# Lançamento do site

Após concluir a implantação e o teste nos ambientes de integração e preparo, você pode iniciar a preparação para o lançamento no site. Primeiro, você deve concluir todo o desenvolvimento e teste antes de trabalhar no ambiente de Produção. Está pronto para iniciar? Analise listas de verificação, práticas recomendadas e etapas finais para iniciar seu site.

Se você tiver verificado essas informações antes de implantar e testar no ambiente de preparo, considere revisar os benefícios do teste no ambiente de preparo primeiro na próxima seção. O ambiente de preparo é um ambiente de quase produção executado em hardware, configurações, arquitetura e serviços semelhantes. Ele pode reduzir seu tempo de inatividade e tornar sua extensão, serviço, configurações personalizadas e componentes essenciais do Teste de aceitação de usuários dos estabelecimentos comerciais para o lançamento de seus sites e lojas.

## Por que testar totalmente a integração, o preparo e a produção?

Recomendamos testar nos ambientes de integração, armazenamento temporário e produção devido à complexidade de garantir que seu código personalizado, temas, extensões e integrações de terceiros trabalhem juntos para operar suas lojas. Os problemas a seguir são comuns que podem ser detectados e resolvidos ao concluir os testes nos ambientes de integração e preparo antes de atualizar o ambiente de produção:

- O armazenamento temporário é compatível com todos os serviços de produção, recursos, dados de banco de dados, pilha de tecnologia, arquitetura e muito mais. Ele espelha a Produção, o que significa que, se ocorrerem erros na Preparação, você terá um aviso antes de ocorrerem na Produção.

- O código que funciona no ambiente de integração local pode não funcionar em ambientes de preparo e produção.

- Os ambientes de integração não são compatíveis com alguns serviços disponíveis em Preparo e Produção, como o Fastly e o New Relic.

- [Teste completamente](../test/guidance.md) seu site com várias ferramentas em Preparo para carga, stress, desempenho e ativos do site.

- Como os ambientes de integração só podem ter bancos de dados preenchidos com dados de teste, não correspondendo a um ambiente de produção, você pode encontrar erros adicionais ou comportamento inesperado ao testar em ambientes de preparo ou produção.

## Pré-requisitos para o lançamento do site

Você precisa das seguintes informações e recursos para se preparar para o lançamento do site:

- Informações de registro CNAME para configurar o DNS

- Lista de todos os domínios de loja a serem adicionados ao certificado

- Certificado SSL/TLS

Como parte da assinatura do Adobe Commerce na infraestrutura em nuvem, o Adobe fornece um certificado SSL/TLS validado pelo domínio, emitido pela Let&#39;s Encrypt. Cada ambiente Pro Production, Staging e Starter Production (`master`) tem um certificado exclusivo que cobre todos os domínios e subdomínios nesse ambiente. Esses certificados são provisionados e carregados no site automaticamente após a atualização da configuração DNS para desenvolvimento e produção. Consulte [Provisionar certificados SSL/TLS](../cdn/fastly-configuration.md#provision-ssltls-certificates).

>[!NOTE]
>
>Se você quiser implantar seu próprio certificado SSL de Validação Estendida para sua empresa, em vez de usar o certificado Vamos Criptografar, contate sua CTA ou [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket).

## Configurar a ferramenta Verificação de segurança

>[!NOTE]
>
>A ferramenta de verificação de segurança usa os seguintes endereços IP públicos:
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>Incluir na lista de permissões Adicione esses endereços IP a uma pesquisa no arquivo de regras de firewall da rede para permitir que a ferramenta verifique seu site. A ferramenta publica solicitações somente para as portas 80 e 443.

A Ferramenta de Verificação de Segurança permite que você monitore regularmente os sites da loja e receba atualizações de riscos de segurança conhecidos, malware e software desatualizado. Essa ferramenta é um serviço gratuito disponível para todas as implementações e versões do Adobe Commerce na infraestrutura em nuvem. Você acessa a ferramenta por meio de sua [conta Commerce Marketplace](https://account.magento.com/customer/account/login).

- Monitorar o status de segurança de seus sites e as atualizações de segurança aplicadas

- Receber atualizações de segurança e notificações específicas do site

Consulte o [Guia do Usuário](https://experienceleague.adobe.com/pt-br/docs/commerce-admin/systems/security/security-scan) para obter informações sobre a configuração e o uso da ferramenta de verificação de segurança. Normalmente, você começa a usar essa ferramenta ao iniciar o teste de aceitação de usuários (UAT).

Cada site examinado deve ser registrado por meio da guia Security Scan (Verificação de segurança). Durante o processo de registro, você deve aceitar a isenção de responsabilidade antes de começar a verificar. Você controla o agendamento e autoriza o usuário a receber notificações quando cada verificação é concluída. Você pode programar varreduras para uma data e hora específicas e recorrentes, ou executar uma varredura sob demanda, conforme necessário.

A Ferramenta de verificação de segurança usa várias sequências de agente do usuário para simular uma atividade real de malware. Você pode ver os seguintes agentes do usuário nos logs de acesso ou de análise:

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## Verificar seu site

1. Acesse sua [conta Commerce Marketplace](https://account.magento.com/customer/account/login).

1. Clique na guia Verificação de Segurança e selecione **Ir para Verificação de Segurança**.

1. Na coluna _Ações_ do site, selecione **Executar Verificação**. Um status de notificação exibe a verificação agendada.

### Para revisar o relatório:

1. Quando o relatório for concluído, uma notificação será exibida.

1. Na linha do site, selecione o relatório que deseja exibir na coluna **Relatórios**. O pedido é do mais recente ao mais antigo.

O relatório lista problemas que incluem verificações com falha, resultados não identificados e verificações bem-sucedidas. Cada entrada fornece informações detalhadas para a verificação, uma lista de problemas a serem investigados e as ações a serem tomadas. Algumas dessas ações podem exigir o download e a instalação de patches de segurança. Adicione os patches necessários a uma ramificação de desenvolvimento na estação de trabalho local antes de adicioná-los à ramificação de produção.

Os resultados da verificação incluem um rótulo que descreve o status aprovado ou reprovado da verificação com informações detalhadas sobre as verificações realizadas:

- &quot;Failed&quot; indica que o site contém uma vulnerabilidade séria.

- &quot;Não identificado&quot; sugere que uma análise mais profunda seja necessária para sua equipe ou provedor de hospedagem para determinar se é necessária mais ação.

Os resultados da verificação também fornecem sugestões de etapas de correção para cada teste de segurança com falha. Os resultados da verificação de segurança são protegidos e visualizados somente pelo usuário registrado. Somente os usuários designados no processo de registro do site recebem notificações de conclusão da verificação.

## Pronto para iniciar seu site

Quando estiver pronto para iniciar o processo de inicialização do site, consulte o seguinte:

- [Lista de verificação de inicialização](checklist.md)

- [Etapas de inicialização](steps.md)
