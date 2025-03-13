---
title: Firewall de aplicativo da Web (WAF)
description: Saiba como o serviço Fastly WAF detecta, registra e bloqueia o tráfego de solicitação mal-intencionado antes que ele possa danificar a rede ou os sites do Adobe Commerce.
feature: Cloud, Configuration, Security
exl-id: f00e35f2-9800-4e24-a4d0-d36fde59a003
source-git-commit: 7e61673b343fb954b53bf7cbae88efaf7bbfab4c
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Firewall de aplicativo da Web (WAF)

Desenvolvido pela Fastly, o serviço de firewall do aplicativo web (WAF) para o Adobe Commerce na infraestrutura em nuvem detecta, registra e bloqueia o tráfego de solicitação mal-intencionado antes que ele possa danificar seus sites ou a rede. O serviço WAF está disponível somente em ambientes de produção.

O serviço WAF oferece os seguintes benefícios:

- **Conformidade com o PCI** — a ativação da WAF garante que as vitrines da Adobe Commerce em ambientes de produção atendam aos requisitos de segurança do PCI DSS 6.6.
- **Política padrão do WAF** — A política padrão do WAF, configurada e mantida pelo Fastly, fornece uma coleção de regras de segurança personalizadas para proteger seus aplicativos Web do Adobe Commerce contra uma grande variedade de ataques, incluindo ataques de injeção, entradas mal-intencionadas, script entre sites, exfiltração de dados, violações de protocolo HTTP e outras [Dez principais ameaças de segurança do OWASP](https://owasp.org/www-project-top-ten/).
- **Integração e habilitação do WAF** — a Adobe implanta e habilita a política padrão do WAF no ambiente de Produção dentro de 2 a 3 semanas após o provisionamento ser concluído.
- **Suporte a operações e manutenção**—
   - O Adobe e o Fastly configuram e gerenciam seus logs, regras e alertas para o serviço do WAF.
   - O Adobe organiza tíquetes de suporte ao cliente relacionados a problemas de serviço do WAF que bloqueiam o tráfego legítimo como problemas de Prioridade 1.
   - As atualizações automatizadas para a versão de serviço do WAF garantem cobertura imediata para explorações novas ou em evolução. Consulte [manutenção e atualizações do WAF](#waf-maintenance-and-updates).

>[!TIP]
>
>Para obter informações adicionais sobre como manter a conformidade com o PCI para sua Adobe Commerce em lojas de infraestrutura em nuvem, consulte [conformidade com o PCI](https://business.adobe.com/products/magento/pci-compliance.html).

## Habilitação do WAF

A Adobe habilita o serviço da WAF em novas contas dentro de 2 a 3 semanas após o provisionamento ser concluído. O WAF é implementado por meio do serviço Fastly CDN. Não é necessário instalar ou manter nenhum hardware ou software.

>[!NOTE]
>
>Antes de usar o serviço WAF, todo o tráfego externo para o seu projeto de infraestrutura em nuvem do Adobe Commerce deve ser roteado pelo serviço Fastly. Consulte [Configurar Fastly](fastly-configuration.md).

## Como funciona

O serviço WAF integra-se ao Fastly e usa a lógica do cache no serviço CDN do Fastly para filtrar o tráfego nos nós globais do Fastly. Habilitamos o serviço WAF em seu ambiente de Produção com uma política padrão do WAF baseada nas [Regras de segurança ModLabs da Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) e nas Dez principais ameaças de segurança da OWASP.

O serviço do WAF inspeciona o tráfego HTTP e HTTPS (solicitações GET e POST) em relação ao conjunto de regras do WAF e bloqueia o tráfego mal-intencionado ou não compatível com regras específicas. O serviço inspeciona somente o tráfego de origem vinculada que tenta atualizar o cache. Como resultado, interrompemos a maioria do tráfego de ataques no cache do Fastly, protegendo seu tráfego de origem de ataques mal-intencionados. Ao processar apenas o tráfego de origem, o serviço WAF preserva o desempenho do cache, introduzindo apenas uma latência estimada de 1,5 milissegundos a 20 milissegundos para cada solicitação não armazenada em cache.

## Solução de problemas de solicitações bloqueadas

Quando o serviço WAF está ativado, ele inspeciona todo o tráfego da Web e administrativo em relação às regras do WAF e bloqueia qualquer solicitação da Web que acione uma regra. Quando uma solicitação é bloqueada, o solicitante vê uma página de erro padrão `403 Forbidden` que inclui uma ID de referência para o evento de bloqueio.

![Página de erro do WAF](../../assets/cdn/fastly-waf-403-error.png)

Você pode personalizar esta página de resposta de erro no Admin. Consulte [Personalizar a página de resposta do WAF](fastly-custom-response.md#customize-the-waf-error-page).

Se a sua página de administrador ou loja do Adobe Commerce retornar uma página de erro `403 Forbidden` em resposta a uma solicitação de URL legítima, envie um [tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case). Copie a ID de referência da página de resposta de erro e cole-a na descrição do ticket.

Para identificar a resposta do WAF para uma solicitação específica usando o New Relic, consulte o seguinte:

- `Agent_response` — Indica o código de resposta do WAF (`200` significa bom e `406` significa bloqueado)
- `sigsci` tags — Marca a solicitação para uma tag de ciências de sinais específica com base na natureza da solicitação

## Manutenção e atualizações do WAF

O Fastly atualiza e implanta patches para novos CVEs/regras de modelo com base em atualizações de regras de terceiros comerciais, pesquisa do Fastly e fontes abertas. O Fastly atualiza as regras publicadas em uma política, conforme necessário, ou quando alterações nas regras estão disponíveis em suas respectivas fontes. Além disso, o Fastly pode adicionar regras que correspondem às classes de regras publicadas na instância do WAF de qualquer serviço depois que o serviço do WAF é ativado. Essas atualizações garantem cobertura imediata para explorações novas ou em evolução.

A Adobe e o Fastly gerenciam o processo de atualização para garantir que as regras do WAF novas ou modificadas funcionem efetivamente no ambiente de Produção antes que as atualizações sejam implantadas no modo de bloqueio.

## Problemas

Se você achar que o WAF está bloqueando solicitações legítimas, elas geralmente são falsos positivos e precisarão ser ignoradas ou ter uma solução alternativa implementada no serviço do WAF. Envie um tíquete de suporte e inclua o URL afetado, as etapas exatas para reproduzir o erro e a referência do erro no formato de texto (em vez de uma captura de tela) para evitar erros de transcrição.

## Limitação

O serviço WAF padrão desenvolvido pela Fastly não é compatível com os seguintes recursos:

- Proteção contra malware ou mitigação de bot — Considere usar [listas de controle de acesso](./fastly-vcl-allowlist.md) ou um serviço de terceiros.
- Limitação de taxa — Consulte [Limitação de taxa](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) na documentação do Fastly, ou consulte [Limitação de taxa](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) na seção de segurança _API da Web do Commerce_.
- Configurando um ponto de extremidade de log para o cliente — Consulte o [serviço PrivateLink](../development/privatelink-service.md) como uma alternativa.

O serviço WAF permite bloquear ou permitir o tráfego com base em endereços IP. Você pode adicionar listas de controle de acesso (ACL) e trechos de VCL personalizados ao serviço Fastly para especificar os endereços IP e a lógica de VCL para bloquear ou permitir o tráfego. Consulte [Fragmentos de VCL personalizados](fastly-vcl-custom-snippets.md).

A filtragem de solicitações TCP, UDP ou ICMP não é suportada pelo serviço WAF. No entanto, essa funcionalidade é fornecida pela proteção DDoS integrada incluída no serviço Fastly CDN. Consulte [Proteção de DDoS](fastly.md#ddos-protection).
