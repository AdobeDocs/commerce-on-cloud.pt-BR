---
user-guide-title: Guia do Commerce na nuvem
user-guide-description: Saiba como gerenciar o aplicativo do Adobe Commerce na infraestrutura em nuvem.
product: magento
feature: Cloud
source-git-commit: fd7879e8f3c9e1965cf4aa3d99824e577971529d
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 7%

---


# Commerce na infraestrutura em nuvem {#user-guide}

+ [Commerce](overview.md)
+ Arquitetura {#architecture}
   + [Infraestrutura em nuvem](architecture/cloud-architecture.md)
   + [Segurança](architecture/security.md)
   + [Pilha de tecnologia](architecture/tech-stack.md)
   + [Arquitetura inicial](architecture/starter-architecture.md)
   + [Fluxo de trabalho inicial](architecture/starter-develop-deploy-workflow.md)
   + [Arquitetura Pro](architecture/pro-architecture.md)
   + [Fluxo de trabalho Pro](architecture/pro-develop-deploy-workflow.md)
   + [Arquitetura dimensionada](architecture/scaled-architecture.md)
   + [Dimensionamento automático](architecture/autoscaling.md)
+ [Introdução](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html?lang=pt-BR)
+ Notas de versão {#release-notes}
   + [Conjunto de ferramentas da nuvem](release-notes/cloud-tools-suite.md)
   + [Pacote ECE-Tools](release-notes/ece-tools-package.md)
   + [Patches da nuvem](release-notes/cloud-patches.md)
   + [Pacote do Cloud Docker](release-notes/cloud-docker.md)
   + [Componentes da nuvem](release-notes/cloud-components.md)
   + [Pacotes na nuvem](release-notes/cloud-packages.md)
   + [Alterações incompatíveis com versões anteriores](release-notes/backward-incompatible-changes.md)
   + [Arquivo de notas de versão](release-notes/cloud-release-archive.md)
+ Projeto na nuvem {#project}
   + [Visão geral do projeto](project/overview.md)
   + [Estrutura de projeto](project/file-structure.md)
   + [Acesso do usuário](project/user-access.md)
   + [Autenticação multifator](project/multi-factor-authentication.md)
   + [Fluxo de atividade](project/activity-stream.md)
   + [Emails de saída](project/outgoing-emails.md)
   + [Serviço de email SendGrid](project/sendgrid.md)
   + [Gerenciamento de ramificação do console](project/console-branches.md)
   + [Endereços IP regionais](project/regional-ip-addresses.md)
+ Ferramentas do desenvolvedor {#dev-tools}
   + [Visão geral](dev-tools/overview.md)
   + CLI da nuvem {#cloud-cli}
      + [Visão geral da CLI](dev-tools/cloud-cli-overview.md)
      + [Referência da CLI](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [Visão geral do pacote](dev-tools/package-overview.md)
      + [Atualização única para usar ECE-Tools](dev-tools/install-package.md)
      + [Atualizar pacote ECE-Tools](dev-tools/update-package.md)
      + [Referência da CLI](dev-tools/ece-tools-cli-reference.md)
      + [Referência de erro](dev-tools/error-reference.md)
   + Integrações {#integrations}
      + [Visão geral](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [Notificações de integridade](integrations/health-notifications.md)
+ Desenvolvimento {#develop}
   + [Visão geral](development/overview.md)
   + [Chaves de autenticação](development/authentication-keys.md)
   + [Gerenciamento de ramificações CLI](development/cli-branches.md)
   + [Conexões seguras](development/secure-connections.md)
   + Implantar {#deploy}
      + [Processo de implantação](deploy/process.md)
      + [Otimização](deploy/optimization.md)
      + [Práticas recomendadas](deploy/best-practices.md)
      + [Implantação baseada em cenário](deploy/scenario-based.md)
      + [Implantação sem tempo de inatividade](deploy/reduce-downtime.md)
      + [Implantação de conteúdo estático](deploy/static-content.md)
      + [Assistentes inteligentes](deploy/smart-wizards.md)
      + [Implantar para preparo e produção](deploy/staging-production.md)
      + [Recuperação de falha de componente](deploy/recover-failed-deployment.md)
   + Teste {#test}
      + [Orientação de testes](test/guidance.md)
      + [Logs](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [Dados de exemplo](test/sample-data.md)
      + [Preparo e produção](test/staging-and-production.md)
      + [Segundo ambiente de preparo](test/second-staging.md)
   + [Serviço PrivateLink](development/privatelink-service.md)
   + [Bloqueio protetor](development/protective-block.md)
   + [Restaurar ambiente](development/restore-environment.md)
   + Armazenamento {#storage}
      + [Gerenciar espaço em disco](storage/manage-disk-space.md)
      + [Consultas ao banco de dados de perfil](storage/profile-database-queries.md)
      + [Fazer backup do banco de dados](storage/database-dump.md)
      + [Gerenciamento de backup](storage/snapshots.md)
   + Atualizações e patches {#upgrade}
      + [Práticas recomendadas](development/best-practices.md)
      + [Atualizar versão do Commerce](development/commerce-version.md)
      + [Aplicar patches](development/apply-patches.md)
+ Configuração {#configure}
   + [Visão geral](environment/overview.md)
   + Aplicativo {#app}
      + [Configurar implantação de aplicativo](application/configure-app-yaml.md)
      + [Configurações do PHP](application/php-settings.md)
      + Propriedades {#properties}
         + [Propriedades do aplicativo](application/properties.md)
         + [Crons](application/crons-property.md)
         + [Firewall (somente para iniciador)](application/firewall-property.md)
         + [Ganchos](application/hooks-property.md)
         + [Variáveis](application/variables-property.md)
         + [Web](application/web-property.md)
         + [Trabalhadores](application/workers-property.md)
      + [Definir cache para arquivos estáticos](application/set-cache.md)
   + Ambiente {#env}
      + [Configurar implantação de ambiente](environment/configure-env-yaml.md)
      + [Níveis e opções variáveis](environment/variable-levels.md)
      + Substituir variáveis {#stage}
         + [Variáveis de ambiente](environment/variables-intro.md)
         + [ADMIN](environment/variables-admin.md)
         + [Variáveis da nuvem](environment/variables-cloud.md)
         + [Global](environment/variables-global.md)
         + [Build](environment/variables-build.md)
         + [Implantar](environment/variables-deploy.md)
         + [Pós-implantação](environment/variables-post-deploy.md)
      + Configurar notificações {#log}
         + [Notificação](environment/set-up-notifications.md)
         + [Manipuladores de log](environment/log-handlers.md)
   + Rotas {#routes}
      + [Configurar rotas](routes/routes-yaml.md)
      + [Armazenamento em cache](routes/caching.md)
      + [Redirecionamentos](routes/redirects.md)
      + [Inclusões do lado do servidor](routes/server-side-includes.md)
   + Serviços {#service}
      + [Configurar serviços](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Valkey](services/valkey.md)
+ Fastly services {#cdn}
   + [Visão geral](cdn/fastly.md)
   + Instalação do Fastly {#setup-fastly}
      + [Configurar os serviços do Fastly](cdn/fastly-configuration.md)
      + [Personalizar configuração do cache](cdn/fastly-custom-cache-configuration.md)
      + [Personalizar páginas de erro e manutenção](cdn/fastly-custom-response.md)
   + [Firewall de Aplicativo Web](cdn/fastly-waf-service.md)
   + [Otimização de imagem](cdn/fastly-image-optimization.md)
   + Personalizar com VCL {#custom-vcl-snippets}
      + [Introdução](cdn/fastly-vcl-custom-snippets.md)
      + [Redirecionar solicitações para um back-end do CMS](cdn/fastly-vcl-wordpress.md)
      + [Bloquear spam de referência](cdn/fastly-vcl-badreferer.md)
      + [INCLUIR NA LISTA DE PERMISSÕES IP](cdn/fastly-vcl-allowlist.md)
      + [INCLUIR NA LISTA DE BLOQUEIOS IP](cdn/fastly-vcl-blocking.md)
      + [Ignorar cache Fastly](cdn/fastly-vcl-bypass-to-origin.md)
   + [Solução de problemas rápida](cdn/fastly-troubleshooting.md)
+ Configurações de armazenamento {#configure-store}
   + [Visão geral](store/overview.md)
   + [Práticas recomendadas](store/best-practices.md)
   + [Tema personalizado](store/custom-theme.md)
   + [Extensões](store/extensions.md)
   + [Módulo B2B](store/b2b-module.md)
   + [Vários sites](store/multiple-sites.md)
   + [Mapa do site e robôs de mecanismo de pesquisa](store/robots-sitemap.md)
   + [Métodos de pagamento do PayPal](store/paypal.md)
   + [Gerenciamento de configuração](store/store-settings.md)
+ Site de inicialização {#launch}
   + [Visão geral](launch/overview.md)
   + [Lista de verificação de inicialização](launch/checklist.md)
   + [Etapas de inicialização](launch/steps.md)
+ Monitorar site {#monitor}
   + [Desempenho](monitor/performance.md)
   + Serviço New Relic {#new-relic}
      + [Visão geral do New Relic](monitor/new-relic-service.md)
      + [Gerenciamento de contas e usuários](monitor/account-management.md)
      + Investigar o desempenho {#investigate}
         + [Políticas, alertas e fluxos de trabalho](monitor/investigate-performance.md)
         + [Assimilação de dados](monitor/ingest-data.md)
         + [Rastrear implantações](monitor/track-deployments.md)
      + [Gerenciamento de logs](monitor/log-management.md)
