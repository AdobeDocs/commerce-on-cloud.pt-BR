---
title: Arquivo de notas de versão para ece-tools
description: Saiba mais sobre as melhorias arquivadas para as ferramentas ece.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# Arquivo de notas de versão para ece-tools

>[!NOTE]
>
>Estas notas de versão fornecem informações e atualizações para o `ece-tools` v2002.0.22 e posterior. Consulte as [Notas de versão do Conjunto de Ferramentas da Nuvem](cloud-tools-suite.md) para obter as atualizações mais recentes para o `ece-tools` e outros pacotes da Nuvem.

## v2002.0.22

A versão 2002.0.22 do `ece-tools` altera a estrutura do pacote `ece-tools` para dissociar a versão dos patches `Adobe Commerce on cloud infrastructure` da versão ECE-Tools. A partir desta versão, patches e correções críticas serão entregues usando o pacote [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), que é uma nova dependência para o pacote `ece-tools`. Fizemos essas alterações para reduzir a complexidade e agendar atualizações de versão e trabalhar com contribuições da comunidade.

- ![novo ícone](../../assets/new.svg) **Alterações no pacote ECE-Tools**

   - ![novo ícone](../../assets/new.svg) Movido os patches do Adobe Commerce do pacote `ece-tools` para um novo pacote do compositor [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches).

   - ![novo ícone](../../assets/new.svg) Atualizou o arquivo `composer.json` do pacote `ece-tools` para adicionar uma dependência do pacote `magento/magento-cloud-patches` v1.0.0.

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a quebra do processo de correção `ece-tools` ao aplicar conjuntos de correção sobre versões somente de segurança, começando com a versão 2.3.2-p2 e posterior. Esse problema foi introduzido pelo novo esquema de controle de versão adotado para [patches somente de segurança](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview).<!--MAGECLOUD-4661-->

- ![Ícone de correção](../../assets/fix.svg) **Correções críticas e de patches**-Atualize seus ambientes de Nuvem com a versão `ece-tools` 2002.0.22 para aplicar os seguintes patches e correções críticas. Esses patches estão incluídos no pacote `magento/magento-cloud-patches` v1.0.0.

   - ![ícone de correção](../../assets/fix.svg) **Patches de segurança do Page Builder para as versões 2.3.1.x e 2.3.2.x**-Corrige um problema na visualização do Page Builder que permite que usuários não autenticados acessem alguns métodos de modelo que podem ser usados para disparar a execução arbitrária de código pela rede (RCE), resultando em vazamentos de informações globais. Esse problema pode ocorrer ao usar versões não compatíveis do Page Builder com as versões 2.3.1 e 2.3.2 do Adobe Commerce.<!--MAGECLOUD-4649-->

   - ![ícone de correção](../../assets/fix.svg) **Patches MSI** - Corrige problemas que causavam erros de indexação e problemas de desempenho ao usar configurações de inventário padrão para gerenciar estoque.<!--MAGECLOUD-4428-->

   - ![Ícone de correção](../../assets/fix.svg) **Compatibilidade com versões anteriores de novas Interfaces de Email**-Corrige um problema de incompatibilidade com versões anteriores causado pela interface do PHP `Magento\Framework\Mail\EmailMessageInterface` introduzida no Adobe Commerce v2.3.3. No escopo deste patch, o novo `EmailMessageInterface` herda do `MessageInterface` antigo, e os módulos principais do Adobe Commerce são revertidos para depender do `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![Ícone de correção](../../assets/fix.svg) **A paginação de catálogo não funciona no Elasticsearch 6.x**-Corrige um problema crítico na paginação de resultados de pesquisa que afeta clientes que usam o Elasticsearch 6.x como mecanismo de pesquisa de catálogo.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![novo ícone](../../assets/new.svg) **Atualizações do Docker**—

   - ![novo ícone](../../assets/new.svg) **Novas Imagens do Docker** — com suporte nas versões 2.3.3 e posteriores<!-- MAGECLOUD-3345 -->

      - PHP versão 7.3.<!-- MAGECLOUD-4017 -->

      - Cache de verniz 6.2.0<!-- MAGECLOUD-4017 -->

   - ![novo ícone](../../assets/new.svg) Adicionado suporte para aplicar a configuração de gancho personalizado especificada em `.magento.app.yaml` no ambiente Docker. Anteriormente, o ambiente Docker suportava somente a configuração de gancho padrão.<!-- MAGECLOUD-3505-->

   - ![novo ícone](../../assets/new.svg) Os arquivos ENV do Docker não são mais gerados durante a compilação do Docker, e o comando `docker:config:convert` está obsoleto. Os dados correspondentes agora estão armazenados no arquivo `docker-compose.yml`.<!-- MAGECLOUD-3816-->

   - ![novo ícone](../../assets/new.svg) **Atualização da imagem do PHP**-Adição do Node.js à imagem do PHP Docker para oferecer suporte aos recursos node, npm e grunt-cli.<!-- MAGECLOUD-3953 -->

- ![novo ícone](../../assets/new.svg) **Atualizações da variável de ambiente**-

   - ![novo ícone](../../assets/new.svg) Adicionada a variável de implantação **LOCK_PROVIDER** para configurar o provedor de bloqueio, o que impede a inicialização de trabalhos cron duplicados e grupos cron. Consulte a descrição da variável no tópico [implantar variáveis](../environment/variables-deploy.md#lock_provider).<!-- MAGECLOUD-4052 -->

   - ![novo ícone](../../assets/new.svg) adicionada a variável de ambiente **CONSUMERS_WAIT_FOR_MAX_MESSAGES** para configurar como os consumidores processam mensagens da fila de mensagens ao usar a variável de ambiente `CRON_CONSUMERS_RUNNER` para gerenciar trabalhos cron. Consulte a descrição da variável no tópico [implantar variáveis](../environment/variables-deploy.md#consumers_wait_for_max_messages).<!-- MAGECLOUD-4071 -->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema que pode causar erros de deadlock do banco de dados quando o trabalho de cron `consumers_runner` inicia várias instâncias do mesmo consumidor em nós diferentes. Agora, se você habilitou a variável de implantação [**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) em seu ambiente, o trabalho `consumers_runner` usa a opção `single-thread` para iniciar uma instância de cada consumidor em apenas um nó.<!-- MAGECLOUD-3913 -->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema que afetava a funcionalidade [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) que usa uma URL de repositório padrão. Agora, se o comando `config:show:default-url` não puder buscar uma URL base, a URL da variável MAGENTO_CLOUD_ROUTES será usada.<!-- MAGECLOUD-3866 -->

- ![novo ícone](../../assets/new.svg) Atualizou as informações de log retornadas pelo comando `module:refresh`. Agora você pode ver uma lista detalhada de módulos habilitados no arquivo `cloud.log`.<!-- MAGECLOUD-2514 -->

- ![novo ícone](../../assets/new.svg) Notificações de aviso e validação de compatibilidade de versão aprimoradas para problemas de compatibilidade entre a versão do Adobe Commerce e os serviços instalados, como Elasticsearch, [!DNL RabbitMQ], Redis e DB.<!-- MAGECLOUD-3535 -->

- ![novo ícone](../../assets/new.svg) Adicionado suporte para RabitMQ versão 3.8.<!-- MAGECLOUD-4674-->

- ![novo ícone](../../assets/new.svg) Validações interativas atualizadas para compatibilidade de serviço a fim de refletir as versões com suporte para as novas versões do Adobe Commerce 2.3.3 e 2.2.10. Consulte [Requisitos do sistema](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) no _Guia de instalação_ para obter as versões recomendadas.<!-- MAGECLOUD-4018 -->

- ![ícone de correção](../../assets/fix.svg) Melhoria na mensagem de log retornada quando o processo de gerenciamento de trabalhos cron na fase de implantação tenta parar um trabalho cron que já foi concluído para esclarecer que esse problema não é um erro. Alterado o nível de log de `INFO` para `DEBUG`.<!-- MAGECLOUD-3653-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema ao executar o comando `setup:upgrade` que não interrompeu o processo de implantação quando ocorreu uma falha durante a tarefa `app:config:import`.<!-- MAGECLOUD-3806 -->

- ![novo ícone](../../assets/new.svg) Alterado o nível de log padrão do manipulador de arquivos para `debug` para reduzir a quantidade de detalhes no log exibido em [!DNL Cloud Console], ao mesmo tempo em que fornece informações detalhadas para depuração.<!-- MAGECLOUD-3871 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava um erro na implantação de conteúdo estático durante a compilação. Após uma instalação e o despejo de configuração `ece-tools`, ocorreu um erro se não houvesse localidades especificadas para o usuário administrador no arquivo `config.php`. Agora, há uma localidade padrão para o usuário administrador no arquivo `config.php`.<!-- MAGECLOUD-3957 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um `Undefined index error` que ocorre quando um comando da CLI `magento-cloud` falha em um ambiente não configurado com uma URL segura (https). Agora, o pacote ECE-Tools usa a URL base (http) se a URL segura não estiver disponível.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![novo ícone](../../assets/new.svg) **Atualizações do Docker**—

   - ![novo ícone](../../assets/new.svg) Agora é possível executar o teste funcional usando o pacote `ece-tools` no ambiente Docker. Consulte [teste de aplicativo](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/).<!-- MAGECLOUD-3129/3684 -->

   - ![novo ícone](../../assets/new.svg) Suporte adicionado para configurar módulos PHP usando o arquivo `.magento.app.yaml`. Quaisquer [Extensões PHP especificadas no `.magento.app.yaml` arquivo](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions) ficam disponíveis nos contêineres PHP Docker.<!-- MAGECLOUD-3357 -->

   - ![novo ícone](../../assets/new.svg) Há novos comandos disponíveis para melhorar a experiência da linha de comando do Docker. Consulte a seção [`bin/magento-docker` da referência de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![novo ícone](../../assets/new.svg) Adicionada a capacidade de usar Mutagen.io para sincronizar arquivos durante o desenvolvimento entre o host local e o Docker.<!-- MAGECLOUD-3559 -->

   - ![ícone de correção](../../assets/fix.svg) Corrigido o caminho padrão ao usar o ambiente Docker. Agora, ao usar o SSH para fazer logon no contêiner Docker, você estará na raiz do projeto no diretório `/app`, conforme esperado.<!-- MAGECLOUD-3582 -->

   - ![ícone de correção](../../assets/fix.svg) Atualizou a biblioteca do Sodium da versão 1.0.11 para a versão 1.0.18 e atualizou a extensão PHP do Sodium.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Os clientes do Adobe Commerce na infraestrutura em nuvem devem [enviar um tíquete de Suporte da Adobe Commerce](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) para atualizar o pacote libsódio em ambientes de Produção e Preparo Pro antes de atualizar para o Adobe Commerce 2.3.2. Atualmente, não é possível atualizar ambientes do Starter para o Adobe Commerce 2.3.2.

   - ![ícone de correção](../../assets/fix.svg) Adicionado os plug-ins de Elasticsearch `analysis-icu` e `analysis-phonetic` a todas as imagens do Docker.<!-- MAGECLOUD-3446 -->

   - ![ícone de correção](../../assets/fix.svg) Validações aprimoradas: ao usar opções para o comando `docker:build`, você deve fornecer um valor ao usar uma opção. Além disso, foi adicionada a validação para a versão do Nó ao usar o comando `docker:build run`.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![novo ícone](../../assets/new.svg) **Atualizações de variáveis de ambiente**—

   - ![novo ícone](../../assets/new.svg) Adicionado suporte para prefixos de tabela de banco de dados usando a variável de ambiente [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![novo ícone](../../assets/new.svg) Adicionado a variável de implantação **FORCE_UPDATE_URLS** para atualizar URLs base ao implantar em ambientes de produção e preparo Pro e Starter. Consulte a definição no conteúdo [implantar variáveis](../environment/variables-deploy.md#force_update_urls).<!-- MAGECLOUD-3602 -->

   - ![novo ícone](../../assets/new.svg) Adicionado a variável de pós-implantação **TTFB_TESTED_PAGES** para configurar testes de página de _Tempo para o Primeiro Byte_ para verificar o desempenho do aplicativo em sites implantados na infraestrutura de nuvem. Consulte a descrição da variável em [variáveis pós-implantação](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![ícone de correção](../../assets/fix.svg) corrigido um problema com o SCD multithread, que causava falhas aleatórias na implantação de conteúdo estático. A solução alternativa envolveu a configuração da variável **SCD_THREADS** como `1`. Agora você pode aumentar a contagem, conforme necessário. Consulte as definições nas [variáveis de implantação](../environment/variables-deploy.md#scd_threads) e nas [variáveis de compilação](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![ícone de correção](../../assets/fix.svg) Você pode configurar a variável de ambiente **WARM_UP_PAGES** para armazenar em cache páginas únicas, vários domínios e várias páginas. Veja a definição expandida no conteúdo [variáveis pós-implantação](../environment/variables-post-deploy.md#warm_up_pages).<!-- MAGECLOUD-3258 -->

- ![ícone de correção](../../assets/fix.svg) Adicionado o arquivo `pub/static/.htaccess` à lista de exclusões. [Correção enviada por Björn Kraus da PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um erro quando todas as mensagens de validação eram exibidas como `Critical` se pelo menos um validador de nível crítico retornasse um erro.<!-- MAGECLOUD-3178 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava uma falha de implantação se a URL de base não existisse no banco de dados.<!-- MAGECLOUD-3075 -->

- ![novo ícone](../../assets/new.svg) Adicionado um novo comando **`env:config:show`** ao pacote `ece-tools` que exibe serviços de ambiente, rotas ou variáveis. Consulte [Serviços, rotas e variáveis](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables). [Recurso enviado por Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava um erro crítico ao tentar instalar o Adobe Commerce 2.2.6 ou anterior com `ece-tools` desenvolvido após a refatoração do shell.<!-- MAGECLOUD-3665 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a falha das instalações do Adobe Commerce 2.1.x e 2.2.x com um aviso sobre o uso de uma versão obsoleta do Carbon.<!-- MAGECLOUD-3704 -->

- ![ícone de correção](../../assets/fix.svg) Diminuiu o nível de log `cloud.log` para saída do shell de `info` para `debug`.<!-- MAGECLOUD-3277 -->

- ![ícone de correção](../../assets/fix.svg) Adicionada a opção `--remove-definers (-d)` ao comando `ece-tools db-dump` para remover definidores do arquivo de despejo.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que substitui o arquivo `env.php` durante uma implantação, resultando em perda de configurações personalizadas. Essa atualização garante que o Adobe Commerce na infraestrutura em nuvem atualize o arquivo `env.php` com cada implantação, preservando as configurações personalizadas.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![novo ícone](../../assets/new.svg) **Atualizações do Docker**—

   - ![novo ícone](../../assets/new.svg) Agora, o ambiente Docker dá suporte à configuração cron definida na propriedade [crons do arquivo .magento.app.yaml](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property).<!-- MAGECLOUD-3150 -->

   - ![novo ícone](../../assets/new.svg) **Novo Contêiner de Docker**—Adicionou um [contêiner de proxy de encerramento do TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) para facilitar o encerramento do SSL de Verniz em HTTPS.<!-- MAGECLOUD-2890 -->

   - ![novo ícone](../../assets/new.svg) **Nova imagem do Docker**—Adicionou uma imagem Node.js para suportar Gulp e outros recursos, como o Teste de Unidade JS Jasmine.<!-- MAGECLOUD-3345 -->

   - ![novo ícone](../../assets/new.svg) **Modos de compilação do Docker**—Agora você pode optar por iniciar o ambiente do Docker no [Modo de produção ou Modo de desenvolvedor](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode). O modo de desenvolvedor dá suporte ao desenvolvimento ativo com permissões de sistema de arquivos completas e graváveis.<!-- MAGECLOUD-3152/3511 -->

   - ![Ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a falha da implantação do Docker com um erro `Name or service not known` se o cache estivesse configurado para um serviço que não estava disponível. Agora, você pode remover um serviço do [`.magento/services.yaml` arquivo](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml). O gerador de configuração do Docker atualiza o serviço no arquivo `docker/config.php.dist` automaticamente.<!-- MAGECLOUD-3369 -->

   - ![novo ícone](../../assets/new.svg) Adição de validações interativas para compatibilidade de serviço. Agora, se um serviço solicitado for incompatível com a versão do Adobe Commerce ou outros serviços, o _modo interativo_ solicitará ao usuário uma mensagem e a opção de continuar. Consulte as [Versões de serviço](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers) disponíveis para o Docker. Use a opção `-n` para ignorar a interatividade para fins de CICD.<!-- MAGECLOUD-3251 -->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema com o comando de composição do Docker `db-dump` que apagava os despejos existentes.<!-- MAGECLOUD-3366 -->

   - ![ícone de correção](../../assets/fix.svg) Corrigido um problema que atribuiu o armazenamento em cache Redis `session`, `default` e `page_cache` à mesma ID de banco de dados.<!-- MAGECLOUD-3172 -->

- ![novo ícone](../../assets/new.svg) **Atualizações de variáveis de ambiente**—

   - ![novo ícone](../../assets/new.svg) A nova variável de ambiente **ELASTICSUITE\_CONFIGURATION** retém as configurações de serviço personalizadas entre as implantações. Consulte a definição no conteúdo [implantar variáveis](../environment/variables-deploy.md#elasticsuite_configuration).<!-- MAGECLOUD-3205 -->

   - ![novo ícone](../../assets/new.svg) Adicionado a variável de ambiente **SCD_MAX_EXECUTION_TIMEOUT** para que você possa aumentar o tempo de conclusão da implantação de conteúdo estático do arquivo `.magento.env.yaml`. Consulte a definição nas [variáveis de implantação](../environment/variables-deploy.md#scd_max_execution_time), nas [variáveis de compilação](../environment/variables-build.md#scd_max_execution_time) e nas [variáveis globais](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![novo ícone](../../assets/new.svg) Adicionado a variável de ambiente **MAGENTO_CLOUD_LOCKS_DIR** para configurar o caminho para o ponto de montagem do provedor de bloqueio na infraestrutura de nuvem. O provedor de bloqueio impede a inicialização de trabalhos cron duplicados e grupos cron. Essa variável é compatível com o Adobe Commerce versão 2.2.5 e posterior e configurada automaticamente. Consulte a definição em [Variáveis de nuvem](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![ícone de correção](../../assets/fix.svg) alterou os valores padrão da variável de ambiente **SCD_THREADS** para determinar automaticamente o valor ideal com base na contagem de threads do CPU detectada. Consulte as definições atualizadas nas [variáveis de implantação](../environment/variables-deploy.md#scd_threads) e nas [variáveis de compilação](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema com uma correção do Mecanismo de Isolamento de Banco de Dados que causava um erro ao atualizar para o Adobe Commerce na infraestrutura de nuvem versão 2002.0.16.<!-- MAGECLOUD-3383 -->

- ![Ícone de correção](../../assets/fix.svg) Adicionado uma correção que substitui _Gráficos de Imagens do Google_ por _Gráficos de Imagens_. Consulte o artigo do DevBlog [Descontinuação e atualização dos Gráficos de Imagem Google para M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![Ícone de correção](../../assets/fix.svg) Adicionado validação para a [variável SEARCH_CONFIGURATION](../environment/variables-deploy.md#search_configuration). Falha na implantação quando a opção &#39;engine&#39; não está definida e `_merge` não é necessário.<!-- MAGECLOUD-3470 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que expunha dados confidenciais após a ocorrência de uma exceção. Agora, as informações confidenciais são mascaradas adequadamente.<!-- MAGECLOUD-3525 -->

- ![ícone de correção](../../assets/fix.svg) Aprimorado nas configurações tolerantes a falhas do pacote Magento Open Source. No caso quando o Adobe Commerce não consegue ler dados da instância `slave` do Redis, uma leitura é feita da instância `master` do Redis. Consulte [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>A versão 2002.0.17 do `ece-tools` inclui um patch de segurança importante. Consulte [Recursos Técnicos: Magento Open Source Patches](https://magento.com/tech-resources/download#download2288).

- ![novo ícone](../../assets/new.svg) **Atualizações de serviço**—Compatível com as seguintes versões do Adobe Commerce: 2.2.8 e posteriores 2.2.x, 2.3.1 e posteriores 2.3.x

   - Adição de suporte para o Elasticsearch versão 6.x.<!-- MAGECLOUD-3196 -->

   - Adição de suporte para Redis versão 5.0.

- ![novo ícone](../../assets/new.svg) **Novas imagens do Docker**—Os seguintes serviços foram adicionados à compilação do Docker:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![novo ícone](../../assets/new.svg) **Nova variável de ambiente**—Anteriormente, havia um tempo limite embutido em código para a compactação SCD. Agora você pode configurar o tempo limite de compactação SCD usando a variável de ambiente **SCD_COMPRESSION_TIMEOUT**. Consulte as definições nas [variáveis de compilação](../environment/variables-build.md#scd_compression_timeout) e o conteúdo das [variáveis de implantação](../environment/variables-deploy.md#scd_compression_timeout).<!-- MAGECLOUD-2870 -->

- ![ícone de correção](../../assets/fix.svg) Adicionada a opção `--use-rewrites` ao comando de instalação para que ele use regravações do servidor Web para links gerados na loja e acesso de Administrador para melhorar a segurança e a experiência do cliente.<!-- MAGECLOUD-3246 -->

- ![ícone de correção](../../assets/fix.svg) Adicionou carimbos de data/hora ao arquivo `var/log/install_upgrade.log` para que ele mostrasse datas para eventos de instalação e atualização.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![novo ícone](../../assets/new.svg) **Atualizações do Docker**—

   - Agora, a configuração de serviço padrão gerada no ambiente Docker é igual à configuração padrão no modelo de Nuvem.<!-- MAGECLOUD-3025 -->

   - Você pode enviar emails do seu ambiente Docker usando o serviço `sendmail`.<!-- MAGECLOUD-2907 -->

   - Adicionada a capacidade de [configurar o Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) para depurar no ambiente do Cloud Docker.<!-- MAGECLOUD-2891 -->

   - Correção de um problema com permissões de serviço Web ao gerar o arquivo `docker-compose.yml`.<!-- MAGECLOUD-2883 -->

- ![novo ícone](../../assets/new.svg) **Aprimoramento de atualização**—Adição de validação para confirmar se a propriedade `autoload` no arquivo `composer.json` contém as alterações de configuração necessárias antes de atualizar para o Adobe Commerce v2.3. Consulte [Versão de atualização](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version).<!-- MAGECLOUD-2392 -->

- ![novo ícone](../../assets/new.svg) O processo de compactação na implantação de conteúdo estático agora inclui todos os ativos gerados nativamente ou personalizados e ocorre durante a fase de compilação no início da seção [`build:transfer`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property). Anteriormente, o processo de compactação ocorria antes da aplicação de minificação e agrupamento personalizados de ativos estáticos. [Correção enviada por Rafael Garcia Lepper da Tryzens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um erro de conexão de banco de dados que ocorria durante a implantação imediatamente após a configuração de uma relação adicional de banco de dados e serviço. Além disso, essa correção soluciona um problema que ocorria durante o processo de configuração do Commerce Reporting for Starter. Para o Iniciante, esta atualização é &quot;obrigatória&quot; para usar os Relatórios do Commerce.<!-- MAGECLOUD-3035 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema de validação com a configuração do banco de dados que causava a falha do processo de implantação.<!-- MAGECLOUD-3003 -->

- ![ícone de correção](../../assets/fix.svg) Atualizou a restrição com a versão apropriada do pacote `symfony/yaml` para usar com [constantes PHP](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants). A análise constante não funciona ao usar uma versão do pacote `symfony/yaml` anterior à 3.2. [Correção enviada por Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![novo ícone](../../assets/new.svg) **Verificação da configuração do ambiente**—Validação adicionada para verificar a versão do PHP e avisar os usuários se eles não estiverem usando a última versão recomendada.<!--MAGECLOUD-2903-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema com o processamento de variáveis JSON malformadas. Agora, se uma variável JSON causar um erro de sintaxe, um aviso será exibido no arquivo `cloud.log` e a implantação continuará usando a variável padrão.<!-- MAGECLOUD-2851 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um erro de conexão que ocorreu durante a implantação imediatamente após a desabilitação do serviço Redis.<!-- MAGECLOUD-2747 -->

- ![novo ícone](../../assets/new.svg) **Alterações de log**—Atualizou o [nível de log](../environment/log-handlers.md#log-levels) de `Info` para `Notice` para os seguintes eventos de processo de compilação e implantação:<!--MAGECLOUD-2925-->

   - Início e término do processo de reconciliação dos módulos instalados em `composer.json` com as configurações compartilhadas no arquivo `app/etc/config.php`

   - Início e término do processo de validação de configuração

   - Início e fim do processo `setup:di:compile` para geração de classes

- ![novo ícone](../../assets/new.svg) **Novas variáveis de ambiente**—

   - **[Variável de implantação RESOURCE_CONFIGURATION](../environment/variables-deploy.md#resource_configuration)** — use esta variável para mapear um nome de recurso para uma conexão de banco de dados.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[Variável global X_FRAME_CONFIGURATION](../environment/variables-global.md#x_frame_configuration)**—Use esta variável para alterar a configuração do cabeçalho `X-Frame-Options` para renderizar uma página do Adobe Commerce em um `<frame>`, `<iframe>` ou `<object>`.<!-- MAGECLOUD-3048 -->

- ![Ícone de correção](../../assets/fix.svg) **Atualizações de variáveis de ambiente**—Alterou as seguintes variáveis de ambiente:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)**—Adicionou a capacidade de pré-carregar o cache para páginas especificadas em todos os domínios definidos para um armazenamento do Adobe Commerce. Anteriormente, se o site foi configurado com vários domínios, o processo de pós-implantação falhou ao pré-carregar o cache para as páginas especificadas em domínios não padrão e retornou o seguinte erro no log pós-implantação: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL**—Atualizou a documentação e o arquivo de amostra `.magento.env.yaml` com os valores padrão corretos para o nível de compactação SCD. Consulte as definições nas [variáveis de compilação](../environment/variables-build.md#scd_compression_level) e o conteúdo das [variáveis de implantação](../environment/variables-deploy.md#scd_compression_level).<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES**—Esta variável de ambiente está obsoleta. Use a [SCD_MATRIX](../environment/variables-build.md#scd_matrix) para controlar a configuração do tema.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX**—Corrigiu o processo de validação para evitar um problema que ocorria quando SCD_MATRIX ignorava um valor de tema que continha caracteres maiúsculos e minúsculos. Consulte as definições nas [variáveis de compilação](../environment/variables-build.md#scd_matrix) e o conteúdo das [variáveis de implantação](../environment/variables-deploy.md#scd_matrix).<!-- MAGECLOUD-2904 -->

   - **Variáveis de ADMINISTRADOR**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - Segurança aprimorada ao gerenciar credenciais para o usuário Administrador usando variáveis de ambiente. Não é mais possível usar as variáveis de ambiente ADMIN_EMAIL, ADMIN_USERNAME e ADMIN_PASSWORD para substituir credenciais de administrador durante atualizações. Se não for possível acessar o painel Administrador, use o recurso _Esqueceu a senha_ ou o comando da CLI `admin:user:create` para criar um novo usuário administrador. Consulte [Acessar o painel de administração](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin).

      - ADMIN_EMAIL não é mais necessário ao atualizar ou aplicar patches.

## v2002.0.15

- ![novo ícone](../../assets/new.svg) **Atualizações do Docker**—

   - Agora, o gerador de Docker usa os serviços especificados nos arquivos de configuração `.magento.app.yaml` e `.magento/services.yaml` ao [criar seu ambiente de Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/). Você pode escolher uma versão de serviço diferente usando parâmetros de compilação.<!-- MAGECLOUD-2888 -->

   - Adição da imagem do PHP 7.2 — Adição do suporte ao PHP 7.2 no Cloud Docker; atualização da [configuração do Launch Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) para incluir a opção `docker:build --php` para especificar a versão do PHP compatível com a sua versão do Adobe Commerce.<!-- MAGECLOUD-2799 -->

   - Adição de um [contêiner Cron](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli/#cron-container) baseado na imagem do PHP-CLI.<!-- MAGECLOUD-2565 -->

   - Adição dos seguintes serviços à build do Docker:

      - [!DNL RabbitMQ] 3.5 e 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 e 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 e 4.0<!-- MAGECLOUD-2886 -->

- ![novo ícone](../../assets/new.svg) **Configurar com constantes PHP**—Suporte adicionado para [constantes PHP](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) no arquivo de configuração `.magento.env.yaml`.<!-- MAGECLOUD- 2575 -->

- ![novo ícone](../../assets/new.svg) **Nova variável de ambiente** — por padrão, somente o ambiente de Produção tem Google Analytics habilitado. Você pode habilitar Google Analytics nos ambientes de Preparo e Integração usando a [variável de ambiente ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![ícone de correção](../../assets/fix.svg) corrigiu um problema que removia configurações cron personalizadas do arquivo `env.php` após uma reimplantação. Agora, as configurações de cron personalizadas permanecem com segurança no arquivo `env.php`.<!-- MAGECLOUD-2923 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido inconsistências nas mensagens e [níveis de log](../environment/log-handlers.md#log-levels) para as fases de compilação, implantação e pós-implantação. Aumento dos níveis de mensagem de log inicial e final de **info** para **notice** para todas as fases e subfases. Adicionadas as mensagens de log inicial e final, quando apropriado.<!-- MAGECLOUD-2919 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema envolvendo processos cron que impedia o início da fase de pós-implantação, quando configurado. Agora, se o gancho pós-implantação estiver habilitado, os processos cron serão habilitados novamente no início da fase pós-implantação.<!-- MAGECLOUD-2862 -->

- ![ícone de correção](../../assets/fix.svg) Resolveu um problema que impedia uma instalação bem-sucedida do Adobe Commerce ao especificar uma configuração de banco de dados personalizada. Anteriormente, o processo de instalação usava a configuração de banco de dados da [variável MAGENTO_CLOUD_RELATIONSHIP](../environment/variables-cloud.md) mesmo que você designasse informações de conexão personalizadas na [variável de ambiente DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![ícone de correção](../../assets/fix.svg) Corrigiu o comando `config:dump` para incluir cada localidade de site na seção `system` do arquivo `config.php`.<!--MAGECLOUD-2740-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que resultava em erros de _aquecimento_ durante a fase de pós-implantação, corrigindo a referência da URL de base de origem.<!--MAGECLOUD-2797-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que gerava arquivos incorretamente durante o processo `setup:di:compile`, o que afetava o módulo de Pagamento Amazon.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![novo ícone](../../assets/new.svg) **Verificar Estado Ideal**—O assistente `ideal-state` agora verifica a configuração atual durante cada implantação e fornece instruções claras para atualizar a configuração para obter uma implantação mais rápida, sem tempo de inatividade.<!--MAGECLOUD-2372-->

- ![ícone de correção](../../assets/fix.svg) **Conformidade com a PCI**—Os protocolos de mensagens do Adobe Commerce na infraestrutura em nuvem foram atualizados para exigir a versão 1.2 do TLS (Transport Layer Security) ao se conectar com serviços de mensagens de terceiros. Se você estiver usando um serviço de mensagem que não seja compatível com a versão 1.2 do TLS, será necessário atualizar seu serviço. Caso contrário, a seguinte mensagem de erro será exibida quando o aplicativo Adobe Commerce tentar se conectar ao servidor de mensagens para enviar um email: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![Novo ícone](../../assets/new.svg) **Aprimoramento da implantação**—Validação adicionada para avisar os clientes se um ambiente de Preparo ou Produção tem `dev`, `debug` ou `debug_logging` opções habilitadas para evitar problemas de desempenho causados por excesso de atividade de log.<!--MAGECLOUD-2517-->

- ![Ícone de correção](../../assets/fix.svg) **Correções de implantação**—

   - Agora o modo de manutenção é ativado no início da fase de implantação e desativado no final. Se a implantação falhar, o site permanecerá no modo de manutenção até que os problemas de implantação sejam resolvidos. Anteriormente, o site retornava ao modo de produção mesmo se a implantação falhasse.<!--MAGECLOUD-2603-->

      - As verificações de validação da fase de implantação foram retrabalhadas para baixar o nível de erro para os seguintes problemas de implantação de `CRITICAL` para `WARNING`, de forma que a implantação fosse concluída. Anteriormente, esses problemas causavam falha na implantação.

      - A configuração do ambiente contém valores incorretos para variáveis de implantação ou de nuvem.

   - A versão do Elasticsearch na infraestrutura em nuvem é incompatível com a versão do módulo elasticsearch/elasticsearch compatível com o Adobe Commerce na infraestrutura em nuvem. Consulte o [artigo sobre solução de problemas de Elasticsearch](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) na Base de Conhecimentos de Suporte da Adobe Commerce.<!--MAGECLOUD-2600-->

   - Correção de um problema com as definições de configuração compartilhadas no arquivo `app/etc/config.php` que causou `recursion detected` erros durante a implantação.<!--MAGECLOUD-2173-->

- ![ícone de correção](../../assets/fix.svg) **Correções relacionadas ao Cron**—

   - Correção de um problema de agendamento de cron que impedia a execução de trabalhos se você especificasse uma frequência de cron diferente do padrão (1 minuto).<!--MAGECLOUD-2602-->

   - Correção de um problema na fase de implantação que permitia que trabalhos cron continuassem em execução durante a implantação, o que pode causar bloqueios de banco de dados e outros problemas críticos. Agora, todos os trabalhos cron são interrompidos antes de a fase de implantação começar e reiniciados após a conclusão da implantação.&lt;!—MAGECLOUD—2537—>

   - Correção do fluxo de trabalho do cron nas versões 2.2.x para desbloquear trabalhos cron congelados, de modo que possam ser interrompidos antes de iniciar a implantação. Anteriormente, um trabalho cron congelado causou a paralisação da implantação.<!--MAGECLOUD-2501-->

- ![ícone de correção](../../assets/fix.svg) alterado o formato do arquivo `config.php` gerado pelo comando `vendor/bin/ece-tools config:dump` para usar sintaxe de matriz curta e recuo de 4 espaços para estar em conformidade com os padrões de codificação do Adobe Commerce.<!--MAGECLOUD-2527-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um erro de implantação que ocorre quando o `.magento.env.yaml` contém `{{ base_url }}` e `{{ unsecure_base_url }}` espaços reservados para configurações da Web em vez da configuração de URL padrão para um projeto Adobe Commerce na infraestrutura em nuvem./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![novo ícone](../../assets/new.svg) **Habilitar implantação sem tempo de inatividade** — Agora o Adobe Commerce na infraestrutura de nuvem enfileira solicitações com as alterações necessárias no banco de dados durante a implantação e aplica as alterações assim que a implantação é concluída. As solicitações podem ser mantidas por até 5 minutos para garantir que nenhuma sessão seja perdida. Consulte [Opções de implantação de conteúdo estático para reduzir o tempo de inatividade de implantação na Nuvem](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![novo ícone](../../assets/new.svg) **Composição do Docker para a Nuvem**—As seguintes melhorias foram feitas no processo de [instalação e configuração do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/):

   - Adição de um comando—`docker:config:convert` para converter arquivos de configuração PHP no formato Docker ENV para simplificar a configuração do ambiente. Agora, você copia os arquivos de configuração do PHP para o diretório Docker e os converte em arquivos ENV do Docker. Consulte [Iniciar Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359-->

   - O processo de instalação do Adobe Commerce na infraestrutura em nuvem agora oferece suporte à implantação em sistemas de arquivos somente leitura e leitura/gravação para emular mais detalhadamente o sistema de arquivos em nuvem. Consulte [Configurar Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).&lt;!—MAGECLOUD—2357—>

   - **Suporte ao serviço Redis**—Adição de uma imagem Redis, que é implantada em um contêiner Docker e configurada automaticamente para funcionar com a instalação do Docker.&lt;!—MAGECLOUD—2442—>

   - Agora você tem o recurso de despejo de banco de dados ao usar o [contêiner de banco de dados](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#database-container) do Cloud Docker. Além disso, você pode [compartilhar arquivos](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container) entre um computador host e um contêiner usando o diretório `docker/mnt`.<!-- MAGECLOUD-2577 -->

   - **Suporte ao serviço de Verniz**— Adição de uma imagem de Verniz, que é implantada automaticamente em um contêiner de Docker. Após a implantação, você pode configurar manualmente o Varnish de acordo com as práticas recomendadas da Adobe Commerce. Consulte [Configurar e usar verniz](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish).&lt;!—MAGECLOUD—2358—>

   - Acesso seguro ao site — Adição de suporte SSL para acessar sua loja da Adobe Commerce e o painel Administração.&lt;!—MAGECLOUD—2360—>

- ![ícone de correção](../../assets/fix.svg) **Aprimoramento do Adobe Commerce no suporte à extensão de infraestrutura na nuvem**—Rebaixamento do requisito de versão mínima para o pacote guzzlehttp/guzzle no [arquivo composer.json](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview) do Adobe Commerce na infraestrutura na nuvem para a versão 6.2, para que o pacote `ece-tools` seja compatível com mais extensões.<!--MAGECLOUD-2205-->

- ![novo ícone](../../assets/new.svg) **Aplicar alterações personalizadas ao aplicativo Adobe Commerce durante a fase de compilação** — Dividimos a fase de compilação em dois processos separados para que você possa usar ganchos para aplicar alterações personalizadas ao conteúdo estático gerado antes de empacotar o aplicativo para implantação. O processo _build:generate_ gera código, aplica patches e gera conteúdo estático. O processo _build:transfer_ transfere o código gerado e o conteúdo estático para o destino final. Consulte [Ganchos de Aplicativo](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->

- ![Ícone de correção](../../assets/fix.svg) **Verificações de configuração de ambiente**—Validação aprimorada da configuração de ambiente para avisar os clientes sobre incompatibilidades de versão e erros de configuração antes de criar e implantar o Adobe Commerce na infraestrutura em nuvem.

   - Validação específica da versão adicionada para identificar variáveis e valores de ambiente sem suporte ou obsoletos.<!--MAGECLOUD-2183-->

   - Adição de uma verificação de compatibilidade de Elasticsearch para avisar os usuários sobre problemas de configuração de Elasticsearch. Agora, a implantação falhará se a versão do serviço Elasticsearch no servidor for incompatível com o Adobe Commerce. Anteriormente, a implantação era bem-sucedida mesmo se a versão do Elasticsearch fosse incompatível, o que causava problemas no catálogo de produtos após a implantação do site.<!--MAGECLOUD-2389-->

     Você pode resolver a incompatibilidade [enviando um tíquete de Suporte](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) para atualizar o Elasticsearch para uma versão compatível ou alterar a configuração do Adobe Commerce para especificar uma versão compatível do cliente Elasticsearch PHP.

      - Para Adobe Commerce versão 2.1.x a 2.2.2, atualize o Elasticsearch para a versão 2.4.

      - Para Adobe Commerce versão 2.2.3 e posterior, atualize o Elasticsearch para a versão 5.2.

      - Se você tiver o Elasticsearch 1.x ou 2.x e não quiser atualizar, atualize o requisito de versão de cliente Elasticsearch Adobe Commerce PHP no composer.json para `"elasticsearch/elasticsearch": "~2.0"`.

   - Validação aprimorada de variáveis de ambiente para identificar definições de configuração que podem causar conflitos durante as fases de criação, implantação e pós-implantação. Por exemplo, uma mensagem de aviso será exibida durante o processo de instalação e atualização se a configuração global para implantação de conteúdo estático entrar em conflito com as configurações na fase de compilação ou implantação.<!--MAGECLOUD-2156-->

- ![Ícone de correção](../../assets/fix.svg) **Atualizações de variáveis de ambiente**—Alterou as seguintes variáveis de ambiente:

   - **[Variável global SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)**—Alterou o valor padrão para `true` para habilitar a minificação de conteúdo de HTML sob demanda, o que minimiza o tempo de inatividade ao implantar em ambientes de Preparo e Produção. Essa configuração é necessária para implantações sem tempo de inatividade.<!--MAGECLOUD-2435-->

   - **[Variável de implantação CLEAN_STATIC_FILES](../environment/variables-deploy.md#clean_static_files)**—Adicionou a capacidade de gerenciar o processamento limpo de arquivos estáticos para o conteúdo estático gerado durante a fase de compilação com base na configuração da variável de ambiente CLEAN_STATIC_FILES. Anteriormente, os arquivos de conteúdo estático gerados durante a fase de compilação sempre eram limpos.<!--MAGECLOUD-1506-->

- ![Ícone de correção](../../assets/fix.svg) **Log**—As seguintes alterações foram feitas para melhorar as mensagens de log e reduzir o tamanho do log:

   - As entradas de log de falha de implantação agora incluem a saída de comando das operações que causam as falhas, mesmo se a configuração do ambiente não especificar o log de nível de depuração. Consulte [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - Adição de log para falhas de implantação que ocorrem quando fábricas geradas exigidas por algumas extensões não podem ser geradas corretamente porque o sistema de arquivos está em um estado somente leitura.<!--MAGECLOUD-2209-->

   - Redução do tamanho do log de implantação e correção de problemas de formatação causados por comandos de instalação que usam a barra de progresso interativa.<!--MAGECLOUD-2402-->

   - Eliminou detalhamento desnecessário e atualizou os níveis de prioridade para algumas instruções de log.<!--MAGECLOUD-2227-->

- ![ícone de correção](../../assets/fix.svg) **Correções específicas do Cron**—

   - As definições de configuração padrão do trabalho cron para o tempo de vida do histórico foram alteradas de 3d (4320 min) para 1h (60 min) para evitar problemas de desempenho e falhas de implantação que podem ocorrer quando a fila do cron é preenchida muito rapidamente.<!--MAGECLOUD-2427-->

   - Melhoria do processo de gerenciamento de trabalhos cron durante a fase de implantação para evitar bloqueios de bancos de dados e outros problemas críticos. Agora, todos os trabalhos cron são interrompidos durante a fase de implantação e reiniciados após a conclusão da implantação.<!--MAGECLOUD-2445-->

   - Correção de um problema com o mecanismo de bloqueio para agendamento de consumidores iniciados por trabalhos cron nas versões 2.2.0 e posteriores do Adobe Commerce para impedir que trabalhos cron iniciem consumidores duplicados.<!--MAGECLOUD-2464-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema com o [processo de compactação de conteúdo estático](../environment/variables-intro.md) (`gzip`) que causava erros de `not overwritten` e `no such file or directory` ao fazer referência ao arquivo compactado durante o processo de implantação.<!-- MAGECLOUD-2182-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que impedia o comando `php ./vendor/bin/ece-tools config:dump` de remover seções redundantes do arquivo `config.php` durante o processo de despejo se a localidade de repositório não fosse especificada. Agora é possível mover facilmente seus arquivos de configuração entre ambientes. Depois de atualizar para o `ece-tools` v2002.0.13, gere novamente os arquivos `config.php` mais antigos com o comando `config:dump` aprimorado. Consulte [Gerenciamento de configurações para configurações de repositório](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings).<!--MAGECLOUD-2444-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava um erro durante a fase de implantação se a configuração de rota no arquivo `.magento/routes.yaml` redirecionasse de um domínio [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) para um domínio `www`.<!--MAGECLOUD-2556-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema com a opção `_merge` da variável [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) que causava resultados de mesclagem incorretos se você não incluísse o parâmetro `engine` no arquivo de configuração `.magento.env.yaml` atualizado. Agora, a operação de mesclagem substitui corretamente apenas os valores especificados no `.magento.env.yaml` atualizado, sem exigir que você defina o parâmetro `engine`.<!--MAGECLOUD-2520-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema de configuração do Redis que habilitou incorretamente o bloqueio de sessão para o Adobe Commerce nas versões 2.2.1 e posteriores da infraestrutura de nuvem, o que pode causar desempenho lento e tempos limite. Agora, o bloqueio de sessão é desativado por padrão. O problema foi causado por uma alteração no comportamento padrão do parâmetro `disable_locking` introduzido na v1.3.4 do pacote do manipulador da sessão Redis. Consulte [pacote colinmollenhour/php-redis-session-abstract](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![novo ícone](../../assets/new.svg) **Composição do Docker para a Nuvem**—Adicionou um comando—`docker:build`—para gerar uma configuração de [Composição do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) a partir do repositório da Nuvem `ece-tools`.<!-- MAGECLOUD-2250 -->

- ![novo ícone](../../assets/new.svg) **Alterar Localidades** — Agora é possível alterar a localidade de armazenamento sem o processo de configuração de exportação e importação. Enquanto o aplicativo estiver em Produção e o SCD_ON_DEMAND estiver habilitado, as opções de localidade de repositório e administrador estarão disponíveis.<!-- MAGECLOUD-2019 -->

- ![novo ícone](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Mapa do site e Robôs**—Criou um [fluxo de trabalho](../store/robots-sitemap.md) para adicionar um arquivo `robots.txt` e gerar um arquivo `sitemap.xml` para uma única configuração de domínio sem exigir uma alteração na infraestrutura.

- ![novo ícone](../../assets/new.svg) **Assistentes**—Foram adicionados dois [assistentes](../deploy/smart-wizards.md) para ajudá-lo com a configuração de Nuvem:<!-- MAGECLOUD-1910 -->

   - `ideal-state`—configurar o estado ideal para um tempo de inatividade mínimo de implantação

   - `master-slave`—configurar o balanceamento de carga para o banco de dados e Redis

- ![novo ícone](../../assets/new.svg) **Atualização de Módulo**—Adicionou um comando de Nuvem—`module:refresh`—para habilitar módulos que foram desabilitados ou que não foram explicitamente habilitados, de modo semelhante ao modo como é feito automaticamente durante uma compilação.<!-- MAGECLOUD-1521 -->

- ![novo ícone](../../assets/new.svg) Adicionou a capacidade de escolher mesclar ou substituir a configuração de serviços usando a opção `_merge` nas configurações [CACHE](../environment/variables-deploy.md#cache_configuration), [SESSION](../environment/variables-deploy.md#session_configuration), [QUEUE](../environment/variables-deploy.md#queue_configuration) e [SEARCH](../environment/variables-deploy.md#search_configuration).<!-- MAGECLOUD-2105 -->

- ![novo ícone](../../assets/new.svg) **Arquivo de amostra da Configuração do Ambiente**—Adicionamos um arquivo de amostra `.magento.env.yaml` ao pacote ECE-Tools que inclui uma descrição detalhada e valores possíveis para cada variável de ambiente.<!-- MAGECLOUD-1908 -->

   - Também adicionamos uma validação profunda para a configuração `.magento.env.yaml` que evita falhas no processo de implantação causadas por valores inesperados. Quando ocorrer uma falha, você receberá uma mensagem de erro detalhada que começa com: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![novo ícone](../../assets/new.svg) Adicionado as [**Variáveis de ambiente**](../environment/variables-intro.md) a seguir:

   - Agora você pode definir várias localidades para cada tema usando a nova variável de ambiente [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix), que reduz a quantidade de arquivos de tema a serem implantados.<!-- MAGECLOUD-1501 -->

   - Adicionada a variável de ambiente [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) para personalizar as conexões de banco de dados para implantação.<!-- MAGECLOUD-2047 -->

   - A nova variável [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) substitui o nível de log mínimo para todos os fluxos de saída sem fazer alterações no código.<!-- MAGECLOUD-2129 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava tempo de inatividade entre as fases de implantação e pós-implantação. Agora, a fase de pós-implantação começa _imediatamente_ após o término da fase de implantação.

- ![ícone de correção](../../assets/fix.svg) corrigiu um problema que não limpava os trabalhos cron bem-sucedidos, aqueles com `status = success`, da programação.<!-- MAGECLOUD-2268 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema com o gancho `post_deploy` que limpava o cache na fase de implantação em vez da fase pós-implantação do projeto.<!-- MAGECLOUD-2113 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema ao usar o SCD com várias localidades, o que gerou o mesmo arquivo `js-translation.json` para cada localidade.<!-- MAGECLOUD-2034 -->

- ![ícone de correção](../../assets/fix.svg) Otimizado o comando `db:dump` no pacote `ece-tools` para evitar o bloqueio de tabelas e aumentar a velocidade.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>A versão 2002.0.11 das ECE-Tools é necessária para a compatibilidade com a versão 2.2.4.

- ![novo ícone](../../assets/new.svg) **Configurando conexões somente leitura com nós não mestres**—Esta versão adiciona a capacidade de configurar uma conexão somente leitura com um nó não mestre para receber tráfego somente leitura (para [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) e para <!--MAGECLOUD-143 -->

- ![novo ícone](../../assets/new.svg) **Assistente de Configuração**—Adicionou um assistente para ajudar a verificar sua configuração para implantação de conteúdo estático. Consulte [Assistentes inteligentes](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![novo ícone](../../assets/new.svg) **Suporte ao Symfony Console**—Suporte adicionado para o Symfony Console 4 com o Adobe Commerce 2.3.<!-- MAGECLOUD-1966 -->

- ![ícone de correção](../../assets/fix.svg) **Otimizações de agendamento do Cron**—Gerenciamento de filas e logs aprimorados para ajudar na depuração de problemas relacionados ao cron.<!-- MAGECLOUD-1607 -->

- ![Ícone de correção](../../assets/fix.svg) Falha na validação da implantação se um valor `ADMIN_EMAIL` ou `ADMIN_USERNAME` for igual a uma conta de administrador existente.<!-- MAGECLOUD-1221 -->

- ![ícone de correção](../../assets/fix.svg) Remoção do suporte SOLR para versões 2.2.x. As versões 2.1.x mantêm a capacidade de habilitar SOLR.<!-- MAGECLOUD-1282 -->

- ![ícone de correção](../../assets/fix.svg) A primeira instalação dos ambientes de Preparo e Produção de um projeto PRO agora inclui prefixos de índice diferentes para o Elasticsearch, a fim de evitar possíveis conflitos ao identificar registros pertencentes a cada ambiente.<!-- MAGECLOUD-1489 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que interrompeu a fase de compilação da arquitetura herdada durante a implantação do conteúdo estático.<!-- MAGECLOUD-2021 -->

- ![Ícone de correção](../../assets/fix.svg) **Melhorias específicas do Cron**—Retrabalhou a implementação do cron:<!-- MAGECLOUD-1607 -->

   - Correção de um problema que fazia com que a fila do cron fosse preenchida rapidamente. Agora ele limpa os trabalhos cron desatualizados de uma maneira mais confiável.

   - A sequência de trabalhos cron foi reorganizada para que todos os trabalhos em threads separados fossem iniciados antes do grupo geral.

   - Melhoria no registro para melhor auxiliar na depuração de problemas do cron.

   - **OBSERVAÇÃO** — esta versão aborda muitos problemas relacionados ao cron. Se você usa atualmente alguns patches relacionados ao cron em _m2-hotfixes_, remova-os.

- ![ícone de correção](../../assets/fix.svg) **melhorias específicas de SCD**—

   - Você pode usar as variáveis de ambiente `VERBOSE_COMMANDS` e `SCD_COMPRESSION_LEVEL` durante as fases de _compilação_ e de_implantação.<!-- MAGECLOUD-1819 -->

   - Correção de um problema que causava falha na implantação com um erro aleatório ao encontrar um valor inesperado para a variável de ambiente `SCD_COMPRESSION_LEVEL`. A validação da configuração foi aprimorada para fornecer notificações significativas. Consulte [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) para obter valores aceitáveis.<!-- MAGECLOUD-2043 -->

   - Correção do comportamento do fluxo de configuração da variável de ambiente `SCD_COMPRESSION_LEVEL` para que as substituições funcionem conforme o esperado.<!-- MAGECLOUD-2044 -->

   - Correção de um problema que impedia a configuração da variável de ambiente `SCD_THREADS` no estágio _implantar_ do arquivo `.magento.env.yaml`.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![novo ícone](../../assets/new.svg) **Implantação de Conteúdo Estático (SCD)** — Há um novo processo de implantação alternativo para gerar conteúdo estático quando solicitado (sob demanda). Isso diminui o tempo de inatividade e melhora o tratamento do cache, gerando os ativos mais críticos.<!-- MAGECLOUD-1285 -->

   - **Nova variável de ambiente** — Adicionada a variável de ambiente global `SCD_ON_DEMAND` para gerar conteúdo estático quando solicitado.<!-- MAGECLOUD-1738 -->

   - **Gancho pós-implantação** — Adição de um gancho `post_deploy` para o arquivo `.magento.app.yaml` que limpa o cache e pré-carrega (aquece) o cache _depois_, o contêiner começa a aceitar conexões. Ele está disponível somente para projetos Pro que contêm ambientes de Preparo e Produção no [!DNL Cloud Console] e para projetos Starter. Embora não seja obrigatório, isso funciona em conjunto com a variável de ambiente `SCD_ON_DEMAND`.<!-- MAGECLOUD-1788 -->

- ![novo ícone](../../assets/new.svg) **Otimização**—Otimizou a movimentação ou cópia de arquivos durante a implantação para melhorar a velocidade da implantação e diminuir cargas no sistema de arquivos.<!-- MAGECLOUD-1842 -->

- ![novo ícone](../../assets/new.svg) **Log de Implantação**—Adicionou a capacidade de habilitar manipuladores Syslog e Graylog Extended Log Format (GELF) para logs de saída durante o processo de implantação. Consulte [Manipuladores de log](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![novo ícone](../../assets/new.svg) Adicionado as [**Variáveis de ambiente**](../environment/variables-intro.md) a seguir:

   - `CRYPT_KEY` — Forneça uma chave criptográfica para outro ambiente ao mover um banco de dados.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Global_ variável de ambiente que ignora a cópia dos arquivos de exibição estática no diretório `var/view_preprocessed` e gera HTML minificado quando solicitado.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_Global_ variável de ambiente para gerar conteúdo estático quando solicitado.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES`—Você pode listar as páginas a serem usadas para pré-carregar o cache. Disponível nas novas [Variáveis pós-implantação](../environment/variables-post-deploy.md).

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que envolvia um patch aplicado localmente quebrando a implantação em uma instância. Agora, o ECE-Tools pode detectar que um patch foi aplicado.<!-- MAGECLOUD-982 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um conflito entre o agrupamento JavaScript e a funcionalidade GZIP. Agora esses recursos funcionam corretamente juntos.<!-- MAGECLOUD-1735 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a falha de comandos ECE-Tools da CLI ao usar versões anteriores do PHP 7.0.x.<!-- MAGECLOUD-1744 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que impedia a implantação de conteúdo estático com a estratégia compacta em várias threads.<!-- MAGECLOUD-1822 -->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema de bloqueio de sessão Redis que causava um atraso de logon de Administrador. Além disso, a correção está disponível para 2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>Você deve [atualizar o metappackage do Adobe Commerce na infraestrutura da nuvem](../dev-tools/install-package.md#update-the-metapackage) para obter esta e todas as atualizações futuras.

- ![novo ícone](../../assets/new.svg) **ece-tools**—O pacote `ece-tools` agora é compatível com o Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![novo ícone](../../assets/new.svg) **Configuração do Redis**—Agora você pode [configurar a página Redis](../environment/variables-deploy.md#cache_configuration) e o cache padrão e o armazenamento de sessão do Redis usando uma variável de ambiente.<!-- MAGECLOUD-1552 -->

- ![novo ícone](../../assets/new.svg) **Melhorias no serviço Search, AMQP e Redis**—Unificamos o fluxo de configuração do serviço para que ele agora se comporte da mesma forma para todos os serviços. Não há mais suporte para a edição manual do arquivo `env.php` para configurar serviços. Em vez disso, você deve usar variáveis de ambiente ou o arquivo `.magento.env.yaml`.<!-- MAGECLOUD-1437 -->

- ![Ícone de correção](../../assets/fix.svg) **Variáveis de ambiente**—

   - O uso do `env:STATIC_CONTENT_THREADS` foi descontinuado e será removido em uma versão futura. Em vez disso, use o [SCD_THREADS](../environment/variables-deploy.md#scd_threads).<!-- MAGECLOUD-1507 -->

   - A variável de ambiente `STATIC_CONTENT_EXCLUDE_THEMES` foi preterida. Em vez disso, você deve usar a variável de ambiente `SCD_EXCLUDE_THEMES`.<!-- MAGECLOUD-1640 -->

- ![Ícone de correção](../../assets/fix.svg) **Log** — simplificamos o logon em operações de correção internas.<!-- MAGECLOUD-1674 -->

- ![ícone de correção](../../assets/fix.svg) Removemos o suporte ao modo `developer` e a variável de ambiente `APPLICATION_MODE` porque eles estavam causando um comportamento inesperado.<!-- MAGECLOUD-1615 -->

- ![ícone de correção](../../assets/fix.svg) Corrigimos um problema que causava falhas na implantação de conteúdo estático relacionado ao Redis. Agora, a implantação de conteúdo estático multithread é executada conforme projetado.<!-- MAGECLOUD-1630 -->

- ![ícone de correção](../../assets/fix.svg) Corrigimos um problema que impedia os usuários de salvar modificações nos campos de configuração no Administrador, que estão marcados como confidenciais após a execução do comando `app:config:dump`.<!-- MAGECLOUD-1175 -->

- ![ícone de correção](../../assets/fix.svg) Adicionamos suporte para uma versão anterior do `symfony/yaml` para corrigir conflitos com alguns pacotes, que ainda não são compatíveis com a versão mais recente.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>Mesclamos `vendor/magento/ece-patches` com `vendor/magento/ece-tools` nesta versão. Não é mais necessário atualizar o pacote `vendor/magento/ece-patches` separadamente.

**Novos recursos:**

- **Logs aprimorados**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Melhoramos as mensagens de log para fornecer explicações melhores quando o processo de criação ou implantação substitui uma variável de ambiente.
   - Agora você pode visualizar o progresso da instalação e da atualização em tempo real. Siga o arquivo `install_update.log` para ver o progresso. Por exemplo,

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Novo comando cron**—Agora você pode desbloquear trabalhos cron travados específicos em vez de parar e reiniciar todos eles com o comando [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451). Não disponível em 2.1.<!-- MAGECLOUD-1367 -->

- **Arquivo de configuração unificado**—Agora você pode configurar estágios de compilação e implantação usando um arquivo [`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml).<!-- MAGECLOUD-1369 -->

- **Arquivos de configuração de backup** — O processo de implantação agora cria automaticamente um backup dos arquivos de configuração `app/etc/env.php` e `app/etc/config.php` após a implantação. Também adicionamos um [novo comando CLI](https://support.magento.com/hc/en-us/articles/360033182871) para restaurar esses arquivos de configuração de um backup.<!-- MAGECLOUD-1372 -->

- **Solução de problemas de erros de validação**—Alteramos o comando que você deve usar para resolver erros de validação quando `config.php` não contiver dados suficientes para a implantação de conteúdo estático. Anteriormente, a mensagem de erro instruía você a executar `bin/magento app:config:dump`. Agora, você deve executar `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **Novas variáveis de ambiente** — Agora você pode usar variáveis de ambiente para conectar serviços personalizados [de pesquisa](../environment/variables-deploy.md#search_configuration) e [baseados em AMQP](../environment/variables-deploy.md#queue_configuration) ao seu site.<!-- MAGECLOUD-1410 -->

- Implementamos o patch inteligente. Agora o pacote aplica patches com base não no Adobe Commerce na versão de infraestrutura de nuvem, mas na versão do pacote com patch.<!--MAGECLOUD-1090-->

**Problemas resolvidos:**

- Corrigimos um problema de log que estava causando erros de compilação.<!-- MAGECLOUD-1162 -->

- Corrigimos um problema que causava exceções de tempo limite ao executar implantações no modo interativo.<!-- MAGECLOUD-1389 -->

- Corrigimos um problema que causava erros ao usar a estratégia compacta para geração de conteúdo estático. Não disponível em 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- Corrigimos um problema que impedia que o script de implantação identificasse corretamente os ambientes de preparo e produção.<!-- MAGECLOUD-1493 -->

- Corrigimos um problema que causava problemas de rede que interrompiam as conexões de banco de dados e causavam falhas durante o processo de instalação e atualização.<!-- MAGECLOUD-1520 -->

- Corrigimos um problema que impedia a exportação dos arquivos de configuração usando o `app:config:dump` mais de uma vez. Não disponível em 2.1.<!--  MAGECLOUD-1567  -->

- Corrigimos um problema de _bloqueio_ da sessão Redis que causava um atraso de logon de _Administrador_. Não disponível em 2.1.<!--  MAGECLOUD-1582  -->

- Corrigimos um problema de implementação relacionado ao controle de versão que estava causando um conflito com outros módulos de correção baseados no Composer.<!-- MAGECLOUD-1450 -->

- Corrigimos um problema que causava problemas de memória do PHP durante a importação.<!-- MAGECLOUD-1310 -->

- Remoção do patch; correção de erro no `colinmollenhour/credis` v1.6 para habilitar o suporte para Adobe Commerce na infraestrutura em nuvem 2.2.1. Não disponível em 2.1.<!-- MAGECLOUD-1033 -->

## v2002.0.7

**Problemas resolvidos:**

- Removemos a vinculação simbólica `var/view_preprocessed` para corrigir um problema que estava causando conflitos de minificação do JavaScript.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Problemas resolvidos:**

- Corrigimos um problema que causava `gzip` erros quando um nome de arquivo ou diretório continha espaços.<!-- MAGECLOUD-1413 -->

- Corrigimos um problema que impedia que os scripts de implantação reconhecessem e habilitassem corretamente as dependências do módulo.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Novos recursos:**

- **Configurar um consumidor cron com uma variável de ambiente**—Agora você pode configurar consumidores cron usando a nova variável de ambiente `CRON_CONSUMERS_RUNNER`.

- **Verificação de configuração**—Agora verificamos componentes críticos durante o processo de compilação/implantação e interrompemos o processo se a verificação falhar, o que evita tempo de inatividade desnecessário porque o site está em modo de manutenção.

- **Criar/implantar notificações**—Adicionamos um arquivo de configuração que você pode usar para [configurar notificações por Slack e/ou email](../environment/set-up-notifications.md) para ações de compilação/implantação em todos os seus ambientes.

- **Compactação de conteúdo estático**—Agora compactamos o conteúdo estático usando [gzip](https://www.gnu.org/software/gzip/) durante as fases de compilação e implantação. Essa compactação, combinada com a compactação Fastly, ajuda a reduzir o tamanho do armazenamento e aumentar a velocidade de implantação. Se necessário, você pode desabilitar a compactação usando uma [opção de compilação](../environment/variables-build.md) ou [variável de implantação](../environment/variables-deploy.md). Consulte os seguintes tópicos para obter mais informações:

   - [Variáveis de ambiente de aplicativo](../application/variables-property.md)

   - [Desempenho de implantação de conteúdo estático](../deploy/static-content.md)

   - [Processo de implantação](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **Gerenciamento de configuração**—Agora geramos automaticamente um arquivo `app/etc/config.php` no seu repositório Git durante a fase de compilação, se ele ainda não existir. O arquivo gerado automaticamente inclui apenas uma lista de módulos e extensões. Se o arquivo já existir, a fase de criação continuará normalmente. Se você seguir o [Gerenciamento de Configuração](../store/store-settings.md) posteriormente, os comandos atualizarão o arquivo sem exigir etapas adicionais. Consulte [Processo de implantação](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) para obter mais informações.

- **Despejos de banco de dados**—Adicionamos um comando da CLI `magento/ece-tools` para criar despejos de banco de dados em todos os ambientes. Para ambientes de Produção de plano Pro, esse comando despeja apenas de um dos três nós de alta disponibilidade, portanto, os dados de produção gravados em um nó diferente durante o despejo podem não ser copiados. Recomendamos colocar o aplicativo no modo de manutenção antes de fazer um despejo de banco de dados em ambientes de Produção. Consulte [Gerenciamento de backup](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots) para obter mais informações.

- **Limitações do intervalo Cron levantadas**—O intervalo cron padrão para todos os ambientes provisionados nas regiões us-3, eu-3 e ap-3 é de 1 minuto. O intervalo cron padrão em todas as outras regiões é de 5 minutos para ambientes de Integração Pro e 1 minuto para ambientes de Preparo e Produção Pro. Para modificar seus trabalhos cron existentes, edite suas configurações no `.magento.app.yaml` ou crie um tíquete de suporte para ambientes de Produção/Preparo. Consulte [Configurar trabalhos cron](../application/crons-property.md#set-up-cron-jobs) para obter mais informações.

**Problemas resolvidos:**

- Corrigimos um problema que estava causando longos tempos de implantação porque o processo de implantação invocava a operação `cache-clean` antes da implantação de conteúdo estático.<!-- MAGECLOUD-1327 -->

- Corrigimos um problema que causava erros durante a etapa de geração de conteúdo estático da implantação em ambientes de Produção.<!-- MAGECLOUD-1322 -->

- Corrigimos um problema que impedia que alguns comandos `magento/ece-tools` registrassem em log a saída em `stderr`.<!-- MAGECLOUD-1264 -->

- Corrigimos um problema que impedia que valores de URL base em `env.php` fossem atualizados em ramificações bifurcadas.<!-- MAGECLOUD-1242 -->

- Corrigimos um problema que fazia com que o comando `magento setup:install` adicionasse um prefixo não seguro (`http://`) a URLs base seguras.<!-- MAGECLOUD-1171 -->

- Corrigimos um problema que impedia que erros de patch causassem falhas de implantação.<!-- MAGECLOUD-1170 -->

- Corrigimos um problema que impedia o `ece-tools` de interromper a execução e gerar uma exceção se não fosse possível aplicar patches.<!-- MAGECLOUD-1152 -->

- Corrigimos um problema que causava erros ao carregar a loja após habilitar a minificação de HTML no Administrador.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Problemas resolvidos:**

- Agora você pode [redefinir manualmente os trabalhos cron travados](https://support.magento.com/hc/en-us/articles/360033099451) usando um comando CLI em todos os ambientes por meio do acesso SSH. O processo de implantação redefine automaticamente os trabalhos cron.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Problemas resolvidos:**

- Corrigimos um problema que fazia com que as páginas expirassem porque o Redis demorava muito para ler/gravar. Agora você pode usar o parâmetro `disable_locking` nas configurações Redis para evitar esse problema.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Problemas resolvidos:**

- O processo de configuração [!DNL RabbitMQ] agora obtém todos os parâmetros necessários automaticamente.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Novos recursos:**

- A infraestrutura do Adobe Commerce na nuvem agora oferece suporte a escopos e [estratégias de implantação de conteúdo estático](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy). Adicionamos o parâmetro `–s` com uma configuração padrão de `quick` para a estratégia de implantação de conteúdo estático. Você pode usar a variável de ambiente [SCD_STRATEGY](../environment/variables-deploy.md) para personalizar e usar essas estratégias com suas ações de compilação e implantação. Esta variável oferece suporte às opções `standard`, `quick` ou `compact`. Se você selecionar `compact`, substituiremos o valor `STATIC_CONTENT_THREADS` por `1`, o que pode retardar a implantação, especialmente em ambientes de produção. Não disponível em 2.1.<!--- MAGECLOUD-1057 -->

- Criamos um arquivo de log em ambientes para capturar e compilar ações de criação e implantação. O arquivo `var/log/cloud.log` está no diretório raiz do aplicativo.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Problemas resolvidos:**

- Refatorado o pacote `ece-tools` para torná-lo compatível com o Adobe Commerce na infraestrutura de nuvem 2.2.0 e superior.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- Corrigimos um problema que impedia o `ece-tools` de interromper a execução e gerar uma exceção se não fosse possível aplicar patches.<!-- MAGECLOUD-1186 -->

- Corrigimos um problema que causava o lançamento de exceções quando a compilação de id (injeção de dependência) era ignorada durante as compilações.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- Corrigimos um problema que fazia com que o processo de implantação substituísse as configurações personalizadas de Redis no arquivo `env.php`.<!-- MAGECLOUD-1019 -->

- Corrigimos um problema que estava causando loops de redirecionamento devido à desabilitação por administrador seguro padrão.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Este pacote não é mais compatível com outras versões do Adobe Commerce na infraestrutura de nuvem e **não deve** ser usado.

### Versão inicial

Versão inicial do `ece-tools` para Adobe Commerce na infraestrutura em nuvem 2.2.0.
