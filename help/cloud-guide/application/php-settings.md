---
title: Configurações do PHP
description: Saiba mais sobre as configurações ideais do PHP para a configuração de aplicativos do Commerce na infraestrutura de nuvem.
feature: Cloud, Configuration, Extensions
exl-id: 83094c16-7407-41fa-ba1c-46b206aa160d
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---

# Configurações do PHP

Você pode escolher qual [versão do PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) executar em seu arquivo `.magento.app.yaml`:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Se estiver atualizando para o PHP 8.1 e posterior, remova o JSON da propriedade [`runtime: extensions:` &#x200B;](properties.md#runtime) no arquivo `.magento.app.yaml` e reimplante. A extensão JSON vem instalada no ambiente de nuvem desde o PHP 8.0.

## Configurar PHP

Você pode personalizar as configurações do PHP para o seu ambiente usando um arquivo `php.ini` anexado à configuração mantida pelo Adobe Commerce.

Em seu repositório, adicione o arquivo `php.ini` à raiz do aplicativo (a raiz do repositório).

>[!TIP]
>
>A configuração incorreta das configurações do PHP pode causar problemas, portanto, somente administradores avançados devem definir essas opções.

### Aumentar limite de memória do PHP

Para aumentar o limite de memória do PHP, adicione a seguinte configuração ao arquivo `php.ini`:

```ini
memory_limit = 1G
```

Para depuração, aumente o valor para 2G.

### Otimizar a configuração do realpath_cache

Defina as seguintes configurações de `realpath_cache` para melhorar o desempenho do aplicativo.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

Essas configurações permitem que processos PHP armazenem em cache caminhos para arquivos em vez de pesquisá-los para cada carregamento de página. Consulte [Ajuste de desempenho](https://www.php.net/manual/en/ini.core.php) na documentação do PHP.

>[!NOTE]
>
>Para obter uma lista das definições de configuração do PHP recomendadas, consulte [Configurações do PHP necessárias](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) no _Guia de instalação_.

### Verificar configurações personalizadas de PHP

Depois de enviar as alterações de `php.ini` para o ambiente em nuvem, você pode verificar se a configuração personalizada do PHP foi adicionada ao ambiente. Por exemplo, use SSH para fazer logon no ambiente remoto, exibir informações de configuração do PHP e filtrar a diretiva `register_argc_argv`:

```bash
php -i | grep register_argc_ar
```

Saída de exemplo:

```text
register_argc_argv => On => On
```

>[!WARNING]
>
>Se você usa o Cloud Docker para Commerce para desenvolvimento local, consulte [Contêineres de serviço do Docker](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#fpm-container) para obter informações sobre como usar um arquivo `php.ini` personalizado em um ambiente do Docker.

## Habilitar extensões

Você pode ativar ou desativar extensões PHP na seção `runtime:extension`. Além disso, as extensões especificadas ficam disponíveis nos contêineres PHP do Docker.

>[!IMPORTANT]
>
>Antes de habilitar extensões, é importante entender que a versão do PHP deve ser compatível com o sistema operacional que hospeda o projeto. Seu ambiente de projeto pode exigir uma atualização do SO pela equipe de infraestrutura antes de você poder continuar.

Exemplo no arquivo `.magento.app.yaml`:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

Use SSH para fazer login em um ambiente e listar as extensões do PHP.

```bash
php -m
```

Para obter detalhes sobre uma extensão específica do PHP, consulte a [Lista de Extensões do PHP](https://www.php.net/manual/en/extensions.alphabetical.php).

A tabela a seguir mostra as extensões compatíveis do PHP ao implantar o Adobe Commerce na plataforma na nuvem.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

Os requisitos do módulo do PHP estão vinculados à versão do Adobe Commerce. Consulte [requisitos do PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### Suporte à extensão

Para projetos Pro, as seguintes extensões exigem suporte adicional para instalação:

- `ioncube`
- `sourceguardian`

Por exemplo, para configurar o PHP para executar somente scripts protegidos pelo SourceGuardian em todos os ambientes, a seguinte opção deve ser definida no arquivo `php.ini`:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Consulte a [seção 3.5 da documentação do SourceGuardian](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Este é um link para uma PDF_.

[Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para obter ajuda sobre como instalar essas extensões PHP em todos os ambientes de Produção e de Pro Staging. Inclua seu arquivo atualizado `.magento/services.yaml`, arquivo `.magento.app.yaml` com a versão atualizada do PHP e quaisquer extensões adicionais do PHP. Para alterações em um ambiente de Produção em tempo real, você deve fornecer um aviso mínimo de 48 horas. Pode levar até 48 horas para a equipe de infraestrutura da nuvem atualizar seu projeto.

>[!WARNING]
>
>Não há suporte para o PHP compilado com depuração e a Investigação pode entrar em conflito com [!DNL XDebug] ou [!DNL XHProf]. Desative essas extensões ao ativar o teste. O Probe está em conflito com algumas extensões PHP como [!DNL Pinba] ou IonCube.

<!-- Last updated from includes: 2025-04-14 09:39:27 -->
