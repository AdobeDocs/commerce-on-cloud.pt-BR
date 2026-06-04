---
title: Segurança avançada Adobe Commerce
description: Saiba como a Segurança avançada adiciona gerenciamento de bot, limitação de taxa avançada e proteção de DDoS de camada 7 à Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Security
exl-id: 7aeb189f-be69-45d5-8163-4748424083c0
source-git-commit: 0b3ef117f85c990c2a01ecb655c930b8c4f61acb
workflow-type: tm+mt
source-wordcount: '2474'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

O [!DNL Adobe Commerce Advanced Security] é um produto que funciona com o [!DNL Adobe Commerce on Cloud Infrastructure] para manter sua loja online rápida, disponível e segura. Isso pode ajudar a proteger a receita, reduzir o tempo de inatividade e manter a confiança do cliente durante eventos de pico de tráfego e ataques automatizados.

[!DNL Adobe Commerce on Cloud Infrastructure] inclui proteção de DDoS [Camada 3 e 4](./fastly.md#ddos-protection) interna e um [Firewall de Aplicativo Web (WAF)](./fastly-waf-service.md). No [modelo de responsabilidade compartilhada](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/security-and-compliance/shared-responsibility), a detecção de DDoS da Camada 7, a proteção de bot e o bloqueio pró-ativo de IP são responsabilidades de comerciante, que o [!DNL Adobe Commerce Advanced Security] foi projetado para resolver.

O [!DNL Advanced Security] estende a proteção da vitrine por meio dos recursos de segurança de borda do Fastly, que oferece gerenciamento de bot, limitação de taxa avançada e proteção de DDoS de Camada 7 como parte de uma plataforma de borda unificada que combina escala, desempenho e segurança na borda da rede.

>[!NOTE]
>
>[!DNL Advanced Security] está disponível somente para projetos [!DNL Adobe Commerce on Cloud Infrastructure] (PaaS).

## Principais recursos

[!DNL Adobe Commerce Advanced Security] inclui as seguintes proteções adicionais:

- **[Gerenciamento de bot](https://docs.fastly.com/products/bot-management)**—Identifica e reduz a atividade de bot indesejada nos seus aplicativos web. O serviço de Gerenciamento de bot distingue entre bots legítimos (rastreadores de mecanismo de pesquisa, bots de redes sociais) e maliciosos, fornecendo classificação em tempo real na borda da rede com opções para bloquear, permitir, desafiar ou limitar a taxa de tráfego.

- **[Proteção DDoS](https://docs.fastly.com/products/fastly-ddos-protection)** — Fornece proteção DDoS de Camada 7 (camada de aplicativo) além da proteção existente de Camada 3 e 4 incluída em todos os projetos [!DNL Adobe Commerce on Cloud Infrastructure]. O serviço de proteção de DDoS absorve ataques volumétricos em grande escala e garante a disponibilidade contínua do aplicativo durante eventos de DDoS (Negação de serviço distribuído), protegendo a receita durante períodos de pico de tráfego.

- **[Limitação Avançada de Taxa](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)** — Fornece regras configuráveis de limitação de taxa que protegem contra abuso URLs, pontos de extremidade de API e recursos de aplicativo específicos. O serviço Limitação avançada de taxa vai além da [limitação básica de taxa](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) disponível por meio do módulo Fastly CDN para direcionar padrões de tráfego específicos e vetores de ataque, reduzindo a tensão na infraestrutura e os custos de nuvem.

>[!NOTE]
>
>Atualmente, as configurações do [!DNL Advanced Security] exigem o envio de um tíquete de suporte. A configuração de autoatendimento por meio da interface do administrador está planejada para uma versão futura. Consulte [Solicitação [!DNL Advanced Security]](#request-advanced-security) para obter mais informações.

>[!IMPORTANT]
>
>**Limitações atuais**
>
>Até o final do terceiro trimestre de 2026, os clientes não poderão modificar ou gerenciar diretamente as regras de gerenciamento de bot.
>
>Para qualquer adição, modificação ou ajuste de regra, entre em contato com o Suporte da Adobe Commerce por meio de um [tíquete de suporte](https://experienceleague.adobe.com/home?lang=pt-BR&support-tab=home#support). A equipe de suporte implementará as alterações solicitadas.
>
>A partir do quarto trimestre de 2026, o Fastly está programado para lançar um recurso complementar que permitirá aos clientes gerenciar as regras de Gerenciamento de bot no painel de Administração do Commerce.

## Regras e proteções padrão

As regras e proteções padrão a seguir estão disponíveis com o [!DNL Advanced Security].

### DDoS de Camada 7

- Os limites de DDoS são incorporados à plataforma Fastly CDN e não podem ser personalizados no momento por cliente.
- Os registros para tráfego bloqueado pelas proteções de DDoS não são diretamente visíveis para os clientes.
- Mediante solicitação, o Suporte da Adobe Commerce pode fornecer detalhes relacionados ao tráfego de DDoS bloqueado.
- Os recursos nativos de encaminhamento de logs de DDoS são esperados em uma versão futura.

### Gerenciamento de bot

As seguintes proteções de gerenciamento de bot de linha de base estão disponíveis no painel Signal Sciences do Fastly.

| Tipo de regra | Status | Visibilidade |
|---|---|---|
| Bloquear tráfego marcado como BOT com suspeita incorreta | Ativado por padrão durante a integração | Visível nos logs do New Relic em `sigsci_tags` |
| Bloquear tráfego com base em qualquer tag específica (tag sigsci) | Configurado apenas quando necessário em colaboração com o cliente | Visível nos logs do New Relic em `sigsci_tags` |
| Limitação de taxa para APIs ou padrões de URL específicos | Configurado apenas quando necessário em colaboração com o cliente | O tráfego bloqueado está visível nos logs do New Relic em `Agent_response` |
| Desafio dinâmico para APIs ou padrões de URL específicos | Configurado apenas quando necessário em colaboração com o cliente | O tráfego bloqueado está visível nos logs do New Relic em `Agent_response` |
| Desafio do navegador | Configurado apenas quando necessário em colaboração com o cliente | O tráfego bloqueado está visível nos logs do New Relic em `Agent_response` |

## Observabilidade — monitorização da proteção de bots e da atividade de NGWAF

Os logs CDN são encaminhados automaticamente para a conta do New Relic do cliente. Para obter detalhes adicionais, consulte [Gerenciamento de logs](../monitor/log-management.md).

Os registros CDN incluem telemetria incorporada da Signal Sciences (Proteção de bot/WAF de última geração), permitindo que os clientes monitorem eventos de segurança diretamente no New Relic.

Os campos principais incluem:

- **`Sigsci_Tags`** — Indica classificações e tags aplicadas pela Signal Sciences.
- **`Agent_response`** — Indica a ação executada pelo agente de Proteção de bot/NGWAF.

Exemplos:

- Para identificar o tráfego bloqueado pela Proteção de bot ou regras NGWAF:

  `Agent_response:"406"`

  Um código de resposta 406 indica que a solicitação foi bloqueada pelos controles de segurança.

- Para identificar solicitações marcadas como bots inválidos suspeitos:

  `Sigsci_Tags:"*SUSPECTED-BAD-BOT*"`

Esses campos podem ser usados para criar painéis, alertas e investigações no New Relic para monitorar a atividade de bot, solicitações bloqueadas e outros eventos relacionados à segurança.

## Os recursos de VCL existentes permanecem inalterados

Habilitar o complemento [!DNL Advanced Security] não modifica nem substitui controles de segurança existentes baseados em Fastly VCL.

Os seguintes recursos existentes de bloqueio de VCL continuam a funcionar sem nenhuma alteração:

- Bloqueio baseado em IP
- Bloqueio geográfico
- Bloqueio baseado no agente do usuário
- Bloqueio baseado em assinatura JA3
- Bloqueio baseado em assinatura JA4

Os clientes podem continuar usando as configurações personalizadas de VCL e as regras de segurança existentes junto com os recursos complementares do [!DNL Advanced Security].

O complemento [!DNL Advanced Security] opera além do Fastly CDN padrão e das proteções de VCL existentes já disponíveis em [!DNL Adobe Commerce on Cloud Infrastructure].

## Cobertura da ameaça

O [!DNL Advanced Security] protege as vitrines de uma variedade de ameaças automatizadas e da camada de aplicativos.

![Posicionamento de segurança avançada na pilha de segurança do Adobe Commerce](../../assets/advanced-security.svg)

### Abuso impulsionado por bot

- **Credencial recheada**—Tentativas automatizadas de fazer login usando credenciais roubadas de violações de dados.
- **Tomada de conta** — Bots que tentam obter acesso não autorizado às contas do cliente.
- **Abuso na criação de conta**—Criação automática de contas falsas para fins de fraude ou abuso.
- **Teste de cartão**—Bots que testam números de cartões de crédito roubados em relação ao seu processador de pagamento.
- **Recarregamento de conteúdo**—Extração automática de dados, preços ou conteúdo do produto da sua loja.
- **Armazenagem de estoque**—Bots que mantêm produtos em carrinhos para evitar compras legítimas.

### Gerenciamento de bot de IA

- **Detecção de rastreador de IA**—Identifica e gerencia rastreadores de IA que coletam conteúdo para treinar modelos de linguagem grandes sem consentimento.
- **Controle do buscador de IA** — Controla os buscadores de IA usados em resultados de Pesquisas com IA em tempo real.
- **Políticas de bot de IA configuráveis**—Distingue entre bots de IA verificados e suspeitos com tipos de sinal configuráveis para aplicação de política.

### Ataques de camada de aplicativo

- **Ataques de DDoS de Camada 7** — Ataques distribuídos direcionados à camada de aplicativo que ignoram as proteções integradas das Camadas 3 e 4. O [!DNL Advanced Security] absorve esses ataques volumétricos na borda antes que eles atinjam seus servidores de origem.
- **Abuso de URL e API** — Ataques direcionados a URLs ou endpoints de API específicos espalhados por um grande número de endereços IP, em que o bloqueio de IP individual não é eficaz.
- **Ataques de eliminação de cache** — Solicitações com parâmetros de consulta manipulados projetados para ignorar o armazenamento em cache do CDN e sobrecarregar o servidor de origem.

### Recursos adicionais

- **Desafios Dinâmicos**—Atribui automaticamente o desafio ideal ao tráfego suspeito. Utiliza PAT (Private Access Tokens, tokens de acesso privado) para validar facilmente uma parte das solicitações sem afetar a experiência do usuário.
- **Tecnologia de engano**—Aborda tentativas de aquisição de conta, retornando informações falsas para os invasores, mitigando seus ataques e, ao mesmo tempo, interrompendo sua capacidade de operar em escala.

## Escolhendo a proteção certa

Use as orientações a seguir para determinar se o [!DNL Advanced Security] é a solução certa para suas necessidades de proteção de vitrine ou se as proteções existentes ou soluções alternativas são mais apropriadas.

### Quando usar [!DNL Advanced Security]

Os cenários a seguir são mais bem tratados com [!DNL Advanced Security]:

| Cenário | Como o [!DNL Advanced Security] ajuda |
|---|---|
| Seu site enfrenta ataques de bot, como enchimento de credenciais, raspagem de conteúdo ou entesouramento de inventário | O gerenciamento de bot identifica e reduz ameaças automatizadas na borda antes que elas atinjam seu aplicativo |
| Você precisa de proteção DDoS de camada 7 além da cobertura integrada das camadas 3 e 4 | A Proteção de DDoS absorve ataques na camada de aplicativos que ignoram as proteções no nível da rede |
| URLs ou endpoints de API específicos são direcionados por tráfego distribuído de alto volume que não pode ser bloqueado pelo IP | A Limitação avançada de taxa fornece controles granulares para endpoints e padrões de tráfego específicos |
| Você deseja gerenciar rastreadores de IA e buscadores que acessam o conteúdo da loja | O Gerenciamento de bot inclui detecção de bot de IA configurável e políticas de aplicação |
| Você precisa de uma solução de segurança de borda compatível com a Adobe integrada à sua CDN Fastly existente | O [!DNL Advanced Security] é executado na mesma plataforma Fastly edge que já atende sua loja |

### Quando usar proteções existentes

Os seguintes cenários são mais bem tratados com as proteções existentes:

| Cenário | Método recomendado |
|---|---|
| Um único IP ou um pequeno conjunto de IPs identificáveis está sobrecarregando seu site com solicitações | Bloqueie os IPs usando o Administrador do Commerce ou a API do Fastly. Use a [proteção de DDoS de Camada 3/4](./fastly.md#ddos-protection) interna e os [trechos de VCL de inclui na lista de bloqueios de IP](./fastly-vcl-blocking.md) existentes. |
| Você precisa bloquear a injeção de SQL, o script entre sites (XSS) ou outras dez principais ameaças da OWASP | O [serviço WAF](./fastly-waf-service.md) incluído bloqueia essas ameaças automaticamente. |
| Seus padrões de ataque de DDoS podem ser controlados com regras básicas de bloqueio de VCL | Use os [trechos de VCL personalizados](./fastly-vcl-custom-snippets.md) existentes já disponíveis com o Adobe Commerce. |

### Quando usar proteções alternativas

Os cenários a seguir são mais bem tratados com proteções alternativas que podem complementar o [!DNL Advanced Security]:

| Cenário | Método recomendado |
|---|---|
| Você precisa de pontuação de fraude em nível de transação ou prevenção de fraude de pagamento | Utilizar uma plataforma específica de prevenção da fraude. O [!DNL Advanced Security] protege no nível da rede de borda e não avalia transações de pagamento individuais. |
| Você precisa do gerenciamento de identidade e acesso (IAM) | Implementar uma solução IAM dedicada. A autenticação de usuários e o gerenciamento de sessões continuam a ser responsabilidades do cliente. |
| Você precisa de testes de segurança de aplicativos estáticos ou dinâmicos (SAST/DAST) | Use ferramentas dedicadas de teste de segurança de aplicativos. A verificação de vulnerabilidade no nível do código não é fornecida. |
| Você precisa de segurança abrangente de API além da limitação de taxa (como validação de esquema ou recursos de gateway de API) | Considere uma plataforma dedicada de segurança de API. |
| Você precisa de ferramentas de conformidade normativa, como varredura de PCI ou geração de relatórios SOC | Use ferramentas dedicadas de gerenciamento de conformidade. |

>[!TIP]
>
>Se você usa atualmente um provedor de proteção de bot de terceiros, a consolidação no [!DNL Advanced Security] pode reduzir a complexidade operacional e eliminar a cobertura de segurança inconsistente entre os provedores. Entre em contato com a equipe de conta da Adobe para avaliar o [!DNL Advanced Security] do seu projeto.

## Posicionamento da pilha de segurança

O [!DNL Advanced Security] se encaixa na arquitetura mais ampla de segurança da Adobe Commerce como uma camada adicional de proteção baseada em borda. Funciona com — e não substitui — as proteções de WAF e DDoS de Camada 3/4 já incluídas com [!DNL Adobe Commerce on Cloud Infrastructure]. As seções a seguir esclarecem como ela se relaciona às proteções existentes e às responsabilidades que permanecem com o cliente.

### Proteções incluídas

[!DNL Adobe Commerce on Cloud Infrastructure] inclui os seguintes recursos de segurança:

- **[WAF (Firewall de Aplicativo Web)](./fastly-waf-service.md)**—Proteção gerenciada contra injeção de SQL, Criação de Scripts entre Sites (XSS) e outras Dez Principais ameaças do OWASP (Open Web Application Security Project). Disponível somente em ambientes de produção.
- **[Proteção de DDoS de Camada 3 e Camada 4](./fastly.md#ddos-protection)**—Proteção integrada contra ataques de camada de rede, como inundações SYN, inundações UDP, ataques baseados em ICMP e ataques de nível TCP. Ativado automaticamente com o Fastly CDN.
- **[Certificados SSL/TLS](./fastly-configuration.md#provision-ssltls-certificates)** — Certificados de criptografia validados por domínio para tráfego HTTPS seguro.
- **[Encobrimento de origem](./fastly.md#origin-cloaking)** — Garante todas as rotas de tráfego pelo Fastly, bloqueando o acesso direto aos servidores de origem.
- **[Trechos de segurança baseados em VCL](./fastly-vcl-custom-snippets.md)** — Regras de VCL (Linguagem de Configuração de Verniz Personalizado) para bloqueio de IP, filtragem e solicitação.

### [!DNL Advanced Security]

O [!DNL Advanced Security] fornece mais proteção além das proteções internas incluídas no [!DNL Adobe Commerce on Cloud Infrastructure], mas a um custo adicional:

- **Gerenciamento de bot**—Detecção e mitigação de bot com base na Edge com gerenciamento de bot de IA.
- **Proteção de DDoS de Camada 7** — Absorção e defesa de DDoS de camada de aplicativo.
- **Limite de Taxa Avançada** — Controles de taxa granulares para URLs e pontos de extremidade de API.
- **Desafios Dinâmicos e Tecnologia de Engano**—Atribuição automatizada de desafios e mitigação de tomada de conta.

### Responsabilidade do cliente

- **Prevenção de fraudes**—Pontuação de fraudes no nível da transação e detecção de fraudes em pagamentos.
- **Gerenciamento de identidade e acesso**—Gerenciamento de autenticação, autorização e sessão do cliente.
- **Teste de segurança do aplicativo**—SAST/DAST e verificação de vulnerabilidade.
- **Configurações de segurança personalizadas** — regras baseadas em VCL, incluis na lista de permissões de IP e.
- **Ferramentas de conformidade** — verificação PCI, relatório de conformidade SOC e ferramentas de auditoria normativa.
- **Fortalecimento em nível de aplicativo**—Autenticação de API baseada em token, normalização de parâmetros de consulta e design de estratégia de cache.

Para obter uma visão geral completa das responsabilidades da Adobe e de segurança do cliente, consulte o [modelo de responsabilidade compartilhada](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/security-and-compliance/shared-responsibility).

## Padrões e proteções de ataque comuns

A tabela a seguir mapeia padrões de ataques comuns para a camada de proteção apropriada na pilha de segurança do Adobe Commerce.

| Padrão de ataque | Tipo | Proteção |
|---|---|---|
| IP único ou conjunto de IPs identificáveis que enviam um grande número de solicitações | DDoS + Bot | Bloquear IPs usando o Administrador do Commerce ou a API do Fastly. A proteção DDoS de camada 3/4 integrada filtra esse tráfego na borda da rede. |
| Ataques a URLs ou APIs específicos espalhados por um grande número de IPs | DDoS + Bot | **[!DNL Advanced Security]**: a Limitação Avançada de Taxa restringe o volume de solicitações por URL. O Gerenciamento de bot identifica e bloqueia o tráfego de bot distribuído. |
| Ataques automatizados em endpoints da REST API sem autenticação adequada | Bot + DDoS | Verifique se os endpoints da API usam autenticação baseada em token. Gire as credenciais se o token estiver comprometido. **[!DNL Advanced Security]**: a Limitação de Taxa Avançada pode proteger os pontos de extremidade expostos. |
| Ataques de eliminação de cache usando parâmetros de consulta manipulados | Bot + DDoS | Excluir parâmetros de consulta não essenciais das chaves de cache. Normalize e restrinja parâmetros de consulta no nível do aplicativo. **[!DNL Advanced Security]**: o Gerenciamento de bot detecta e bloqueia o tráfego de eliminação automática de cache. |
| Tentativas de injeção de SQL ou script entre sites (XSS) | WAF | O [serviço WAF](./fastly-waf-service.md) incluído bloqueia essas ameaças automaticamente usando regras de segurança gerenciadas. |

### Comportamento de bloqueio do WAF

O seguinte comportamento do WAF se aplica a todos os projetos [!DNL Adobe Commerce on Cloud Infrastructure], independentemente de [!DNL Advanced Security] estar ou não habilitado. O serviço WAF incluído usa o seguinte comportamento de bloqueio para sinais de ataque comuns:

- As solicitações de **injeção de SQL** são bloqueadas imediatamente, mesmo para uma única solicitação correspondente.
- As solicitações identificadas com os seguintes sinais de ameaça de um IP mal-intencionado conhecido são bloqueadas imediatamente: Backdoor, Attack Tooling, CMDEXE, Log4J JNDI, Traversal e XSS.
- As solicitações de IPs não mal-intencionados que exibem os sinais de ameaça acima são bloqueadas quando excedem os seguintes limites:

| Interval | Limite | Verificar frequência |
|---|---|---|
| 1 minuto | 50 solicitações | A cada 20 segundos |
| 10 minutos | 350 solicitações | A cada 3 minutos |
| 1 hora | 1.800 solicitações | A cada 20 minutos |

## Solicitação [!DNL Advanced Security]

Para solicitar [!DNL Advanced Security]:

>[!NOTE]
>
>O [!DNL Advanced Security] está disponível por um custo adicional e requer uma assinatura ativa do [!DNL Adobe Commerce on Cloud Infrastructure] (PaaS).

1. Entre em contato com sua equipe de conta da Adobe ou representante de vendas da Adobe para discutir o [!DNL Advanced Security] para o seu projeto.

1. Após comprar o [!DNL Advanced Security], [envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket) solicitando a habilitação do [!DNL Advanced Security]. Inclua a ID do projeto [!DNL Adobe Commerce on Cloud Infrastructure] e os ambientes que exigem ativação (por exemplo, Produção e Preparo).

1. O Adobe ativa o [!DNL Advanced Security] no serviço Fastly e configura as políticas de proteção iniciais. A ativação geralmente é concluída em alguns dias úteis após o envio do tíquete.

1. Você recebe a confirmação de que [!DNL Advanced Security] está ativo, juntamente com detalhes sobre as proteções habilitadas para seus ambientes.

>[!NOTE]
>
>Atualmente, as alterações de configuração de [!DNL Advanced Security] exigem [envio de um tíquete de suporte](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=pt-BR#submit-ticket). A configuração de autoatendimento por meio da interface do administrador está planejada para uma versão futura.

## Limitação

[!DNL Advanced Security] fornece proteção de vitrine de camada de borda. Os seguintes recursos não estão disponíveis e são melhor tratados com soluções complementares:

- **A pontuação de fraude em nível de transação**—[!DNL Advanced Security] não avalia transações de pagamento individuais quanto ao risco de fraude. Use uma plataforma dedicada de prevenção de fraudes para pontuação no nível da transação.
- **Gerenciamento de identidade e acesso (IAM)**—[!DNL Advanced Security] não gerencia autenticação de usuário, autorização ou gerenciamento de sessão. Essas responsabilidades permanecem como responsabilidades do cliente.
- **O teste de segurança de aplicativo estático e dinâmico (SAST/DAST)**—[!DNL Advanced Security] não inclui verificação de vulnerabilidade em nível de código nem teste de penetração.
- **Segurança de API** — Embora a Limitação Avançada de Taxas possa proteger os pontos de extremidade de API contra abuso, não são fornecidos recursos abrangentes de segurança de API, como validação de esquema e gerenciamento de gateway de API.
- **A prevenção completa contra fraudes**—[!DNL Advanced Security] concentra-se na proteção de vitrine de camada de borda e não é uma plataforma completa de gerenciamento de fraudes.
- **As ferramentas de conformidade**—[!DNL Advanced Security] não fornecem recursos de verificação PCI, relatório de conformidade SOC ou auditoria normativa.
