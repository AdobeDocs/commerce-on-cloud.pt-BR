---
title: Patches da nuvem para o Commerce
description: Consulte uma lista das melhorias mais recentes no pacote de Patches na nuvem.
recommendations: noDisplay, catalog
last-substantial-update: 2025-06-03T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: e447e19d89edeaec84314c52b377f3712e0f0400
workflow-type: tm+mt
source-wordcount: '2451'
ht-degree: 0%

---

# Patches da nuvem para o Commerce

O pacote [Patches da Nuvem](https://github.com/magento/magento-cloud-patches) fornece um conjunto de patches necessários que melhoram a integração de todas as versões do Adobe Commerce com ambientes na Nuvem e oferecem suporte à entrega rápida de correções críticas.

O pacote Cloud Patches for Commerce é uma dependência do pacote ECE-Tools e é instalado e atualizado quando você instala ou atualiza o pacote ECE-Tools. Você também pode usar e gerenciar Patches da nuvem para o Commerce como um pacote independente para aplicar patches a um projeto do Adobe Commerce que não esteja na plataforma de nuvem. Estas notas de versão descrevem as melhorias mais recentes neste pacote.

>[!TIP]
>
>Para garantir que seu projeto tenha todos os patches necessários, atualize para a [versão mais recente das ece-tools](../dev-tools/update-package.md).

>[!NOTE]
>
>Consulte [Aplicar patches](../development/apply-patches.md) para obter instruções sobre como aplicar patches aos seus projetos.

O pacote `magento/magento-cloud-patches` usa a seguinte sequência de versão: `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.8 {#latest}

Data de lançamento: 03 de junho de 2025

- ![ícone de correção](../../assets/fix.svg) **Compatibilidade aprimorada com 2.4.8**-Bibliotecas de terceiros atualizadas para melhor compatibilidade com 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.7

Data de lançamento: 05 de maio de 2025

- ![novo ícone](../../assets/new.svg) **Patch atualizado para Commerce 2.4.4 para 2.4.8**—Este é um patch atualizado para [CVE-2025-24434](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch), que foi lançado em 1.1.7<!-- MCLOUD-13619 -->

## v1.1.6

Data de lançamento: 24 de abril de 2025

- ![novo ícone](../../assets/new.svg) **Patch atualizado para Commerce 2.4.4 para 2.4.7**—Este é um patch atualizado para [CVE-2025-24434](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08), que foi lançado em 1.1.4<!-- MCLOUD-13240 -->

## v1.1.5

Data de lançamento: 15 de abril de 2025

- ![novo ícone](../../assets/new.svg) **Adição de correção para B2B 1.5.2**—Correção para o problema ACP2E-3833 com B2B módulo 1.5.2 e MariaDB 10.6<!-- MCLOUD-13605	-->

## v1.1.4

Data de lançamento: 13 de fevereiro de 2025

- ![novo ícone](../../assets/new.svg) **Adição de patch para Commerce 2.4.4 a 2.4.7**—Esta atualização patches [CVE-2025-24434](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08).<!-- MCLOUD-13240	 - -->

## v1.1.3

Data de lançamento: 6 de fevereiro de 2025

- ![novo ícone](../../assets/new.svg) **PHP 8.4**—Suporte adicionado para PHP 8.4.<!-- MCLOUD-13149	 - -->

## v1.1.2

Data de lançamento: 5 de novembro de 2024

- ![Ícone de correção](../../assets/fix.svg) **Adição de correção para Commerce 2.4.4 para 2.4.7**—Esta atualização corrige uma vulnerabilidade crítica [CVE-2024-45115](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73) para Adobe Commerce ao usar o módulo B2B.<!-- MCLOUD-12980 - -->

## v1.1.1

Data de lançamento: 5 de novembro de 2024

- ![Ícone de correção](../../assets/fix.svg) **Adição de correção para o Commerce 2.4.4 para 2.4.7**—Esta atualização corrige uma vulnerabilidade crítica do [CVE-2024-34102](https://experienceleague.adobe.com/pt-br/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting.<!-- MCLOUD-12980 - -->

## v1.1.0

Data de lançamento: 7 de outubro de 2024

- ![Ícone de correção](../../assets/fix.svg) **Código refatorado**—Remoção do suporte a versões antigas do PHP (7.4, 7.3, 7.2) e bibliotecas relacionadas.<!-- MCLOUD-9278 - -->
- ![Ícone de correção](../../assets/fix.svg) **Versão do Monolog atualizada**—Suporte adicionado para monolog 3.6.<!-- MCLOUD-12855 - -->
- ![ícone de correção](../../assets/fix.svg) **Patch para o Servidor de Aplicativos** — Resolve um problema conhecido com o Servidor de Aplicativos GraphQL. Especificamente, o `CatalogGraphQl\\Model\\Config\\AttributeReader` na versão 2.4.7 continha um erro que poderia fazer com que solicitações do GraphQL recuperassem respostas com base na configuração desatualizada de Atributos.<!-- ACPT-1876 -->

## v1.0.27

Data de lançamento: 21 de maio de 2024

- **Suporte para PHP 8.3**—Este patch resolve erros de compatibilidade entre o php 8.3 e a versão do pacote compositor.

## v1.0.26

Data de lançamento: 8 de abril de 2024

- ![novo ícone](../../assets/new.svg) **PHP** — Suporte adicionado para PHP 8.3.

## v1.0.25

Data de lançamento: 16 de janeiro de 2024

- **Melhorias de cache**-Este patch melhora a eficiência do cache de layout, reduzindo significativamente o uso de memória, para as versões 2.4.4 e posteriores do Adobe Commerce.<!-- MCLOUD-11514 -->
- **Melhorias nos trabalhos do CRON**-Este patch corrige o problema em que os trabalhos perdidos aguardam desnecessariamente bloqueios de trabalhos do CRON para as versões 2.4.4 e posteriores do Adobe Commerce.<!-- MCLOUD-11329 -->

## v1.0.24

Data de lançamento: 15 de setembro de 2023

- **Aprimoramento de desempenho**-Este patch corrige um problema que afeta o desempenho reduzindo o número de vezes que as mesmas configurações de implantação são carregadas para o Adobe Commerce 2.4.6 para 2.4.6-p1<!-- MCLOUD-10604 -->

## v1.0.23

Data de lançamento: 31 de julho de 2023

- **Remoção do patch MCLOUD-10604** - Este patch foi movido para QPT.<!-- MCLOUD-10736 -->

## v1.0.22

Data de lançamento: 19 de junho de 2023

- **Assistente/saída QPT CLI aprimorado**—Adicionou um aviso ao assistente/saída QPT CLI que o lembra de verificar os detalhes e requisitos do patch se houver dependências.<!-- ACP2E-1963 -->
- **Patches adicionados para o Commerce 2.4.6:**
   - Correção da validação `regexp cache tag`.<!-- MCLOUD-10226 -->
   - Desempenho aprimorado reduzindo o número de vezes que as mesmas configurações de implantação são carregadas.<!-- MCLOUD-10604 -->
- **Adição de patches para o Commerce 2.3.7 a 2.4.6**—Correção de um problema que causava um incremento de um valor aleatório em vez de um incremento de 1 para as tabelas `catalog_product_entity_*`.<!-- MCLOUD-10032 -->
- **Adição de patches para o Commerce 2.4.0 a 2.4.6**—Correção de um erro informando que `The file can't be deleted. Warning!unlink: No such file or directory`, que ocorreu ao liberar o cache JS/CSS do Administrador.<!-- MCLOUD-10279 -->

## v1.0.21

Data de lançamento: 10 de março de 2023

- **Suporte aprimorado para o PHP 8.2**—Corrigiu problemas de compatibilidade com certas versões do PHP 8.2.x para suportar Commerce 2.4.6.

## v1.0.20

Data de lançamento: 27 de outubro de 2022

- **Patch de melhorias no cache L2 adicionado**—Este patch corrige um problema na liberação do cache L2 local para o Commerce versões 2.4.0 e 2.4.1.<!-- MCLOUD-7845 -->

## v1.0.19

Data de lançamento: 13 de setembro de 2022

- **Suporte avançado para PHP 8.1**—Corrigiu problemas de compatibilidade com certas versões do PHP 8.1.x.

## v1.0.18

Data de lançamento: 11 de agosto de 2022

Patch crítico para o Adobe Commerce 2.4.5:

- **Problema com pedidos usando pagamentos do Braintree**—Este patch resolve um problema crítico que impedia os administradores de fazer novos pedidos ou repedidos.<!-- MCLOUD-9137 -->

Consulte [O administrador não pode criar pedido/reordenação quando o pagamento do Braintree está habilitado](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html?lang=pt-BR).

## v1.0.17

Data de lançamento: 24 de maio de 2022

Correção de restrições para patches de segurança no arquivo `patches.json`.

## v1.0.16

Data de lançamento: 31 de março de 2022

Patch crítico para o Adobe Commerce 2.3.3-p1 e versões posteriores:

Patches atualizados para resolver uma vulnerabilidade **crítica**, resultando na execução de código remoto não autenticado.<!-- MCLOUD-8479 -->

Consulte o [Boletim de segurança do Adobe APSB22-12](https://helpx.adobe.com/br/security/products/magento/apsb22-12.html).

## v1.0.15

Data de lançamento: 10 de março de 2022

- **Suporte ao PHP 8.1**—Adicionou suporte ao PHP 8.1 e eliminou o suporte ao PHP 7.0 e 7.1.
- **Patch adicionado para Adobe Commerce 2.3.3**—Moeda fixa exibida na página do produto.

## v1.0.14

Data de lançamento: 13 de fevereiro de 2022

Patch crítico para o Adobe Commerce 2.3.3-p1 e versões posteriores:

Adição de um patch para resolver uma vulnerabilidade **crítica**, resultando na execução de código remoto não autenticado.<!-- MCLOUD-8461 -->

Consulte o [Boletim de segurança do Adobe APSB22-12](https://helpx.adobe.com/br/security/products/magento/apsb22-12.html).

## v1.0.13

Data de lançamento: 25 de outubro de 2021

- **Atualizar Monólogo** — Atualizou a versão mínima necessária para o pacote `monolog` para `^2.3`.<!-- ACMP-1263 -->
- **Método PHP Incompatível**—Corrigido método PHP incompatível para Adobe Commerce versões 2.4.3 e 2.3.7-p1.<!-- AC-384 -->
- **Erro de PHP**—Corrigido um erro `PHP error 'Undefined variable: errorMessage' ...` que ocorreu durante a tentativa de aplicar um patch.<!-- ACP2E-138 -->

## v1.0.12

Data de lançamento: 12 de agosto de 2021

Patch crítico para Adobe Commerce 2.4.3 e 2.3.7-p1:

- **Problema com limitação de taxa de API** — Este patch corrige um limite de taxa padrão que impedia que APIs da Web processassem solicitações com mais de 20 itens em uma matriz. Este patch aumenta o valor padrão do limite de taxa. Consulte as [notas de versão do Adobe Commerce 2.4.3](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

Data de lançamento: 29 de julho de 2021

- **Correção de um problema causado pela aplicação do patch de navegação em Camadas B2B**—Para clientes que aplicaram o patch de navegação em Camadas B2B, essa correção resolve um erro `Undefined offset` que é exibido na página Pesquisa após alternar o modo de exibição Loja.<!--MCLOUD-5287-->

- **Patch de Finalização do Paypal**—Corrige um problema do Adobe Commerce 2.3.7 com o PayPal Express, no qual o preço do pedido inserido anteriormente é exibido.<!--MC-42674-->

- **Suporte à categoria de patch** — Adição de suporte ao processamento de categorias de patch e origens de origem atribuídas a Patches de Qualidade. As categorias permitem que os clientes usem filtros e classificações para localizar patches com mais rapidez ao usar a [Ferramenta de Patches de Qualidade](https://github.com/magento/quality-patches) e a Ferramenta de Análise do Site (SWAT). <!--MC-38577-->

## v1.0.10

Data de lançamento: 10 de maio de 2021

- **Compatibilidade com o Adobe Commerce 2.3.7**—Conflito resolvido de dependências do compositor para instalação no Adobe Commerce 2.3.7.<!--MC-42131-->
- **Correção de um problema causado pela aplicação de um patch agrupado várias vezes**. A aplicação de um patch agrupado (um que inclui outros patches obsoletos) mais de uma vez poderia reverter os pacotes obsoletos incluídos. Agora, todos os patches são aplicados apenas uma vez. Tentar aplicar o mesmo pacote novamente mostra uma mensagem de que o patch já foi aplicado.<!--MC-41912-->
- **Patch de navegação em camadas B2B**—Corrigido outro problema que impedia a navegação em camadas de mostrar todas as opções do produto quando o usuário habilita o Catálogo Compartilhado B2B.<!--MCLOUD-7742-->

## v1.0.9

Data de lançamento: 1 de fevereiro de 2021

- **Patch de navegação em camadas B2B**—Corrigido o problema que impedia que a navegação em camadas mostrasse todas as opções de produto quando o Catálogo Compartilhado B2B estava habilitado.<!--MCLOUD-6923-->
- **Compatibilidade com o PHP 7.4**—Correção de um problema de compatibilidade de patches de nuvem com o PHP 7.4.<!--MCLOUD-7367-->
- **Patches obsoletos tornam-se visíveis**—Corrigido um problema de patches de nuvem em que os patches obsoletos tornam-se visíveis na tabela de patches após a aplicação de um patch de substituição que contém todo o conteúdo do patch obsoleto. Isso pode acontecer se você aplicar um patch que combinou vários outros patches.<!--MC-40626-->
- **Falhas silenciosas ao aplicar patches**—Corrigiu um problema de patches de nuvem no qual o comando `git apply` silenciosamente falhou ao aplicar patches em alguns ambientes.<!--MC-40529-->

## v1.0.8

Data de lançamento: 14 de outubro de 2020

- **Atualizações de compatibilidade para magento/magento-cloud-patches**—Atualizou as restrições de versão `symfony` e `semver` no arquivo `composer.json` para compatibilidade com o Adobe Commerce 2.4.1 e versões posteriores.<!--MCLOUD-7111-->

## v1.0.7

Data de lançamento: 14 de outubro de 2020

- **Patches do Redis para Adobe Commerce 2.3.0 para 2.3.5, 2.4.0**—Os patches do Redis foram atualizados para oferecer suporte à adição de produtos a uma categoria durante a implementação de um cache de Nível 2. <!--MCLOUD-6659-->

- **Correção do Braintree VBE** — Corrige um problema que gerava um erro quando um Administrador tentava exibir um Relatório de Liquidação do Braintree. <!--MCLOUD-6684-->

- Agora, o comando `ece-patches apply` usa o comando Unix `patch` para aplicar patches se o Git não estiver disponível no sistema host. <!--MCLOUD-7069-->

## v1.0.6

Data de lançamento:

- **Patches Redis para Adobe Commerce 2.3.0 - 2.3.4**—Otimize a comunicação e melhore o desempenho
   - Reduza o tamanho das transferências de rede entre Redis e Adobe Commerce
   - Corrigir condições de corrida em operações de carregamento e gravação do Redis
   - Reescreva o adaptador de cache base para manipular erros ao salvar
   - Diminuir o consumo de Redis CPU<!--MCLOUD-6139-->

- **Patches Redis para Adobe Commerce 2.3.0 - 2.3.5**—Melhore o desempenho e corrija erros
   - Corrija a implementação de Cache lock para evitar bloqueios infinitos
   - Melhorar o mecanismo de bloqueio atual
   - Implementar bloqueios assinados para impedir o desbloqueio de solicitações paralelas
   - Corrija o seguinte erro que ocorre na operação de gravação Redis: `OOM command not allowed when used memory > maxmemory`
   - Corrigir o processamento do cache limpo pela marca `cat_p` que é executada durante atualizações de produto<!--MCLOUD-6110-->

- Correção de um problema que causava um erro ao aplicar o patch `amzn/amazon-pay-module` necessário ao Adobe Commerce em projetos de infraestrutura em nuvem com Adobe Commerce v2.2.6 ou 2.3.5, que não incluem esse módulo. Agora, o processo de patch ignora o patch `amzn/amazon-pay-module` se o módulo não estiver instalado.<!--MCLOUD-6588-->

## v1.0.5

Data de lançamento: 26 de junho de 2020

- **Melhorias no desempenho do Redis**—Adiciona recursos de otimização do Redis às versões 2.3.3 e 2.3.4 do Adobe Commerce. Essas correções foram incluídas na versão 2.3.5 do Adobe Commerce.<!--MCLOUD-5771-->

- **Enriquecimento de log do New Relic**—Adiciona a Interface do Processador de Monolog, necessária para suportar melhorias nos recursos de log do New Relic introduzidos nos Componentes de Nuvem do Commerce versão 1.0.4. Este patch é necessário para implantar o Adobe Commerce 2.1.x. Se o patch não for aplicado, a compilação falhará durante o processo `di:compile`.<!--MCLOUD-6029-->

## v1.0.4

Data de lançamento: 12 de maio de 2020

- **Check-out de Pagamento do Amazon**—Corrige um problema com o widget de pagamento de Pagamento do Amazon que impedia os clientes de alterar o método de pagamento na etapa _Verificação e Pagamentos_ durante o processo de check-out.<!--MCLOUD-5930-->

- **Exibição do produto na página Categoria**—Corrige um problema que impedia que os produtos fossem exibidos na página de categoria no modo de exibição _Mostrar todas as páginas_.<!--MCLOUD-5684-->

- **Carregamento de imagem do Page Builder** — Corrige um problema de interface do Page Builder que às vezes causava o seguinte erro ao carregar imagens para a galeria de imagens: `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Suprimir avisos desnecessários de geração de mapa de site** — Adiciona uma nova tentativa quando ocorrem erros durante a geração do mapa de site e ignora a notificação por email do cliente nos casos em que os erros podem ser recuperados automaticamente.<!--MCLOUD-3025-->

- **Aprimoramento de desempenho do site**—Corrige um problema de desempenho com a função `Magento\Framework\App\DeploymentConfig\Reader::load`, que periodicamente apresentou longos tempos de carregamento que afetaram o desempenho do site. <!--MCLOUD-5650-->

- Atualização da atribuição de patch para patches do método de pagamento para direcionar os módulos de pagamento em vez do pacote base do Magento (magento/magento2-base), de modo que os patches de pagamento sejam aplicados somente se os módulos de pagamento existirem.<!--MCLOUD-5666-->

- Patches atualizados para compatibilidade com o Magento Open Source.<!--MCLOUD-5701-->

## v1.0.3

Data de lançamento: 28 de abril de 2020

- Adição de uma correção para o patch &quot;O FPC está sendo desativado durante as implantações&quot; para oferecer suporte ao Adobe Commerce 2.3.5.

## v1.0.2

Data de lançamento: 27 de fevereiro de 2020

Esta versão inclui os seguintes patches e correções críticas:

- **Atualizações de compatibilidade do magento/magento-cloud-patches**

   - Restrições de versão `symfony` e `semver` atualizadas no arquivo `composer.json` para compatibilidade com o Adobe Commerce 2.4 e versões posteriores.<!--MAGECLOUD-5127-->

   - Restrições atualizadas no `composer.json` para compatibilidade com as versões do `ece-tools` 2002.0.22 e posteriores 2002.0.x.

- **Check-out do PayPal Express**—Publicado em 12 de fevereiro de 2020, este patch resolve um problema que afeta pedidos feitos com o Check-out do PayPal Express, onde o endereço de envio do pedido especifica uma região do país que foi inserida manualmente no campo de texto em vez de ser selecionada no menu suspenso na página Envio. Consulte a descrição completa do patch na página de download de patches.

- **Correção de implantação de aplicativo**—Adicionou um patch para corrigir um problema que desabilitava o cache de página inteira durante o processo de implantação. Este patch se aplica ao Adobe Commerce 2.3.2 e versões posteriores.

- **Parâmetro de escopo para API Assíncrona/em Massa**—Atualizou este patch para corrigir um erro de sintaxe no arquivo `composer.json`. Este patch se aplica ao Magento Open Source 2.3.1 e 2.3.2. Consulte a descrição completa do patch na página de download de patches.

## v1.0.1

Data de lançamento: 6 de fevereiro de 2020

Incluímos todos os patches do Magento Open Source 2.x da página de downloads de software na versão magento/magento-cloud-patches v1.0.1. Se você tiver copiado patches no projeto anteriormente, remova-os para evitar conflitos.

Esta versão inclui os seguintes patches e correções críticas:

- **Corrigir bloqueios de cron e melhorar o bloqueio de cron**—

   - Corrige um problema em que alguns trabalhos cron não estão sendo executados devido a um valor de status incorreto na tabela `cron_schedule`. Agora, usamos a estrutura Adobe Commerce lock para verificar e atualizar o status do trabalho cron em vez de usar a tabela `cron_schedule`. Os trabalhos do CRON que terminaram com um status de erro são repetidos durante a próxima execução do CRON, em vez de aguardarem 24 horas.

   - Adiciona uma operação _retry_ para evitar deadlock durante atualizações dos dados na tabela `cron_schedule`.

- **Atualização de `magento/magento-cloud-patches` para incluir todos os patches disponíveis para o Magento Open Source 2.x**—Atualização do pacote magento/magento-cloud-patches para incluir todos os patches do Magento Open Source 2.x disponíveis na página de downloads de software. Se você tiver copiado patches do Magento Open Source no seu projeto Adobe Commerce na infraestrutura em nuvem anteriormente, remova-os para evitar conflitos.<!--MAGECLOUD-4606-->

- **Correção de paginação de catálogo do Elasticsearch** — Substituiu o patch de paginação de catálogo do Elasticsearch entregue em magento/magento-cloud-patches v1.0 por uma correção mais eficaz.<!--MAGECLOUD-4847-->

- **Patches do Page Builder** — Em Patches da nuvem para o Commerce 1.0.0, agrupamos patches do Page Builder para solucionar uma vulnerabilidade conhecida de execução remota de código (RCE) do Page Builder, com a correção inicial baseada no Adobe Commerce 2.3.3. Atualizamos esses patches com uma implementação mais estável baseada no Adobe Commerce 2.3.4., que inclui várias otimizações para corrigir o problema.<!--MAGECLOUD-4884-->

  Se você tiver o pacote magento/magento-cloud-patches 1.0.0, ainda estará protegido contra problemas de vulnerabilidade do RCE no Page Builder. Se você atualizar para 1.0.1 ou posterior, terá uma implementação melhor da mesma correção.

## v1.0.0

Data de lançamento: 14 de novembro de 2019

Esta é a primeira versão do pacote [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), que é uma nova dependência da versão 2002.0.22 ou posteriores do pacote `ece-tools`.

Esta versão inclui os seguintes patches e correções críticas:

- **Patches de segurança do Page Builder para as versões 2.3.1.x e 2.3.2.x**—Corrige um problema na visualização do Page Builder que permite que usuários não autenticados acessem alguns métodos de modelo que podem ser usados para disparar a execução arbitrária de código na rede (RCE), resultando em vazamentos de informações globais. Esse problema pode ocorrer ao usar versões não compatíveis do Page Builder com as versões 2.3.1 e 2.3.2 do Adobe Commerce.<!--MAGECLOUD-4649-->

- **Patches MSI** — corrige problemas que causavam erros de indexação e problemas de desempenho ao usar configurações de inventário padrão para gerenciar estoque.<!--MAGECLOUD-4428-->

- **Compatibilidade com Versões Anteriores de novas Interfaces de Email**-Corrige um problema de incompatibilidade com versões anteriores causado pela interface do PHP `Magento\Framework\Mail\EmailMessageInterface` introduzida no Adobe Commerce v2.3.3. No escopo deste patch, o novo `EmailMessageInterface` herda do `MessageInterface` antigo, e os módulos principais do Adobe Commerce são revertidos para depender do `MessageInterface`.<!--MAGECLOUD-4422-->

- **A paginação de catálogo não funciona no Elasticsearch 6.x** — Corrige um problema crítico com a paginação de resultados de pesquisa que afeta clientes que usam o Elasticsearch 6.x como mecanismo de pesquisa de catálogo.<!--MAGECLOUD-4448-->
