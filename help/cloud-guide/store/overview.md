---
title: Visão geral das opções de armazenamento e do gerenciamento de configuração
description: Personalize sua loja da Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Configuration, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# Visão geral das opções de armazenamento e do gerenciamento de configuração

Há várias maneiras de personalizar seu armazenamento, como adicionar um tema personalizado, instalar uma extensão ou impor uma configuração específica em ambientes de infraestrutura em nuvem. Você pode definir configurações para serviços específicos diretamente em ambientes de Preparo e Produção. Você pode configurar vários sites e lojas. A configuração da Loja ajuda a definir essas opções na estação de trabalho local e implantar configurações específicas em todos os ambientes.

Para acessar sua vitrine eletrônica, use o comando `magento-cloud url` e responda aos prompts. Ou você pode encontrar a url no [!DNL Cloud Console] em **Site de acesso**.

## Configurar opções de loja

As opções de armazenamento incluem o seguinte:

* [Módulo B2B (B2B)](b2b-module.md)
* [Temas personalizados](custom-theme.md)
* [Extensões](extensions.md)
* [Vários sites](multiple-sites.md)
* [Serviços de pagamento](paypal.md)

## Configurar serviços e integrações

Há [arquivos de configuração](../environment/overview.md) específicos que gerenciam determinados comportamentos de implantação em ambientes remotos. Você pode revisar esses tópicos separadamente:

* [Implantação do aplicativo](../application/configure-app-yaml.md)
* [Ações de criação e implantação de ambientes](../environment/configure-env-yaml.md)
* [Rotas de solicitação de entrada](../routes/routes-yaml.md)
* [Serviços compatíveis](../services/services-yaml.md)

## Gerenciamento de configuração

Após configurar as opções, os serviços e as integrações da loja, use o gerenciamento de configurações para implantar essas configurações em todos os ambientes de forma consistente e com tempo de inatividade mínimo. Consulte [Gerenciamento de Configuração](store-settings.md).
