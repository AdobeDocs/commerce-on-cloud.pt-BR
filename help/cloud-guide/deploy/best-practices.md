---
title: Práticas recomendadas de implantação
description: Descubra as práticas recomendadas para implantar o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Práticas recomendadas de implantação

Os scripts de criação e implantação são ativados quando você mescla códigos em um ambiente remoto. Esses scripts usam os [arquivos de configuração](../environment/overview.md) do ambiente e o código do aplicativo para provisionar a infraestrutura de nuvem com dados e serviços apropriados. Além disso, esses scripts são usados para instalar ou atualizar o aplicativo do Adobe Commerce, serviços de terceiros e extensões personalizadas no ambiente de nuvem.

O processo de criação e implantação é um pouco diferente para cada plano:

- **Planos iniciais** — Para o ambiente de integração, cada ramificação ativa cria e implanta em um ambiente completo para acesso e teste. Teste todo o código depois de mesclar com a ramificação `staging`. Para iniciar seu site, envie `staging` por push para `master` para implantar no ambiente de Produção. Você tem acesso total a todas as ramificações por meio dos comandos [!DNL Cloud Console] e CLI.

- **Planos profissionais**—Para o ambiente de integração, cada ramificação ativa cria e implanta em um ambiente completo para acesso e teste. Mescle o código na ramificação `integration` antes de mesclar com os ambientes de preparo e produção. Você pode mesclar com os ambientes de Preparo e Produção usando o [!DNL Cloud Console] ou usando comandos SSH e CLI do `magento-cloud`.

## Rastrear o processo

É possível rastrear ações de compilação e implantação em tempo real usando o terminal ou as [!DNL Cloud Console] mensagens de status—`in-progress`, `pending`, `success` ou `failed`—exibidas durante o processo de implantação. Você pode exibir detalhes nos arquivos de log. Consulte [Exibir logs](../test/log-locations.md).

Se você estiver usando repositórios GitHub externos, o log de operações não será exibido na sessão do GitHub. No entanto, você ainda pode seguir atividades na interface para o repositório externo e o [!DNL Cloud Console]. Consulte [Integrações](../integrations/overview.md).

>[!NOTE]
>
>Em ambientes de integração, não é possível visualizar os logs de implantação do [!DNL Cloud Console]. Esse recurso está disponível somente para ambientes de produção e de preparo. No entanto, você pode visualizar logs para cada fase da implantação em qualquer ambiente usando os logs de [compilação e implantação](../test/log-locations.md#build-and-deploy-logs). Para obter informações sobre solução de problemas, consulte a [Referência de erro de implantação](../dev-tools/error-reference.md).

Você pode habilitar o [Rastrear implantações com o New Relic](../monitor/track-deployments.md) para monitorar eventos de implantação e analisar o desempenho entre implantações.

## Práticas recomendadas para builds e implantação

Revise estas práticas recomendadas e considerações para seu processo de implantação:

- **Verifique se você está executando a versão mais recente do pacote `ece-tools`**

  Consulte as [Notas de versão para ECE-Tools](../release-notes/ece-tools-package.md).

- **Siga o processo de compilação e implantação**

  Verifique se você tem o código correto em cada ambiente para evitar a substituição de configurações ao mesclar código entre ambientes. Por exemplo, para aplicar alterações de configuração a todos os ambientes, modifique e teste as alterações no ambiente local antes de implantar no ambiente de integração remota. Em seguida, implante e teste as alterações no ambiente de preparo antes de implantar na produção. Quando você mescla de um ambiente para outro, a implantação substitui todo o código no ambiente, exceto as configurações específicas do ambiente.

- **Usar as mesmas variáveis entre ambientes**

  Os valores dessas variáveis podem diferir entre os ambientes; no entanto, normalmente são necessárias as mesmas variáveis em cada ambiente. Consulte [Gerenciamento de configurações para configurações de armazenamento](../store/store-settings.md).

- **Manter valores e dados confidenciais de configuração em variáveis específicas do ambiente**

  Esses valores incluem variáveis especificadas usando a CLI da Nuvem, o [!DNL Cloud Console] ou adicionadas ao arquivo `env.php`. Consulte [Níveis de variáveis](../environment/variable-levels.md).

- **Verifique se todo o código está disponível na ramificação do ambiente**

  Fazer referência ao código de outras ramificações, como uma ramificação privada, pode causar problemas durante o processo de compilação e implantação. Por exemplo, se você fizer referência a um tema de uma ramificação privada, ele não estará acessível e não poderá ser criado com o código do aplicativo.

- **Adicionar extensões, integrações e código em ramificações iteradas**

  Fazer e testar as alterações localmente, enviar para `integration`, depois para `staging` e `production`. Teste e resolva os problemas em cada ambiente antes de mesclar as atualizações com o próximo ambiente. Algumas extensões e integrações devem ser ativadas e configuradas em uma ordem específica devido às dependências. Adicionar e testar em grupos pode tornar o processo de criação e implantação muito mais fácil e ajudar a determinar onde ocorrem problemas.

- **Verifique as versões e relações do serviço e a capacidade de conexão**

  Verifique os serviços disponíveis para seu aplicativo e certifique-se de que você esteja usando a versão mais atual e compatível. Consulte [Relacionamentos de serviço](../services/services-yaml.md#service-relationships) e [Requisitos do sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=pt-BR) no _Guia de instalação_ para obter as versões recomendadas.

- **Testar localmente e no ambiente de integração antes de implantar em Preparo e Produção**

  Identifique e corrija problemas em seus ambientes locais e de integração para evitar um tempo de inatividade prolongado ao implantar em ambientes de preparo e produção.

  >[!TIP]
  >
  >Há comandos do [assistente inteligente](../deploy/smart-wizards.md) que você pode usar para verificar se a configuração do seu projeto de nuvem segue as práticas recomendadas para configuração de compilação e implantação, incluindo a estratégia de implantação de conteúdo estático (SCD).

- **Após concluir o teste em ambientes locais e de integração, implante e teste no ambiente de preparo**

  Consulte [Teste de preparo e produção](../test/staging-and-production.md).

- **Verificar configuração do ambiente de Produção**

  Antes de implantar na produção, conclua as seguintes tarefas:

   - Verifique se você pode se conectar a todos os três nós no ambiente de Produção usando o [SSH](../development/secure-connections.md).

   - Verifique se os Indexadores estão definidos como _Atualizar na Agenda_. Consulte [Modos de indexação](https://developer.adobe.com/commerce/php/development/components/indexing/) no _Guia de Desenvolvedor de Extensão_.

   - Prepare o ambiente atualizando quaisquer variáveis específicas do ambiente no código de Produção, verificando a disponibilidade e a compatibilidade do serviço e fazendo quaisquer outras alterações de configuração necessárias.

- **Monitorar o processo de implantação**

  Revise as mensagens de status da implantação e reduza os problemas conforme necessário. Revise os [logs](../test/log-locations.md#) da nuvem para obter mensagens de log detalhadas.

## Cinco fases de criação e implantação da integração

As fases a seguir ocorrem no ambiente de desenvolvimento local e no ambiente de integração. Para planos Pro, o código não é implantado nos ambientes de armazenamento temporário ou produção nessas fases iniciais.

### Fase 1: validação de código e configuração

Ao configurar inicialmente um projeto, [o modelo de infraestrutura em nuvem](https://github.com/magento/magento-cloud) fornece uma base para os arquivos de código. Este repositório de código foi clonado para seu projeto como a ramificação `master`.

- **Para Início**—`master` a ramificação é seu ambiente de Produção.
- **Para Pro**—`master` começa como ramificação de origem para o ambiente de integração.

Crie uma ramificação no `master` para seu código personalizado, extensões e módulos e integrações de terceiros. Há um ambiente de integração remota para testar seu código na nuvem.

Quando você envia seu código do espaço de trabalho local para o repositório remoto, uma série de verificações e validações de código é concluída antes do início da criação e implantação dos scripts. O servidor Git integrado valida o que você está enviando e faz alterações. Por exemplo, se você adicionar um serviço OpenSearch, o servidor Git integrado verificará se a topologia do cluster foi modificada adequadamente.

Se você tiver um erro de sintaxe em um arquivo de configuração, o servidor Git rejeitará o push. Consulte [Bloco Protetor](../development/protective-block.md).

Esta fase também executa `composer install` para recuperar dependências.

### Fase 2: criação

>[!NOTE]
>
>Durante a fase de criação, o site não está no modo de manutenção e se ocorrerem erros ou problemas não será desativado. Ele cria apenas o código que foi alterado desde a build anterior.

Esta fase cria a base de código e executa ganchos na seção `build` de `.magento.app.yaml`. O gancho de compilação padrão é o comando `php ./vendor/bin/ece-tools` e executa o seguinte:

- Aplica patches em `vendor/magento/ece-patches` e patches opcionais específicos do projeto em `m2-hotfixes`
- Regenera o código e a configuração de [injeção de dependência](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/implementation-playbook/glossary) (ou seja, o diretório `generated/`, que inclui `generated/code` e `generated/metapackage`) usando `bin/magento setup:di:compile`.
- Verifica se o arquivo [`app/etc/config.php`](../store/store-settings.md) existe na base de código. O Adobe Commerce gera automaticamente esse arquivo se ele não for detectado durante a fase de criação e incluir uma lista de módulos e extensões. Se existir, a fase de criação continua normalmente, compacta arquivos estáticos usando GZIP e implanta, o que reduz o tempo de inatividade na fase de implantação. Consulte as [opções de compilação](../environment/variables-build.md) para saber mais sobre como personalizar ou desabilitar a compactação de arquivos.

>[!WARNING]
>
>Neste ponto, o cluster não foi criado, portanto, não tente se conectar a um banco de dados ou suponha que haja um processo de daemon ativo.

Após a compilação do aplicativo, ele é montado em um **sistema de arquivos somente leitura**. Você pode configurar pontos de montagem específicos que serão de leitura/gravação. Não é possível transferir um FTP para o servidor e adicionar módulos. Em vez disso, você deve adicionar código ao repositório local e executar o `git push`, que compila e implanta o ambiente. Para obter a estrutura do projeto, consulte [Estrutura de diretório de projeto local](../project/file-structure.md).

### Fase 3: Preparar a slug

O resultado da fase de compilação é um sistema de arquivos somente leitura conhecido como _slug_. Essa fase cria um arquivamento e coloca o slug em armazenamento permanente. Na próxima vez que você enviar o código, se um serviço não for alterado, ele usará o slug do arquivamento.

- Acelera a integração contínua ao reutilizar o código inalterado
- Se o código for alterado, atualiza a descrição da próxima build a ser reutilizada
- Permite a reversão instantânea de uma implantação, se necessário
- Inclui arquivos estáticos se o arquivo `app/etc/config.php` existir na base de código

O slug inclui todos os arquivos e pastas **excluindo as seguintes** montagens configuradas em `magento.app.yaml`:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Fase 4: implantação de slugs e cluster

Seus aplicativos e todos os serviços de [back-end](https://experienceleague.adobe.com/pt-br/docs/commerce-operations/implementation-playbook/glossary) são provisionados da seguinte maneira:

- Monta cada serviço em um contêiner, como servidor Web, OpenSearch, [!DNL RabbitMQ]
- Monta o sistema de arquivos de leitura e gravação (montado em uma grade de armazenamento distribuída altamente disponível)
- Configura a rede para que os serviços possam &quot;ver&quot; uns aos outros (e somente uns aos outros)

>[!NOTE]
>
>Faça as alterações em uma ramificação Git após a conclusão da compilação e da implantação e envie novamente. Todos os sistemas de arquivos de ambiente são _somente leitura_. Um sistema somente leitura garante implantações determinísticas e melhora drasticamente a segurança do site, pois nenhum processo pode gravar no sistema de arquivos. Também funciona para garantir que seu código seja idêntico nos ambientes de integração, preparo e produção.

### Fase 5: Ganchos de implantação

>[!NOTE]
>
>Esta fase coloca o aplicativo [!DNL Commerce] no modo de manutenção até que a implantação seja concluída.

A última etapa executa um script de implantação, que pode ser usado para tornar os dados anônimos em ambientes de desenvolvimento, limpar caches e consultar ferramentas de integração contínua externa. Quando esse script é executado, você tem acesso a todos os serviços em seu ambiente, como o Redis.

Se o arquivo `app/etc/config.php` não existir na base de código, os arquivos estáticos serão compactados usando `gzip` e implantados durante esta fase, o que aumenta a duração da fase de implantação e da manutenção do site.

>[!NOTE]
>
>Consulte [implantar variáveis](../environment/variables-deploy.md) para saber mais sobre como personalizar ou desabilitar a compactação de arquivos.

Há dois ganchos de implantação. O gancho `pre-deploy.php` conclui a limpeza e a recuperação necessárias dos recursos e do código gerados no gancho de compilação. O gancho `php ./vendor/bin/ece-tools deploy` executa uma série de comandos e scripts:

- Se o Adobe Commerce estiver **não instalado**, ele será instalado com `bin/magento setup:install`, atualizará a configuração de implantação, `app/etc/env.php`, e o banco de dados para seu ambiente especificado, como Redis e URLs de sites. **Importante:** quando você concluiu a [Primeira implantação](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html?lang=pt-BR) durante a instalação, o Adobe Commerce foi instalado e implantado em todos os ambientes.

- Se o Adobe Commerce **estiver instalado**, faça as atualizações necessárias. O script de implantação executa o `bin/magento setup:upgrade` para atualizar o esquema e os dados do banco de dados (o que é necessário após atualizações de extensão ou de código principal) e também atualiza a configuração de implantação, `app/etc/env.php`, e o banco de dados para o seu ambiente. Finalmente, o script de implantação limpa o cache do Adobe Commerce.

- O script gera opcionalmente conteúdo estático da Web usando o comando `magento setup:static-content:deploy`.

- Usa escopos (`-s` sinalizador em scripts de compilação) com uma configuração padrão de `quick` para estratégia de implantação de conteúdo estático. Você pode personalizar a estratégia usando a variável de ambiente [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). Para obter detalhes sobre essas opções e recursos, consulte [Estratégias de implantação de arquivos estáticos](../deploy/static-content.md) e o sinalizador `-s` para [Implantar arquivos de exibição estáticos](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=pt-BR).

>[!NOTE]
>
>O script de implantação usa os valores definidos pelos arquivos de configuração no diretório `.magento` e, em seguida, o script exclui o diretório e seu conteúdo. O ambiente de desenvolvimento local não é afetado.

### Pós-implantação: configurar roteamento

Enquanto a implantação está em execução, o processo interrompe o tráfego de entrada no ponto de entrada por 60 segundos e reconfigura o roteamento para que o tráfego da Web chegue ao cluster recém-criado.

A implantação bem-sucedida remove o modo de manutenção para permitir acesso normal e cria arquivos de backup (BAK) para os arquivos de configuração `app/etc/env.php` e `app/etc/config.php`.

Habilite a geração de conteúdo estático usando a variável `SCD_ON_DEMAND` e configure o [`post_deploy` gancho](../application/hooks-property.md) para que ele limpe o cache e pré-carregue (aquece) o cache _depois_, o contêiner começa a aceitar conexões e _durante_ tráfego de entrada normal.

Para revisar logs de compilação e implantação, consulte [Exibir logs](../test/log-locations.md#view-and-manage-logs).
