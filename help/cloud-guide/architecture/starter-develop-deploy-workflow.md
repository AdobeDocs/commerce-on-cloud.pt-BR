---
title: Fluxo de trabalho do projeto inicial
description: Saiba como usar os workflows de desenvolvimento e implantação do Starter.
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '2103'
ht-degree: 0%

---

# Fluxo de trabalho do projeto inicial

A infraestrutura do Adobe Commerce na nuvem inclui um único repositório Git com uma ramificação `master` para o ambiente de produção que pode ser ramificada para criar um ambiente de preparo e vários ambientes de integração para trabalho de teste e desenvolvimento. Você pode ter até quatro ambientes ativos, incluindo um ambiente `master` para seu servidor de produção. Consulte [Arquitetura inicial](starter-architecture.md) para obter uma visão geral.

Para seus ambientes, siga o fluxo de trabalho do [!UICONTROL Development > Staging > Production] para desenvolver e implantar seu site.

- **Ambiente de produção (site ativo)** — Fornece um ambiente de produção completo com todos os serviços compilados e implantados do código na ramificação `master`.
- **Ambiente de preparo** — Fornece um ambiente de preparo completo que corresponde ao ambiente de produção com todos os serviços compilados e implantados de uma ramificação `staging` que você cria ao clonar de `master`.
- **Ambientes de integração** — Fornece até dois ambientes de desenvolvimento ativos que você cria a partir da ramificação `staging`. O ambiente `integration` não oferece suporte a serviços de terceiros, como o Fastly e o New Relic.

Para suas filiais, você pode seguir qualquer metodologia de desenvolvimento. Por exemplo, você pode seguir uma metodologia Agile, como scrum, para criar ramificações para cada sprint.

Em cada sprint, é possível criar ramificações para cada história de usuário. Todas as histórias se tornam testáveis. Você pode mesclar continuamente para a ramificação sprint e validar essa ramificação continuamente. Quando o sprint terminar, você poderá mesclar a ramificação do sprint com `master` para implantar todas as alterações do sprint na produção sem precisar lidar com um afunilamento de teste.

## Fluxo de trabalho de desenvolvimento

O desenvolvimento e a implantação nos planos iniciais começam com seu projeto inicial. Você cria seu projeto com o &quot;site em branco&quot;, que é um repositório de código de modelo do Adobe Commerce na infraestrutura em nuvem com uma loja totalmente preparada. Isso cria uma ramificação `master` com uma cópia do código do seu ambiente de produção.

O fluxo de trabalho de desenvolvimento inclui o seguinte:

- [Clonar e ramificar](#clone-and-branch) de `master` para criar `staging` ramificações de desenvolvimento
- [Desenvolver código](#develop-code) e instalar extensões localmente em uma ramificação de desenvolvimento, incluindo atualizações do [!DNL Composer]
- [Configurar](#configure-store) suas configurações de armazenamento e extensão
- [Gerar arquivos de gerenciamento de configuração](#generate-configuration-management-files)
- [Enviar código](#push-code-and-test) e configuração para compilar e implantar nos ambientes `staging` e `production`

![Desenvolver e implantar fluxo de trabalho](../../assets/starter/workflow.png)

Você também tem algumas etapas opcionais para ajudar a desenvolver e testar seu código e seus dados de loja:

- [Instalar dados de exemplo](#optional-install-sample-data) em seu armazenamento
- [Enviar dados do repositório de produção](#optional-pull-production-data) para ambientes

Este processo pressupõe que você tenha configurado seu [espaço de trabalho de desenvolvedor local](../development/overview.md).

### Clonar e ramificar

Para um novo projeto do Plano Inicial, uma ramificação `master` foi clonada do repositório Git da infraestrutura em nuvem do Adobe Commerce. Para começar a ramificar e trabalhar com código, clone a ramificação `master` no ambiente local.

O formato do comando clone do Git é:

```bash
git fetch origin
```

```bash
git pull origin <environment-ID>
```

Na primeira vez que você começar a trabalhar em ramificações para o seu projeto inicial, crie uma ramificação `staging`. Isso cria uma ramificação de código correspondente à ramificação `master` que é implantada em um ambiente de preparo para testar a configuração e as alterações de código antes da implantação no ambiente de produção.

Em seguida, crie ramificações a partir do `staging` para desenvolver código, adicionar extensões e configurar integrações de terceiros. Sempre que você desenvolver um código personalizado, adicionar extensões, integrar a um serviço de terceiros, trabalhar em uma ramificação de desenvolvimento criada a partir da ramificação `staging`. Você tem quatro ambientes de integração ativos disponíveis. Ao enviar uma ramificação ativa, um desses ambientes de integração implanta automaticamente seu código para teste.

O formato do comando da ramificação Git é:

```bash
git checkout <branch-name>
```

O formato do comando `branch` da CLI da Nuvem é:

```bash
magento-cloud environment:branch <environment-name> <parent-environment-ID>
```

![Ramificação do Mestre](../../assets/starter/branching.png)

### Desenvolver código

Usando a ramificação base do Adobe Commerce no código de infraestrutura em nuvem, você pode começar a instalar extensões, desenvolver código personalizado, adicionar temas e muito mais.

Use uma estratégia de ramificação com seu trabalho de desenvolvimento. Usar uma ramificação para fazer todo o seu trabalho de uma só vez pode dificultar os testes. Por exemplo, você pode seguir as metodologias de integração contínua e sprint para funcionar:

- Adicione algumas extensões e as configure com a primeira ramificação
- Enviar esse código, testar e mesclar para Preparo e Produção
- Configure completamente seus serviços no `services.yaml` e adicione um tema
- Enviar esse código, testar e mesclar para Preparo e Produção
- Integrar a um serviço de terceiros
- Enviar esse código, testar e mesclar para Preparo e Produção

Até que sua loja esteja totalmente criada, configurada e pronta para ser iniciada. Mas continue lendo: há muitas opções para sua loja e configuração de código.

>[!NOTE]
>
>Não conclua ainda nenhuma configuração na sua estação de trabalho local.

![Enviar código do local](../../assets/starter/push-code.png)

### Configurar loja

Quando estiver pronto para configurar o armazenamento, envie todo o código para o ambiente `integration`. Defina as configurações de armazenamento do Administrador para o ambiente de integração, não em seu ambiente local. Você pode encontrar a URL clicando em **Acessar site** no [!DNL Cloud Console]

Para obter as melhores informações sobre configurações, consulte a documentação do Adobe Commerce e as extensões instaladas. Estes são alguns links e ideias que ajudam você a começar:

- [Práticas recomendadas para configuração de armazenamento](../store/best-practices.md) para práticas recomendadas específicas na nuvem
- [Configuração básica](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/store-details) para acesso de administrador de loja, nome, idiomas, moedas, identidade visual, sites, exibições de loja e muito mais
- [Tema](https://experienceleague.adobe.com/en/docs/commerce-admin/content-design/content-menu#design-features) para a aparência do site e lojas, incluindo CSS e layouts
- [Configuração do sistema](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/guide-overview) para funções, ferramentas, notificações e sua chave de criptografia para o banco de dados
- Configurações de extensão usando a documentação

Além das configurações de loja, você pode configurar vários sites e lojas, serviços configurados e muito mais. Consulte [Configurar armazenamento](../store/overview.md).

### Gerar arquivos de gerenciamento de configuração

Se você estiver familiarizado com o Adobe Commerce, poderá se preocupar com a forma de fazer com que as definições de configuração do banco de dados em desenvolvimento cheguem aos ambientes de preparo e produção. Anteriormente, você tinha que copiar todas as suas configurações no papel ou em um arquivo e aplicar manualmente as configurações a outros ambientes. Ou você pode ter descarregado seu banco de dados e enviado esses dados para outro ambiente.

O Adobe Commerce na infraestrutura em nuvem fornece um conjunto de dois comandos do [Gerenciamento de Configuração](../store/store-settings.md) que exportam as definições de configuração do seu ambiente para um arquivo. Estes comandos só estão disponíveis para o **Adobe Commerce na infraestrutura de nuvem 2.2 e posterior**.

- `php .vendor/bin/ece-tools config:dump` — Exporta apenas as definições de configuração inseridas ou modificadas por padrão para um arquivo de configuração. _Recomendado_.
- `php bin/magento app:config:dump` — Exporta todas as definições de configuração, incluindo modificadas e padrão, para um arquivo de configuração.

Arquivo gerado `app/etc/config.php`.

Gere o arquivo no ambiente de integração em que você configurou o Adobe Commerce. Percorra o processo de geração do arquivo, adicionando-o à ramificação e implantando-o.

**Observações importantes** sobre o Gerenciamento de Configurações:

- Qualquer configuração incluída no arquivo gerado pelo comando `app:config:dump` está bloqueada para edição, ou é somente leitura, no ambiente implantado. Essa é uma razão pela qual o Adobe recomenda o uso do comando `.vendor/bin/ece-tools config:dump`.

  Por exemplo, você instala um módulo para o Fastly no ambiente de desenvolvimento. Você só pode configurar esse módulo no ambiente de preparo e produção. O uso do comando `.vendor/bin/ece-tools config:dump` mantém esses campos padrão editáveis ao implantar suas alterações de desenvolvimento no ambiente de preparo e produção.

- O arquivo gerado pode ser longo, dependendo do tamanho da implantação. O comando `.vendor/bin/ece-tools config:dump` gera um arquivo menor que o arquivo gerado pelo comando `app:config:dump`.

Se você estiver usando o Adobe Commerce versão 2.2 ou posterior, os comandos de gerenciamento de configuração fornecerão um recurso adicional para proteger dados confidenciais, como credenciais de sandbox para um módulo do PayPal. Durante o processo de exportação, todos os valores que contêm dados confidenciais são exportados para um arquivo de configuração separado—`env.php` no diretório `app/etc/`. Esse arquivo permanece no ambiente local e não é copiado quando você envia seu código para outra ramificação. Você também pode criar variáveis de ambiente com comandos CLI em todas as versões do Adobe Commerce na infraestrutura em nuvem.

![Gerar variáveis de ambiente](../../assets/starter/env-variables.png)

Consulte [Gerenciamento de Configuração](../store/store-settings.md).

### Enviar código e testar

Neste ponto, você deve ter uma ramificação de código desenvolvida com um arquivo de configuração (`config.local.php` ou `config.php`) pronto para teste.

Toda vez que você envia código de seu ambiente local, uma série de scripts de criação e implantação é executada. Esses scripts geram novo código e o implantam no ambiente remoto. Por exemplo, se você estiver enviando uma ramificação de desenvolvimento do seu ambiente local para a ramificação remota, um ambiente correspondente atualizará serviços, código e conteúdo estático.

Você pode acessar diretamente esse ambiente com um URL de armazenamento, URL de administração e SSH. Esses ambientes incluem um servidor Web, um banco de dados e serviços configurados. Quando estiver pronto, você poderá começar a implantar e testar no ambiente de preparo.

Para obter mais informações, consulte [Fluxo de trabalho de implantação](#deployment-workflow).

### Opcional: instalar dados de amostra

Se você precisar de alguns dados de exemplo ao desenvolver sua loja, poderá instalar dados de amostra. Esses dados simulam uma loja ativa, incluindo clientes, produtos e outros dados. Esses dados de amostra funcionam melhor com um Adobe Commerce de &quot;site em branco&quot; na instalação do modelo de infraestrutura na nuvem ao criar seu projeto. Como prática recomendada, remova os dados de amostra antes de entrar em funcionamento. Consulte [Instalar dados de amostra opcionais](../test/sample-data.md).

![Instalar dados de exemplo opcionais](../../assets/starter/sample-data.png)

### Opcional: Extrair dados de produção

Adicione todos os seus produtos, catálogos, conteúdo do site e assim por diante, diretamente ao ambiente `production`. Ao adicionar esses dados ao ambiente de produção, você pode fornecer preços atualizados, cupons, estoque de estoque, anúncios de vendas, informações sobre ofertas futuras e muito mais para seus clientes. Esses dados não incluem configurações de extensão, que você configura na ramificação de desenvolvimento local.

À medida que você desenvolve recursos, adiciona extensões e temas de design, ter dados reais para trabalhar é útil. A qualquer momento, você pode [criar um despejo do banco de dados](../storage/database-dump.md) a partir do ambiente de produção e enviá-lo por push para seus ambientes de Preparo e integração, conforme necessário.

Para ajudar a exportar dados de produção como dados de teste para uso em ambientes de preparo e integração:

- [Execute os utilitários de suporte](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) comandos da CLI (Recomendado) ao exportar um backup protegido do cliente e armazenar dados usando sua chave de criptografia do Adobe Commerce

- Ferramenta [Coleta de dados](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/support#data-collector) para gerar e exportar dados

Para migrar esses dados, consulte [Migrar e implantar arquivos e dados estáticos](../deploy/staging-production.md#migrate-static-files).

![Puxar e limpar dados de produção](../../assets/starter/data-code-process.png)

>[!NOTE]
>
>Antes de enviar os dados para outro ambiente, você deve considerar a limpeza dos dados. Você tem algumas opções, incluindo [usar utilitários de suporte](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) ou desenvolver um script para limpar os dados do cliente.

>[!WARNING]
>
>Não empurre um banco de dados de um ambiente de integração ou de preparo para um ambiente de produção. Se você fizer isso, os dados do ambiente de integração ou de preparo substituirão os dados de produção em tempo real, incluindo vendas, pedidos, clientes novos e atualizados e muito mais.

## Fluxo de trabalho de implantação

Conforme detalhado nas informações da arquitetura, a infraestrutura do Adobe Commerce na nuvem é orientada pelo Git. A implantação do Adobe Commerce na infraestrutura em nuvem faz parte dos processos de push do Git para ramificações.

Ao enviar o código ramificado do ambiente local para a ramificação remota, uma série de scripts de criação e implantação é iniciada.

Criar scripts:

- O site no ambiente de destino continua em execução durante uma criação

- Verificar e executar o Adobe Commerce em patches e hotfixes de infraestrutura em nuvem

- Compilar seu código com um log de criação e implantação

- Verifique o Gerenciamento de Configuração, se a implantação de conteúdo estático ocorrer durante esta fase

- Criar ou usar uma slug de código inalterado para um processo mais rápido

- Provisionar todos os serviços e aplicativos de back-end

Implantar scripts:

- Coloca o site no ambiente de destino no modo de manutenção

- Implanta conteúdo estático se não for concluído durante a compilação

- Instala ou atualiza o Adobe Commerce na infraestrutura em nuvem

- Configurar roteamento para tráfego

Quando estiver totalmente concluída, sua loja volta a ficar online, ao vivo, com todos os seus códigos e configurações atualizados.

Consulte [Processo de implantação](../deploy/process.md).

### Encaminhar para Preparo e testar

Sempre envie seu código em iterações para o ambiente `staging` para testes completos. Na primeira vez que você usar este ambiente, deverá configurar alguns serviços, incluindo o [Fastly](/help/cloud-guide/cdn/fastly.md) e o [New Relic](../monitor/new-relic-service.md). Além disso, configure gateways de pagamento, envio, notificações e outros serviços vitais com sandbox ou credenciais de teste.

O armazenamento temporário é um ambiente de pré-produção que fornece todos os serviços e configurações o mais próximo possível da produção. Teste minuciosamente cada serviço, verifique suas ferramentas de teste de desempenho, execute o teste de UAT como administrador e como cliente, até sentir que sua loja está pronta para produção.

Consulte [Implantar seu armazenamento](../deploy/staging-production.md).

### Encaminhar para produção

Ao enviar para a ramificação `master`, você está enviando para o ambiente `production`. Conclua as atividades de configuração e teste no ambiente de produção da mesma forma que fazia no ambiente de preparo, com uma diferença importante. No ambiente de produção, use credenciais ativas para configuração e teste. No momento em que você inicia o site, os clientes podem concluir as compras e os administradores podem gerenciar a loja ao vivo.

Consulte [Implantar seu armazenamento](../deploy/staging-production.md).

### Lançamento do site

Há uma apresentação clara para colocar seu site no ar. Depois de concluir essas etapas, sua loja pode entregar produtos em seu tema personalizado para venda imediatamente.

Consulte [Inicialização do site](../launch/overview.md).

## Integração contínua

Seguindo suas metodologias de ramificação e desenvolvimento, você pode desenvolver facilmente novos recursos, configurar alterações e adicionar extensões para desenvolver e implantar atualizações continuamente.

Todos os ambientes de infraestrutura em nuvem oferecem suporte à integração contínua para atualizações constantes. Esse fluxo de trabalho suporta versões várias vezes por dia ou em uma programação definida de acordo com suas necessidades comerciais.

- Criar ramificações de desenvolvimento com recursos e alterações futuros

- Testar o código no ambiente `integration`

- Implantar e testar no ambiente `staging`

- Implantar no ambiente `production`
