---
title: Notas de versão do ECE-Tools
description: Consulte uma lista das melhorias mais recentes no pacote ECE-Tools.
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 562fd6e1dcd09600e00d034a94509b2dfd69d1ef
workflow-type: tm+mt
source-wordcount: '3314'
ht-degree: 0%

---

# Notas de versão do ECE-Tools

O pacote [ece-tools](https://github.com/magento/ece-tools) é um conjunto de scripts e ferramentas criado para gerenciar e implantar projetos na nuvem. Estas notas de versão descrevem as últimas melhorias neste pacote, que faz parte do [Conjunto de ferramentas da nuvem para o Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Consulte [Atualizar ECE-Tools](../dev-tools/update-package.md) para obter informações sobre como atualizar para a versão mais recente do pacote `ece-tools`.

O pacote `ece-tools` usa a seguinte sequência de controle de versão: `200<major>.<minor>.<patch>`

As notas de versão incluem:

- ![novo ícone](../../assets/new.svg) Novos recursos
- ![ícone de correção](../../assets/fix.svg) Correções e melhorias

<!--Add release notes below-->

## v2002.2.9 {#latest}

Data de lançamento: 06 de novembro de 2025

- ![Ícone de correção](../../assets/fix.svg) **Pacote Symfony** - Suporte adicionado para os pacotes YAML Symfony mais recentes.<!-- MCLOUD-14020 -->
- ![ícone de correção](../../assets/fix.svg) **Limpeza de cache corrigida para serviços ativos** - Validação de serviço ativo adicionada.<!-- MCLOUD-14166 -->

## v2002.2.8

Data de lançamento: 08 de outubro de 2025

- ![novo ícone](../../assets/new.svg) **AtiveMQ** - Suporte adicionado para AtiveMQ.<!-- MCLOUD-13770 -->
- ![novo ícone](../../assets/new.svg) **AtiveMQ**-Adição de testes funcionais.<!-- MCLOUD-13813 -->


## v2002.2.7

Data de lançamento: 07 de agosto de 2025

- ![ícone de correção](../../assets/fix.svg) **Correções do PHP 8.4** - Compatibilidade de tipo adicionada.<!-- MCLOUD-13965 -->
- ![ícone de correção](../../assets/fix.svg) **Validador de EOL** - Datas atualizadas do fim da vida útil (EOL).<!-- MCLOUD-13929 -->
- ![novo ícone](../../assets/new.svg) **Valkey**-Adição dos testes funcionais do PHP 8.2 e PHP 8.3.<!-- MCLOUD-13610 -->
- ![ícone de correção](../../assets/fix.svg) **Validador de Valkey**-Corrigiu a mensagem de aviso de ferramentas ECE.<!-- MCLOUD-13896 -->
- ![Ícone de correção](../../assets/fix.svg) **Ferramentas ECE**-Adição de melhorias nos Testes de unidade.<!-- MCLOUD-13838 -->
- ![novo ícone](../../assets/new.svg) **Validador para serviços** - Adição de suporte a novas versões de Opensearch, MariaDB e PHP.<!-- MCLOUD-13923 -->
- ![novo ícone](../../assets/new.svg) **Opensearch3**-Suporte adicionado para Opensearch3.<!-- MCLOUD-13763 -->
- ![Ícone de correção](../../assets/fix.svg) **Suporte a Opensearch para 2.4.4-p7/p12**-Atualização do script de validação.<!-- MCLOUD-13945 -->
- ![novo ícone](../../assets/new.svg) **Testes do Opensearch3**-Testes funcionais adicionados.<!-- MCLOUD-13769 -->

## v2002.2.6

Data de lançamento: 03 de junho de 2025

- ![ícone de correção](../../assets/fix.svg) **Compatibilidade aprimorada com 2.4.8**-Bibliotecas de terceiros atualizadas para melhor compatibilidade com 2.4.8<!-- MCLOUD-13707 -->

## v2002.2.5

Data de lançamento: 27 de maio de 2025

- ![novo ícone](../../assets/new.svg) **Compatibilidade com Valkey Estendida**-Compatibilidade com Valkey Estendida no Adobe Commerce.<!-- MCLOUD-13595 -->
- ![ícone de correção](../../assets/fix.svg) **Validador RabbitMQ atualizado**-Validador atualizado para RabbitMQ.<!-- MCLOUD-13589 -->
- ![ícone de correção](../../assets/fix.svg) **Validador de MariaDB atualizado**-Validador de ece-tools atualizado para MariaDB 10.11.<!-- MCLOUD-13593 -->
- ![Ícone de correção](../../assets/fix.svg) **Compatibilidade estendida do Opensearch2** - O Opensearch2 é compatível com as versões 2.4.4 mais recentes.<!-- MCLOUD-13710 -->

## v2002.2.4

Data de lançamento: 24 de abril de 2025

- ![ícone de correção](../../assets/fix.svg) **Opensearch2 para 2.4.4/2.4.5**—Corrigiu um problema relacionado ao suporte para `opensearch2` nas versões do Adobe Commerce 2.4.4/2.4.5.<!-- MCLOUD-13607 -->

## v2002.2.3

Data de lançamento: 9 de abril de 2025

- ![ícone de correção](../../assets/fix.svg) **Corrigir Valkey** Corrigiu um problema com a configuração personalizada valkey.<!-- MCLOUD-13569 -->
- ![ícone de correção](../../assets/fix.svg) **Validador de correção**-Validador corrigido para RabbitMQ 4.0.<!-- MCLOUD-13560 -->

## v2002.2.2

Data de lançamento: 7 de abril de 2025

## v2002.2.2

Data de lançamento: 7 de abril de 2025

- ![novo ícone](../../assets/new.svg) **Valkey**—Suporte adicionado para um novo serviço (Valkey), que é uma substituição para Redis.<!-- MCLOUD-13455 -->
- ![Ícone de correção](../../assets/fix.svg) **Opensearch2 para 2.4.4/2.4.5**—Suporte adicionado para `opensearch2` nas versões do Adobe Commerce 2.4.4/2.4.5.<!-- MCLOUD-13493 -->

## v2002.2.1

Data de lançamento: 6 de fevereiro de 2024

- ![novo ícone](../../assets/new.svg) **PHP 8.4**—Suporte adicionado para PHP 8.4.<!-- MCLOUD-13145 -->
- ![Ícone de correção](../../assets/fix.svg) **Validador para Opensearch**-Corrigido o validador que gerou uma mensagem enganosa sobre a versão errada do serviço.<!-- MCLOUD-13184 -->

## v2002.2.0

Data de lançamento: 7 de outubro de 2024

- ![novo ícone](../../assets/new.svg) **MariaDB 11.4**-Suporte adicionado de MariaDB 11.4.
- ![ícone de correção](../../assets/fix.svg) **Código refatorado**-Remoção do suporte às versões antigas do PHP 7.4, 7.3, 7.2 e bibliotecas relacionadas.<!-- MCLOUD-9278 -->
- ![Ícone de correção](../../assets/fix.svg) **Versão do Monolog atualizada**-Suporte adicionado para monolog 3.6.<!-- MCLOUD-12855 -->
- ![Ícone de correção](../../assets/fix.svg) **Validador para RabbitMQ, MariaDB e PHP** - Corrigido o validador que gerou uma mensagem enganosa sobre a versão incorreta do serviço.

## v2002.1.19

Data de lançamento: 21 de maio de 2024

- ![novo ícone](../../assets/new.svg) **Lua**—Adicionada a opção useLua para CACHE_CONFIGURATION.
- ![Ícone de correção](../../assets/fix.svg) **Validador**—Validadores atualizados para novas versões do Redis e RabbitMQ.

## v2002.1.18

Data de lançamento: 8 de abril de 2024

- ![novo ícone](../../assets/new.svg) **PHP** — Suporte adicionado para PHP 8.3.
- ![ícone de correção](../../assets/fix.svg) **Validador** - Validador de EOL atualizado.

## v2002.1.17

Data de lançamento: 16 de janeiro de 2024

- ![Ícone de correção](../../assets/fix.svg) **Validador para Elasticsearch e OpenSearch**—Corrigiu o validador que gerou uma mensagem enganosa para instalar um serviço de pesquisa quando o LiveSearch está habilitado.<!-- MCLOUD-10167 -->
- ![Ícone de correção](../../assets/fix.svg) **Aviso de implantação**—Corrigiu um problema que resultava em avisos de implantação sobre pastas não vazias.<!-- MCLOUD-8958 -->

## v2002.1.16

Data de lançamento: 16 de outubro de 2023

- ![novo ícone](../../assets/new.svg) **Variável de ambiente global ENABLE_WEBHOOKS**—Adicionou a variável global [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) para usar com webhooks do Commerce para se conectar a um ponto de extremidade externo, como a ação de tempo de execução do App Builder ou um sistema de gerenciamento de inventário de terceiros.

## v2002.1.15

Data de lançamento: 31 de julho de 2023

- ![ícone de correção](../../assets/fix.svg) **Códigos de erro**—Esquema de código de erro atualizado e gerador de documento de código de erro.
- ![Ícone de correção](../../assets/fix.svg) **Validador do modelo Redis personalizado** - Atualizado o validador dos modelos back-end Redis personalizados. [Consulte o exemplo para a configuração de cache](../environment/variables-deploy.md#cache_configuration).
- ![Ícone de correção](../../assets/fix.svg) **Validador para RabbitMQ** - suporte adicionado para RabbitMQ 3.11
- ![ícone de correção](../../assets/fix.svg) **Corrigido o link errado**-Corrigido o link errado para a documentação de integração no modelo de email de boas-vindas.

## v2002.1.14

Data de lançamento: 10 de março de 2023

- ![novo ícone](../../assets/new.svg) **PHP**—Suporte adicionado para o PHP 8.2.
- ![novo ícone](../../assets/new.svg) **Validadores para Serviços**—Os validadores para os serviços necessários do Commerce 2.4.6 foram atualizados: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x e RabbitMQ 3.9.
- ![ícone de correção](../../assets/fix.svg) **ece-tools-db-dump**—Corrigiu um problema que fazia com que a operação `db-dump` parasse prematuramente.

## v2002.1.13

Data de lançamento: 27 de outubro de 2022

- ![novo ícone](../../assets/new.svg) **Suporte adicionado para Adobe I/O Events para Adobe Commerce**. Os desenvolvedores de extensão agora podem usar a estrutura [Adobe I/O Events](https://developer.adobe.com/events/docs/) para enviar informações de eventos Commerce de instâncias da Nuvem para seus aplicativos, gravadas para o [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/). O Adobe I/O Events para Adobe Commerce está em Visualização de Parceiro.<!-- CEXT-932 -->
- ![novo ícone](../../assets/new.svg) **Validador para configuração do OPcache**—Adicionou um validador para verificar a configuração do OPcache para caminhos excluídos.<!-- MCLOUD-9485 -->
- ![ícone de correção](../../assets/fix.svg) **Correção de um problema com a configuração de cache do GraphQL**—Agora o ECE-Tools mantém o valor `id_salt` do GraphQL na configuração `cache` no arquivo `app/etc/env.php`.<!-- MCLOUD-9486 -->

## v2002.1.12

Data de lançamento: 13 de setembro de 2022

- ![novo ícone](../../assets/new.svg) **Habilitar`synchronous_replication`**—ECE-Tools define `synchronous_replication=>true` no arquivo `app/etc/env.php` quando `MYSQL_USE_SLAVE_CONNECTION` está habilitado. Essa configuração afeta somente o Commerce 2.4.6+. Consulte a descrição da variável `MYSQL_USE_SLAVE_CONNECTION` em [Implantar variáveis](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![novo ícone](../../assets/new.svg) **OpenSearch** — Adição de funcionalidade para configurar e definir o mecanismo `opensearch` para a próxima versão 2.4.6 do Adobe Commerce. Consulte [Configurar o serviço OpenSearch](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Data de lançamento: 4 de agosto de 2022

- ![ícone de correção](../../assets/fix.svg) **ElasticSuite Validator e OpenSearch**—Corrigiu o problema do validador de verificação de integridade do ElasticSuite quando OpenSearch está instalado.<!-- MCLOUD-8767 -->
- ![Ícone de correção](../../assets/fix.svg) **Tipos de retorno para comandos de implantação**—Tipos de retorno corrigidos para comandos de implantação.<!-- AC-3208 -->
- ![ícone de correção](../../assets/fix.svg) **[!DNL RabbitMQ]problema com a nova instalação do Commerce 2.4.5**—Corrigido [!DNL RabbitMQ] problema de falha na nova instalação do Commerce 2.4.5.<!-- MCLOUD-9059 -->

## v2002.1.10

Data de lançamento: 31 de março de 2022

- ![ícone de correção](../../assets/fix.svg) **Elasticsearch 7.10**—Os validadores foram atualizados para suportar a versão 7.10 do Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Data de lançamento: 10 de março de 2022

- ![novo ícone](../../assets/new.svg) **OpenSearch**—Adicionou suporte ao OpenSearch para as versões 2.4.4, 2.4.3-p2 e 2.3.7-p3 do Adobe Commerce.<!-- MCLOUD-8296 -->
- ![novo ícone](../../assets/new.svg) **PHP**—Suporte adicionado para o PHP 8.1.
- ![ícone de correção](../../assets/fix.svg) **symfony/process**—Adicionou a compatibilidade com o symfony/process ^5.3.<!-- MCLOUD-8283 -->

- ![novo ícone](../../assets/new.svg) **Vários processos do consumidor**—Adicionou uma opção `multiple_processes` para que você possa especificar o número de processos a serem gerados para cada consumidor. Consulte a descrição da variável `CRON_CONSUMERS_RUNNER` em [Implantar variáveis](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![novo ícone](../../assets/new.svg) **Esquema OpenSearch e caminho completo do host**—Adicionou a capacidade de configurar um esquema Elasticsearch e um caminho completo do host.
- ![ícone de correção](../../assets/fix.svg) **AWS S3**—Alterou o método de habilitação do AWS S3.
- ![Ícone de correção](../../assets/fix.svg) **Corrigir leitor de opções_do_driver**—Adição da leitura da configuração de opções_do_driver da conexão do banco de dados do arquivo `env.php` por `ece-tools` para validadores.<!-- MCLOUD-8420 -->

## v2002.1.8

Data de lançamento: 25 de outubro de 2021

- ![novo ícone](../../assets/new.svg) **Local de despejo alternativo**—Adicionou a opção `--dump-directory` para que você possa escolher um diretório de destino para um despejo de BD. Agora `/app/var/dump-main` é o diretório de destino padrão para um despejo de BD. Consulte [Gerenciamento de backup: Despejar seu banco de dados](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![Ícone de correção](../../assets/fix.svg) **Atualizar Monólogo**—Atualizou a versão mínima necessária para o pacote `monolog` para `^2.3`.<!-- ACMP-1263 -->
- ![Ícone de correção](../../assets/fix.svg) **Atualizar Symfony**—Atualizou as dependências do Symfony para serem compatíveis com o Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![ícone de correção](../../assets/fix.svg) **Recurso/resolver carregamento automático**—Corrigiu um problema ao implantar em um ambiente de integração e ao ver o erro `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Data de lançamento: 29 de julho de 2021

**Atualizações de configuração**—

- ![novo ícone](../../assets/new.svg) Adicionado suporte para o Composer 2.0.<!--MCLOUD-8003-->

- ![Ícone de correção](../../assets/fix.svg) **Requisitos do compositor atualizados para`symphony/console`**—Atualizou os requisitos de versão ECE-Tools `composer.json` para o pacote `symphony/console` para corrigir um problema que causava a falha dos comandos `di:compile` com o seguinte erro: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![ícone de correção](../../assets/fix.svg) atualizou as verificações de software do fim da vida útil (`eol.yaml`) para incluir o Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Data de lançamento: 20 de abril de 2021

- ![novo ícone](../../assets/new.svg) **Credenciais de autenticação Redis**—Adicionou a capacidade de ler credenciais de autorização Redis da propriedade `relationships` durante a fase de implantação.<!--MCLOUD-7694-->

- ![novo ícone](../../assets/new.svg) **credenciais de autorização do Elasticsearch**—Adicionou a capacidade de ler credenciais de autorização do Elasticsearch da propriedade `relationships` durante a fase de implantação.<!--MCLOUD-7695-->

- ![Novo ícone](../../assets/new.svg) **Serviço de armazenamento de sessão dedicado**—Adicionou `redis-session` como segunda opção para armazenamento de sessão. Você pode usar o serviço `redis-session` para armazenar informações de sessão e usar o serviço `redis` para cache a fim de fornecer melhor desempenho.<!--MCLOUD-7698-->

- ![novo ícone](../../assets/new.svg) **Mensagens SPLIT_DB obsoletas**—Adicionou aviso de validador e mensagens críticas para a opção `SPLIT_DB` obsoleta para o Adobe Commerce 2.4.2 e sua remoção no Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![ícone de correção](../../assets/fix.svg) **versão do Elasticsearch de relações**—Validador do Fixed Service para recuperar a versão correta do Elasticsearch das propriedades `relationships` no Cloud Docker e em ambientes de integração.<!--MCLOUD-7572-->

- ![ícone de correção](../../assets/fix.svg) **Validação flexível da porta Redis**—O Redis agora pode validar a porta em uma conexão de cache personalizada a partir da URL `server`. Por exemplo, você pode adicionar o número da porta ao URL do servidor da seguinte maneira: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Isso ajuda a evitar erros de validação em que a opção `port` está ausente ou incorreta.<!--MCLOUD-7722-->

- ![ícone de correção](../../assets/fix.svg) **Atualização para o Adobe Commerce 2.4.2**—Correção de um problema que fazia com que os usuários executassem manualmente o `bin/magento setup:upgrade` para tornar seus sites operacionais após a atualização para o Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Data de lançamento: 1 de fevereiro de 2021

- ![novo ícone](../../assets/new.svg) **Armazenamento remoto**—Adicionou a variável de ambiente `REMOTE_STORAGE` para habilitar Projetos na Nuvem para armazenamento remoto de arquivos de mídia usando um serviço de armazenamento, como o AWS S3. Esta opção de configuração faz parte do pacote ECE-Tools, mas não tem suporte no Adobe Commerce na infraestrutura de nuvem.<!--MCLOUD-7153-->

- ![novo ícone](../../assets/new.svg) **Novo comando `cloud:config:validate`**—Comando adicionado `php vendor/bin/ece-tools cloud:config:validate` para validar a configuração `.magento.env.yaml` antes de enviar alterações para o ambiente de Nuvem remoto.<!--MCLOUD-7120-->

- ![novo ícone](../../assets/new.svg) **Liberando o opcache**—Adicionou suporte para a opção PHP `opcache.enable_cli` para liberar o OPcache antes de executar o gancho de implantação. Essa configuração redefine a configuração do cache para garantir que as definições de configuração atuais sejam aplicadas em cada implantação.<!--MCLOUD-7015-->

- ![novo ícone](../../assets/new.svg) **Validação do BD Aurora**—Atualizou a validação do serviço de banco de dados para que seja compatível com o banco de dados Aurora.<!--MCLOUD-7269-->

- ![novo ícone](../../assets/new.svg) **Nova variável de ambiente SCD_NO_PARENT** — Adicionada a variável de ambiente `SCD_NO_PARENT` (para Adobe Commerce >=2.4.2) para gerenciar a geração de conteúdo estático para temas pai.<!--MCLOUD-7284-->

- ![ícone de correção](../../assets/fix.svg) **Limites e comandos de memória**—Correção de um problema em que os comandos `php vendor/bin/ece-tools` não funcionariam se o tamanho do arquivo `cloud.log` excedesse o limite de memória do PHP. Em vez de ler todo o arquivo `cloud.log` na memória, agora lemos apenas um subconjunto menor de dados do arquivo de log.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![Ícone de correção](../../assets/fix.svg) **Conexões de banco de dados personalizadas**—Corrigido um problema de configuração `.magento.env.yaml` no qual as conexões de banco de dados personalizadas definidas para `DATABASE_CONFIGURATION` não eram usadas. As configurações de conexão não foram adicionadas a `app/etc/env.php`.<!--MCLOUD-7426-->

- ![ícone de correção](../../assets/fix.svg) **Logs de erros vazios**—Correção de um problema que causava a falha das implantações se `cloud.error.log` estivesse vazio.<!--MCLOUD-7296-->

- ![ícone de correção](../../assets/fix.svg) **validação de MariaDB 10.3**—Validação de MariaDB 10.3 corrigida para Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![ícone de correção](../../assets/fix.svg) **Cache:flush registrando em log**—Entradas de log aprimoradas para indicar o início e término da etapa `cache:flush`.<!--MCLOUD-7503-->

## v2002.1.4

Data de lançamento: 19 de novembro de 2020

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava falha na implantação quando o mecanismo de pesquisa especificado na variável de ambiente `SEARCH_CONFIGURATION` é um valor diferente de `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Data de lançamento: 9 de novembro de 2020

**Atualizações de infraestrutura**—

- ![novo ícone](../../assets/new.svg) Adicionado suporte a ECE-Tools para o diretório `pub/static` somente leitura quando o conteúdo estático está definido para implantação no estágio de compilação.<!--MC-37699-->

- ![novo ícone](../../assets/new.svg) Adição de suporte para Elasticsearch 7.9 e Redis 6 para compatibilidade com versões futuras do Adobe Commerce.<!--MCLOUD-7191-->

- ![ícone de correção](../../assets/fix.svg) Atualizado o ECE-Tools `composer.json` para adicionar uma dependência necessária para a Ferramenta de correções de qualidade. Isso corrige uma dependência circular que existia entre os pacotes ECE-Tools e magento-cloud-patches.<!--MCLOUD-6910-->

**Melhorias na validação e no log**—

- ![novo ícone](../../assets/new.svg) Validação de mecanismo de pesquisa adicionada para garantir que `elasticsearch` esteja definido para o Adobe Commerce na infraestrutura de nuvem 2.4 e posterior. Se a validação falhar, a implantação será interrompida com uma mensagem de erro crítica que sugere correções para o problema. Consulte [Erros Críticos, Estágio de Implantação](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![novo ícone](../../assets/new.svg) Adição da validação do Elasticsearch para verificar a compatibilidade entre a versão do serviço Elasticsearch e a versão do Adobe Commerce.<!--MCLOUD-7193-->

- ![novo ícone](../../assets/new.svg) Atualizou a mensagem de erro de compatibilidade do Elasticsearch para mostrar as versões do Elasticsearch compatíveis com o módulo Adobe Commerce Elasticsearch. A mensagem de erro agora fornece as versões específicas do Elasticsearch a serem instaladas na infraestrutura em nuvem para que sejam compatíveis com o módulo do Elasticsearch usado por sua versão do Adobe Commerce. Consulte [Erros de Aviso, Estágio de Implantação](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![novo ícone](../../assets/new.svg) adicionou os erros de aviso `2026` e `2027` para a configuração de variável de ambiente `MAGE_MODE` inválida. O único valor válido é `production`. Antes desta correção, `MAGE_MODE` poderia ser definido como `developer` sem erros de implantação, apenas para causar erros mais tarde ao tentar gravar em arquivos somente leitura. Consulte [Erros de Aviso](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![Ícone de correção](../../assets/fix.svg) Corrigida a validação de serviços Redis, RabbitMQ e MySQL para garantir que essas versões sejam compatíveis com a versão do Adobe Commerce. Versões válidas desses serviços agora são gravadas em `cloud.log`.<!--MCLOUD-7098-->

- ![ícone de correção](../../assets/fix.svg) Atualizou o `cloud.log` para incluir o limite de solicitações simultâneas para envio de solicitações durante o aquecimento do cache. Este valor está configurado na variável pós-implantação [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency).<!--MCLOUD-5563-->

**Atualizações de comando CLI**—

- O ![novo ícone](../../assets/new.svg) adicionou comandos CLI (`cloud:config:create` e `cloud:config:update`) para criar e atualizar o arquivo `.magento.env.yaml` com uma configuração que pode incluir uma ou mais variáveis de compilação, implantação e pós-implantação. Consulte [Criar arquivo de configuração da CLI](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Atualizações da variável de ambiente**—

- ![novo ícone](../../assets/new.svg) Adicionado a variável de compilação [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload). Definir a variável como `true` impede que o aplicativo execute o comando `composer dump-autoload` durante a instalação do Cloud Docker para Commerce. A variável só é relevante para o Cloud Docker de contêineres do Commerce com sistemas de arquivos graváveis (criados para teste e desenvolvimento usando `./vendor/bin/ece-docker build:compose --with-test`). Com essas instalações, ignorar o comando `composer dump-autoload` evita erros ao executar outros comandos que tentam acessar arquivos de um diretório `generated` excluído.<!--MCLOUD-6939-->

## v2002.1.2

Data de lançamento: 5 de agosto de 2020

**Melhorias na validação e no log**—

- ![novo ícone](../../assets/new.svg) Adicionado o arquivo `schema.error.yaml` que inclui todas as notificações de erro e aviso que podem ocorrer durante o processo de compilação, implantação e pós-implantação, juntamente com sugestões para resolver os erros. As informações neste arquivo também estão disponíveis no _Guia da Nuvem para o Commerce_. Consulte [Referência de mensagem de erro para ece-tools](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![novo ícone](../../assets/new.svg) alterou as entradas do log de erros da nuvem (`/var/log/cloud.error.log`) para o formato JSON para facilitar a análise programática do log.<!--MCLOUD-5879-->

- ![novo ícone](../../assets/new.svg) Adição de verificações de erro adicionais para compilar, implantar e pós-implantar o processamento e verificações existentes aprimoradas:

   - Código de erro 2026 — Falha ao restaurar alguns dados gerados durante a fase de compilação para os diretórios montados

   - Código de erro 3004 — Não é possível criar arquivos de backup

   - Código de erro 102 — Adição de verificações adicionais para problemas que ocorrem quando o arquivo `env.php` não é gravável <!--MCLOUD-6221-->

- ![novo ícone](../../assets/new.svg) Adicionado a variável de ambiente **QUALITY_PATCHES** para especificar um ou mais patches de qualidade a serem aplicados durante o processo de implantação. Consulte [Criar variáveis](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Data de lançamento: 25 de junho de 2020

- ![novo ícone](../../assets/new.svg) **Atualizações de infraestrutura**—

   - ![novo ícone](../../assets/new.svg) **Melhorias no log**—Recurso de rastreamento de log aprimorado ao atribuir códigos de saída a erros críticos de implantação e ao expor os códigos de saída em notificações de mensagens de erro e eventos de log. Consulte [Referência de mensagem de erro para ece-tools](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![novo ícone](../../assets/new.svg) Melhoria do processo para despejos de banco de dados (`vendor/bin/ece-tools db-dump`) e atualização de mensagens de log para esclarecer que a operação de despejo de banco de dados alterna o aplicativo para o modo de manutenção, interrompe os processos de fila do consumidor e desabilita os trabalhos cron antes do início do despejo.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema para garantir que a URL do projeto seja atualizada corretamente ao implantar em ambientes de Preparo e Produção. Agora, `ece-tools` usa a URL para a rota com o atributo `primary:true` definido na configuração de rota do projeto. Consulte [Implantar variáveis](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![ícone de correção](../../assets/fix.svg) atualizou o fluxo de trabalho do cenário de compilação `generate.xml` para aplicar patches. Os patches devem ser aplicados anteriormente para atualizar o Adobe Commerce para corrigir qualquer problema que possa causar falha nas etapas `di:compile` e `module:refresh`.<!--MCLOUD-5941-->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema no processo de instalação que retornava incorretamente o erro `Crypt key missing`. O valor `crypt/key` é gerado automaticamente durante a instalação.<!--MCLOUD-6120-->

- ![novo ícone](../../assets/new.svg) **Atualizações de serviço**—

   - ![novo ícone](../../assets/new.svg) Adicionado suporte para PHP 7.4 e MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![novo ícone](../../assets/new.svg) **Atualizações de variáveis de ambiente**—

   - ![novo ícone](../../assets/new.svg) Adicionada a variável **SCD_USE_BALER** para habilitar o módulo Baler para agrupamento de JavaScript durante o processo de compilação da infraestrutura de nuvem do Adobe Commerce. Consulte a descrição da variável em [variáveis de compilação](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![novo ícone](../../assets/new.svg) Adicionado a variável de ambiente **REDIS_BACKEND** para configurar o modelo de back-end Redis para o cache Redis para Adobe Commerce 2.3.5 ou posterior. Consulte a descrição da variável em [implantar variáveis](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![novo ícone](../../assets/new.svg) **atualizações de comando CLI**—

   - ![novo ícone](../../assets/new.svg) Atualizou os seguintes comandos CLI com uma opção para log mais detalhado:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     O nível de log para cada chamada é determinado pela configuração da variável [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) no arquivo `.magento.env.yaml`.<!--MCLOUD-3503-->

- ![novo ícone](../../assets/new.svg) **Melhorias na validação**—

   - ![novo ícone](../../assets/new.svg) **verificações de compatibilidade do Elasticsearch 7.x**—Validação do Elasticsearch atualizada para verificações de compatibilidade de software do Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![novo ícone](../../assets/new.svg) **Verificações de versão de serviço e validação de EOL atualizadas**—Validação atualizada para verificar as versões de serviço instaladas em relação aos requisitos do Adobe Commerce 2.4.<!--MCLOUD-6144-->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema de validação para que a seguinte mensagem de aviso de pós-implantação fosse exibida somente se a configuração de gancho `post-deploy` estivesse ausente do arquivo `.magento.app.yaml`:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![novo ícone](../../assets/new.svg) **Adição da validação para dependências do Zend Framework**—Adição da validação de dependência de compositor para o Zend Framework que migrou para o projeto Laminas. Se as dependências necessárias estiverem ausentes, a seguinte mensagem de erro será exibida durante o processo de criação.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Consulte [Verificar dependências do Zend Framework](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![novo ícone](../../assets/new.svg) **Adicionada validação para `env.php` arquivos e dados**—Adicionadas verificações para o `env.php` arquivo e dados durante o processo de instalação e atualização.<!--MCLOUD-5991-->

      - Se o arquivo `env.php` estiver ausente na instalação e o valor `crypt/key` não estiver especificado no arquivo `.magento.app.yaml`, a implantação falhará com a seguinte notificação:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Se a instalação não incluir o arquivo `env.php` ou a configuração contiver apenas um tipo de cache, o comando `cron:enable` será executado durante o processo de atualização para restaurar o arquivo com todos os `cache_types`. A notificação a seguir é adicionada ao log:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Data de lançamento: 6 de fevereiro de 2020

- ![novo ícone](../../assets/new.svg) **Atualizações de infraestrutura**—

   - ![novo ícone](../../assets/new.svg) **Adição de um pacote separado do Cloud Docker para Commerce**—Dissociação entre o pacote do Docker e o pacote `ece-tools` para manter a qualidade do código e fornecer versões independentes. As atualizações e correções relacionadas a `ece-tools` são gerenciadas no [repositório GitHub da magento-cloud-docker](https://github.com/magento/magento-cloud-docker).<!--MAGECLOUD-2927-->

   - ![novo ícone](../../assets/new.svg) **Atualização dos recursos de patch**—A funcionalidade de patch do pacote ECE-Tools foi movida para um pacote [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) separado. Durante a implantação, `ece-tools` usa o novo pacote para aplicar patches. Consulte as [notas de versão de patches de nuvem](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![novo ícone](../../assets/new.svg) **Dependências atualizadas do Composer**—Atualizou o arquivo `composer.json` do Adobe Commerce na infraestrutura de nuvem com uma dependência para o pacote `magento/magento-cloud-docker`. Agora, `ece-tools` inclui dependências para todos os pacotes em [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Esses pacotes são instalados e atualizados automaticamente quando você instala ou atualiza o `ece-tools`.

- ![novo ícone](../../assets/new.svg) **Suporte para implantações baseadas em cenário**—<!--MAGECLOUD-4101-->

   - ![novo ícone](../../assets/new.svg) Agora é possível personalizar os processos de compilação, implantação e pós-implantação usando arquivos de configuração XML para substituir ou personalizar a configuração padrão.

   - ![novo ícone](../../assets/new.svg) **Alterou a configuração `hooks` em`.magento.app.yaml`**—Atualizamos o formato de configuração `hooks` para oferecer suporte a implantações baseadas em cenário. O formato herdado da versão anterior das ECE-Tools 2002.0.x ainda é suportado. No entanto, é necessário atualizar para o novo formato para usar o recurso de implantação baseada em cenário. Consulte [Implantações baseadas em cenário](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Antes de atualizar para a versão 2002.1.0 das Ferramentas ECE, reveja o [retroativo   alterações incompatíveis](backward-incompatible-changes.md) para saber mais sobre as alterações que podem exigir   atualizar a configuração ou os processos do projeto Adobe Commerce on cloud infrastructure.

- ![novo ícone](../../assets/new.svg) **Atualizações de serviço**—

   - ![novo ícone](../../assets/new.svg) Suporte adicionado para o PHP 7.3.<!--MAGECLOUD-4022-->

   - ![novo ícone](../../assets/new.svg) Adicionado suporte para RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![novo ícone](../../assets/new.svg) Validação adicionada para verificar as versões de serviço instaladas em relação à data de EOL de cada serviço. Agora, os clientes recebem uma notificação se uma versão do serviço estiver dentro de três meses da data EOL, e um aviso se a data EOL estiver no passado.<!--MAGECLOUD-4076-->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema de configuração do Elasticsearch para garantir que as configurações corretas do Elasticsearch sejam definidas em todos os ambientes.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consulte [Versões de serviço](../services/services-yaml.md#service-versions) para obter uma lista de serviços usados no Adobe Commerce na infraestrutura em nuvem e a compatibilidade de suas versões com o modelo da Nuvem.

- ![novo ícone](../../assets/new.svg) **Atualizações de variáveis de ambiente**—

   - ![novo ícone](../../assets/new.svg) Estendeu a funcionalidade da variável de ambiente `WARM_UP_PAGES` para oferecer suporte ao pré-carregamento de cache para páginas de produto específicas. Consulte a definição expandida no tópico [variáveis pós-implantação](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-4444-->

   - ![novo ícone](../../assets/new.svg) Adicionado a variável de ambiente `ERROR_REPORT_DIR_NESTING_LEVEL` para simplificar o gerenciamento de dados do relatório de erros no diretório `<magento_root>/var/report/`. Consulte a descrição da variável no tópico [variáveis de compilação](../environment/variables-build.md#error_report_dir_nesting_level).

   - ![ícone de correção](../../assets/fix.svg) Removido as variáveis de ambiente `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT` e `STATIC_CONTENT_SYMLINK`. Ver [Alterações incompatíveis com versões anteriores](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema no processo de configuração do Conjunto Elástico para que a configuração padrão fosse substituída conforme esperado ao configurar a variável de implantação `ELASTICSUITE_CONFIGURATION` sem a opção `_merge`.<!--MAGECLOUD-4388-->

- ![novo ícone](../../assets/new.svg) **atualizações de comando CLI**—

   - ![novo ícone](../../assets/new.svg) **Novo comando cron**—Agora é possível gerenciar manualmente o processamento de cron no ambiente do Adobe Commerce na infraestrutura de nuvem usando os comandos `cron:disable` e `cron:enable`. Use o comando disable para interromper todos os processos cron ativos e desativar todos os trabalhos cron. Use o comando enable para reativar os processos do cron quando estiver pronto. Consulte [Desabilitar trabalhos cron](../application/crons-property.md#disable-cron-jobs).

   - ![novo ícone](../../assets/new.svg) **Melhoria no relatório de erros**—Adição de um melhor registro para falhas de comando CLI que ocorrem durante o processamento de ECE-Tools.<!--MAGECLOUD-4849-->

   - ![novo ícone](../../assets/new.svg) **Remover comandos de compilação obsoletos**— Os seguintes comandos de compilação foram removidos: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump` e `ece-tools docker` renomeados para `ece-docker`. Ver [Alterações incompatíveis com versões anteriores](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![novo ícone](../../assets/new.svg) Removido o arquivo `build_options.ini` obsoleto e adicionada validação para falhar a compilação se o arquivo existir. Use o arquivo [.magento.env.yaml](../environment/configure-env-yaml.md) para configurar as opções de compilação.

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a falha do processo de compilação quando o arquivo `config.php` estava vazio.<!--MAGECLOUD-4127-->

## 2002.0.23

Data de lançamento: 27 de fevereiro de 2020

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema de compatibilidade com as versões do `ece-tools` 2002.0.x que impedia que a geração de conteúdo estático sob demanda fosse concluída com êxito no modo de produção.

## Versões anteriores

Consulte o [arquivo de notas de versão](cloud-release-archive.md) da versão 2002.0.22 e anteriores.
