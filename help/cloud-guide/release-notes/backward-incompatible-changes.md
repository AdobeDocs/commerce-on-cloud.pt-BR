---
title: Alterações incompatíveis com versões anteriores
description: Saiba mais sobre compatibilidade com versões anteriores ao atualizar projetos existentes na nuvem.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Alterações incompatíveis com versões anteriores

Alterações incompatíveis com versões anteriores podem exigir que você ajuste a configuração e os processos da nuvem para projetos existentes da nuvem ao atualizar para a versão mais recente do pacote `ece-tools` ou outro Conjunto de ferramentas da nuvem para pacotes do Commerce.

## Alterações no pacote `ece-tools`

Algumas funcionalidades incluídas anteriormente no pacote `ece-tools` agora são fornecidas em pacotes separados. Esses pacotes são dependências do compositor para `ece-tools`, que são instaladas e atualizadas automaticamente quando você instala ou atualiza as ferramentas ece.

A nova arquitetura não deve afetar seus processos de instalação ou atualização. No entanto, talvez seja necessário alterar alguns processos e sintaxe de comando ao trabalhar com o Adobe Commerce em um projeto de infraestrutura em nuvem. Para obter detalhes, reveja as seguintes informações de alterações incompatíveis com versões anteriores e as [notas de versão do Conjunto de Ferramentas da Nuvem](cloud-tools-suite.md).

### Alterações de requisito de versão de serviço

Alteramos o requisito mínimo de versão do PHP de 7.0.x para 7.1.x para projetos na nuvem que usam o `ece-tools` v2002.1.0 e posterior. Se a configuração de seu ambiente especifica o PHP 7.0, atualize a [configuração do php](../application/php-settings.md) no arquivo `.magento.app.yaml`.

>[!WARNING]
>
>Devido à alteração do requisito de versão do PHP, o `ece-tools` 2002.1.0 oferece suporte somente ao Adobe Commerce em projetos de infraestrutura em nuvem que executam o Adobe Commerce 2.1.15 ou posterior. Se o seu projeto usa uma versão anterior, você deve [atualizar](../development/commerce-version.md) antes de atualizar para o `ece-tools` 2002.1.0.

### Alterações na configuração do ambiente

A tabela a seguir fornece informações sobre variáveis de ambiente e outros arquivos de configuração de ambiente que foram removidos ou descontinuados no `ece-tools` v2002.1.0.

| Item | Substituição |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` variável | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` variável | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` variável | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` variável | Nenhum. Agora, a compilação sempre cria um link simbólico para o diretório de conteúdo estático `pub/static`. |
| `build_options.ini` arquivo | Use o arquivo [`.magento.env.yaml`](../application/configure-app-yaml.md) para configurar variáveis de ambiente para gerenciar ações de compilação e implantação em todos os seus ambientes.<p>Se você criar um ambiente de nuvem que inclua o arquivo `build_options.ini`, a compilação falhará. |

### Alterações no comando da CLI

A tabela a seguir resume as alterações de comando da CLI no ECE-Tools v2002.1.0 que podem exigir a atualização de comandos ou scripts.

| Comando | Substituição |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

Em versões anteriores do ECE-Tools, você poderia usar os comandos `m2-ece-build` e `m2-ece-deploy` para configurar ganchos de implantação no arquivo `.magento.app.yaml`. Ao atualizar para v2002.1.0, verifique a configuração do `hooks` no arquivo `.magento.app.yaml` quanto aos comandos obsoletos e substitua-os, se necessário.

## Alterações nos patches de nuvem

- **Remover patches baixados**-O pacote `magento/magento-cloud-patches` agrupa todos os patches disponíveis na página [downloads de software](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) e os aplica automaticamente ao implantar na Nuvem. Para evitar conflitos de patch depois de atualizar para ECE-Tools 2002.1.0 ou posterior, remova todos os patches fornecidos pela Adobe que você baixou e adicionou ao projeto manualmente.

- **Atualizando o comando aplicar patches** - Movemos o comando para aplicar patches do diretório `vendor/bin/ece-tools` para o diretório `vendor/bin/ece-patches`. Se você usar este comando para aplicar patches manualmente, use o novo caminho.

  > Aplicar patches manualmente

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Alterações no Cloud Docker

- **O requisito mínimo de versão do PHP agora é o PHP 7.1**-Se o seu Cloud Docker para o host Commerce estiver executando uma versão anterior, atualize para o PHP v7.1 ou posterior.

- **Alterações no comando do Cloud Docker para Commerce**-

   - **Atualizando o Cloud Docker para comandos do Commerce para operações de compilação do Docker**-Movemos o Cloud Docker para comandos do Commerce do diretório `vendor/bin/ece-tools` para o diretório `vendor/bin/ece-docker`. Atualize seus scripts e comandos para usar o novo caminho.

     Depois de atualizar para o `ece-tools` 2002.1.0, use o seguinte comando para exibir os comandos `ece-docker` disponíveis.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Atualizando os comandos docker-compose da Nuvem**-Renomeamos o caminho para o arquivo de comando de `./bin/docker` para `./bin/magento-docker`. Atualize seus scripts e comandos para usar o novo caminho.

   - **O contêiner Cron não está mais incluído na configuração padrão do Docker**-Agora, você deve adicionar a opção `--with-cron` ao comando `ece-docker build:compose` para incluir o contêiner Cron na configuração do ambiente Docker. Consulte [Gerenciar trabalhos do cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs) no guia do _Cloud Docker for Commerce_.

     Os scripts que geravam contêineres anteriormente com trabalhos cron agora estão sem o contêiner cron.

   - **Usando contêineres temporários**-Em versões anteriores, os contêineres criados pelas operações de comando `bin/magento-docker` não foram removidos, portanto, você pode usá-los para outras operações. Agora, os comandos `magento-docker` removem todos os contêineres criados após a conclusão do comando.

     Para manter um contêiner criado por uma operação docker-compose, use o comando `docker-compose run` em vez do comando `bin/magento-docker`.

   - **Executando ganchos pós-implantação**-O comando `cloud-deploy` não executa mais ganchos pós-implantação. Use o novo comando `cloud-post-deploy` para executar ganchos pós-implantação após a implantação. Atualize seus scripts para adicionar o comando para executar ganchos pós-implantação.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     Como alternativa, se você usar comandos `docker-compose` diretamente, execute o comando `docker-compose run deploy cloud-post-deploy` após o comando de implantação.

- **Atualizando o banco de dados** - O contêiner Banco de Dados agora está armazenado no volume Docker persistente `magento-db`. Ao atualizar o ambiente do Docker, o banco de dados não é mais excluído automaticamente. Se necessário, use um dos comandos a seguir para removê-lo manualmente.

   - Remover o contêiner `magento-db`:

     ```bash
     docker volume rm magento-db
     ```

   - Remova todos os volumes associados ao desligar os contêineres Docker:

     ```bash
     docker-compose down -v
     ```

- **Substituir configurações de sincronização de arquivos para arquivos mortos e de backup**-O arquivo morto e os arquivos de backup com as seguintes extensões não são mais sincronizados quando o docker-sync ou o mutagen são usados: SQL, GZ, ZIP e BZ2. Você pode substituir a sincronização de arquivos padrão desses tipos de arquivos renomeando o arquivo para que termine com uma extensão diferente. Por exemplo: `synchronize-me.zip-backup`
