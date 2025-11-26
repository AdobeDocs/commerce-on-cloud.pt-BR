---
title: Visão geral do desenvolvimento
description: Prepare-se para o desenvolvimento local com um projeto do Adobe Commerce na infraestrutura em nuvem.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Visão geral do desenvolvimento

Os ambientes remotos do Adobe Commerce na infraestrutura em nuvem são **Somente leitura**, incluindo todos os ambientes de Início e todos os ambientes de Integração Pro, Preparo e Produção. Em um ambiente de desenvolvimento local, você pode gravar e testar o código antes de enviá-lo para um ambiente de integração para testes e implantação adicionais em preparo e produção.

Antes de preparar seu espaço de trabalho local, verifique se você tem suas [credenciais](../../get-started/prepare-workspace.md). O desenvolvimento local requer a instalação do PHP e do Composer, a menos que você opte por usar o [Cloud Docker para Commerce](#docker-environment).

## Pacotes obrigatórios

O Adobe Commerce na infraestrutura em nuvem usa o Composer para gerenciar as dependências e as atualizações de projetos. Para o desenvolvimento local, você deve instalar as versões do PHP e do Composer que sejam compatíveis com o seu projeto na nuvem. Por exemplo, se você estiver usando o modelo de nuvem [!DNL Commerce] 2.4.8, poderá ver que o arquivo de configuração [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml) usa **PHP 8.4** e **Composer 2.8.4**.

O Composer instala as bibliotecas e dependências necessárias para seu projeto no diretório `vendor`. Os seguintes arquivos do Composer necessários estão no diretório raiz do projeto:

- `composer.json`—Use o arquivo `composer.json` para gerenciar instalações e atualizações de produtos.
- `composer.lock`—O arquivo `composer.lock` armazena um conjunto de dependências de versão exata que satisfaz as restrições de versão de cada requisito para cada pacote na árvore de dependência do projeto.

**Comandos comuns:**

| Comando | Descrição |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Atualizações para as versões mais recentes das dependências refletidas no arquivo `composer.json`. Isso atualiza o arquivo `composer.lock`. |
| `composer install` | Lê o arquivo `composer.lock` para baixar dependências. É uma prática recomendada manter uma cópia atualizada do `composer.lock` no repositório do projeto. |

{style="table-layout:auto"}

Depois de adicionar, confirmar e enviar o código atualizado, o processo de implantação executa automaticamente o comando `composer install` durante a [fase de compilação](../deploy/process.md#build-phase-build-phase).

### Metappackage da nuvem

O Adobe Commerce na infraestrutura em nuvem usa um metapackage que requer `magento/product-enterprise-edition`. Para obter as atualizações mais recentes para a versão mais recente do Commerce, use a seguinte sintaxe de restrição:

```text
>=current_version <next_version
```

Por exemplo, para usar a versão mais recente do Adobe Commerce 2.4.9, defina `2.4.8` como a versão &quot;atual&quot; e `2.4.9` como a versão &quot;próxima&quot; no arquivo `composer.json`:

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

Os principais pacotes desse metapackage são os seguintes:

- **fornecedor/magento/ece-tools**—O pacote `ece-tools` é compatível com o Adobe Commerce versão 2.1.4 e posterior para fornecer um conjunto avançado de recursos que você pode usar para gerenciar seu projeto do Adobe Commerce na infraestrutura em nuvem. Ele contém scripts e comandos do Adobe Commerce na infraestrutura em nuvem projetados para ajudar a gerenciar seu código e criar e implantar automaticamente seus projetos. Consulte a [`ece-tools` visão geral do pacote](../dev-tools/package-overview.md).
- **vendor/magento/product-enterprise-edition**—Este metapackage requer componentes de aplicativos, incluindo módulos, estruturas, temas e muito mais.
- **vendor/fastly2/magento2** — este módulo gerencia a CDN e os serviços do Fastly para os ambientes Preparo e Produção Pro e Produção de Início. Consulte [Fastly services](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **fornecedor/magento/módulo-paypal-on-boarding**—este módulo fornece checkout do gateway de pagamento do PayPal conectando-se à sua conta de comerciante do PayPal. Consulte [Ferramenta de integração do PayPal](../store/paypal.md).
- **vendor/aem/rum** — Este módulo gerencia a ferramenta de coleta de dados [Telemetria Operacional](../monitor/operational-telemetry.md).

>[!TIP]
>
>Consulte [Pacotes da nuvem para o Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) nas _Notas de versão do Commerce_ para obter uma lista de dependências e licenças de terceiros.

## Ambiente do Docker

Você pode usar a ferramenta Cloud Docker for Commerce para emular o Adobe Commerce em ambientes de produção e desenvolvimento de infraestrutura em nuvem para desenvolvimento local. O Cloud Docker para Commerce não requer que o PHP e o Composer sejam instalados localmente.

- [Desenvolvimento local com o Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) no site do Adobe Developer
- [Arquitetura do Docker e comandos comuns](../dev-tools/cloud-docker.md)
- [Notas de versão do Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Para obter informações sobre como usar os serviços de hospedagem baseados em Git com o Adobe Commerce na infraestrutura em nuvem, consulte [Integrações](../integrations/overview.md).
