---
title: Implantação de conteúdo estático
description: Saiba mais sobre as estratégias para implantar conteúdo estático, como imagens, scripts e CSS, no Adobe Commerce em projetos de infraestrutura em nuvem.
feature: Cloud, Build, Deploy, SCD
exl-id: 8f30cae7-a3a0-4ce4-9c73-d52649ef4d7a
source-git-commit: 325b7584daa38ad788905a6124e6d037cf679332
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Estratégias de implantação de conteúdo estático

A implantação de conteúdo estático (SCD) tem um impacto significativo no processo de implantação da loja, que depende do volume de conteúdo a ser gerado (como imagens, scripts, CSS, vídeos, temas, localidades e páginas da Web) e quando gerar o conteúdo. Por exemplo, a estratégia padrão gera conteúdo estático durante a [fase de implantação](process.md#deploy-phase-deploy-phase) quando o site está em modo de manutenção; no entanto, essa estratégia de implantação leva tempo para gravar o conteúdo diretamente no diretório `pub/static` montado. Você tem várias opções ou estratégias para ajudar a melhorar o tempo de implantação, dependendo das suas necessidades.

## Otimizar o conteúdo do JavaScript e do HTML

Você pode usar o agrupamento e a minificação para criar conteúdo otimizado do JavaScript e do HTML durante a implantação de conteúdo estático.

### Reduzir conteúdo

Você pode melhorar o tempo de carregamento do SCD durante o processo de implantação se ignorar a cópia dos arquivos de exibição estáticos no diretório `var/view_preprocessed` e gerar o HTML _minified_ quando solicitado. Você pode ativar isso definindo a variável de ambiente global [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) como `true` no arquivo `.magento.env.yaml`.

>[!NOTE]
>
>A partir da versão 2002.0.13 do pacote `ece-tools`, o valor padrão da variável SKIP_HTML_MINIFICATION é definido como `true`.

Você pode economizar **mais** tempo de implantação e espaço em disco, reduzindo o número de arquivos de tema desnecessários. Por exemplo, você pode implantar o tema `magento/backend` em inglês e um tema personalizado em outros idiomas. Você pode definir essas configurações de tema com a variável de ambiente [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix).

## Escolha de uma estratégia de implantação

As estratégias de implantação diferem com base na escolha entre gerar conteúdo estático durante a fase de _compilação_, a fase de _implantação_ ou _sob demanda_. Como visto no gráfico a seguir, gerar conteúdo estático durante a fase de implantação é a opção menos ideal. Mesmo com o HTML minificado, cada arquivo de conteúdo deve ser copiado para o diretório `~/pub/static` montado, o que pode levar muito tempo. Gerar conteúdo estático sob demanda parece ser a escolha ideal. No entanto, se o arquivo de conteúdo não existir no cache gerado no momento em que é solicitado, o que adiciona tempo de carregamento à experiência do usuário. Portanto, gerar conteúdo estático durante a fase de criação é o ideal.

![Comparação de Carregamento de SCD](../../assets/scd-load-times.png)

### Configuração do SCD na compilação

Gerar conteúdo estático durante a fase de compilação com HTML minificado é a configuração ideal para [**zero-downtime** implantações](reduce-downtime.md), também conhecido como **estado ideal**. Em vez de copiar arquivos para uma unidade montada, ele cria um link simbólico do diretório `./init/pub/static`.

A geração de conteúdo estático requer acesso a temas e localidades. O Adobe Commerce armazena temas no sistema de arquivos, que é acessível durante a fase de criação; no entanto, o Adobe Commerce armazena localidades no banco de dados. O banco de dados _não_ está disponível durante a fase de compilação. Para gerar o conteúdo estático durante a fase de compilação, você deve usar o comando `config:dump` no pacote `ece-tools` para mover localidades para o sistema de arquivos. Ele lê as localidades e as salva no arquivo `app/etc/config.php`.

>[!NOTE]
>Após executar o comando `config:dump` no pacote `ece-tools`, as configurações despejadas no arquivo [ do `config.php` são bloqueadas (esmaecidas) no painel Administrador](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin). a única maneira de atualizar essas configurações no Administrador é excluí-las do arquivo localmente e reimplantar o projeto.
>&#x200B;>Além disso, sempre que você adicionar um novo site/grupo de armazenamento à sua instância, lembre-se de executar o comando `config:dump` para garantir que o banco de dados esteja sincronizado. Você também pode escolher [quais configurações devem ser despejadas](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration?lang=en) no arquivo `config.php`.
>&#x200B;>Se você excluir a configuração de armazenamento/grupo de armazenamento/site do arquivo `config.php` porque os campos estão esmaecidos, mas não executam essa etapa, as novas entidades que não foram despejadas serão excluídas do banco de dados na próxima implantação.

**Para configurar seu projeto para gerar o SCD na compilação**:

1. Na estação de trabalho local, altere para o diretório do projeto.
1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Mova as localidades para o sistema de arquivos e atualize o [`config.php` arquivo](../development/commerce-version.md#create-a-configphp-file).

1. O arquivo de configuração `.magento.env.yaml` deve conter os seguintes valores:

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) é `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) no estágio de compilação é `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) é `compact`

1. Verifique a configuração do [gancho Pós-implantação](../application/hooks-property.md) no arquivo `.magento.app.yaml`.

1. Verifique suas configurações executando o [Assistente inteligente para o estado ideal](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### Definição do SCD sob demanda

A geração de SCD sob demanda é ideal para um fluxo de trabalho de desenvolvimento no ambiente de integração. Ele diminui o tempo de implantação para que você possa revisar rapidamente suas implementações e executar testes de integração. Habilite a variável de ambiente [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) no estágio global do arquivo `.magento.env.yaml`. A variável SCD_ON_DEMAND substitui todas as outras configurações relacionadas ao SCD e limpa o conteúdo existente do diretório `~/pub/static`.

Ao usar a estratégia de solicitação de SCD, é útil pré-carregar o cache com as páginas que você espera solicitar, como a página inicial. Adicione a lista de páginas esperadas na variável de ambiente [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) no estágio pós-implantação do arquivo `.magento.env.yaml`.

>[!WARNING]
>
>Não use a estratégia SCD on-demand no ambiente de produção.

### Ignorando SCD

Às vezes, você pode optar por ignorar completamente a geração de conteúdo estático. Você pode definir a variável de ambiente [SKIP_SCD](../environment/variables-build.md#skipscd) no estágio global para ignorar outras configurações relacionadas ao SCD. Isso não afeta o conteúdo existente no diretório `~/pub/static`.
