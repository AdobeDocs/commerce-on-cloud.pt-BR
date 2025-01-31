---
title: Orientação de testes
description: Leia sobre os tipos de teste e as práticas recomendadas para iniciar o Adobe Commerce na infraestrutura em nuvem.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Orientação de testes

Depois de configurar e personalizar seu projeto do Adobe Commerce na infraestrutura em nuvem, é uma prática recomendada testar seu aplicativo completamente antes de iniciar o site da loja. O teste ajuda a gerenciar adequadamente as expectativas do tamanho do cluster e a dimensionar adequadamente para as necessidades futuras dos negócios.

## Teste funcional

Durante o desenvolvimento, é importante realizar testes funcionais completos no projeto de infraestrutura em nuvem do Adobe Commerce. Consulte a seguinte orientação para executar testes funcionais no ambiente do Docker:

- **Teste de aplicativo**—Use a [Estrutura de Teste Funcional (MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) do Magento para testar aplicativos no ambiente do Cloud Docker.

- **Teste de código**—Use a [estrutura de teste de reconciliação do PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/) para validar o código destinado a contribuir com repositórios de pacotes na nuvem.

## Práticas recomendadas antes do lançamento

Considere os seguintes tipos de teste como prática recomendada a ser executada antes do lançamento do site:

- **Teste de carga**—Realize um teste de carga para entender o comportamento do sistema sob uma carga esperada. Por exemplo, teste um número simultâneo de usuários ativos no aplicativo, fazendo com que cada usuário execute um número específico de transações dentro da duração definida. Esse teste revela o tempo de resposta de transações importantes e essenciais aos negócios, como o comportamento do banco de dados ou do servidor de aplicativos. Um teste de carga pode ajudar a identificar gargalos.

- **Teste de esforço**—Desafie os limites superiores de capacidade dentro do sistema para determinar se o sistema tem desempenho suficiente quando a carga atual está bem acima do máximo esperado.

- **Verificação de Segurança**—o Adobe fornece uma [Ferramenta de Verificação de Segurança](../launch/overview.md#set-up-the-security-scan-tool) gratuita para seus sites.

- **Teste de penetração**—É um ataque cibernético simulado autorizado em um sistema de computador projetado para avaliar a segurança do sistema. O teste de penetração ajuda a identificar deficiências ou vulnerabilidades, incluindo a possibilidade de as partes não autorizadas obterem acesso a dados e características do sistema.

>[!WARNING]
>
>Os clientes não têm permissão para realizar avaliações de segurança da infraestrutura da AWS ou dos próprios serviços da AWS. Se você descobrir um problema de segurança em qualquer um dos serviços da AWS observado em sua avaliação de segurança, [contate a Segurança da AWS](mailto:aws-security@amazon.com) imediatamente. Consulte [Políticas de clientes da AWS para testes de penetração](https://aws.amazon.com/security/penetration-testing/).
