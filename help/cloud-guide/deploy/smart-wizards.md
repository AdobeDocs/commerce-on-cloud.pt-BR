---
title: Assistentes inteligentes
description: Saiba como usar assistentes inteligentes para avaliar se o projeto do Adobe Commerce na infraestrutura em nuvem está seguindo as práticas recomendadas de implantação.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Assistentes inteligentes

Os assistentes inteligentes podem ajudar você a determinar se a configuração da nuvem segue as práticas recomendadas. Os assistentes disponíveis ajudam nas seguintes configurações:

- Estado ideal para tempo mínimo de inatividade de implantação
- Configuração de balanceamento de carga para banco de dados e Redis
- Implantação de conteúdo estático (SCD) para o estágio sob demanda, de criação ou de implantação

Cada um dos comandos do assistente inteligente fornece uma resposta de verificação e, se aplicável, uma recomendação para a configuração adequada.

| Comando | Descrição |
| ------- | ------------|
| `wizard:ideal-state` | Verifique se o SCD está no estágio _build_, a variável `SKIP_HTML_MINIFICATION` é `true` e o gancho post_deploy configurado no ambiente de nuvem. Não utilizar no ambiente de desenvolvimento local. |
| `wizard:master-slave` | Verifique se as variáveis `REDIS_USE_SLAVE_CONNECTION` e `MYSQL_USE_SLAVE_CONNECTION` são `true`. |
| `wizard:scd-on-demand` | Verifique se a variável de ambiente global `SCD_ON_DEMAND` é `true`. |
| `wizard:scd-on-build` | Verifique se a variável de ambiente global `SCD_ON_DEMAND` é `false` e a variável de ambiente `SKIP_SCD` é `false` para o estágio _build_. Verifica se o arquivo `config.php` contém informações de lojas, grupos de lojas e sites. |
| `wizard:scd-on-deploy` | Verifique se a variável de ambiente global `SCD_ON_DEMAND` é `false` e a variável de ambiente `SKIP_SCD` é `false` para o estágio _deploy_. Verifica se o arquivo `config.php` contém _NOT_ a lista de lojas, grupos de lojas e sites com informações relacionadas. |

Como exemplo, você pode verificar se a sua configuração ativa corretamente o recurso SCD sob demanda:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Uma configuração bem-sucedida retorna:

```
SCD on-demand is enabled
```

Uma configuração com falha retorna:

```
SCD on-demand is disabled
```

## Verificar uma configuração ideal

A configuração _ideal_ para o projeto na nuvem ajuda a minimizar o tempo de inatividade da implantação, aquecendo o cache e gerando conteúdo estático quando solicitado pelo usuário. Este assistente é executado automaticamente durante o processo de implantação. Se a Nuvem não estiver configurada para este _estado ideal_, você receberá uma mensagem semelhante à seguinte:

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

Com base na saída, você precisa fazer as seguintes correções na sua configuração:

1. Ative a variável Skip HTML minification.

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Configure o gancho pós-implantação.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Envie alterações de código e execute o teste novamente. Quando sua configuração for _ideal_, você receberá a seguinte mensagem.

   ```
   Ideal state is configured
   ```
