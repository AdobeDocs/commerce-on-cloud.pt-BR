---
title: Arquitetura inicial
description: Saiba mais sobre os ambientes compatíveis com a arquitetura Starter.
feature: Cloud, Paas
exl-id: 2f16cc60-b5f7-4331-b80e-43042a3f9b8f
source-git-commit: 2236d0b853e2f2b8d1bafcbefaa7c23ebd5d26b3
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 0%

---

# Arquitetura inicial

Sua arquitetura inicial do Adobe Commerce na infraestrutura em nuvem aceita até **quatro** ambientes, incluindo um ambiente `master` que contenha o código inicial do projeto, o ambiente de preparo e até dois ambientes de integração.

Todos os ambientes estão em contêineres PaaS (Platform as a service). Esses contêineres são implantados em contêineres altamente restritos em uma grade de servidores. Esses ambientes são somente leitura, aceitando alterações de código implantado de ramificações enviadas do espaço de trabalho local. Cada ambiente fornece um banco de dados e um servidor da Web.

>[!NOTE]
>
>Não é possível alterar as permissões em pastas somente leitura em nenhum dos ambientes Iniciais. Essa restrição protege a integridade e a segurança do aplicativo. As permissões de pasta nesses sistemas de arquivos somente leitura não podem ser alteradas — até mesmo o suporte não pode modificá-las. Quaisquer alterações devem ser feitas de uma ramificação no ambiente de desenvolvimento local e enviadas para o ambiente de aplicativo. Você pode usar qualquer metodologia de desenvolvimento e ramificação que desejar. Ao obter acesso inicial ao seu projeto, crie um ambiente `staging` do ambiente `master`. Em seguida, crie o ambiente `integration` ramificando de `staging`.

## Arquitetura do ambiente inicial

O diagrama a seguir mostra as relações hierárquicas dos ambientes iniciais.

![Modo de exibição de alto nível do projeto inicial](../../assets/starter/architecture.png)

## Ambiente de produção

O ambiente de produção fornece o código-fonte para implantar o Adobe Commerce na infraestrutura da nuvem que executa suas vitrines para um ou vários sites. O ambiente de produção usa o código da ramificação `master` para configurar e habilitar o servidor Web, o banco de dados, os serviços configurados e o código do aplicativo.

Como o ambiente `production` é somente leitura, use o ambiente `integration` para fazer alterações de código, implante na arquitetura do `integration` para o `staging` e, finalmente, no ambiente `production`. Consulte [Implantar armazenamento](../deploy/staging-production.md) e [Inicialização do site](../launch/overview.md).

A Adobe recomenda testar totalmente na ramificação `staging` antes de enviar para a ramificação `master`, que é implantada no ambiente `production`.

## Ambiente de preparo

A Adobe recomenda criar uma ramificação chamada `staging` de `master`. A ramificação `staging` implanta código no ambiente de preparo para fornecer um ambiente de pré-produção para testar código, módulos e extensões, gateways de pagamento, envio, dados de produtos e muito mais. Esse ambiente fornece a configuração de todos os serviços para corresponder ao ambiente de produção, incluindo o Fastly, o New Relic APM e a pesquisa.

Seções adicionais neste guia fornecem instruções para implantações de código final e testes de interações no nível de produção em um ambiente de preparo seguro. Para melhor desempenho e teste de recursos, replique o banco de dados no ambiente de preparo.

>[!WARNING]
>
>A Adobe recomenda testar cada interação do comerciante e do cliente no ambiente de preparo antes de implantar no ambiente de produção. Consulte [Implantar seu armazenamento](../deploy/staging-production.md) e [Testar implantação](../test/staging-and-production.md).

## Ambiente de integração

Os desenvolvedores usam o ambiente `integration` para desenvolver, implantar e testar:

- código de aplicativo do Adobe Commerce

- Código personalizado

- Extensões

- Serviços

**Casos de uso recomendados:**

Os ambientes de integração são projetados para testes e desenvolvimento limitados. Por exemplo, você pode usar o ambiente de integração para concluir as seguintes tarefas:

- Verifique se as alterações nos processos de integração contínua (CI) são compatíveis com a nuvem

- Teste fluxos de trabalho críticos em páginas principais como Página inicial, Categoria, Página de detalhes do produto (PDP), Check-out e Administração

Para obter o melhor desempenho no ambiente de integração, siga estas práticas recomendadas:

- Restringir o tamanho do catálogo - Para referência, os Dados de amostra contêm cerca de 2.048 produtos. Tente reduzir o tamanho do catálogo para cerca de 4.000 a 5.000 produtos.
Para verificar o número de produtos no catálogo, execute a seguinte consulta MySQL:

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Reduzir o número de grupos de clientes - ter muitos grupos de clientes pode afetar o desempenho da indexação e o desempenho geral.

- Limitar o uso a um ou dois usuários simultâneos

- Desative os trabalhos cron e execute manualmente conforme necessário

Você pode ter até **dois** ambientes de Integração ativos. Você cria um Ambiente de integração criando uma ramificação a partir da ramificação `staging`. Ao criar um ambiente de integração, o nome do ambiente corresponde ao nome da ramificação. Um ambiente de integração inclui um servidor Web e um banco de dados. Ela não inclui todos os serviços, por exemplo, o Fastly CDN e o New Relic não estão disponíveis.

Você pode ter um número ilimitado de ramificações inativas para armazenamento de código. Para acessar, exibir e testar uma ramificação inativa, é necessário ativá-la

{{enhanced-integration-envs}}

## Pilha de tecnologia de produção e preparo

Os ambientes de produção e de preparo incluem as seguintes tecnologias. Você pode modificar e configurar essas tecnologias por meio do arquivo [`.magento.app.yaml`](../application/configure-app-yaml.md).

- Fastly para cache HTTP e CDN
- Servidor Web Nginx falando com PHP-FPM, uma instância com vários workers
- Servidor Redis
- Elasticsearch para pesquisa de catálogo do Adobe Commerce 2.2 para 2.4.3-p2
- OpenSearch para pesquisa de catálogo para Adobe Commerce 2.3.7-p3, 2.4.3-p2 e 2.4.4 e posterior
- Filtragem de saída (firewall de saída)

### Serviços

O Adobe Commerce na infraestrutura em nuvem oferece suporte aos seguintes serviços no momento: PHP, MySQL (MariaDB), Elasticsearch (Adobe Commerce 2.2 a 2.4.3-p2), OpenSearch (2.3.7-p3, 2.4.3-p2, 2.4.4 e posterior), Redis e [!DNL RabbitMQ].

Cada serviço é executado em um contêiner seguro e separado. Os containers são gerenciados juntos no projeto. Alguns serviços são padrão, como os seguintes:

- Roteador HTTP (tratamento de solicitações recebidas, mas também armazenamento em cache e redirecionamentos)

- servidor de aplicativo PHP

- Git

- Shell seguro (SSH)

### Versões de software

O Adobe Commerce na infraestrutura em nuvem usa o sistema operacional Debian GNU/Linux e o servidor Web NGINX. Você não pode atualizar este software, mas pode configurar versões para o seguinte:

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

Nos ambientes de preparo e produção, use o Fastly para CDN e armazenamento em cache. A versão mais recente da extensão CDN do Fastly é instalada durante o provisionamento inicial do projeto. Você pode atualizar a extensão do para obter as correções de erros e melhorias mais recentes. Consulte [Módulo Fastly CDN para Magento 2](https://github.com/fastly/fastly-magento2). Além disso, você tem acesso ao [New Relic](../monitor/account-management.md) para monitoramento de desempenho.

Use os arquivos a seguir para configurar as versões de software que deseja usar na implementação.

- [&quot;.magento.app.yaml&quot;](../application/configure-app-yaml.md)

- [&quot;route.yaml&quot;](../routes/routes-yaml.md)

- [&quot;services.yaml&quot;](../services/services-yaml.md)

### Backup e recuperação de desastres

Você pode criar um backup do banco de dados e do sistema de arquivos usando o [!DNL Cloud Console] ou a CLI. Consulte [Gerenciamento de backup](../storage/snapshots.md).

## Preparação para o desenvolvimento

O fluxo de trabalho a seguir resume o processo para ramificar o código, desenvolver e implantar a loja:

1. Configurar o ambiente local

1. Clonar a ramificação `master` no ambiente local

1. Criar uma ramificação `staging` de `master`

1. Criar ramificações para desenvolvimento de `staging`

1. Enviar código para o Git, que cria e implanta em um ambiente para testes

Consulte as seções a seguir para obter instruções detalhadas e apresentações para desenvolver, testar e implantar sua loja:

- [Fluxo de trabalho inicial de desenvolvimento e implantação](starter-develop-deploy-workflow.md)

- [Desenvolvimento do Docker](../dev-tools/cloud-docker.md) (ambiente de desenvolvimento local habilitado pelo Cloud Docker para Commerce)

- [Gerenciar ramificações](../project/console-branches.md)

- [Implante sua loja](../deploy/staging-production.md)

- [Lançamento do site](../launch/overview.md)
