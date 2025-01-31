---
title: Visão geral dos arquivos de configuração
description: Saiba mais sobre como configurar o ambiente de infraestrutura em nuvem para oferecer suporte à implantação e ao gerenciamento de sua loja personalizada do Adobe Commerce.
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Visão geral dos arquivos de configuração

Os ambientes do Adobe Commerce na infraestrutura em nuvem incluem contêineres com aplicativos, serviços e um banco de dados para fornecer um sistema completo para sua base de código e arquivos de aplicativo do Adobe Commerce.

Você pode definir configurações de aplicativo, rotas, criar e implantar ações e notificações para dar suporte aos ambientes do projeto usando os seguintes arquivos de configuração:

| Configuração | Nome do arquivo | Descrição |
| ------------- | -------- | ----------- |
| [Aplicativo](../application/configure-app-yaml.md) | `.magento.app.yaml` | Define como criar e implantar o Adobe Commerce, incluindo serviços, ganchos e trabalhos cron. |
| [Ambiente](configure-env-yaml.md) | `.magento.env.yaml` | Centraliza o gerenciamento de ações de criação e implantação em todos os seus ambientes, incluindo o armazenamento temporário e a produção profissionais, usando variáveis de ambiente. |
| [Rotas](../routes/routes-yaml.md) | `.magento/routes.yaml` | Configurar armazenamento em cache, redirecionamentos e inclusões do lado do servidor. |
| [Serviço](../services/services-yaml.md) | `.magento/services.yaml` | Define os serviços que a Adobe Commerce usa por nome e versão. Por exemplo, este arquivo pode incluir versões de MariaDB, extensões PHP, Redis, RabbitMQ e Elasticsearch ou OpenSearch. Você deve abrir um tíquete de suporte para enviar essas alterações para os ambientes de preparo e produção do plano Pro. |
| [configurações do PHP](../application/php-settings.md#configure-php) | `php.ini` | Um arquivo opcional que pode ser adicionado ao projeto. As configurações contidas nesse arquivo são anexadas às mantidas pela infraestrutura de nuvem. |

{style="table-layout:auto"}

## Atualizações de configuração para ambientes Pro

Para ambientes de preparo e produção profissionais da infraestrutura em nuvem do Adobe Commerce, você pode atualizar muitas opções de configuração em seu ambiente de desenvolvimento local e confirmar as alterações para aplicá-las a esses ambientes. No entanto, você deve [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para atualizar as seguintes opções de configuração:

- Instalar ou atualizar serviços no arquivo `.magento/services.yaml`.
- Altere a configuração das propriedades `mounts` e `disk` no arquivo `.magento.app.yaml`.

{{pro-self-service-warning}}
