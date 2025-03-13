---
title: Fluxo de trabalho do projeto Pro
description: Saiba como usar os fluxos de trabalho de desenvolvimento e implantação do Pro.
feature: Cloud, Iaas, Paas
exl-id: efe41991-8940-4d5c-a720-80369274bee3
source-git-commit: b4905acf71e4cb71eb369cb6d4bb3abe9ada4e9d
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 0%

---

# Fluxo de trabalho do projeto Pro

O projeto Pro inclui um único repositório Git com uma ramificação global `master` e três ambientes principais:

1. Ambiente de **Produção** para iniciar e manter o site ativo
1. Ambiente **de preparo** para teste com todos os serviços
1. **Ambiente de integração** para desenvolvimento e teste

![Lista de ambientes profissionais](../../assets/pro-environments.png)

Esses ambientes são `read-only`, aceitando alterações de código implantado de ramificações enviadas pelo seu espaço de trabalho local. Consulte a [Arquitetura Pro](pro-architecture.md) para obter uma visão geral completa dos ambientes Pro. Consulte [[!DNL Cloud Console]](../project/overview.md#cloud-console) para obter uma visão geral da lista de ambientes Pro na visualização de projeto.

O gráfico a seguir demonstra o fluxo de trabalho de desenvolvimento e implantação do Pro, que usa uma abordagem simples de ramificação Git. Você [desenvolve](#development-workflow) código usando uma ramificação ativa com base no ambiente `integration`, _enviando_ e _enviando_ alterações de código de e para sua ramificação remota, Ativa. Você implanta o código verificado _mesclando_ a ramificação remota na ramificação base, o que ativa um processo [de compilação e implantação](#deployment-workflow) automatizado para esse ambiente.

![Exibição de alto nível do fluxo de trabalho de desenvolvimento da arquitetura Pro](../../assets/pro-dev-workflow.png)

## Fluxo de trabalho de desenvolvimento

O ambiente de integração fornece uma única ramificação de base `integration` contendo seu Adobe Commerce no código de infraestrutura em nuvem. Você pode criar uma ramificação de ambiente ativa adicional. Isso permite até duas ramificações ativas implantadas nos contêineres do Platform as a service (PaaS). Não há limite para o número de ambientes inativos. No entanto, quanto mais ambientes inativos houver, mais tempo levará para o Cloud Console carregar.

{{enhanced-integration-envs}}

Os ambientes de projeto oferecem suporte a um processo de integração flexível e contínuo. Comece clonando a ramificação `integration` na pasta local do projeto. Crie uma ramificação ou várias ramificações, desenvolva novos recursos, configure alterações, adicione extensões e implante atualizações:

- **Buscar** alterações de `integration`

- **Ramificação** de `integration`

- **Desenvolver código** em uma estação de trabalho local, incluindo atualizações de [!DNL Composer]

- **Enviar** alterações de código para remoto e validar

- **Mesclar** para `integration` e testar

Com uma ramificação de código desenvolvida e os arquivos de configuração correspondentes, as alterações de código estão prontas para serem mescladas à ramificação `integration` para testes mais abrangentes. O ambiente `integration` também é melhor para:

- **Integrando serviços de terceiros**—Nem todos os serviços estão disponíveis no ambiente PaaS.

- **Gerando arquivos de gerenciamento de configuração**—Algumas configurações são _Somente Leitura_ em um ambiente implantado.

- **Configurando seu armazenamento**—Você deve configurar totalmente todas as definições de armazenamento usando o ambiente de integração. Você pode encontrar a **URL de Administrador de Armazenamento** na exibição de ambiente da _integração_ em _[!DNL Cloud Console]_.

## Fluxo de trabalho de implantação

Sempre que você envia código de sua estação de trabalho local para o ambiente remoto ou mescla o código para uma ramificação de ambiente, os scripts de criação e implantação geram um novo código e provisionam os serviços configurados para o ambiente remoto.

Criar ações de script:

- O site no ambiente de destino continua a ser executado durante uma criação

- Verificar e executar o Adobe Commerce em patches e hotfixes de infraestrutura em nuvem

- Compilar código com um log de criação e implantação

- Verifique o Gerenciamento de Configuração, a implantação de conteúdo estático ocorre durante esta fase

- Criar ou usar uma slug de código inalterado para acelerar o processo

- Provisionar todos os serviços e aplicativos de back-end

Implantar ações de script:

- Colocar o site no ambiente de destino em um modo de _Manutenção_

- Implantar conteúdo estático se não for concluído durante a compilação

- Instalar ou atualizar o Adobe Commerce na infraestrutura em nuvem

- Configurar roteamento para tráfego

Após o processo de criação e implantação, sua loja volta a ficar online com as alterações e configurações de código mais recentes. Consulte [Processo de implantação](../deploy/process.md).

### Mesclar para integração

Combine todas as alterações de código verificadas mesclando a ramificação de desenvolvimento ativa na ramificação base `integration`. Você pode testar todas as alterações na ramificação `integration` antes de promover alterações no ambiente de preparo.

### Mesclar para preparo

O armazenamento temporário é um ambiente de pré-produção que fornece todos os serviços e configurações o mais próximo possível do ambiente de produção. Sempre envie suas alterações de código do ambiente `integration` para o ambiente `staging` por push para que você possa realizar testes completos com todos os serviços. Na primeira vez que usar o ambiente de preparo, você deve configurar serviços, como o [Fastly CDN](../cdn/fastly.md) e o [New Relic](../monitor/new-relic-service.md). Configure gateways de pagamento, envio, notificações e outros serviços essenciais com sandbox ou credenciais de teste.

É melhor testar completamente cada serviço, verificar suas ferramentas de teste de desempenho e executar o teste de UAT como administrador e como cliente, até sentir que sua loja está pronta para o ambiente de produção. Consulte [Implantar seu armazenamento](../deploy/staging-production.md).

{{second-staging}}

### Mesclar para produção

Após testes completos no ambiente de preparo, mescle-o ao ambiente de produção e teste completamente usando credenciais ativas. No momento em que você iniciar o site de produção, os clientes deverão ser capazes de concluir as compras e os administradores deverão ser capazes de gerenciar a loja em tempo real. Consulte os seguintes tópicos para obter uma apresentação detalhada e clara sobre como implantar sua loja e colocar em funcionamento:

- [Implante sua loja](../deploy/staging-production.md)
- [Lançamento do site](../launch/overview.md)

### Mesclar para Mestre Global

Sempre envie uma cópia do código de produção para Global `master` caso haja uma necessidade emergente de depurar o ambiente de produção sem interromper os serviços.

**não** crie uma ramificação a partir do `master` Global. Use a ramificação `integration` para criar ramificações novas e ativas para desenvolvimento e correções.
