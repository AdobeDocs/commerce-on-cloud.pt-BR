---
title: Implantação sem tempo de inatividade
description: Saiba como reduzir o tempo de inatividade geral ao implantar o Adobe Commerce em projetos de infraestrutura em nuvem.
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
source-git-commit: b831bc5bce0f76ec8972b3578c500508dd4d7d41
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Implantação sem tempo de inatividade

O Adobe Commerce na infraestrutura em nuvem executa o aplicativo no modo de [_manutenção_](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode) durante a fase de implantação, o que coloca o site offline até que a implantação seja concluída. O tempo em que o site de produção está no modo de manutenção depende do tamanho do site, do número de alterações aplicadas durante a implantação e da configuração para implantação de conteúdo estático. É possível configurar seu projeto para que ele seja implantado com um efeito de tempo de inatividade **zero**.

Durante o processo de implantação, todas as conexões ficam em fila por até 5 minutos, preservando quaisquer sessões ativas e ações pendentes, como adicionar ao carrinho ou fazer checkout. Após a implantação, a fila é liberada e as conexões continuam sem interrupção. Para usar essa _suspensão de conexão_ a seu favor e reduzir a implantação para _zero_ tempo de inatividade, você deve configurar seu projeto para usar a estratégia de implantação mais eficiente.

>[!NOTE]
>
>Para verificar se o projeto na nuvem está configurado de forma ideal para minimizar o tempo de inatividade da implantação, use o [Assistente Inteligente](smart-wizards.md). O Assistente inteligente verifica a configuração atual e orienta você pelos ajustes de configuração recomendados para ativar as práticas recomendadas para implantações sem tempo de inatividade.

Use as seguintes etapas para reduzir a quantidade de tempo que seu armazenamento leva para implantar uma atualização na Produção:

1. [Atualize para o pacote `ece-tools`](../dev-tools/install-package.md) ou [atualize a versão `ece-tools`](../dev-tools/update-package.md)
Seu projeto de infraestrutura do Adobe Commerce na nuvem deve ter o pacote `ece-tools` mais recente para que você tenha as ferramentas disponíveis para configurar uma implantação ideal. Se você tiver o `ece-tools` mais recente, continue para a próxima etapa.

   >[!NOTE]
   >
   >Embora seja uma prática recomendada usar o pacote `ece-tools` mais recente, o método de implantação sem tempo de inatividade funciona com o `ece-tools` [versão 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) e posterior.

1. [Configurar a implantação de conteúdo estático](static-content.md)
Se a implantação de conteúdo estático falhar na fase de implantação, seu site ficará preso no modo de manutenção. Quando ocorre uma falha durante a fase de criação, o processo evita o tempo de inatividade porque nunca inicia a fase de implantação. [A geração de conteúdo estático durante a fase de compilação com o HTML minificado](static-content.md#setting-the-scd-on-build), também conhecido como estado ideal, é a configuração ideal para implantações sem tempo de inatividade e _previne_ o tempo de inatividade em caso de falha.

1. [Configurar o gancho pós-implantação](../application/hooks-property.md)
Você deve configurar o gancho pós-implantação para limpar e aquecer o cache. Por padrão, a limpeza do cache ocorre durante a fase de implantação, quando o site está inativo. Mover o cache limpo para a fase de pós-implantação significa que o cache permanece ativo até que a fase de implantação seja concluída e, em seguida, você pode limpar com segurança o cache.

   Personalize a lista de páginas usadas para pré-carregar o cache com a [variável de ambiente WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages).

1. [Reduzir arquivos de tema](../environment/variables-deploy.md#scdmatrix)
Você pode reduzir o número de arquivos de tema desnecessários configurando a variável de ambiente SCD\_MATRIX.

1. [Acelere a implantação de conteúdo estático](../environment/variables-deploy.md#scdthreads)
Você pode acelerar o processo de implantação atualizando a variável de ambiente SCD\_THREADS para aumentar o número de threads para implantação de conteúdo estático.

>[!NOTE]
>
>Você pode validar a configuração do seu projeto para implantação ideal [executando o assistente de estado ideal](smart-wizards.md#verifying-an-ideal-configuration).
