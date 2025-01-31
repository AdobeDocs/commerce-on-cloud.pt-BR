---
title: Pacote [!DNL ECE-Tools]
description: Saiba mais sobre o pacote  [!DNL ECE-Tools]  e como ele ajuda a gerenciar e implantar o Adobe Commerce.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Pacote de ferramentas ECE

O pacote [!DNL ECE-Tools] é um conjunto de scripts e ferramentas criado para gerenciar e implantar o aplicativo [!DNL Commerce]. O pacote `ece-tools` simplifica muitos processos, como o gerenciamento de trabalhos cron, a verificação da configuração do projeto e a aplicação de patches de Adobe e hot fixes. Você pode exibir e contribuir com o [repositório de código aberto [!DNL ECE-Tools] no GitHub][ece-repo].

{{ece-tools-package}}

O pacote `ece-tools` é compatível com o Adobe Commerce, começando com a versão 2.1.4, e contém scripts e comandos do Adobe Commerce na infraestrutura em nuvem projetados para ajudar a gerenciar seu código e compilar e implantar seus projetos automaticamente.

A seguir, uma lista dos comandos `ece-tools` disponíveis:

```bash
php ./vendor/bin/ece-tools list
```

## Criar e implantar

O pacote `ece-tools` contém comandos para executar operações para os estágios de compilação, implantação e pós-implantação da inicialização do Adobe Commerce no aplicativo de infraestrutura em nuvem. Por exemplo, o comando `php ./vendor/bin/ece-tools build` inicia o processo de compilação do aplicativo.

Por padrão, estes `ece-tools` comandos estão na [propriedade de ganchos](../application/hooks-property.md) do arquivo de configuração `.magento.app.yaml`.

## Gerador de configuração do Docker

O pacote `ece-tools` inclui uma dependência para o pacote [magento/magento-cloud-docker], que fornece arquivos de funcionalidade e configuração para imagens do Docker para iniciar um ambiente de desenvolvimento do Docker para Adobe Commerce na infraestrutura em nuvem. Você também pode executar o Cloud Docker para Commerce como um pacote independente. Consulte [Desenvolvimento do Docker](../dev-tools/cloud-docker.md).

## Serviços, rotas e variáveis

Você pode usar o pacote `ece-tools` para exibir informações detalhadas sobre as [Variáveis da nuvem](../environment/variables-cloud.md) codificadas em Base64 usadas em qualquer ambiente da nuvem. O comando a seguir mostra todos os serviços, rotas e variáveis.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Para exibir um conjunto específico de informações, use o seguinte formato:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` — Exibe os dados da relação da variável de ambiente `MAGENTO_CLOUD_RELATIONSHIPS`, definida no arquivo `services.yaml`.
- `routes` — Exibe as rotas configuradas para o projeto usando a variável de ambiente `MAGENTO_CLOUD_ROUTES`.
- `variables` — Exibe as variáveis configuradas para o projeto usando a variável de ambiente `MAGENTO_CLOUD_VARIABLES`.

Exemplo de saída para a opção `services`:

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Verificar a configuração do ambiente

Há um conjunto de comandos de verificação disponíveis para ajudar a avaliar a configuração do seu projeto. Consulte [Assistentes inteligentes](../deploy/smart-wizards.md) na seção _Otimizar implantação_ para obter uma descrição detalhada de cada comando de assistente. O comando `wizard:ideal-state` é executado automaticamente durante a fase de compilação. Para verificar o estado ideal do projeto:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Você deve executar o comando `wizard:ideal-state` no ambiente de Nuvem remoto. O comando sempre retorna o erro `The configured state is not ideal` quando executado no ambiente de desenvolvimento local.

Saída de exemplo:

```
Ideal state is configured
```

Consulte as [Notas de versão para ece-tools](../release-notes/cloud-tools-suite.md).

## Patches de Adobe e patches personalizados

O pacote `ece-tools` inclui uma dependência para o pacote [magento/magento-cloud-patches], que fornece patches de Adobe e hot fixes que melhoram a integração de todas as versões do Adobe Commerce com ambientes na nuvem e oferecem suporte à entrega rápida de correções críticas. O &quot;também fornece patches personalizados que você adiciona ao projeto de infraestrutura do Adobe Commerce na nuvem. Consulte [Aplicar patches](../development/apply-patches.md).

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patches]: https://github.com/magento/magento-cloud-patches
