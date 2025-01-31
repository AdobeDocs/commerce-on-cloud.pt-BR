---
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---
# Trechos de nuvem

## aviso de Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>O Elasticsearch 7.11 e posterior não é compatível com o Adobe Commerce na infraestrutura em nuvem. As versões do Adobe Commerce 2.3.7-p3, 2.4.3-p2 e 2.4.4 e posteriores são compatíveis com o serviço OpenSearch. As instalações locais continuam a suportar o Elasticsearch.

## Integração aprimorada {#enhanced-integration-envs}

>[!NOTE]
>
>Os projetos provisionados antes de 5 de junho de 2020 tinham vários ambientes de integração menores. Se você precisar de um ambiente de Integração maior para teste e desenvolvimento, solicite uma atualização para os ambientes de Integração aprimorada. Consulte o artigo [Solicitação de ambiente de integração](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) na _Central de Ajuda da Adobe Commerce_ para obter detalhes.

## Opções de mesclagem {#merge-options}

Por padrão, o processo de implantação substitui todas as configurações no arquivo `env.php`; no entanto, você pode optar por mesclar um ou mais valores para uma configuração de serviço sem substituir todos os valores.

Defina a opção `_merge` como uma das opções a seguir:

- `true`—**Mesclar** os valores de serviço configurados com os valores de variável de ambiente.
- `false`—**Substituir** os valores de serviço configurados com os valores de variável de ambiente.

## Repositório privado {#private-repository}

>[!NOTE]
>
>A Adobe recomenda o uso de um repositório privado para o seu projeto do Adobe Commerce na infraestrutura em nuvem para proteger qualquer informação proprietária ou trabalho de desenvolvimento, como extensões e configurações confidenciais.

## Aviso de autoatendimento do profissional {#pro-self-service-warning}

>[!WARNING]
>
>Alguns **projetos profissionais** exigem um tíquete de suporte para atualizar a configuração de rota no arquivo `routes.yaml` e a configuração cron no arquivo `.magento.app.yaml`. O Adobe recomenda atualizar e testar arquivos de configuração YAML em um ambiente de Integração e, em seguida, implantar alterações no ambiente de Preparo. Se suas alterações não forem aplicadas aos sites de Preparo após a reimplantação e não houver mensagens de erro relacionadas no log, você **DEVE** [Enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) que descreva as tentativas de alterações de configuração. Inclua quaisquer arquivos de configuração YAML atualizados no ticket.

## Suporte a serviços profissionais {#pro-update-service}

>[!TIP]
>
>Para projetos Pro, você deve [enviar um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para instalar ou atualizar os [serviços](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html) somente em ambientes `Staging` e `Production`.
>
>Indique as mudanças de serviço necessárias, inclua os arquivos atualizados do `.magento.app.yaml` e do `services.yaml` e informe a versão do PHP no tíquete. Para alterações de autoatendimento na versão, extensões ou configurações do ambiente do PHP, consulte [configurações do PHP](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html) em _Configuração do aplicativo_.
>
>Para alterações em um ambiente de Produção em tempo real (**Somente profissionais**), é necessário um aviso de no mínimo 48 horas. Isso permite que a equipe de infraestrutura da nuvem tenha tempo suficiente para empacotar recursos e realizar uma atualização segura. O período de aviso começa quando a equipe de infraestrutura reconhece a solicitação e programa a atualização, exceto nos finais de semana. Por exemplo, para que os upgrades de serviço sejam concluídos na segunda-feira, uma confirmação do upgrade agendado deve ser recebida até quarta-feira. Durante períodos de pico de demanda, pode levar mais tempo para processar sua solicitação.

## Backups Pro {#pro-backups}

>[!TIP]
>
>Em ambientes de preparo e produção profissionais, você deve [enviar um tíquete de suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para recuperar um backup específico, anotando a data, a hora e o fuso horário no tíquete.
>
>O Adobe **não** restaura quaisquer ambientes a partir de um backup automático. Consulte [Restaurar um instantâneo do BD de Preparo ou Produção](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) para obter ajuda sobre como escolher um método para restaurar um instantâneo de Preparo ou Produção.

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
>Alguns projetos exigem um fluxo de trabalho de desenvolvimento mais sofisticado. Para atender a essa necessidade, o Adobe oferece um [ambiente de preparo adicional](/help/cloud-guide/test/second-staging.md) como uma opção complementar à sua infraestrutura em nuvem.

## Instrução de serviço {#service-instruction}

Use as instruções a seguir para a configuração do serviço em ambientes Pro Integration e Starter, incluindo a ramificação `master`.

>[!NOTE]
>
>[Envie um tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) para alterar a configuração do serviço em ambientes de Produção e Preparo Profissionais.

## Alteração de serviço {#service-change-tip}

>[!TIP]
>
>Após a configuração inicial do serviço, você pode alterar a versão do software de um serviço instalado atualizando os arquivos de configuração `services.yaml` e `.magento.app.yaml`. Consulte [Alterar versão de serviço](/help/cloud-guide/services/services-yaml.md#change-service-version) para obter orientação sobre como atualizar ou rebaixar um serviço.

## Dica de implantação travada {#stuck-deployment-tip}

>[!TIP]
>
>Para obter ajuda com implantações paralisadas, use o [solucionador de problemas de implantação do Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) na _Central de Ajuda do Commerce_.

## Atualização para ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Se você usar uma versão do Adobe Commerce na infraestrutura em nuvem que não contenha o pacote `ece-tools`, será necessário executar uma [atualização única](/help/cloud-guide/dev-tools/install-package.md) para o projeto em nuvem, a fim de remover pacotes obsoletos. Se você usa atualmente o pacote `ece-tools` e precisa atualizá-lo, consulte [Atualizar o pacote ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Dica de atualização {#upgrade-tip}

>[!TIP]
>
>Antes de iniciar uma atualização ou um processo de patch, crie uma ramificação ativa do ambiente de integração e faça check-out da nova ramificação para a estação de trabalho local. Dedicar uma ramificação à atualização ou ao processo de patch ajuda a evitar interferência com o trabalho em andamento.

<!-- Fastly-related snippets begin -->

## Logon de administrador {#admin-login-step}

1. [Faça logon](/help/get-started/onboarding.md#access-your-admin-panel) no Administrador.

## Automatizar a implantação de trecho de VCL personalizado {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Em vez de carregar manualmente trechos de VCL personalizados, você pode adicionar trechos ao diretório `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` em seu ambiente. Os trechos neste diretório são carregados automaticamente quando você clica em _carregar VCL para Fastly_ no Commerce Admin. Consulte [Implantação automatizada de trechos de VCL personalizados](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) no módulo Fastly CDN para obter a documentação do Magento 2.

<!-- Fastly-related snippets end -->
