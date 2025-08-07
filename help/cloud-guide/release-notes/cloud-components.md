---
title: Componentes na nuvem do Commerce
description: Consulte uma lista das melhorias mais recentes no pacote de componentes da nuvem.
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: b90959335c91dd0631d270ebb522524cf1db6ff0
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# Componentes na nuvem do Commerce

O pacote de [Componentes da Nuvem](https://github.com/magento/magento-cloud-components) fornece funcionalidade principal estendida do Adobe Commerce para sites implantados na infraestrutura da Nuvem. Este pacote é uma dependência do pacote ECE-Tools. Estas notas de versão descrevem as últimas melhorias neste pacote, que é um componente do [Conjunto de ferramentas da nuvem para o Commerce](cloud-tools-suite.md).

O pacote `magento/magento-cloud-components` usa a seguinte sequência de versão: `<major>.<minor>.<patch>`

As notas de versão incluem:

- ![novo ícone](../../assets/new.svg) Novos recursos
- ![ícone de correção](../../assets/fix.svg) Correções e melhorias

<!--Add release notes below-->

## v1.1.3 {#latest}

Data de lançamento: 07 de agosto de 2025

- ![novo ícone](../../assets/new.svg) **PHP 8.4**—Teste funcional adicionado para o PHP 8.4 e correções.<!-- MCLOUD-13313 -->

## v1.1.2

Data de lançamento: 03 de junho de 2025

- ![ícone de correção](../../assets/fix.svg) **Compatibilidade aprimorada com 2.4.8**-Bibliotecas de terceiros atualizadas para melhor compatibilidade com 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.1

Data de lançamento: 6 de fevereiro de 2025

- ![novo ícone](../../assets/new.svg) **PHP 8.4**—Suporte adicionado para PHP 8.4.<!-- MCLOUD-13148	 - -->
- ![ícone de correção](../../assets/fix.svg) **Correção para aquecimento de cache**—Correção de um problema com URLs de categoria durante o aquecimento de cache.<!-- MCLOUD-12454 - -->


## v1.1.0

Data de lançamento: 7 de outubro de 2024

- ![Ícone de correção](../../assets/fix.svg) **Código refatorado**—Remoção do suporte às versões antigas do PHP 7.4, 7.3, 7.2 e bibliotecas relacionadas.<!-- MCLOUD-9278 - -->
- ![Ícone de correção](../../assets/fix.svg) **Versão do Monolog atualizada**—Suporte adicionado para monolog 3.6.<!-- MCLOUD-12855 - -->

## v1.0.14

Data de lançamento: 8 de abril de 2024

- ![novo ícone](../../assets/new.svg) **PHP**—Suporte adicionado para o PHP 8.3.

## v1.0.13

Data de lançamento: 10 de março de 2023

- ![novo ícone](../../assets/new.svg) **Suporte aprimorado para PHP 8.2**—Corrigiu problemas de compatibilidade com determinadas versões do PHP 8.2.x para suportar Commerce 2.4.6.

## v1.0.12

Data de lançamento: 13 de setembro de 2022

- ![Ícone de correção](../../assets/fix.svg) **Erros no warmup**—Corrigiu um problema que tentava [warmup](../environment/variables-post-deploy.md#warm_up_pages) quando a visibilidade da página estava definida como [**Não Visível Individualmente**](https://experienceleague.adobe.com/pt-br/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure) no Administrador, resultando em `ERROR: Warming up failed: <link to page>` erros no log de implantação.<!-- MCLOUD-9134 -->

## v1.0.11

Data de lançamento: 4 de agosto de 2022

- ![ícone de correção](../../assets/fix.svg) **Suporte adicionado para compatibilidade com o Symfony 5.4**—Correções para compatibilidade com o Symfony 5.4.<!-- AC-3550 -->

## v1.0.10

Data de lançamento: 10 de março de 2022

- ![novo ícone](../../assets/new.svg) **Suporte para PHP 8.1**—Adicionou suporte para PHP 8.1 e descartou suporte para PHP 7.1.

## v1.0.9

Data de lançamento: 25 de outubro de 2021

- ![Ícone de correção](../../assets/fix.svg) **Atualizar Monólogo**—Atualizou a versão mínima necessária para o pacote `monolog` para `^2.3`.<!-- ACMP-1263 -->

## v1.0.8

Data de lançamento: 29 de julho de 2021

- ![Ícone de correção](../../assets/fix.svg) **Remoção de barras à direita de URLs geradas automaticamente**—Remoção das barras à direita de URLs de Páginas de Categoria geradas durante o aquecimento do cache.<!--MCLOUD-7192-->

## v1.0.7

Data de lançamento: 9 de setembro de 2020

- ![novo ícone](../../assets/new.svg) **Melhorias no log**—Reduza o tamanho do arquivo `cache.log` para melhorar o desempenho.<!--MCLOUD-6859-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um erro de tipo nos valores de configuração de cache que causava a falha do comando da CLI `php bin/magento cache:evict`.

## v1.0.6

Data de lançamento: 5 de agosto de 2020

- ![novo ícone](../../assets/new.svg) **Melhorar o desempenho do Redis**—Adicionou o comando `./bin/magento cache:evict` para remover chaves Redis expiradas, o que reduz o uso de memória do Redis para melhorar o desempenho.<!--MCLOUD-6023-->

- ![ícone de correção](../../assets/fix.svg) Remoção do suporte para *Logs do New Relic no Contexto* para corrigir um problema de desempenho.<!--MCLOUD-6422-->

## v1.0.5

Data de lançamento: 25 de junho de 2020

- ![ícone de correção](../../assets/fix.svg) corrigido um problema introduzido na versão 1.0.4 da magento/magento-cloud-components que causava a falha da operação de liberação de cache durante a fase de implantação, interrompendo o processo de implantação.

## v1.0.4

Data de lançamento: 25 de junho de 2020

- ![novo ícone](../../assets/new.svg) **Logs do New Relic Implementados no Contexto**—Os logs de aplicativos gerados pelo Adobe Commerce agora são exibidos em rastreamentos no New Relic para melhorar os recursos de solução de problemas.<!--MCLOUD-6029-->

- ![novo ícone](../../assets/new.svg) **Log aprimorado**—Log adicionado para rastrear a invalidação de cache e os eventos de reindexação completa.<!--MCLOUD-6157-->

## v1.0.3

Data de lançamento: 27 de fevereiro de 2020

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema de compatibilidade com versões do `ece-tools` 2002.0.x que usam versões mais antigas do PHP.

## v1.0.2

Data de lançamento: 6 de fevereiro de 2020

- ![novo ícone](../../assets/new.svg) Estendeu a funcionalidade da variável de ambiente `WARM_UP_PAGES` para oferecer suporte ao pré-carregamento de cache para páginas de produto específicas. Consulte o tópico [variáveis pós-implantação](../environment/variables-post-deploy.md#warm_up_pages) para obter uma descrição detalhada do recurso.<!--MAGECLOUD-4444-->

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema em que uma URL de armazenamento inválida causa falha no gancho pós-implantação ao usar a funcionalidade `WARM_UP_PAGES` para preencher o cache. Esse problema ocorreu somente quando as regravações de URL foram desabilitadas.<!-- MAGECLOUD-4094 -->

## v1.0.1

Data de lançamento: 23 de julho de 2019

- ![ícone de correção](../../assets/fix.svg) Corrigido um problema que afetava a funcionalidade [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) que usa uma URL de repositório padrão. Agora, se o comando `config:show:default-url` não puder buscar uma URL base, então a URL da variável MAGENTO_CLOUD_ROUTES será usada.<!-- MAGECLOUD-3866 -->

## v1.0.0

Data de lançamento: 12 de junho de 2019

Esta é a primeira versão do pacote [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components), que é uma nova dependência do pacote `ece-tools` versão 2002.0.20 e posterior.

- ![novo ícone](../../assets/new.svg) Adicionada a capacidade de usar padrões regex para configurar a variável de ambiente **WARM_UP_PAGES** para armazenar em cache páginas únicas, vários domínios e várias páginas. Consulte [Variáveis pós-implantação](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
