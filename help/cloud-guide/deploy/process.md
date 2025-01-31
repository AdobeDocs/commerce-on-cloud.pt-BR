---
title: Processo de implantação
description: Saiba como a implantação funciona para o Adobe Commerce em projetos de infraestrutura em nuvem.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Processo de implantação

O processo de implantação começa quando você executa uma mesclagem, envio por push ou sincronização de seu ambiente, ou quando você aciona uma [reimplantação manual](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). O processo de implantação leva tempo, mas há maneiras de otimizar a implantação que dependem se você está desenvolvendo e testando ou trabalhando com um site ativo. Principalmente, você pode controlar a [implantação de conteúdo estático](static-content.md).

Há três fases distintas do processo de implantação: criação, implantação e pós-implantação. Cada fase executa ações específicas com recursos limitados:

## Fase de compilação ![Fase de compilação](../../assets/status-build.png)

A fase _build_ monta contêineres para os serviços definidos nos arquivos de configuração, instala dependências com base no arquivo `composer.lock` e executa os ganchos de compilação definidos no arquivo `.magento.app.yaml`. Sem a capacidade de se conectar a qualquer serviço ou acessar o banco de dados, a fase de criação depende dos recursos limitados ao ambiente.

## ![Fase de implantação](../../assets/status-deploy.png) Fase de implantação

A fase _implantar_ coloca uma suspensão temporária em solicitações de entrada e faz a transição do site para o [modo de manutenção](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). A fase de implantação usa os novos contêineres e, após a montagem do sistema de arquivos, abre conexões de rede, ativa os serviços definidos na seção `relationships` do arquivo `.magento.app.yaml` e executa os ganchos de implantação definidos no arquivo `.magento.app.yaml`. Tudo é _somente leitura_, exceto os diretórios definidos no arquivo `.magento.app.yaml`. Por padrão, a propriedade [`mounts` ](../application/properties.md#mounts) inclui os seguintes diretórios:

- `app/etc` — contém os arquivos de configuração `env.php` e `config.php`
- `pub/media` — contém todos os dados de mídia, como produtos ou categorias
- `pub/static` — contém arquivos estáticos gerados
- `var` — contém arquivos temporários criados durante o tempo de execução

Todos os outros diretórios têm permissões somente leitura. O novo site se torna ativo no final da fase de implantação, à medida que sai do modo de manutenção e libera a suspensão temporária em solicitações recebidas.

Na fase de implantação, cópias dos arquivos de configuração de implantação `app/etc/config.php` e `app/etc/env.php` são salvas com a extensão BAK. Consulte [Configurações de armazenamento](../store/store-settings.md#restore-configuration-files) para saber mais sobre como restaurar esses arquivos.

## ![Fase de pós-implantação](../../assets/status-post-deploy.png) Fase de pós-implantação

A fase _pós-implantação_ executa os ganchos pós-implantação definidos no arquivo `.magento.app.yaml`. A execução de qualquer ação nesta fase pode afetar o desempenho do site; no entanto, você pode usar a variável de ambiente [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) para preencher o cache.

## ![Verificar estado](../../assets/status-verify.png) Verificar configurações

Você pode testar a configuração ideal para o estado do seu projeto executando os [Assistentes inteligentes](smart-wizards.md).

>[!NOTE]
>
>Com o `ece-tools` 2002.1.0 e versões posteriores, você pode usar o recurso de implantação baseada em cenário para personalizar os processos de compilação, implantação e pós-implantação do seu projeto Adobe Commerce na infraestrutura em nuvem. Consulte [Implantação baseada em cenário](scenario-based.md).
