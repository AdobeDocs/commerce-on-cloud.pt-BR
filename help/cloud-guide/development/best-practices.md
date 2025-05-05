---
title: Práticas recomendadas para atualizar seu projeto
description: Consulte uma lista de práticas recomendadas para atualizar os arquivos do projeto.
feature: Cloud, Best Practices, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Práticas recomendadas para atualizar seu projeto

Siga as práticas recomendadas para compilações e implantações e use o fluxo de trabalho [Atualizações e patches](../development/commerce-version.md) para atualizar seu aplicativo. Use as diretrizes a seguir para planejar seu trabalho de atualização e pós-atualização:

- **Faça backup do seu projeto**-Antes de atualizar a Adobe Commerce e quaisquer extensões personalizadas ou de terceiros, faça backup do banco de dados nos ambientes de Integração, Preparo e Produção. Consulte [Fazer backup do banco de dados](../development/commerce-version.md#project-backup).

- **Verificar problemas de compatibilidade**-

   - Verifique se todos os temas personalizados são compatíveis com a nova versão do Adobe Commerce

   - Depois de atualizar extensões personalizadas e de terceiros, use o comando `magento-cloud local:build` para validar as dependências do Composer antes de implantar.

   - Revise as notas de versão e a documentação de extensão do Adobe Commerce para garantir que você implementou qualquer solução alternativa ou alteração de configuração necessária para resolver problemas funcionais conhecidos e bugs relacionados à versão e extensões atualizadas do Adobe Commerce.

   - Verifique se as versões instaladas do serviço são compatíveis com a nova versão do Adobe Commerce e atualize os serviços conforme necessário. Consulte [Serviços](../services/services-yaml.md).

   - Teste seu banco de dados para resolver qualquer problema introduzido pelas atualizações da versão e das extensões do Adobe Commerce.

   - Faça as atualizações necessárias nas configurações específicas do ambiente antes de implantar no ambiente remoto.

   - Certifique-se de que a versão do serviço de pesquisa é compatível com a versão do cliente PHP. Consulte [Configurar Elasticsearch](../services/elasticsearch.md) ou [Configurar OpenSearch](../services/opensearch.md).

- **Verifique a conectividade do banco de dados e o armazenamento disponível em ambientes remotos**-

   - Use o SSH para fazer logon no servidor remoto e verificar a conexão com o banco de dados MySQL. Consulte [Conectar ao banco de dados](../services/mysql.md#connect-to-the-database).

   - Verificar o armazenamento disponível no ambiente remoto - Use o comando `disk free` para exibir e gerenciar o espaço em disco disponível nos seus ambientes de Nuvem. Consulte [Gerenciar espaço em disco](../storage/manage-disk-space.md).

      - Verifique o tamanho do banco de dados atualizado e se o arquivo `services.yaml` tem espaço em disco suficiente alocado.

      - Libere espaço em disco - Limpe o cache e limpe os diretórios `/log` e `/tmp` antes de implantar.

- **Planeje e execute uma atualização bem-sucedida em ambientes locais e de integração, antes de implantar em Preparo**. Após a atualização, teste sua implantação e resolva os problemas.

- **Mesclar código para Preparo e, em seguida, para Produção** - Testar e resolver quaisquer problemas no ambiente de Preparo antes de enviar as alterações para o ambiente de Produção.

- **Concluir tarefas pós-atualização**-

   - Use o SSH para fazer logon no servidor remoto e verifique o seguinte:

      - Verifique o status do indexador e reindexe conforme necessário. Consulte [Gerenciar os indexadores](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=pt-BR) no _Guia de configuração_.

      - Verifique os logs do `cron` e a tabela `cron_schedule` no banco de dados do Adobe Commerce para verificar o status do cron e execute novamente os trabalhos do cron, conforme necessário.
Consulte [Logging](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=pt-BR#logging) no _Guia de Configuração_.

   - Conclua o UAT de teste de aceitação do usuário pós-atualização em ambientes de preparo e produção e corrija quaisquer problemas relacionados a atualizações de extensões personalizadas e de terceiros.
