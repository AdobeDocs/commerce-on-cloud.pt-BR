---
title: Pacote do Cloud Docker
description: Consulte uma lista das melhorias mais recentes no pacote do Cloud Docker.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: bbf11023474cb6ac6b3b881c40897c3260542de9
workflow-type: tm+mt
source-wordcount: '3806'
ht-degree: 0%

---

# Pacote do Cloud Docker

O pacote [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) fornece funcionalidade e imagens do Docker para implantar o Adobe Commerce em um ambiente de Nuvem local. Estas notas de versão descrevem as últimas melhorias neste pacote, que é um componente do [Conjunto de ferramentas da nuvem para o Commerce](cloud-tools-suite.md).

O pacote `magento/magento-cloud-docker` usa a seguinte sequência de versão: `<major>.<minor>.<patch>`

As notas de versão incluem:

- ![novo ícone](../../assets/new.svg) Novos recursos
- ![ícone de correção](../../assets/fix.svg) Correções e melhorias

<!--Add release notes below-->

## v1.4.6 {#latest}

Data de lançamento: 13 de novembro de 2025

- ![Ícone de correção](../../assets/fix.svg) **Pacote Symfony** - Suporte adicionado para os pacotes YAML Symfony mais recentes.<!-- MCLOUD-14020 -->

## v1.4.5

Data de lançamento: 08 de outubro de 2025

- ![novo ícone](../../assets/new.svg) **AtiveMQ**—Adicionou suporte a AtiveMQ no Cloud-Docker com testes funcionais.<!-- MCLOUD-13771 -->

## v1.4.4

Data de lançamento: 07 de agosto de 2025

- ![ícone de correção](../../assets/fix.svg) **PHP 8.4**—Testes do PHP 8.4 adicionados.<!-- MCLOUD-13311 -->
- ![ícone de correção](../../assets/fix.svg) **Extensão FTP** - Correção adicionada para extensão FTP.<!-- MCLOUD-13843 -->
- ![novo ícone](../../assets/new.svg) **Imagem Opensearch3**—Suporte a Opensearch3 adicionado.<!-- MCLOUD-13766 -->
- ![novo ícone](../../assets/new.svg) **Testes do Opensearch3**—Adicionou testes do PHP 8.4 para Opensearch3.<!-- MCLOUD-13768 -->
- ![novo ícone](../../assets/new.svg) **Valkey**—Suporte adicionado para Valkey.<!-- MCLOUD-13558 -->

## v1.4.3

Data de lançamento: 03 de junho de 2025

- ![ícone de correção](../../assets/fix.svg) **Compatibilidade aprimorada com 2.4.8**-Bibliotecas de terceiros atualizadas para melhor compatibilidade com 2.4.8<!-- MCLOUD-13707- -->

## v1.4.2

Data de lançamento: 7 de abril de 2025

- ![novo ícone](../../assets/new.svg) **PHP 8.4**—Adição de `php-cli` imagens 8.4 e `php-fpm` imagens 8.4.


## v1.4.1

Data de lançamento: 6 de fevereiro de 2025

- ![novo ícone](../../assets/new.svg) **PHP 8.4**—Suporte adicionado para PHP 8.4.


## v1.4.0

Data de lançamento: 7 de outubro de 2024

- ![Ícone de correção](../../assets/fix.svg) **Código refatorado**—Removeu o suporte de versões antigas do PHP (7.4, 7.3, 7.2) e bibliotecas e imagens relacionadas.

## v1.3.7

Data de lançamento: 8 de abril de 2024

- ![novo ícone](../../assets/new.svg) **PHP** — Adição de suporte para imagens do PHP 8.3 e PHP 8.3.
- ![novo ícone](../../assets/new.svg) **Nginx** — imagem nginx v. 1.24 adicionada.
- ![novo ícone](../../assets/new.svg) **Opensearch** - Imagem adicionada OpenSearch v. 2.12, 1.3.
- ![novo ícone](../../assets/new.svg) **Composer** - Versão do Composer atualizada para 2.2.23.

## v1.3.6

Data de lançamento: 31 de julho de 2023

- ![novo ícone](../../assets/new.svg) **Nova versão de serviço adicionada**—OpenSearch 2.5.
- ![novo ícone](../../assets/new.svg) **Habilitar cache do Composer**—Agora você pode estender a configuração do Docker para habilitar o cache limpo do Composer ao iniciar o contêiner do Docker. Consulte [Estender a configuração do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure) no guia do _Cloud Docker for Commerce_.

## v1.3.5

Data de lançamento: 10 de março de 2023

- ![novo ícone](../../assets/new.svg) **ionCube**—Adicionou a extensão ionCube para a imagem do PHP 8.1.
- ![novo ícone](../../assets/new.svg) **Adição de novas versões de serviço**—OpenSearch 2.3 e 2.4, PHP 8.2, Varnish 7.1.1.
- ![novo ícone](../../assets/new.svg) **Suporte aprimorado para PHP 8.2**—Corrigiu problemas de compatibilidade com determinadas versões do PHP 8.2.x para suportar Commerce 2.4.6.
- ![ícone de correção](../../assets/fix.svg) **Problema do Composer**—Correção de problemas que ocorriam após a atualização da versão do Composer nos contêineres do Docker.

## v1.3.4

Data de lançamento: 27 de outubro de 2022

- ![novo ícone](../../assets/new.svg) **Adição de novas imagens em verniz**—Adição de imagens para verniz 6.5, 7.0 e 7.1.<!-- MCLOUD-7879 -->

## v1.3.3

Data de lançamento: 13 de setembro de 2022

- ![novo ícone](../../assets/new.svg) **Suporte ao Apple M1 (ARM64)**—Adição de alterações nas imagens do Docker para habilitar o suporte à arquitetura Apple M1 (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![Ícone de correção](../../assets/fix.svg) **Mailhog**—Corrigiu um problema onde o serviço de Mailhog não capturava emails enquanto estava no modo de desenvolvedor.<!-- MCLOUD-8643 -->
- ![ícone de correção](../../assets/fix.svg) **init-docker.sh**—Corrigiu o validador de versões de serviço no script `init-docker.sh`.<!-- MCLOUD-8765 -->

## v1.3.2

Data de lançamento: 31 de março de 2022

- ![novo ícone](../../assets/new.svg) **Imagem do Elasticsearch 7.10 adicionada**<!-- MCLOUD-8548 -->

## v1.3.1

Data de lançamento: 10 de março de 2022

- ![novo ícone](../../assets/new.svg) **Suporte ao PHP 8.1**—Suporte adicionado para o PHP 8.1.
- ![novo ícone](../../assets/new.svg) **OpenSearch**—Adicionou imagens das versões 1.1 e 1.2 do OpenSearch.
- ![novo ícone](../../assets/new.svg) **Compositor 2.1**—Define o compositor 2.1.x como padrão nas imagens do PHP 8.x.
- ![novo ícone](../../assets/new.svg) **melhorias nas imagens do PHP**—

   - Adição de imagens do PHP 8.1
   - Atualização do xDebug versão 3.1.2
   - xmlrpc 1.0.0RC3 atualizado

- ![ícone de correção](../../assets/fix.svg) **Melhorias no Elasticsearch e no OpenSearch**—Melhorias no Elasticsearch e no OpenSearch Dockerfiles; removeu a imagem do Elasticsearch 5.2.
- ![Corrigir ícone](../../assets/fix.svg) **Extensão de sódio**—Habilitou a extensão `sodium` por padrão em todas as imagens PHP.
- ![ícone de correção](../../assets/fix.svg) **Volume de cache do Composer** — Caminho fixo para o volume de cache do Composer ter pacotes do Composer em cache.
- ![Ícone de correção](../../assets/fix.svg) **Limitação de memória em nginx**—Limitação de memória corrigida na imagem NGINX.

## v1.3.0

Data de lançamento: 25 de outubro de 2021

- ![ícone de correção](../../assets/fix.svg) **Melhorar fluxo de trabalho do modo de desenvolvedor**—Anteriormente, você precisava especificar o modo nas etapas de compilação e implantação. Agora, a opção `--mode` na etapa `build` determina o modo na etapa `deploy` posterior. Não é mais necessário definir o modo após a implantação. Consulte [Modo de desenvolvedor](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode).<!-- ACMP-1086 -->
- ![ícone de correção](../../assets/fix.svg) **Melhorias no sistema de arquivos somente leitura**—<!-- ACMP-1106 -->
   - Correção de um problema que iniciava um contêiner PHP para configuração de email.
   - Pode usar variáveis de ambiente em arquivos INI.
   - Certifique-se de que os pontos de entrada do PHP não precisem de permissão de gravação.
- ![Ícone de correção](../../assets/fix.svg) **Atualizar Nó**—Atualiza a versão de Nó fornecida; ao instalar o Nó em imagens PHP-CLI, ele agora usa a versão LTS atual.<!-- ACMP-1539 -->
- ![Ícone de correção](../../assets/fix.svg) **Atualizar Symfony**—Atualizou as dependências de configuração do Symfony para serem compatíveis com o Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## v1.2.4

Data de lançamento: 29 de julho de 2021

- ![novo ícone](../../assets/new.svg) **Novo `Zookeeper` contêiner**—Adicionou um [contêiner do Zookeeper](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#zookeeper-container) para gerenciar a configuração do provedor de bloqueio para projetos que não estão implantados na infraestrutura do Adobe Commerce na nuvem.<!--MCLOUD-8000-->

- ![novo ícone](../../assets/new.svg) **Adicionado suporte para o Composer 2.0.**—O Composer versão 2.0 foi adicionado ao arquivo de configuração do Composer para oferecer suporte a atualizações do Composer 1.0 que está se aproximando do fim da vida útil.<!--MCLOUD-8003-->

## v1.2.3

Data de lançamento: 14 de junho de 2021

- ![novo ícone](../../assets/new.svg) **Adição do PHP 8.0**—Atualização do PHP para a versão 8.0, permitindo que você aproveite todos os novos recursos e otimizações que o PHP 8.0 inclui.<!--MCLOUD-7941-->
- ![novo ícone](../../assets/new.svg) **Atualizado para o Vernish 6.6 e o Elasticsearch 7.11.2**—Os links a seguir fornecem informações sobre a versão do [Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) e do Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![novo ícone](../../assets/new.svg) **Adição da extensão `ioncube` para a imagem do PHP 7.4**—A extensão `ioncube` foi adicionada novamente à imagem do PHP 7.4 após ter sido excluída inicialmente da atualização do PHP 7.3 para o PHP 7.4. *[Enviado por &#x200B;](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![novo ícone](../../assets/new.svg) **Adição de uma opção de sincronização de arquivo:`manual-native`**—A opção de sincronização de arquivo `manual-native` fornece controle manual sobre a sincronização, que fornece o melhor desempenho para ambientes macOS e Windows. Leia sobre como usar a opção `manual-native` no [Modo de desenvolvedor](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode) e [Sincronizando dados em um ambiente de desenvolvedor do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data#file-synchronization-options).<!--MCLOUD-7977-->
- ![novo ícone](../../assets/new.svg) **Remoção de exclusões de volume dos comandos `up` e `down`**—A opção `--volume` foi removida dos comandos `bin/magento-docker up` e `bin/magento-docker down`, substituída pelo novo comando `bin/magento-docker init` com um aviso de perda de dados. Essa alteração ajuda a evitar a perda acidental de dados. *[Enviado por joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![Ícone de correção](../../assets/fix.svg) **Atualização do valor `CN` para o certificado gerado**—Remoção do valor codificado `CN` do Dockerfile. Este valor criou um erro de certificado (`NET::ERR_CERT_INVALID`) que fez com que a opção `--host` do comando `ece-docker build:compose` fosse ignorada.<!--MCLOUD-7934-->

## v1.2.2

Data de lançamento: 20 de abril de 2021

- ![novo ícone](../../assets/new.svg) **Atualizado `host.docker.internal` para ser independente de plataforma**—Agora você pode criar os mesmos scripts Docker Compose para Ubuntu, Windows e macOS. O uso do Xdebug no Ubuntu não requer mais uma variável de ambiente separada. [Correção enviada por Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![novo ícone](../../assets/new.svg) **Init-docker.sh** atualizado—Adicionou o objeto `mounts` à variável de ambiente `MAGENTO_CLOUD_APPLICATION`. [Correção enviada por Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![novo ícone](../../assets/new.svg) **Init-docker.sh** atualizado—Atualizou o script `init-docker.sh` com o PHP 7.4 e versões Cloud Docker 1.2.1. [Correção enviada por Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![novo ícone](../../assets/new.svg) **Sódio habilitado por padrão**—Habilitou a extensão PHP `sodium` por padrão nas imagens do PHP Docker.<!--MCLOUD-7548-->
- ![novo ícone](../../assets/new.svg) **`custom-registry`opção**—Adicionou uma opção `--custom-registry` ao comando `php ./vendor/bin/ece-docker build:compose` para usar seu próprio registro de imagens.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![novo ícone](../../assets/new.svg) **Remoção de versões antigas do Elasticsearch**—Remoção das versões 1.7 e 2.4 do Elasticsearch das imagens do Elasticsearch.<!--MCLOUD-7504-->
- ![novo ícone](../../assets/new.svg) **Gerando automaticamente certificados NGINX**—Removeu os certificados existentes da imagem NGINX. Os certificados NGINX agora são gerados automaticamente a cada nova implantação para melhorar a segurança.<!--MCLOUD-7396-->
- ![Ícone de correção](../../assets/fix.svg) **Habilitado`opcache.validate_timestamps`**—Habilitou a configuração do PHP `opcache.validate_timestamps` por padrão no modo de desenvolvedor. Habilitar essa configuração corrigiu o problema onde as alterações no sistema de arquivos não eram reconhecidas no Docker.<!--MCLOUD-7466-->
- ![ícone de correção](../../assets/fix.svg) **Corrigido`build:custom:compose`**—Corrigido o comando `build:custom:compose` para gerar um erro quando os arquivos não puderem ser substituídos durante o processo de compilação. Gerar um erro evita situações em que `docker-compose up` poderia estar usando os arquivos errados.<!--MCLOUD-7457-->
- ![ícone de correção](../../assets/fix.svg) **Opção `--sync_engine="native"` corrigida**—Corrigiu o problema onde, no modo de produção (`--mode="production"`), a opção `--sync_engine="native"` não criava entradas para pastas locais no arquivo `docker.composer.yml`.<!--MCLOUD-7254-->
- ![ícone de correção](../../assets/fix.svg) **Erros de validação de versão de serviço corrigidos**—Versões de serviço adicionadas para [!DNL RabbitMQ], Elasticsearch e outros serviços à propriedade `type` na variável `MAGENTO_CLOUD_RELATIONSHIP`. Adicionar essas versões à variável `relationships` corrigiu os erros de validação que ocorriam durante a fase de implantação.<!--MCLOUD-7572-->

## v1.2.1

Data de lançamento: 21 de dezembro de 2020

- ![novo ícone](../../assets/new.svg) **Opções de comando NGINX**—Adicionou opções de comando de compilação para alterar o número de NGINX `worker_processes` e NGINX `worker_connections` para TLS e serviços Web. O parâmetro `worker_process` mantém a capacidade de definir o valor como `auto`. Exemplos: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![novo ícone](../../assets/new.svg) **opção de comando TLS**—Adicionada a opção de comando de compilação para criar uma configuração sem o serviço TLS. Exemplo: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![novo ícone](../../assets/new.svg) **Consumo de memória do NGINX**—Reduziu a memória consumida pelo processo do NGINX para TLS e serviços Web.<!--MCLOUD-7259-->

- ![novo ícone](../../assets/new.svg) **Blackfire**—Desabilitou a extensão PHP Blackfire por padrão na imagem do Cloud Docker.

- ![ícone de correção](../../assets/fix.svg) **contêiner PHP-FPM**—Corrigiu a verificação de integridade do contêiner PHP-FPM alterando o `WEB_PORT` de `80` para `8080`.<!--MCLOUD-7232-->

- ![ícone de correção](../../assets/fix.svg) **Nomeação de volume inválida**—Corrigido um erro com nomeação de volume inválida no modo de desenvolvedor.<!--MCLOUD-7442-->

- ![Ícone de correção](../../assets/fix.svg) **Porta upstream de NGINX**—Atualizou a imagem Docker NGINX 1.19 para usar a porta 8080 e evitar um loop infinito. [Correção enviada por Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

Data de lançamento: 9 de novembro de 2020

- ![novo ícone](../../assets/new.svg) **Atualizações de contêiner—**

   - ![novo ícone](../../assets/new.svg) **contêiner PHP-FPM**—Suporte adicionado para a extensão gnupg PHP. [Correção enviada por G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![Ícone de correção](../../assets/fix.svg) **Contêiner de banco de dados**—Corrigiu a verificação de integridade do contêiner de banco de dados adicionando a senha de banco de dados necessária ao comando de verificação de integridade.<!--MCLOUD-7122-->

   - ![novo ícone](../../assets/new.svg) **contêiner Elasticsearch**

      - Adição de suporte ao Elasticsearch 7.9 para compatibilidade com versões futuras do Adobe Commerce.<!--MCLOUD-7190-->

      - **Configuração do plug-in do Elasticsearch** — Adição de suporte para usar as informações de configuração do plug-in do Elasticsearch do arquivo `services.yaml` para gerar o arquivo `docker-compose.yaml` para um ambiente do Cloud Docker for Commerce. Consulte [plug-ins do Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Suporte ao plug-in do Elasticsearch**—Suporte adicionado para os seguintes plug-ins do Elasticsearch: `analysis-icu`, `analysis-phonetic`, `analysis-stempel` e `analysis-nori`. Os plug-ins `analysis-icu` e `analysis-phonetic` são instalados por padrão. Você pode adicionar ou remover os `analysis-stempel` e `analysis-nori` plug-ins conforme necessário.<!--MCLOUD-2789-->

   - ![novo ícone](../../assets/new.svg) **contêiner CLI**

      - **Executar comandos dentro de contêineres PHP do Docker**—Agora você pode usar a CLI do Cloud Docker para executar comandos dentro de contêineres PHP no ambiente do Docker sem precisar instalar o PHP no host. Por exemplo, o comando a seguir cria a configuração: `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Consulte [CLI do Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference#cloud-docker-cli). [Correção enviada por G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - Adição do OpenSSH-client aos contêineres da CLI do PHP. Agora, você pode usar o encaminhamento ssh-agent para o Composer se o arquivo `composer.json` contiver repositórios Git privados que exigem um cliente ssh para usar comandos do Composer.<!--MCLOUD-6008-->

   - ![ícone de correção](../../assets/fix.svg) **Contêiner TLS**—Agora, o [contêiner TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#tls-container) é baseado na imagem do Docker `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` em vez da imagem do CentOS. Essa alteração corrige problemas que causavam erros ao enviar solicitações HTTPS entre contêineres no ambiente do Cloud Docker.<!--MCLOUD-6469-->

   - ![Novo ícone](../../assets/new.svg) **Contêiner de teste**—Adicionou um contêiner de teste para teste de aplicativo e adicionou a opção `--with-test` ao comando Docker `build:compose` para criar o contêiner somente ao testar no ambiente Docker. Consulte [teste de aplicativo](https://developer.adobe.com/commerce/cloud-tools/docker/test-application-testing).<!--MCLOUD-6394-->

   - ![novo ícone](../../assets/new.svg) **contêiner FPM-XDEBUG**

      - ![novo ícone](../../assets/new.svg) **Configurar Xdebug no Linux**—Adicionada a opção `--set-docker-host` ao comando `ece-docker build:compose` para configurar o valor `host.docker.internal` no contêiner Xdebug. Essa opção é necessária para usar o Xdebug em sistemas Linux. Consulte [Configurar Xdebug para Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug).<!--MCLOUD-6430-->

      - ![ícone de correção](../../assets/fix.svg) Corrigido a configuração da variável Xdebug para o Docker ENTRYPOINT para resolver `uninitialized "with_xdebug" variable` erros nos logs. [Correção enviada por Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![novo ícone](../../assets/new.svg) **Alterações na configuração do Docker**

   - **Configuração de MailHog**—Agora você pode usar as seguintes opções de comando `ece-docker build:compose` para desabilitar MailHog e especificar portas: `--no-mailhog`, `--mailhog-http-port` e `--mailhog-smtp-port`. Consulte [Configurar email](https://developer.adobe.com/commerce/cloud-tools/docker/configure#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Para o Cloud Docker para Commerce 1.2.0 e posterior, a Adobe agora fornece imagens do Docker para cada versão de patch, e o gerador de configuração do Docker cria a configuração do Docker com uma versão de patch especificada, em vez de usar a mais recente. Anteriormente, o gerador de configuração do Docker criava a configuração usando a versão de patch mais recente, o que poderia quebrar o Cloud Docker para ambientes Commerce criados com uma versão anterior.<!--MCLOUD-7093-->

   - **Especificar imagens e versões personalizadas na configuração personalizada do Cloud Docker**—O comando `build:custom:compose` foi atualizado com opções para especificar imagens e versões personalizadas ao gerar um arquivo de configuração personalizado composto pelo Docker (`docker-compose.yaml`). Consulte [Criar uma configuração personalizada de composição do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose). <!--MCLOUD-7089-->

   - Atualização da configuração do host Docker para expor a porta 443 para habilitar o acesso ao Adobe Commerce (`https://magento2.docker`) de todos os contêineres CLI. Você pode alterar a porta padrão adicionando a opção `--tls-port` ao gerar o arquivo de configuração Docker.<!--MCLOUD-6806-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a falha da compilação do Cloud Docker para Commerce se o arquivo `app/etc/env.php` existisse.<!--MCLOUD-6732-->

- ![ícone de correção](../../assets/fix.svg) Atualizou a configuração de compilação para substituir volumes nomeados por volumes regulares para evitar problemas ao implantar o Cloud Docker para Commerce no Linux ou o WSL2 (Subsistema do Windows para Linux).<!--MCLOUD-6732-->

- ![ícone de correção](../../assets/fix.svg) atualizou o Cloud Docker para testes funcionais do Commerce para oferecer suporte ao Composer 2.0.<!--MCLOUD-7183-->

## v1.1.2

Data de lançamento: 9 de setembro de 2020

- ![novo ícone](../../assets/new.svg) Suporte adicionado para o Elasticsearch 7.7<!--MCLOUD-6219-->

## v1.1.1

Data de lançamento: 5 de agosto de 2020

- ![Ícone de correção](../../assets/fix.svg) **Configuração de email atualizada**—Atualizou o Cloud Docker padrão para configuração do Commerce para oferecer suporte ao serviço MailHog em vez de usar SendMail. Consulte [Configurar email](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-5624-->

- ![ícone de correção](../../assets/fix.svg) Restaurada a biblioteca PS para a configuração de ambiente do Cloud Docker para corrigir `ps:  command not found` erros.<!--MCLOUD-6621-->

- ![ícone de correção](../../assets/fix.svg) Atualização da configuração padrão do Cloud Docker para Commerce para remover a montagem automática do ponto de entrada do banco de dados e dos volumes do MariaDB para corrigir `Cannot create container for service db` erros que podem ocorrer ao iniciar o ambiente do Cloud Docker.

  Agora, você pode configurar o ambiente do Cloud Docker para montar os diretórios de banco de dados adicionando as seguintes opções ao comando `ece-docker build:compose`: `--with-entry-point` e `with-mariadb-conf`. Consulte [Opções de configuração de serviço](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![novo ícone](../../assets/new.svg) **atualizações de comando CLI**

| Ação | Comando |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Adicione um ponto de entrada ao contêiner do banco de dados para restaurar o banco de dados do backup | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Adicionar um volume de configuração do MariaDB | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

Data de lançamento: 25 de junho de 2020

- ![novo ícone](../../assets/new.svg) **Adição de suporte para a solução de desempenho de banco de dados dividido**—Agora você pode configurar e implantar um armazenamento usando a solução de desempenho de banco de dados dividido no ambiente do Cloud Docker.<!--MCLOUD-3740-->

- ![novo ícone](../../assets/new.svg) **Suporte para implantação do Adobe Commerce e do Magento Open Source**—Agora você pode usar o Cloud Docker for Commerce para implantar um ambiente de desenvolvimento local para projetos que não estejam hospedados no Adobe Commerce na infraestrutura em nuvem.<!--MCLOUD-5667-->

- ![novo ícone](../../assets/new.svg) **suporte ao Blackfire.io**—Adicionou suporte para usar a [extensão do Blackfire.io](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire) para teste de desempenho automatizado. [Correção enviada por Adarsh Manickam da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![novo ícone](../../assets/new.svg) **Atualizações do contêiner**

   - **Verniz** — Agora o verniz é o cache padrão ao implantar o Adobe Commerce em um ambiente do Cloud Docker usando uma versão compatível do modelo de aplicativo na nuvem. Consulte [Contêiner de verniz](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container).<!--MCLOUD-2634-->

   - Adicionada a opção `--no-varnish` para ignorar a instalação do serviço Verniz ao gerar o arquivo de configuração do Cloud Docker.<!--MCLOUD-2634-->

   - ![novo ícone](../../assets/new.svg) **Banco de dados**

      - Adição do suporte para o banco de dados MySQL. Agora, você pode configurar o ambiente do Cloud Docker com MariaDB ou MySQL. Consulte [Opções de configuração de serviço](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - Adição da capacidade de definir as configurações de incremento e deslocamento para replicação de banco de dados ao gerar o arquivo de composição do Docker. Consulte [Contêineres de serviço](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![novo ícone](../../assets/new.svg) **PHP-FPM**

      - Adição de suporte ao PHP 7.4. [Correção enviada por Mohanela Murugan da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - Adição da capacidade de copiar um arquivo `php.ini` no diretório do projeto raiz para o ambiente do Cloud Docker e aplicar configurações personalizadas de PHP aos contêineres PHP-FPM e CLI. Consulte [Personalizar configurações do PHP](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#customize-php-settings). [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - Adição de uma verificação de integridade do contêiner. [Correção enviada pelo Visanth Sampath da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![Ícone de correção](../../assets/fix.svg) **Node.js**—Atualizou a versão padrão do Node.js da versão 8 para a versão 10 para melhorar a segurança. A versão 8 do Node.js está obsoleta e não é mais atualizada com correções de erros ou patches de segurança. [Correção enviada por Mohan Elamurugan da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![novo ícone](../../assets/new.svg) **Elasticsearch**

      - Suporte adicionado para Elasticsearch 6.8, 7.2, 7.5 e 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Adicionada a capacidade de personalizar a [configuração do contêiner do Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-container) ao gerar o arquivo de configuração de composição do Docker.<!--MCLOUD-3059-->

      - Adicionada a opção `--no-es` às opções de configuração do serviço para gerar o arquivo de configuração Docker Compose. Use essa opção para ignorar a instalação do contêiner Elasticsearch e usar a pesquisa MySQL. Esta opção só tem suporte para o Adobe Commerce versões 2.3.5 e anteriores.<!--MCLOUD-3766-->

   - ![novo ícone](../../assets/new.svg) **contêiner FPM-XDEBUG**—Adicionou uma opção de configuração de serviço para instalar e configurar o Xdebug para depurar o PHP no ambiente do Cloud Docker. Consulte [Configurar Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug).<!--MCLOUD-4098-->

- ![novo ícone](../../assets/new.svg) **Alterações na configuração do Docker**

   - Adição de verificações de integridade para os contêineres de serviço PHP-FPM, Redis, Elasticsearch e MySQL Docker.<!--MCLOUD-3335 and MCLOUD-5856-->

   - Alterado o modo de sincronização de arquivos padrão para `native` no Modo de desenvolvedor.<!--MCLOUD-3890 -->

   - Adição de informações de versão à imagem de contêiner do serviço Docker genérica ao gerar o arquivo `docker-compose.yml`.<!--MCLOUD-3878-->

   - Melhoria na capacidade de lidar com grandes respostas do contêiner upstream de PHP-FPM ao aumentar o valor `fastcgi_buffers` para o servidor Nginx.<!--MCLOUD-5980-->

   - Desempenho de sincronização de arquivos mutagen aprimorado ao adicionar uma segunda sessão de sincronização para sincronizar arquivos no diretório `vendor`. Essa alteração impede que o mutagen fique paralisado durante o processo de sincronização de arquivos. [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![novo ícone](../../assets/new.svg) **atualizações de comando CLI**

| Ação | Comando |
| -------- | --------------- |
| Limpar cache Redis | `bin/magento-docker flush-redis` |
| Limpar cache de verniz | `bin/magento-docker flush-varnish` |
| Ignorar instalação padrão do Verniz | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Personalizar opções do Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Remover configuração do Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Configurar o contêiner de BD com o MySQL versão 5.6 ou 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Especificar URL de base personalizada | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Adicionar contêiner para configuração Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![ícone de correção](../../assets/fix.svg) Corrigido a configuração da sincronização de arquivos mutagen para impedir que mutagen criasse sessões obsoletas. [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema de configuração que causava erros de sintaxe no log de composição do Docker ao iniciar o contêiner PHP-FPM. [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![Ícone de correção](../../assets/fix.svg) Correção de erros de conflito de volume que às vezes ocorriam ao usar vários ambientes do Docker. [Correção enviada por G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que causava a falha do comando `ece-docker build:compose` se a configuração incluísse Blackfire.io. [Correção enviada por G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![ícone de correção](../../assets/fix.svg) Atualizado a configuração de imagem da CLI do PHP para evitar erros de falta de memória ocorridos ao instalar vários pacotes usando o Cloud Docker para Commerce. [Correção enviada por Mohan Elamurugan da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![Ícone de correção](../../assets/fix.svg) Adicionado suporte para vários usuários do MySQL no ambiente do Cloud Docker. Em versões anteriores, a operação `build:compose` falhará se o arquivo `magento.app.yaml` especificar vários usuários do banco de dados. [Correção enviada por G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![ícone de correção](../../assets/fix.svg) Removido `rsyslog` do Cloud Docker para contêineres PHP do Commerce para resolver problemas de compatibilidade que causavam notificações de aviso durante a implantação. O Cloud Docker não usa o utilitário rsyslog.<!--MCLOUD-6173-->

## v1.0.0

Data de lançamento: 5 de fevereiro de 2020

- ![novo ícone](../../assets/new.svg) **Criado um pacote separado para entregar`Cloud Docker for Commerce`**—Moveu o código-fonte para entregar o Cloud Docker para Commerce do repositório `ece-tools` para o [novo repositório `magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) para manter a qualidade do código e fornecer versões independentes. O novo pacote é uma dependência para ECE-Tools v2002.1.0 e posteriores.

  Ao atualizar ece-tools, você também atualiza o pacote `magento/magento-cloud-docker` para a versão 1.0.0. Se você usou o Cloud Docker para Commerce com uma versão anterior do `ece-tools` (2002.0.x), revise as [incompatibilidades anteriores](backward-incompatible-changes.md) e atualize seu projeto como scripts, comandos e processos, conforme necessário.

- ![novo ícone](../../assets/new.svg) **Adição do controle de versão das imagens do Docker**—Atualize agora o pacote `magento/magento-cloud-docker` para obter as imagens atualizadas.<!--MAGECLOUD-4737-->

- ![novo ícone](../../assets/new.svg) **Atualizações do contêiner**—

   - ![novo ícone](../../assets/new.svg) **contêiner PHP-FPM**—

      - ![novo ícone](../../assets/new.svg) **Adição de suporte a Node.js**—Atualização da imagem PHP-FPM para oferecer suporte aos recursos node, npm e grunt-cli dentro do contêiner PHP.<!--MAGECLOUD-3953-->

      - ![novo ícone](../../assets/new.svg) **Adição de suporte para [ionCube](https://www.ioncube.com/)**—Atualização da configuração padrão do Docker para oferecer suporte ao ionCube no ambiente de desenvolvimento do Docker local.<!--MAGECLOUD-4354-->

   - ![novo ícone](../../assets/new.svg) **Contêiner da Web**—

      - ![novo ícone](../../assets/new.svg) **Personalizar configuração do NGINX**—Adicionou a capacidade de montar um arquivo personalizado `nginx.conf` no ambiente do Cloud Docker for Commerce. Consulte [contêiner da Web](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#web-container).<!--MAGECLOUD-4204-->

      - ![novo ícone](../../assets/new.svg) **Certificados NGINX gerados automaticamente**—O arquivo de configuração do Docker agora inclui a configuração para gerar automaticamente certificados NGINX para o contêiner da Web.<!--MAGECLOUD-4258-->

   - ![novo ícone](../../assets/new.svg) **Novo contêiner Selenium**—Adicionou um [contêiner Selenium](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#selenium-container) para suportar o teste de aplicativos Adobe Commerce usando o MFTF (Estrutura de Teste Funcional) do Magento.<!--MAGECLOUD-4040-->

   - ![novo ícone](../../assets/new.svg) **[!DNL RabbitMQ]versão suporte**—Atualizado a configuração de contêiner [!DNL RabbitMQ] para suportar [!DNL RabbitMQ] versão 3.8.<!--MAGECLOUD-4674-->

   - ![Ícone de correção](../../assets/fix.svg) **Contêiner de banco de dados persistente** — O volume de banco de dados `magento-db: /var/lib/mysql` agora persiste depois que você interrompe e remove a configuração do Docker e restaura quando você reinicia a configuração do Docker. Agora, você deve excluir manualmente o volume do banco de dados. Consulte [Contêineres de banco de dados].<!--MAGECLOUD-3978-->

   - ![novo ícone](../../assets/new.svg) **Contêiner TLS**—

      - ![novo ícone](../../assets/new.svg) **Atualização da imagem base do contêiner para usar a imagem oficial**—A imagem do [contêiner TLS da nuvem](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#tls-container) agora se baseia na imagem oficial do `debian:jessie` Docker.—<!--MAGECLOUD-4163-->

      - ![novo ícone](../../assets/new.svg) **Adição de suporte para o [Proxy de Terminação TLS de Libra]**—O [arquivo de configuração de Libra](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) adiciona as seguintes variáveis ENV para personalizar a configuração do Docker para o contêiner TLS:

         - **`TimeOut`** — Define o valor de tempo limite de Tempo até o Primeiro Byte (TTFB). O valor padrão é de 300 segundos.

         - **`RewriteLocation`** — Determina se o proxy Libra reescreve o local no URL da solicitação por padrão. O padrão é `0`, para evitar que a regravação interrompa os redirecionamentos para sites externos, como um site SSO externo. [Correção enviada por Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![novo ícone](../../assets/new.svg) Aumento do valor de tempo limite na configuração do contêiner TLS de 15 para 300 segundos. [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![novo ícone](../../assets/new.svg) **Contêiner de verniz**—

      - ![novo ícone](../../assets/new.svg) **Atualizado a imagem base do contêiner para usar a imagem oficial**—O [contêiner de Verniz da Nuvem](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container) agora se baseia na imagem oficial do `centos` Docker.<!--MAGECLOUD-4163-->

      - ![novo ícone](../../assets/new.svg) **Configuração de tempo limite padrão aprimorada**-Adição da configuração `.first_byte_timeout` e `.between_bytes_timeout` ao contêiner Verniz. Ambos os valores de tempo limite são padronizados como `300s` (5 minutos). [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![ícone de correção](../../assets/fix.svg) **Ignorar Verniz durante as sessões de Xdebug**—Atualizou a configuração do contêiner Verniz para retornar `pass` sobre as solicitações recebidas quando Xdebug estava habilitado. Em versões anteriores, não era possível usar o Xdebug se o ambiente do Docker incluísse verniz. [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![novo ícone](../../assets/new.svg) **Alterações na configuração do Docker**—

   - ![novo ícone](../../assets/new.svg) **Gerenciar montagens e volumes do seu projeto**—Adicionou a capacidade de gerenciar montagens e volumes ao iniciar um ambiente do Docker para desenvolvimento local. Consulte [Compartilhando dados do projeto].<!--MAGECLOUD-3248-->

   - ![novo ícone](../../assets/new.svg) **Suporte para o modo ponte de rede**—Adicionou suporte para o modo ponte de rede para habilitar conexões entre contêineres Docker na rede local.<!--MAGECLOUD-4165-->

   - ![novo ícone](../../assets/new.svg) **Contêiner do Cron desabilitado por padrão**—Para melhorar o desempenho, o contêiner do Cron não é mais configurado por padrão quando você compila o ambiente do Docker. Você pode usar a opção `--with-cron` no comando de compilação do Docker para adicionar um contêiner Cron ao seu ambiente. Consulte [Gerenciamento de trabalhos cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure#manage-cron-jobs).<!--MAGECLOUD-5181-->

   - ![novo ícone](../../assets/new.svg) **Parar de sincronizar arquivos de backup grandes**—Despejos de BD e arquivos mortos—ZIP, SQL, GZ e BZ2—adicionados à lista de exclusão nos arquivos `dist/docker-sync.yml` e `dist/mutagen.sh`. A sincronização de arquivos grandes (>1 GB) pode causar um período de inatividade e os arquivos de backup normalmente não exigem sincronização, pois você pode gerá-los novamente.<!--MAGECLOUD-3979-->

- ![novo ícone](../../assets/new.svg) **Alterações de comando**—

   - ![ícone de correção](../../assets/fix.svg) Renomeou o arquivo `./bin/docker` como `./bin/magento-docker` para corrigir um problema que causava a quebra de alguns ambientes do Docker porque o arquivo `./bin/docker` substitui arquivos binários existentes do Docker. Esta é uma [alteração incompatível com versões anteriores](backward-incompatible-changes.md) que requer atualizações para seus scripts e comandos.<!-- MAGECLOUD-4038 -->

   - ![novo ícone](../../assets/new.svg) **Adicionada uma opção de configuração de serviço para expor a porta do banco de dados ao host**—Use a opção `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` para expor a porta do banco de dados ao host ao compilar o arquivo `docker-compose.yml`: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![novo ícone](../../assets/new.svg) **Novo comando pós-implantação** — Anteriormente, os ganchos pós-implantação definidos no arquivo `.magento.app.yaml` eram executados automaticamente depois de você ter implantado o Adobe Commerce em um contêiner do Cloud Docker usando o comando `cloud-deploy`. Agora, você deve emitir um comando `cloud-post-deploy` separado para executar os ganchos pós-implantação após a implantação. Veja as instruções de inicialização atualizadas para o [desenvolvedor](https://developer.adobe.com/commerce/cloud-tools/docker/deploy) e o modo de [produção](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode).<!--MAGECLOUD-3996-->

   - ![novo ícone](../../assets/new.svg) Adicionada a opção `--rm` aos comandos `./bin/magento-docker` para a compilação e implantação de contêineres. Isso remove o contêiner após a conclusão da tarefa.<!--MAGECLOUD-4205-->

   - ![novo ícone](../../assets/new.svg) **Atualizações para `build:compose` comando**—

      - ![novo ícone](../../assets/new.svg) Adicionada a opção `--sync-engine="native"` ao comando `docker-build` para desabilitar a sincronização de arquivos ao gerar o arquivo de configuração Docker Compose no modo de desenvolvedor. Use essa opção ao desenvolver em sistemas Linux, que não exigem sincronização de arquivos para o desenvolvimento local do Docker. Consulte [Sincronização de dados no ambiente do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![novo ícone](../../assets/new.svg) alterou a configuração de sincronização de arquivos padrão de `docker-sync` para `native`. [Correção enviada por Mathew Beane da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![novo ícone](../../assets/new.svg) **Melhorias na validação**—

   - ![novo ícone](../../assets/new.svg) Adicionado validação ao processo de implantação para ambientes de desenvolvimento do Docker local para verificar se a configuração do ambiente de nuvem inclui a chave de criptografia necessária para descriptografar o banco de dados. Agora, você receberá uma mensagem de erro no log se a configuração do ambiente não especificar um valor para a chave de criptografia.<!--MAGECLOUD-4423-->

   - ![novo ícone](../../assets/new.svg) Adicionado uma verificação de integridade de contêiner ao serviço Elasticsearch para garantir que o serviço esteja pronto antes de continuar com o processamento de compilação e implantação. Se a verificação de integridade retornar um erro, o contêiner será reiniciado automaticamente.<!--MAGECLOUD-4456-->
