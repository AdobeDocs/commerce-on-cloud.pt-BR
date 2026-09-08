---
title: Manual de investigação
description: Saiba como investigar a sobrecarga da largura de banda do CDN, pesquisar a carga de bot e rastreador e tráfego mal-intencionado usando o Adobe Commerce Traffic Insights, além de quando escalar.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# Manual de investigação

O aplicativo [!DNL Adobe Commerce Traffic Insights] foi projetado para ajudá-lo a investigar os seguintes problemas:

- Excesso de largura de banda
- carga do rastreador
- Tráfego mal-intencionado

Como alternativa, você pode solicitar a [Segurança avançada: gerenciamento nativo de bot, DDoS da Camada 7 e limite de taxa](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting), caminho de escalonamento nativo da Adobe quando a mitigação manual for insuficiente. Cada etapa faz referência ao widget que exibe o sintoma, para que você possa mover de uma métrica para uma ação concreta.

>[!WARNING]
>
>As sugestões nesta página são apenas diretrizes. Sempre valide qualquer regra de bloqueio em relação ao seu próprio tráfego antes de implantá-lo.

## Excedência de largura de banda da CDN

Antes de considerar as sobreposições de largura de banda, entenda como a largura de banda é cobrada. O tráfego de **todos** serviços Fastly agrupados com a conta [!DNL Adobe Commerce on Cloud Infrastructure], incluindo cada ambiente de preparo **e** de produção, conta para o uso comum em comparação com a permissão anual em seu contrato. Comece em **Largura de Banda > Largura de Banda Total** e atribua o volume com **Largura de Banda por Tipo de Conteúdo** e **Detalhes de Largura de Banda por Domínio**.

### Conteúdo de mídia

Algumas lojas servem legitimamente uma grande parte da largura de banda como mídia por causa de seu catálogo. Se a **Largura de Banda por Tipo de Conteúdo** mostrar uma quantidade significativa de largura de banda de mídia, considere as seguintes atenuações:

- Faça um experimento com a [conversão rápida com perdas](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion) para fornecer imagens menores e de qualidade inferior.
- Investigue o [Fastly Deep Image Otimization](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization) para gerar imagens redimensionadas no lado da Rede de Entrega de Conteúdo (CDN).

### Arquivos grandes

Alguns sites contêm arquivos grandes ou respostas específicas e pesadas, como integrações ou exportações do Enterprise Resource Planning (ERP). Use **URLs por Largura de Banda** para examinar as colunas **Largura de Banda** e **Tamanho Médio** para localizar esses arquivos grandes. Você pode usar **Segmento de caminho lvl 1 por largura de banda** para uma exibição de nível superior.

### 404s pesados

Uma página **404 do Adobe Commerce não encontrada** geralmente é uma página pesada, com tema estilizado (~1,5 MB) e **não armazenável em cache**, portanto, 404s repetidos podem gerar tráfego anormal. Até mesmo um recurso trivial ausente, como o `favicon.ico`, pode se transformar em uma página `404` pesada, em vez de em um arquivo pequeno. Use as colunas **404** e **404 BW** em **Detalhes de Largura de Banda por Domínio**, **URLs por Largura de Banda**, **Principais IPs por Largura de Banda** e **Estatísticas por Sub-redes IP** para localizar clientes, IPs e URLs que geram consistentemente um volume 404. Em seguida, reduza ou limite esse acesso. Por exemplo, retorne um `403` mais leve.

### Baixa taxa de acertos FPC

A [!DNL Adobe] recomenda habilitar o Fastly [shielding](https://www.fastly.com/documentation/guides/concepts/shielding/) para que um agregador de cache CDN principal forneça a origem, permitindo que menos solicitações cheguem a ele a partir dos Pontos de Presença ([POPs](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)) locais mais próximos do cliente. Consulte [verificando sua configuração](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding).

O tráfego de POP para cliente e de blindagem para POP é contado separadamente e, embora a resposta do cliente seja compactada, o tráfego de blindagem para POP [ não é compactado](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) para preservar o suporte a Inclusões Laterais do Edge ([ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/)). Isso significa que uma baixa taxa de ocorrência de FPC (Full Page Cache) direciona a largura de banda muito mais alta em páginas dinâmicas. Confirme o sintoma com **Taxa de Acertos de FPC**, **Estatísticas de FPC por Domínio** e **Largura de Banda de Segmento de Rede de CDN**.

Geralmente, uma taxa de ocorrência baixa é impulsionada por um grande volume de rastreadores de mecanismo de pesquisa (consulte [bots e rastreadores de pesquisa](#search-bots-and-crawlers)). Outra mitigação é [servir um cache obsoleto aos rastreadores](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/) quando disponível. Se a causa forem invalidações de cache amplas e frequentes, use **Invalidação de cache por tags** e **FPC Idade por URLs principais** para encontrar as tags/URLs com churn.

## Pesquisar bots e rastreadores

Para medir o impacto do rastreador, comece em **Bots conhecidos por largura de banda** e **Detalhes do impacto de bots conhecidos** para ver quais bots são mais ativos e [filtre](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use) por um bot específico para estudar somente suas solicitações.

### Muitas solicitações

A causa mais comum de um bot de pesquisa enviar muitas solicitações ocorre ao analisar páginas que contêm `<meta name="robots" content="index,follow">`. Os bots podem seguir links de navegação superior e em camadas em um loop quase infinito. Considere as seguintes opções para resolver esse problema:

>[!WARNING]
>
> Consulte um especialista em Otimização do mecanismo de pesquisa (SEO) antes de restringir a atividade do rastreador. O novo treinamento pode afetar negativamente seu SEO.

- Adicione `nofollow` aos links de navegação superior e de navegação em camadas, por exemplo `<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`.
- Altere a meta tag da página para `index,nofollow` — como uma [definição de configuração de design](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt) comum ou por tipo de página com extensões personalizadas. Mantenha a precisão de `sitemap.xml` para que os bots sempre tenham uma lista atualizada de páginas a serem indexadas.
- A atualização `robots.txt` para bloquear caminhos e os bots de recursos não deve acessar.
- Observe que a diretiva `crawl-delay` não faz parte do Protocolo de exclusão oficial de robôs, mas funciona para alguns bots, como Bingbot, Slurp, SEMrushBot e alguns outros. O Googlebot ignora esta diretiva.
- Adicionar regras de limite de taxa. Há [proteção contra rastreadores abusiva](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection) nativa no módulo Fastly. Para obter um controle mais fino, um trecho [de VCL (Linguagem de Configuração de Verniz) personalizado](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets) pode retornar `429` (Muitas Solicitações) ou `405` (Método Não Permitido) para um regex usuário-agente com um limite de taxa individual. Consulte a documentação do rastreador para obter o método e o código de resposta preferidos. Consulte a [orientação sobre limitação de taxa de VCL da Fastly](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/).
- A IA e os rastreadores do modelo de linguagem grande (LLM) são um caso especial cada vez maior. Eles nem sempre se identificam de forma consistente, portanto, as regras agente-usuário do VCL podem ficar para trás. O complemento [Segurança Avançada](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security) da Adobe tem [gerenciamento de bot nativo](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting) que pode distinguir rastreadores de IA suspeitos e verificados na borda, o que não é possível com o VCL sozinho.

### Bloquear rastreadores indesejados

Se determinados mecanismos de pesquisa gerarem tráfego significativo e não forem importantes para os negócios, eles poderão ser totalmente bloqueados:

- Alguns bots seguem `robots.txt` alterações de 1 a 2 dias depois, depois de reler e atualizar suas regras de análise.
- Se um rastreador ignorar `robots.txt`, bloqueie-o com um trecho de VCL personalizado ([exemplo](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent)). Alguns rastreadores documentam isso explicitamente como o método preferido ou único de controle de frequência.

## Scripts maliciosos e scrapers

Use o aplicativo Insights de tráfego para identificar as direções comuns de ataque, filtrando por áreas de foco conforme necessário. Se as solicitações com sinalizador vermelho vierem predominantemente de determinados IPs, sub-redes ou geolocalizações (**Principais IPs por Contagem de Solicitações**, **Estatísticas por Sub-redes IP**, **Estatísticas por País**), considere bloqueá-las com um VCL Fastly personalizado.

Todo projeto de infraestrutura em nuvem já tem uma linha de base de proteção automática, independentemente de qualquer configuração que você faça. O WAF (Web Application Firewall) incluído bloqueia imediatamente a injeção de SQL e os sinais de IP maliciosos conhecidos (backdoor, ferramentas de ataque, CMDEXE, Log4J-JNDI, travessia, XSS) e limites de taxa de outros IPs não maliciosos depois que eles cruzam 50 solicitações/minuto, 350 solicitações/10 minutos ou 1.800 solicitações/hora. Esta linha de base é o que **Solicitações por resposta do WAF** e as colunas de sinal do WAF nas tabelas deste aplicativo estão indicando. Um pico nessas colunas não significa necessariamente que você não está sendo protegido.

- Fique atento a enchimento de credenciais, tomada de conta, criação de contas falsas, testes de cartão, raspagem de conteúdo e entesouramento de inventário/carrinho. Esses padrões de abuso orientados por bot são exibidos na guia **Atividade de bots e Análise de solicitações**. Tráfego de alto volume e baixa diversidade que acessa pontos de extremidade de logon, conta, check-out ou catálogo é a assinatura a ser procurada em **Principais IPs por Contagem de Solicitações** e **Detalhes conhecidos do impacto de bots**.
- Proteja os pontos de extremidade da API de check-out e check-out contra ataques de bot com o [Google reCAPTCHA](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/captcha/security-google-recaptcha).
- Use o limite de taxa nativo do módulo Fastly [proteção de caminho](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection).
- Verifique os [sinais do WAF da Próxima Geração](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/) no campo `Sigsci_Tags` separado por vírgula e combine correspondências de sinal relevantes em uma regra de bloqueio direcionada. O valor de uma solicitação suspeita pode se parecer com `BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT`. A WAF rotula um IP com `SITE-FLAGGED-IP` até um limite antes de começar a bloquear automaticamente. Os widgets **Sinais de Anomalia e Ataque do WAF**, **Sinais de Bots do WAF** e **Solicitações por Resposta do WAF**, e as colunas de WAF nas tabelas de IP, sub-rede e país são exibidos.
- Consulte o artigo da Adobe sobre [bloqueio de tráfego mal-intencionado para Adobe Commerce no nível do Fastly](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level) para ver abordagens comuns.
- Para cenários complexos em que o bloqueio manual não é uma opção viável, como campanhas de bot sustentadas, ataques espalhados por vários IPs/APIs ou DDoS (Negação de Serviço Distribuída) de Camada 7, considere primeiro o complemento [Segurança Avançada](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security) da Adobe (consulte [gerenciamento de bot nativo](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)). Ela é executada na mesma borda do Fastly que atende sua loja. Se você precisar de recursos fora do escopo, um serviço gerenciado de mitigação de bot de terceiros com integração nativa do Fastly, como [Datadome](https://docs.datadome.co/docs/module-fastly) ou [HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/) (antigo PerimeterX), é a alternativa sugerida. Todas essas opções adicionam custos adicionais.

## Segurança avançada: gerenciamento nativo de bot, DDoS de camada 7 e limitação de taxa

As seções anteriores discutem o que pode ser feito com os dados do aplicativo de Insights de tráfego e com o Fastly VCL manual. Para cenários em que isso não é suficiente, como campanhas de bot sustentadas ou em evolução, DDoS da Camada 7 (camada de aplicativo) ou dispersão de abuso em muitos IPs e pontos de extremidade de API, a Adobe oferece a [Segurança avançada](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security).

A Segurança avançada é um complemento pago para o [!DNL Adobe Commerce on Cloud Infrastructure] que adiciona gerenciamento de bot de borda (incluindo detecção de rastreador e busca de IA), proteção de DDoS de Camada 7 e limitação avançada de taxa na mesma plataforma Fastly que já está atendendo a loja. Consulte [Segurança avançada](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security) para obter todos os recursos, as limitações atuais e como solicitá-las.

Depois de adquirido e ativado, use o aplicativo Insights de tráfego para verificar se a Segurança avançada está funcionando. Suas decisões são relatadas pelos mesmos campos `Sigsci_Tags` e `Agent_response` atrás dos **Sinais de Anomalia e Ataque do WAF**, **Sinais de Bots do WAF** e **Solicitações por Resposta do WAF**. Compare esses widgets antes e depois de ativá-los para confirmar se estão agindo ativamente em seu tráfego.
