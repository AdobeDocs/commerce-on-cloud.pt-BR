---
source-git-commit: 7f2934af84c947046fed3a32c3b6e2937aed418a
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# Códigos de erro do ECE-Tools

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## Erros Críticos

Erros críticos indicam um problema com a configuração do projeto Commerce na infraestrutura em nuvem que causa falha na implantação, por exemplo, configuração incorreta, não compatível ou ausente para as configurações necessárias. Antes de implantar, é necessário atualizar a configuração para resolver esses erros.

### Fase de criação

| Código de erro | Etapa de criação | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 2 |  | Não é possível gravar no arquivo `./app/etc/env.php` | O script de implantação não pode fazer as alterações necessárias no arquivo `/app/etc/env.php`. Verifique as permissões do sistema de arquivos. |
| 3 |  | Configuração não definida no arquivo `schema.yaml` | Configuração não definida no arquivo `./vendor/magento/ece-tools/config/schema.yaml`. Verifique se o nome da variável de configuração está correto e definido. |
| 4 |  | Falha ao analisar o arquivo `.magento.env.yaml` | O formato de arquivo `./.magento.env.yaml` é inválido. Use um analisador YAML para verificar a sintaxe e corrigir erros. |
| 5 |  | Não é possível ler o arquivo `.magento.env.yaml` | Não é possível ler o arquivo `./.magento.env.yaml`. Verifique as permissões do arquivo. |
| 6 |  | Não é possível ler o arquivo `.schema.yaml` | Não é possível ler o arquivo `./vendor/magento/ece-tools/config/magento.env.yaml`. Verifique as permissões do arquivo e reimplante (`magento-cloud environment:redeploy`). |
| 7 | módulos de atualização | Não é possível gravar no arquivo `./app/etc/config.php` | O script de implantação não pode fazer as alterações necessárias no arquivo `/app/etc/config.php`. Verifique as permissões do sistema de arquivos. |
| 8 | validate-config | Não é possível ler o arquivo `composer.json` | Não é possível ler o arquivo `./composer.json`. Verifique as permissões do arquivo. |
| 9 | validate-config | O arquivo `composer.json` não tem a seção de carregamento automático necessária | A seção `autoload` necessária está ausente do arquivo `composer.json`. Compare a seção de carregamento automático com o arquivo `composer.json` no modelo de Nuvem e adicione a configuração ausente. |
| 10 | validate-config | O arquivo `.magento.env.yaml` contém uma opção que não está declarada no esquema ou uma opção configurada com um valor ou estágio inválido | O arquivo `./.magento.env.yaml` contém configuração inválida. Verifique o log de erros para obter informações detalhadas. |
| 11 | módulos de atualização | Falha no comando: `/bin/magento module:enable --all` | Tente executar `composer update` localmente. Em seguida, confirme e envie por push o arquivo `composer.lock` atualizado. Além disso, consulte o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 12 | apply-patches | Falha ao aplicar patch |  |
| 13 | set-report-dir-nested-level | Não é possível gravar no arquivo `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | Falha ao copiar arquivos de dados de amostra |  |
| 15 | compile-id | Falha no comando: `/bin/magento setup:di:compile` | Verifique o `cloud.log` para obter mais informações. Adicione `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obter uma saída de comando mais detalhada. |
| 16 | dump-autoload | Falha no comando: `composer dump-autoload` | Falha no comando `composer dump-autoload`. Verifique o `cloud.log` para obter mais informações. |
| 17 | enfardador | Falha no comando para executar `Baler` para agrupamento de JavaScript | Verifique a variável de ambiente `SCD_USE_BALER` para verificar se o módulo Baler está configurado e habilitado para agrupamento JS. Se você não precisar do módulo Baler, defina `SCD_USE_BALER: false`. |
| 18 | compress-static-content | O utilitário necessário não foi encontrado (tempo limite, bash) |  |
| 19 | deploy-static-content | Falha no comando `/bin/magento setup:static-content:deploy` | Verifique o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 20 | compress-static-content | Falha na compactação de conteúdo estático | Verifique o `cloud.log` para obter mais informações. |
| 21 | backup-data: conteúdo estático | Falha ao copiar conteúdo estático para o diretório `init` | Verifique o `cloud.log` para obter mais informações. |
| 22 | backup-data: writable-dirs | Falha ao copiar alguns diretórios graváveis para o diretório `init` | Falha ao copiar diretórios graváveis na pasta `./init`. Verifique as permissões do sistema de arquivos. |
| 23 |  | Não é possível criar um objeto do agente de log |  |
| 24 | backup-data: conteúdo estático | Falha ao limpar o diretório `./init/pub/static/` | Falha ao limpar a pasta `./init/pub/static`. Verifique as permissões do sistema de arquivos. |
| 25 |  | Não é possível localizar o pacote do Composer | Se você instalou a versão do aplicativo Adobe Commerce diretamente do repositório GitHub, verifique se a variável de ambiente `DEPLOYED_MAGENTO_VERSION_FROM_GIT` está configurada. |
| 26 | validate-config | Remova a configuração do módulo Magento Braintree que não é mais compatível com o Adobe Commerce e o Magento Open Source 2.4 e versões posteriores. | O suporte para o módulo Braintree não está mais incluído no Magento 2.4.0 e posterior. Remova a variável CONFIG_STORES_DEFAULT_PAYMENT_BRAINTREE_CHANNEL da seção de variáveis do arquivo `.magento.app.yaml`. Para obter suporte ao pagamento do Braintree, use uma extensão oficial do Commerce Marketplace. |

### Implantar estágio

| Código de erro | Implantar etapa | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 101 | pré-implantação: cache | Configuração de cache incorreta (porta ou host ausente) | A configuração de cache não tem os parâmetros necessários `server` ou `port`. Verifique o `cloud.log` para obter mais informações. |
| 102 |  | Não é possível gravar no arquivo `./app/etc/env.php` | O script de implantação não pode fazer as alterações necessárias no arquivo `/app/etc/env.php`. Verifique as permissões do sistema de arquivos. |
| 103 |  | Configuração não definida no arquivo `schema.yaml` | Configuração não definida no arquivo `./vendor/magento/ece-tools/config/schema.yaml`. Verifique se o nome da variável de configuração está correto e se está definido. |
| 104 |  | Falha ao analisar o arquivo `.magento.env.yaml` | Configuração não definida no arquivo `./vendor/magento/ece-tools/config/schema.yaml`. Verifique se o nome da variável de configuração está correto e se está definido. |
| 105 |  | Não é possível ler o arquivo `.magento.env.yaml` | Não é possível ler o arquivo `./.magento.env.yaml`. Verifique as permissões do arquivo. |
| 106 |  | Não é possível ler o arquivo `.schema.yaml` |  |
| 107 | pré-implantação: clean-redis-cache | Falha ao limpar o cache Redis | Falha ao limpar o cache Redis. Verifique se a configuração do cache Redis está correta e se o serviço Redis está disponível. Consulte [Configurar serviço Redis](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html?lang=pt-BR). |
| 140 | pré-implantação: clean-valkey-cache | Falha ao limpar o cache Valkey | Falha ao limpar o cache Valkey. Verifique se a configuração do cache Valkey está correta e se o serviço Valkey está disponível. Consulte [Serviço Valkey de Instalação](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/valkey.html?lang=pt-BR). |
| 108 | pré-implantação: set-production-mode | Falha no comando `/bin/magento maintenance:enable` | Verifique o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 109 | validate-config | Configuração de banco de dados incorreta | Verifique se a variável de ambiente `DATABASE_CONFIGURATION` está configurada corretamente. |
| 110 | validate-config | Configuração de sessão incorreta | Verifique se a variável de ambiente `SESSION_CONFIGURATION` está configurada corretamente. A configuração deve conter pelo menos o parâmetro `save`. |
| 111 | validate-config | Configuração de pesquisa incorreta | Verifique se a variável de ambiente `SEARCH_CONFIGURATION` está configurada corretamente. A configuração deve conter pelo menos o parâmetro `engine`. |
| 112 | validate-config | Configuração de recurso incorreta | Verifique se a variável de ambiente `RESOURCE_CONFIGURATION` está configurada corretamente. A configuração deve conter pelo menos o parâmetro `connection`. |
| 113 | validate-config:elasticsuite-integridade | O ElasticSuite está instalado, mas o serviço Elasticsearch não está disponível | Verifique se a variável de ambiente `SEARCH_CONFIGURATION` está configurada corretamente e se o serviço Elasticsearch está disponível. |
| 114 | validate-config:elasticsuite-integridade | O ElasticSuite está instalado, mas outro mecanismo de pesquisa é usado | O ElasticSuite está instalado, mas outro mecanismo de pesquisa está configurado. Atualize a variável de ambiente `SEARCH_CONFIGURATION` para habilitar o Elasticsearch e verifique a configuração do serviço Elasticsearch no arquivo `services.yaml`. |
| 115 |  | Falha na execução da consulta ao banco de dados |  |
| 116 | instalação-atualização: configuração | Falha no comando `/bin/magento setup:install` | Verifique os `cloud.log` e `install_upgrade.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 117 | install-update: config-import | Falha no comando `app:config:import` | Verifique o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 118 |  | O utilitário necessário não foi encontrado (tempo limite, bash) |  |
| 119 | install-update: deploy-static-content | Falha no comando `/bin/magento setup:static-content:deploy` | Verifique o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 120 | compress-static-content | Falha na compactação de conteúdo estático | Verifique o `cloud.log` para obter mais informações. |
| 121 | deploy-static-content:generate | Não é possível atualizar a versão implantada | Não é possível atualizar o arquivo `./pub/static/deployed_version.txt`. Verifique as permissões do sistema de arquivos. |
| 122 | clean-static-content | Falha ao limpar arquivos de conteúdo estático |  |
| 123 | install-update: split-db | Falha no comando `/bin/magento setup:db-schema:split` | Verifique o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 124 | clean-view-preprocessed | Falha ao limpar a pasta `var/view_preprocessed` | Não é possível limpar a pasta `./var/view_preprocessed`. Verifique as permissões do sistema de arquivos. |
| 125 | install-update: reset-password | Falha ao atualizar o arquivo `/var/credentials_email.txt` | Falha ao atualizar o arquivo `/var/credentials_email.txt`. Verifique as permissões do sistema de arquivos. |
| 126 | install-update: update | Falha no comando `/bin/magento setup:upgrade` | Verifique os `cloud.log` e `install_upgrade.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 127 | clean-cache | Falha no comando `/bin/magento cache:flush` | Verifique o `cloud.log` para obter mais informações. Para obter uma saída de comando mais detalhada, adicione a opção `VERBOSE_COMMANDS: '-vvv'` ao arquivo `.magento.env.yaml`. |
| 128 | disable-maintenance-mode | Falha no comando `/bin/magento maintenance:disable` | Verifique o `cloud.log` para obter mais informações. Adicione `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obter uma saída de comando mais detalhada. |
| 129 | install-update: reset-password | Não é possível ler o modelo de redefinição de senha |  |
| 130 | install-update: cache_type | Falha no comando: `php ./bin/magento cache:enable` | O comando `php ./bin/magento cache:enable` é executado somente quando o Adobe Commerce foi instalado, mas o arquivo `./app/etc/env.php` estava ausente ou vazio no início da implantação. Verifique o `cloud.log` para obter mais informações. Adicione `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obter uma saída de comando mais detalhada. |
| 131 | install-update | O valor da chave `crypt/key` não existe no arquivo `./app/etc/env.php` ou na variável de ambiente de nuvem `CRYPT_KEY` | Este erro ocorre se o arquivo `./app/etc/env.php` não estiver presente quando a implantação do Adobe Commerce começar ou se o valor `crypt/key` estiver indefinido. Se você migrou o banco de dados de outro ambiente, recupere o valor da chave de criptografia desse ambiente. Em seguida, adicione o valor à variável de ambiente de nuvem [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html?lang=pt-BR#crypt_key) no ambiente atual. Consulte [Chave de criptografia do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html?lang=pt-BR#gather-credentials). Se você tiver removido acidentalmente o arquivo `./app/etc/env.php`, use o seguinte comando para restaurá-lo dos arquivos de backup criados de uma implantação anterior: comando da CLI `./vendor/bin/ece-tools backup:restore`.&quot; |
| 132 |  | Não é possível conectar-se ao serviço Elasticsearch | Verifique se há credenciais válidas do Elasticsearch e se o serviço está em execução |
| 137 |  | Não é possível conectar ao serviço OpenSearch | Verifique se há credenciais OpenSearch válidas e se o serviço está em execução |
| 133 | validate-config | Remova a configuração do módulo Magento Braintree que não é mais compatível com o Adobe Commerce ou Magento Open Source 2.4 e versões posteriores. | O suporte para o módulo Braintree não está mais incluído no Adobe Commerce ou Magento Open Source 2.4.0 e posterior. Remova a variável CONFIG_STORES_DEFAULT_PAYMENT_BRAINTREE_CHANNEL da seção de variáveis do arquivo `.magento.app.yaml`. Para obter suporte ao Braintree, use uma extensão oficial do Braintree Payments da Commerce Marketplace. |
| 134 | validate-config | O Adobe Commerce e o Magento Open Source 2.4.0 exigem a instalação do serviço Elasticsearch | Instalar o serviço Elasticsearch |
| 138 | validate-config | O Adobe Commerce e o Magento Open Source 2.4.4 exigem a instalação do serviço OpenSearch ou Elasticsearch | Instalar serviço OpenSearch |
| 135 | validate-config | O mecanismo de pesquisa deve ser definido para Elasticsearch para Adobe Commerce e Magento Open Source >= 2.4.0 | Verifique a variável SEARCH_CONFIGURATION para a opção `engine`. Se estiver configurado, remova a opção ou defina o valor como &quot;elasticsearch&quot;. |
| 136 | validate-config | O banco de dados dividido foi removido a partir do Adobe Commerce e do Magento Open Source 2.5.0. | Se você usar um banco de dados dividido, será necessário reverter ou migrar para um único banco de dados ou usar uma abordagem alternativa. |
| 139 | validate-config | Mecanismo de pesquisa incorreto | Esta versão do Adobe Commerce ou Magento Open Source não é compatível com OpenSearch. Usar as versões 2.3.7-p3, 2.4.3-p2 ou posterior |

### Estágio de pós-implantação

| Código de erro | Etapa pós-implantação | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 201 | is-deploy-failed | Falha no estágio de implantação |  |
| 202 |  | O arquivo `./app/etc/env.php` não é gravável | O script de implantação não pode fazer as alterações necessárias no arquivo `/app/etc/env.php`. Verifique as permissões do sistema de arquivos. |
| 203 |  | Configuração não definida no arquivo `schema.yaml` | Configuração não definida no arquivo `./vendor/magento/ece-tools/config/schema.yaml`. Verifique se o nome da variável de configuração está correto e se está definido. |
| 204 |  | Falha ao analisar o arquivo `.magento.env.yaml` | O formato de arquivo `./.magento.env.yaml` é inválido. Use um analisador YAML para verificar a sintaxe e corrigir erros. |
| 205 |  | Não é possível ler o arquivo `.magento.env.yaml` | Verifique as permissões do arquivo. |
| 206 |  | Não é possível ler o arquivo `.schema.yaml` |  |
| 207 | aquecimento | Falha ao pré-carregar algumas páginas de aquecimento |  |
| 208 | tempo para o primeiro byte | Falha ao testar o tempo até o primeiro byte (TTFB) |  |
| 227 | clean-cache | Falha no comando `/bin/magento cache:flush` | Verifique o `cloud.log` para obter mais informações. Adicione `VERBOSE_COMMANDS: '-vvv'` a `.magento.env.yaml` para obter uma saída de comando mais detalhada. |

### Geral

| Código de erro | Etapa geral | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 243 |  | Configuração não definida no arquivo `schema.yaml` | Verifique se o nome da variável de configuração está correto e se foi definido. |
| 244 |  | Falha ao analisar o arquivo `.magento.env.yaml` | O formato de arquivo `./.magento.env.yaml` é inválido. Use um analisador YAML para verificar a sintaxe e corrigir erros. |
| 245 |  | Não é possível ler o arquivo `.magento.env.yaml` | Não é possível ler o arquivo `./.magento.env.yaml`. Verifique as permissões do arquivo. |
| 246 |  | Não é possível ler o arquivo `.schema.yaml` |  |
| 247 |  | Não é possível gerar um módulo para eventos | Verifique o `cloud.log` para obter mais informações. |
| 248 |  | Não é possível habilitar um módulo para eventos | Verifique o `cloud.log` para obter mais informações. |
| 249 |  | Falha ao gerar o módulo AdobeCommerceWebhookPlugins | Verifique o `cloud.log` para obter mais informações. |
| 250 |  | Falha ao habilitar o módulo AdobeCommerceWebhookPlugins | Verifique o `cloud.log` para obter mais informações. |

## Erros de Aviso

Erros de aviso indicam um problema com a configuração do projeto Commerce na infraestrutura em nuvem, como configurações incorretas, obsoletas, sem suporte ou ausentes para recursos opcionais que podem afetar o funcionamento do site. Embora um aviso não cause falha na implantação, você deve revisar as mensagens de aviso e atualizar a configuração para resolvê-las.

### Fase de criação

| Código de erro | Etapa de criação | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 1001 | validate-config | O arquivo app/etc/config.php não existe |  |
| 1002 | validate-config | O .O arquivo /build_options.ini não é mais suportado |  |
| 1003 | validate-config | A seção de módulos está ausente no arquivo de configuração compartilhado |  |
| 1004 | validate-config | A configuração não é compatível com esta versão do Magento |  |
| 1005 | validate-config | Opções de SCD ignoradas |  |
| 1006 | validate-config | O estado configurado não é ideal |  |
| 1007 | enfardador | O agrupamento Baler JS não pode ser usado |  |

### Implantar estágio

| Código de erro | Implantar etapa | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 2001 | pré-implantação:cache | O cache está configurado para um serviço Redis que não está disponível. Configuração ignorada. |  |
| 2032 | pré-implantação:cache | O cache está configurado para um serviço Valkey que não está disponível. Configuração ignorada. |  |
| 2002 | validate-config | O estado configurado não é ideal |  |
| 2003 | validate-config | O valor de nível de aninhamento de diretório para o relatório de erros não foi configurado |  |
| 2004 | validate-config | Configuração inválida no .arquivo /pub/errors/local.xml. |  |
| 2005 | validate-config | Os dados do administrador são usados para criar um usuário administrador apenas durante a instalação inicial. Quaisquer alterações nos dados de Admin são ignoradas durante o processo de atualização. | Após a instalação inicial, é possível remover os dados do administrador da configuração. |
| 2006 | validate-config | O usuário administrador não foi criado, pois o email do administrador não foi definido | Após a instalação, é possível criar um usuário administrador manualmente: use o ssh para se conectar ao seu ambiente. Em seguida, execute o comando `bin/magento admin:user:create`. |
| 2007 | validate-config | Atualizar versão do php para a versão recomendada |  |
| 2008 | validate-config | O suporte a Solr foi descontinuado no Adobe Commerce e no Magento Open Source 2.1. |  |
| 2009 | validate-config | O Solr não é mais compatível com o Adobe Commerce e o Magento Open Source 2.2 ou posterior. |  |
| 2010 | validate-config | O serviço Elasticsearch é instalado na camada de infraestrutura, mas não é usado como um mecanismo de pesquisa. | Considere remover o serviço Elasticsearch da camada de infraestrutura para otimizar o uso de recursos. |
| 2011 | validate-config | A versão do serviço Elasticsearch na camada de infraestrutura não é compatível com a versão atual do módulo elasticsearch/elasticsearch, usado pelo aplicativo Adobe Commerce. |  |
| 2012 | validate-config | A configuração atual não é compatível com esta versão do Adobe Commerce |  |
| 2013 | validate-config | Opções de SCD ignoradas porque o processo de implantação não foi executado na fase de compilação |  |
| 2014 | validate-config | A configuração contém variáveis ou valores obsoletos |  |
| 2015 | validate-config | A configuração do ambiente não é válida |  |
| 2016 | validate-config | A configuração do tipo JSON não pode ser decodificada |  |
| 2017 | validate-config | A configuração atual não é compatível com esta versão do Adobe Commerce |  |
| 2018 | validate-config | Alguns serviços passaram no fim da vida útil |  |
| 2019 | validate-config | A opção de configuração de pesquisa MySQL está obsoleta | Em vez disso, use Elasticsearch. |
| 2029 | validate-config | O banco de dados dividido foi descontinuado no Adobe Commerce e no Magento Open Source 2.4.2 e será removido na versão 2.5. | Se você usar um banco de dados dividido, deve começar a planejar a reversão para um único banco de dados ou a migração para um único banco de dados ou usar uma abordagem alternativa. |
| 2020 | install-update | Instalação do Adobe Commerce concluída, mas o arquivo de configuração `app/etc/env.php` estava ausente ou vazio. | Os dados necessários são restaurados das configurações do ambiente e do arquivo .magento.env.yaml. |
| 2021 | install-update:db-connection | Para bancos de dados divididos, use conexões personalizadas |  |
| 2022 | install-update:db-connection | Você alterou para uma configuração de banco de dados incompatível com a conexão subordinada. |  |
| 2023 | install-update:split-db | A habilitação de um banco de dados dividido é ignorada. |  |
| 2024 | install-update:split-db | A variável SPLIT_DB não tem a configuração para tipos de conexão divididos. |  |
| 2025 | install-update:split-db | Conexão subordinada não definida. |  |
| 2026 | pré-implantação:restaurar-gravável-dirs | Falha ao restaurar alguns dados gerados durante a fase de compilação para os diretórios montados | Verifique o `cloud.log` para obter mais informações. |
| 2027 | validate-config:image-mode-variable | O valor de modo da variável de ambiente MAGE_MODE não é suportado | Remova a variável de ambiente MAGE_MODE ou altere seu valor para &quot;produção&quot;. O Adobe Commerce na infraestrutura em nuvem é compatível somente com o modo de &quot;produção&quot;. |
| 2028 | armazenamento remoto | Não foi possível habilitar o armazenamento remoto. | Verifique as credenciais do armazenamento remoto. |
| 2030 | validate-config | Os serviços Elasticsearch e OpenSearch são instalados na camada de infraestrutura. Adobe Commerce e Magento Open Source 2.4.4 e posteriores usam OpenSearch por padrão | Considere remover o serviço Elasticsearch ou OpenSearch da camada de infraestrutura para otimizar o uso de recursos. |

### Estágio de pós-implantação

| Código de erro | Etapa pós-implantação | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 3001 | validate-config | O log de depuração está ativado no Adobe Commerce | Para economizar espaço em disco, não ative o log de depuração para seus ambientes de produção. |
| 3002 | aquecimento | Não é possível buscar URLs de armazenamento |  |
| 3003 | aquecimento | Não é possível buscar o url da loja |  |
| 3004 | backup | Não é possível criar arquivos de backup |  |

### Geral

| Código de erro | Etapa geral | Descrição do erro (Título) | Ação sugerida |
| - | - | - | - |
| 4001 |  | Não é possível obter a contagem de processadores do sistema: |  |
