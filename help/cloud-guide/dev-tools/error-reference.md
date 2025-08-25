---
title: Mensagens de erro do pacote ECE-Tools
description: Consulte uma lista de códigos de erro e mensagens que podem ocorrer durante os processos de criação, implantação e pós-implantação do Adobe Commerce na infraestrutura em nuvem.
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Mensagens de erro para ECE-Tools

Esta referência de mensagem de erro fornece informações para solucionar erros que podem ocorrer durante os processos de criação, implantação e pós-implantação do Adobe Commerce na infraestrutura em nuvem.

Todas as mensagens de erro críticas e de aviso que ocorrem durante a implantação são gravadas nos arquivos `var/log/cloud.log` e `/var/log/cloud.error.log`. O arquivo de log de erros da nuvem contém apenas erros da implantação mais recente. Um arquivo vazio indica uma implantação bem-sucedida sem erros.

No arquivo `cloud.error.log`, cada entrada é formatada como uma cadeia de caracteres JSON para facilitar a análise:

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

As mensagens de erro são categorizadas por um dos estágios de implantação: criação, implantação e pós-implantação. Cada seção fornece uma lista de erros associados com as seguintes informações para cada erro:

- **Código de erro**: o identificador atribuído pela Adobe Commerce para a mensagem de erro
- **Estágio**: indica se o erro ocorreu durante o estágio de compilação, implantação ou pós-implantação
- **Etapa**: indica a etapa no cenário de implantação que pode retornar o erro. Se a coluna _Etapa_ estiver em branco, o erro será um erro geral que pode ser retornado por várias etapas ou durante operações de pré-processamento. Consulte [Implantação baseada em cenário](../deploy/scenario-based.md) para obter mais informações sobre as etapas de compilação, implantação e pós-implantação.
- **Sugestão**: fornece orientação para solucionar e resolver o erro
- **Título (Descrição do erro)**: uma descrição que resume a causa do erro
- **Tipo**: indica se o erro é um erro crítico ou um aviso

{{$include /help/_includes/automated/ece-tools-error-codes.md}}

<!-- Last updated from includes: 2025-05-28 21:01:41 -->
