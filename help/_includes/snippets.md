---
source-git-commit: 67ed09e3b7c5f5218407b6648e8ca2c32933bbda
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---
# Trechos de nuvem

## Aviso do Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>O Elasticsearch 7 e posterior não é compatível com o Adobe Commerce na infraestrutura em nuvem. O Adobe Commerce 2.4.4 e posterior é compatível com o serviço OpenSearch.

## Integração aprimorada {#enhanced-integration-envs}

>[!NOTE]
>
>Os projetos provisionados antes de 5 de junho de 2020 tinham vários ambientes de integração menores. Se você precisar de um ambiente de Integração maior para teste e desenvolvimento, solicite uma atualização para os ambientes de Integração aprimorada. Consulte o artigo [Solicitação de ambiente de integração](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27242) na _Central de Ajuda da Adobe Commerce_ para obter detalhes.

## Opções de mesclagem {#merge-options}

Por padrão, o processo de implantação substitui todas as configurações no arquivo `env.php`; no entanto, você pode optar por mesclar um ou mais valores para uma configuração de serviço sem substituir todos os valores.

Defina a opção `_merge` como uma das opções a seguir:

- `true`—**Mesclar** os valores de serviço configurados com os valores de variável de ambiente.
- `false`—**Substituir** os valores de serviço configurados com os valores de variável de ambiente.

## Repositório privado {#private-repository}

>[!NOTE]
>
>A Adobe recomenda usar um repositório privado para seu projeto Adobe Commerce na infraestrutura em nuvem para proteger qualquer informação proprietária ou trabalho de desenvolvimento, como extensões e configurações confidenciais.

## Aviso de autoatendimento do profissional {#pro-self-service-warning}

>[!WARNING]
>
>Alguns **projetos profissionais** exigem a assistência do Suporte da Adobe para atualizar as configurações de rota no arquivo `routes.yaml` e as configurações cron no arquivo `.magento.app.yaml`. A Adobe recomenda fazer e validar primeiro todas as alterações de configuração do YAML em um ambiente de integração e, em seguida, implantá-las no ambiente de preparo.
>
>
>Se suas alterações não forem refletidas nos sites de Preparo após a reimplantação e não houver mensagens de erro relacionadas no log, você **deve** [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). No ticket, descreva claramente as alterações de configuração que você tentou e anexe quaisquer arquivos de configuração YAML atualizados no ticket.

## Backups Pro {#pro-backups}

>[!TIP]
>
>Para recuperar um backup específico em ambientes de Preparo e Produção Pro, [envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) anotando a data, a hora e o fuso horário no tíquete.
>
>O Adobe **não** restaura qualquer ambiente a partir de um backup automático. Consulte [Restaurar um instantâneo do BD de Preparo ou Produção](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production) para obter ajuda sobre como escolher um método para restaurar um instantâneo de Preparo ou Produção.

## Aviso de reimplantação {#redeploy-warning}

>[!WARNING]
>
>O processo de implantação começa quando você executa uma mesclagem, envio por push ou sincronização de seu ambiente, ou quando você aciona uma reimplantação manual, durante a qual o aplicativo [!DNL Commerce] está em modo de manutenção. Para um ambiente de produção, a Adobe recomenda concluir esse trabalho fora do horário de pico para evitar interrupções do serviço.

## Espaço reservado da rota {#route-placeholder}

>[!NOTE]
>
>Os exemplos de configuração de rota a seguir usam modelos de rota com espaços reservados. O espaço reservado `{default}` representa o domínio padrão configurado para o site. Se o seu projeto tiver vários domínios, use o espaço reservado `{all}` para configurar o roteamento para o domínio padrão e todos os aliases. Consulte [Configurar rotas](/help/cloud-guide/routes/routes-yaml.md).

## Tempo de SCD {#scd-timing-warning}

>[!WARNING]
>
>Se tiver problemas com arquivos de conteúdo estático no aplicativo após a implantação, como arquivos de tema personalizados ausentes, aumente o tempo de execução máximo esperado para 900 segundos ou mais.

## Implantação baseada em cenário {#scenarios}

>[!NOTE]
>
>Com o [!DNL ECE-Tools] 2002.1.0 e versões posteriores, você pode usar o recurso de implantação baseada em cenário para personalizar os processos de compilação, implantação e pós-implantação do seu projeto Adobe Commerce na infraestrutura em nuvem. Consulte [Implantação baseada em cenário](/help/cloud-guide/deploy/scenario-based.md).

## Segundo preparo {#second-staging}

>[!NOTE]
>
>Alguns projetos exigem um fluxo de trabalho de desenvolvimento mais sofisticado. Para atender a essa necessidade, a Adobe oferece um [ambiente de preparo adicional](/help/cloud-guide/test/second-staging.md) como opção complementar à sua infraestrutura em nuvem.

## Instrução de serviço {#service-instruction}

Use as instruções a seguir para a configuração do serviço em ambientes Pro Integration e Starter, incluindo a ramificação `master`.

>[!NOTE]
>
>Para alterar a configuração do serviço em ambientes de Produção e Preparo Profissionais, [Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Para obter os requisitos de agendamento e as orientações de disponibilidade do cliente, consulte o [Suporte a serviços profissionais](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) em _Configurar serviços_.

## Alteração de serviço {#service-change-tip}

>[!TIP]
>
>Após a configuração inicial do serviço, você pode alterar a versão do software de um serviço instalado atualizando os arquivos de configuração `services.yaml` e `.magento.app.yaml`. Consulte [Alterar versão de serviço](/help/cloud-guide/services/services-yaml.md#change-service-version) para obter orientação sobre como atualizar ou rebaixar um serviço. Este método de autoatendimento não se aplica a ambientes de Preparo ou Produção Pro — consulte [Suporte a serviços Pro](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) em _Configurar serviços_.

## Dica de implantação travada {#stuck-deployment-tip}

>[!TIP]
>
>Para obter ajuda com implantações paralisadas, use o [solucionador de problemas de implantação do Adobe Commerce](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-29640) na _Central de Ajuda do Commerce_.

## Atualização para ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Para remover pacotes obsoletos em versões do Adobe Commerce na infraestrutura de nuvem que não contêm o pacote `ece-tools`, é necessário executar uma [atualização única](/help/cloud-guide/dev-tools/install-package.md) para o projeto de nuvem. Se você usa atualmente o pacote `ece-tools` e precisa atualizá-lo, consulte [Atualizar o pacote ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Dica de atualização {#upgrade-tip}

>[!TIP]
>
>Antes de iniciar uma atualização ou um processo de patch, crie uma ramificação ativa do ambiente de integração e faça check-out da nova ramificação para a estação de trabalho local. Dedicar uma ramificação à atualização ou ao processo de patch ajuda a evitar interferência com o trabalho em andamento.

## Valkey no New Relic {#valkey-newrelic}

>[!NOTE]
>
>O New Relic ainda pode mostrar o Redis mesmo após a migração para o Valkey.
>
>Espera-se que o New Relic continue se referindo ao serviço de cache como Redis mesmo após o ambiente ter sido migrado para o Valkey.
>
>Valkey é uma bifurcação de código aberto da Redis, e algumas ferramentas e integrações continuam a identificar o serviço usando a nomenclatura Redis em vez de um rótulo distinto da Valkey. Esse comportamento não indica necessariamente que o Redis ainda está instalado.

<!-- Fastly-related snippets begin -->

## Logon de administrador {#admin-login-step}

1. [Faça logon](/help/get-started/onboarding.md#access-your-admin-panel) no Administrador.

## Automatizar a implantação de trecho de VCL personalizado {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Em vez de carregar manualmente trechos de VCL personalizados, você pode adicionar trechos ao diretório `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` em seu ambiente. Os trechos neste diretório são carregados automaticamente quando você clica em _carregar VCL para Fastly_ no Commerce Admin. Consulte [Implantação de trechos de VCL personalizados automatizada](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) no módulo CDN Fastly para obter a documentação do Magento 2.

<!-- Fastly-related snippets end -->
