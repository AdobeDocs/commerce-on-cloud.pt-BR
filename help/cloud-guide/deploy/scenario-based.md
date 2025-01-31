---
title: Implantação baseada em cenário
description: Saiba como personalizar o Adobe Commerce em implantações de infraestrutura em nuvem usando arquivos de configuração personalizados.
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Implantação baseada em cenário

Com o `ece-tools` 2002.1.0 e versões posteriores, você pode usar o recurso de implantação baseada em cenário para personalizar o comportamento de implantação padrão.
Este recurso usa **cenários** e **etapas** na configuração:

- **Configuração do cenário**-Cada gancho de implantação é um *cenário*, que é um arquivo de configuração XML que descreve a sequência e os parâmetros de configuração para concluir tarefas de implantação. Configure os cenários na seção `hooks` do arquivo `.magento.app.yaml`.

- **Configuração de etapa** - Cada cenário usa uma sequência de *etapas* que descrevem programaticamente as operações necessárias para concluir as tarefas de implantação. Você configura as etapas em um arquivo de configuração de cenário baseado em XML.

A infraestrutura do Adobe Commerce na nuvem fornece um conjunto de [cenários padrão](https://github.com/magento/ece-tools/tree/2002.1/scenario) e [etapas padrão](https://github.com/magento/ece-tools/tree/2002.1/src/Step) no pacote `ece-tools`. Você pode personalizar o comportamento de implantação criando arquivos de configuração XML personalizados para substituir ou personalizar a configuração padrão. Você também pode usar cenários e etapas para executar código de módulos personalizados.

## Adicionar cenários usando ganchos de criação e implantação

Você adiciona os cenários para compilação e implantação do Adobe Commerce à seção `hooks` do arquivo `.magento.app.yaml`. Cada gancho especifica os cenários a serem executados durante cada fase. O exemplo a seguir mostra a configuração do cenário padrão.

> `magento.app.yaml` ganchos

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>Com o lançamento do `ece-tools` 2002.1.x, há um novo formato de [configuração de ganchos](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html). O formato herdado das versões do `ece-tools` 2002.0.x ainda é suportado. No entanto, é necessário atualizar para o novo formato para usar o recurso de implantação baseada em cenário.

## Revisar etapas do cenário

Na configuração de gancho, cada cenário é um arquivo XML que contém etapas para executar tarefas de criação, implantação ou pós-implantação. Por exemplo, o arquivo `scenario/transfer` inclui três etapas: `compress-static-content`, `clear-init-directory` e `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Estender um cenário padrão

O exemplo a seguir estende o cenário de implantação padrão ao anexar arquivos adicionais de configuração de implantação à configuração de gancho.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Durante a implantação, os cenários personalizados são mesclados com o cenário padrão com base nas seguintes regras:

- Os cenários são priorizados com base em sua sequência na definição do gancho com o último cenário listado com a prioridade mais alta.

  No exemplo, os cenários têm a seguinte prioridade:

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (cenário padrão ou de linha de base)

- As etapas do cenário de maior prioridade substituem as etapas com o mesmo nome nos outros cenários. Novas etapas são adicionadas à configuração. As mesmas regras se aplicam para mais de dois cenários com cada cenário sendo priorizado da direita para a esquerda, por exemplo (C → B → A).

### Remover etapas padrão

Você remove etapas dos cenários padrão usando o parâmetro `skip`.

Por exemplo, para ignorar as etapas `enable-maintenance-mode` e `set-production-mode` no cenário de implantação padrão, crie um arquivo de configuração que inclua a seguinte configuração.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Para usar o arquivo de configuração personalizado, atualize o arquivo `.magento.app.yaml` padrão.

> `.magento.app.yaml` com cenário de implantação personalizado

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Substituir etapas padrão

Os cenários personalizados podem substituir etapas padrão para fornecer implementação personalizada. Para fazer isso, use o nome de etapa padrão como o nome da etapa personalizada.

Por exemplo, no [cenário de implantação padrão], a etapa `enable-maintenance-mode` executa o script PHP [EnableMaintenanceMode] padrão.

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Para substituir esta etapa, crie um arquivo de configuração de cenário personalizado para executar um script diferente quando a etapa `enable-maintenance-mode` for executada.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Alterar a prioridade da etapa

Os cenários personalizados podem alterar a prioridade das etapas padrão. A etapa a seguir altera a prioridade da etapa `enable-maintenance-mode` de `300` para `10` de forma que a etapa seja executada anteriormente no cenário de implantação.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

Neste exemplo, a etapa `enable-maintenance-mode` se move para o início do cenário porque tem uma prioridade mais baixa do que todas as outras etapas no cenário de implantação padrão.

### Exemplo: estender o cenário de implantação

O exemplo a seguir personaliza o [cenário de implantação padrão] com as seguintes alterações:

- Substitui a etapa `remove-deploy-failed-flag` por uma etapa personalizada
- Ignora a subetapa `clean-redis-cache` na etapa de pré-implantação
- Ignora a etapa `unlock-cron-jobs`
- Ignora a etapa `validate-config` para desabilitar validadores críticos
- Adiciona uma nova etapa de pré-implantação

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Para usar esse script em seu projeto, adicione a seguinte configuração ao arquivo `.magento.app.yaml` para seu projeto do Adobe Commerce na infraestrutura em nuvem:

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>Você pode revisar os [cenários padrão](https://github.com/magento/ece-tools/tree/2002.1/scenario) e a [configuração de etapa padrão](https://github.com/magento/ece-tools/tree/2002.1/src/Step) no repositório GitHub `ece-tools` para determinar quais cenários e etapas personalizar para as tarefas de compilação, implantação e pós-implantação do seu projeto.

## Adicionar um módulo personalizado para estender `ece-tools`

O pacote `ece-tools` fornece interfaces de API padrão que seguem os padrões de Versão Semântica. Todas as interfaces de API estão marcadas com a anotação **@api**. Você pode substituir a implementação da API padrão pela sua própria, criando um módulo personalizado e modificando o código padrão conforme necessário.

Para usar o módulo personalizado com o Adobe Commerce na infraestrutura em nuvem, você deve registrar seu módulo na lista de extensões do pacote `ece-tools`. O processo de registro é semelhante ao processo usado para registrar módulos no Adobe Commerce.

**Para registrar um módulo com o `ece-tools` pacote**:

1. Crie ou estenda o arquivo `registration.php` na raiz do módulo.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Atualize a seção `autoload` do arquivo de configuração do módulo para incluir o arquivo `registration.php` para carregar automaticamente os arquivos do módulo em `composer.json`.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Adicione o arquivo `config/services.xml` ao módulo. Esta configuração foi mesclada com `config/services.xml` do pacote `ece-tools`.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Para saber mais sobre a injeção de dependência, consulte [Injeção de dependência Symfony](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[cenário de implantação padrão]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[Script PHP EnableMaintenanceMode]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
