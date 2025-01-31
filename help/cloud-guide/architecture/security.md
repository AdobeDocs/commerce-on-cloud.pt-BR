---
title: Segurança da infraestrutura em nuvem
description: Saiba mais sobre como o Adobe mantém segura a infraestrutura em nuvem do Adobe Commerce.
feature: Cloud, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# Segurança

A [arquitetura de plano Pro](pro-architecture.md) da Adobe Commerce foi projetada para fornecer um ambiente altamente seguro. Cada cliente é implantado em seu próprio ambiente de servidor isolado, separado dos outros clientes. Os detalhes de segurança do ambiente de produção estão descritos abaixo.

## Navegadores da Web

A maior parte do tráfego que entra e sai do ambiente de nuvem é proveniente de navegadores da Web dos consumidores. O tráfego do consumidor pode ser protegido usando HTTPS para todas as páginas do site (usando uma certificação SSL compartilhada ou o certificado SSL do próprio cliente por uma taxa adicional). As páginas de check-out e de conta são sempre fornecidas usando HTTPS. A prática recomendada é distribuir todas as páginas com HTTPS.

## Rede de entrega de conteúdo

O Fastly fornece uma proteção de CDN (Content Delivery Network) e DDoS (Distributed Denial of Service). O Fastly CDN ajuda a isolar o acesso direto aos servidores de origem. O DNS público só aponta para a Fastly Network. A solução Fastly DDoS protege contra ataques altamente disruptivos de Camada 3 e Camada 4 e ataques mais complexos de Camada 7. Os ataques de Camada 7 podem ser bloqueados usando regras personalizadas com base em solicitações HTTP/HTTPS inteiras e em critérios de cliente e solicitação, incluindo cabeçalhos, cookies, caminho de solicitação e IP do cliente ou indicadores como localização geográfica.

Consulte [Visão geral dos serviços do Fastly](../cdn/fastly.md).

## Firewall de Aplicativo Web

O Fastly Web Application Firewall (WAF) é usado para fornecer proteção adicional. O WAF do Fastly usa regras de terceiros de fontes comerciais e de código aberto, como o Conjunto de Regras Principal do OWASP. Além disso, são empregadas regras específicas do Adobe Commerce. Os clientes estão protegidos contra os principais ataques de camada de aplicativo, incluindo ataques de injeção e entradas mal-intencionadas, script entre sites, exfiltração de dados, violações de protocolo HTTP e outras dez principais ameaças da OWASP.

As regras do WAF são atualizadas pela Adobe Commerce caso novas vulnerabilidades sejam detectadas, permitindo que o Managed Services corrija virtualmente problemas de segurança antes dos patches de software. O Fastly WAF não fornece serviços de limitação de taxa ou detecção de bot. Se desejar, os clientes podem licenciar um serviço de detecção de bot de terceiros compatível com o Fastly.

Consulte [Firewall do Aplicativo Web (WAF)](../cdn/fastly-waf-service.md).

## Nuvem privada virtual

O ambiente de produção do plano do Adobe Commerce Pro é configurado como uma Nuvem privada virtual (VPC) para que os servidores de produção sejam isolados e tenham capacidade limitada de se conectar e sair do ambiente de nuvem. Somente conexões seguras com os servidores na nuvem são permitidas. Protocolos seguros, como SFTP ou rsync, podem ser usados para transferências de arquivos.

Os clientes podem usar túneis SSH para proteger as comunicações com o aplicativo. O acesso ao AWS PrivateLink pode ser fornecido por uma taxa adicional. Todas as conexões com esses servidores são controladas usando Grupos de segurança do AWS, um firewall virtual que limita as conexões com o ambiente. Os recursos técnicos dos clientes podem acessar esses servidores usando SSH.

## Criptografia

O EBS (Elastic Block Store) da Amazon é usado para armazenamento. Todos os volumes EBS são criptografados usando o algoritmo AES-256, o que significa que os dados são criptografados em repouso. O sistema também criptografa dados em trânsito entre a CDN e a origem e entre os servidores de origem. As senhas do cliente são armazenadas como hashes. As credenciais confidenciais, incluindo as credenciais do gateway de pagamento, são criptografadas usando o algoritmo SHA-256.

O aplicativo Adobe Commerce não oferece suporte à criptografia no nível da coluna ou da linha quando os dados não estão em repouso ou não estão em trânsito entre servidores. O cliente pode gerenciar chaves de criptografia no aplicativo. As chaves usadas pelo sistema são armazenadas no Sistema de Gerenciamento de Chaves da AWS e devem ser gerenciadas pela Managed Services para fornecer partes do serviço.

## Detecção e resposta do endpoint

[!DNL CrowdStrike Falcon], um agente leve de EDR (Detecção e Resposta de Ponto de Extremidade) de próxima geração está instalado em todos os pontos de extremidade (incluindo servidores) no Adobe. Os agentes EDR protegem dados e sistemas de Adobe com monitoramento e coleta contínuos em tempo real, o que permite a identificação e resposta rápidas às ameaças.

## Teste de penetração

A Managed Services realiza testes de inserção regulares do sistema de nuvem Adobe Commerce com o aplicativo pronto para uso. Os clientes são responsáveis por qualquer teste de inserção de seu aplicativo personalizado.

## Gateway de pagamento

O Adobe Commerce exige integrações de gateway de pagamento, onde os dados de cartão de crédito são transmitidos diretamente do navegador do consumidor para o gateway de pagamento. Os dados do cartão nunca estão disponíveis em nenhum dos ambientes Managed Services do plano Adobe Commerce Pro. As ações nas transações pelo aplicativo de comércio eletrônico são concluídas usando uma referência à transação do gateway.

## aplicativo Adobe Commerce

O Adobe testa regularmente o código principal do aplicativo em busca de vulnerabilidades de segurança. Patches para defeitos e problemas de segurança são fornecidos aos clientes. A Equipe de segurança do produto valida os produtos da Adobe Commerce seguindo as diretrizes de segurança de aplicativos da OWASP. Várias ferramentas de avaliação de vulnerabilidade de segurança e fornecedores externos são usadas para testar e verificar a conformidade. As ferramentas de segurança incluem:

- Verificação estática e dinâmica de Veracode
- Verificação do código-fonte RIPS
- Serviços de verificação de vulnerabilidades da Trustwave e da Alert Logic
- Burp Suite Pro
- OWASPZAP
- andSqlMap

A base de código completa é verificada com essas ferramentas quinzenalmente. Os clientes são notificados sobre patches de segurança por emails diretos, notificações no aplicativo e na [Central de Segurança](https://helpx.adobe.com/security.html).

Os clientes devem garantir que esses patches sejam aplicados em seus aplicativos personalizados dentro de 30 dias do lançamento, de acordo com as diretrizes de PCI. O Adobe também fornece uma [Ferramenta de Verificação de Segurança](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan) que permite aos comerciantes monitorar regularmente seus sites e receber atualizações sobre riscos de segurança conhecidos, malware e acesso não autorizado. A Ferramenta de verificação de segurança é um serviço gratuito e pode ser executada em qualquer versão do Adobe Commerce.

Para incentivar os pesquisadores de segurança a identificar e relatar vulnerabilidades, a Adobe Commerce tem um [programa de retribuição de bugs](https://hackerone.com/magento), além de testes internos. Além disso, o cliente recebe o código fonte completo do aplicativo para sua própria análise, se desejado.

## Sistema de arquivos somente leitura

Todo o código executável é implantado em uma imagem de sistema de arquivos somente leitura, o que reduz drasticamente as superfícies que estão disponíveis para ataques. O processo de implantação cria uma imagem Squash-FS para reduzir as oportunidades de inserir código PHP ou JavaScript no sistema ou modificar arquivos de aplicativos Adobe Commerce.

## Implantação remota

A única maneira de obter o código executável no ambiente de produção do Managed Services é executá-lo por meio de um processo de provisionamento. O provisionamento envolve enviar o código-fonte do repositório de origem para um repositório remoto que inicia um processo de implantação. O acesso a esse destino de implantação é controlado, assim você tem controle total sobre quem pode acessar o destino de implantação. Todas as implantações do código do aplicativo no ambiente de não produção são controladas pelo cliente.

## Logs

Todas as atividades do AWS são registradas no AWS CloudTrail. O sistema operacional, o servidor de aplicativos e os logs de banco de dados são armazenados nos servidores de produção e armazenados em backups. Todas as alterações no código-fonte são registradas em um repositório Git. O histórico de implantação está disponível na [Interface da Web do Projeto](../project/overview.md) do Adobe Commerce. Todo o acesso ao suporte é registrado e as sessões de suporte são gravadas.

Consulte [Exibir e gerenciar logs](../test/log-locations.md).

## Dados sensíveis

Os dados confidenciais podem abranger informações pessoais de consumidores ou dados confidenciais de clientes da Managed Services. A proteção de dados confidenciais de clientes e consumidores é uma obrigação essencial para o Adobe Commerce Managed Services. Os clientes da Managed Services e do Adobe têm obrigações legais em relação a informações de identificação pessoal. Além dos recursos de segurança da arquitetura, há outros controles para limitar a distribuição e o acesso a dados confidenciais.

Os clientes são proprietários de seus dados e têm controle sobre onde esses dados estão localizados. O cliente especifica o local onde residem suas instâncias de produção e desenvolvimento. Eles também especificam qual local é usado para o ambiente de relatórios do Adobe Commerce com o Commerce e se esse aplicativo de relatórios do Adobe Commerce tem ou não acesso a informações de identificação pessoal. As instâncias de Produção podem estar na maioria das regiões do AWS, enquanto os ambientes de desenvolvimento e de Relatórios do Adobe Commerce podem ser encontrados atualmente nos Estados Unidos ou na União Europeia.

Os dados confidenciais podem passar pela rede de servidores do Fastly CDN, mas não são armazenados na rede do Fastly. Todos os parceiros incluídos na oferta da Managed Services têm obrigações contratuais para garantir a proteção de dados confidenciais. A Managed Services não move dados confidenciais do cliente ou do consumidor dos locais especificados pelo cliente.

Como parte da prestação dos serviços incluídos na oferta da Managed Services, a equipe da Managed Services requer acesso a sistemas de produção que contenham dados confidenciais. Esses funcionários são treinados em suas obrigações para proteger dados confidenciais de clientes e consumidores. O acesso a esses sistemas é limitado aos funcionários que precisam de acesso. Esses funcionários só têm acesso pelo tempo necessário para fornecer esses serviços.

## Regulamento Geral sobre a Proteção de Dados

O Regulamento Geral sobre a Proteção de Dados (GDPR) é uma estrutura legal que define diretrizes para a coleta e o processamento de informações pessoais de pessoas físicas na União Europeia (UE). Esses regulamentos se aplicam independentemente de onde o site esteja baseado e os visitantes da UE têm acesso a ele.

Os visitantes devem ser notificados dos dados coletados por um site e consentir explicitamente na coleta de informações. Os sites devem notificar os visitantes se os dados pessoais mantidos pelo site forem violados.

O comerciante ou a empresa que opera o site deve ter um responsável pela proteção de dados dedicado que supervisiona a segurança de dados do site. O Responsável pela privacidade de dados (ou a equipe de gerenciamento de sites) deve estar disponível para contato se um visitante solicitar que seus dados sejam apagados.

O GDPR solicita que todas as informações de identificação pessoal (como nomes, raça e data de nascimento) coletadas sejam anonimizadas ou pseudonimizadas.

>[!NOTE]
>
>Esta página fornece uma visão geral do que deve ser considerado para o GDPR. Consulte o _[Guia de Segurança e Conformidade](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/overview)_ para obter detalhes sobre como a Adobe Commerce armazena informações pessoais. Para determinar como sua empresa deve cumprir quaisquer obrigações legais, consulte seu advogado ou consulte o [texto oficial](https://eur-lex.europa.eu/eli/reg/2016/679/oj).

## Backups

Os backups são executados a cada hora nas últimas 24 horas de operação. Após o período de 24 horas, os backups são retidos de acordo com uma programação usando o serviço de snapshot do AWS EBS. Consulte [Política de retenção](pro-architecture.md#retention-policy).

O serviço cria um backup independente no armazenamento redundante. Como os volumes EBS são criptografados, os backups também são criptografados. Além disso, o Managed Services realiza backups sob demanda de acordo com as solicitações dos clientes.

Sua abordagem de backup e recuperação da Managed Services usa uma arquitetura de alta disponibilidade combinada com backups completos do sistema. Cada projeto é replicado — todos os dados, código e ativos — em três zonas de disponibilidade da AWS separadas; cada zona com um data center separado.

Consulte [Gerenciamento de instantâneos e backup](../storage/snapshots.md).
