---
source-git-commit: 79ac13115bd3f275651a5477f2939c8f00a5a985
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 0%

---
# Suporte a serviços profissionais e disponibilidade para o cliente

## Suporte a serviços profissionais

Para solicitar e concluir uma atualização de serviço Pro em Preparo ou Produção, siga estas etapas:

1. **Para instalar ou atualizar os [serviços](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml) somente nos ambientes `Staging` e `Production`**, envie um [tíquete de Suporte da Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket).

   No ticket, especifique as alterações de serviço necessárias, inclua os arquivos `.magento.app.yaml` e `.magento/services.yaml` atualizados e observe a versão do PHP de destino.

   A versão do PHP, as atualizações do Composer, as extensões e as configurações do ambiente são alterações de autoatendimento. O Adobe pode precisar atualizar o agente New Relic para compatibilidade de versão do PHP. Consulte [configurações do PHP](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings) em _Configuração do aplicativo_.

   >[!IMPORTANT]
   >
   >Ao selecionar o campo **[!UICONTROL Environment]** no formulário de tíquete, use a nomenclatura de ambiente do Adobe. Por exemplo, selecione Preparo mesmo se você chamar esse ambiente de **Desenvolvimento** internamente. Você pode mencionar seu nome interno na descrição, mas o campo [!UICONTROL Environment] deve usar a nomenclatura da Adobe.

1. **Confirme o agendamento da atualização** por meio do processo de duas partes da Adobe: primeiro você confirma a data e a hora solicitadas e, em seguida, o suporte a envia à equipe de infraestrutura para confirmação final.

   As alterações na produção (somente Pro) exigem um aviso de pelo menos dois dias úteis, exceto nos finais de semana. Por exemplo, a equipe de infraestrutura em nuvem deve reconhecer uma atualização de segunda-feira até a quarta-feira anterior. Espere um lead time adicional durante o pico da demanda. Para evitar atrasos, responda à solicitação inicial pelo menos 48 horas antes da janela. A atualização não é considerada programada até que você receba a confirmação final.

   >[!NOTE]
   >
   >Fornecer janelas de manutenção em UTC. As atualizações de preparo não são agendadas com antecedência e normalmente são concluídas no mesmo dia da solicitação.
   >
   >Após uma atualização do RabbitMQ, reimplante o ambiente para reinicializar as filas de mensagens.

1. **Valide a atualização** em um ambiente de Preparo ou Integração antes de agendá-la na Produção.

   Problemas causados por módulos de terceiros, código personalizado ou compatibilidade de dependência geralmente surgem durante a reimplantação que segue uma atualização de serviço. Para validar várias atualizações de serviço, uma ordem razoável é Valkey ou Redis, RabbitMQ, OpenSearch e MariaDB. Esta não é uma sequência obrigatória. Os upgrades de bancos de dados têm o maior impacto operacional e merecem a maior cautela.

   A Adobe não garante a duração exata de uma janela de manutenção de Produção com antecedência, pois o tempo depende do ambiente e dos serviços envolvidos. Use o tempo que leva a atualização em preparo como uma estimativa prática ao planejar a janela Produção.

1. **Reimplante o ambiente** depois que o Adobe concluir a atualização do serviço para que a alteração entre em vigor, mesmo que a versão do aplicativo Adobe Commerce não seja alterada.

   Se a atualização incluir o OpenSearch, também planeje uma reindexação completa. A Adobe não pode garantir tempo de inatividade zero para uma atualização de serviço. Portanto, planeje uma janela de manutenção que permita tempo para reimplantar, reindexar, se necessário, e validar a loja e o administrador antes de reabrir o site.

## Disponibilidade do cliente durante atualizações

**Um representante de sua equipe ou parceiro de implementação deve estar disponível online durante a janela de atualização de Produção agendada.** O agendamento durante um período de tráfego baixo não tira as mãos da atualização. O Adobe gerencia a atualização da infraestrutura em nuvem, mas não pode validar o comportamento do aplicativo, as integrações, o código personalizado ou os fluxos de trabalho de negócios.

O representante disponível deve poder:

- **Monitorar** a loja e as transações comerciais críticas durante e após a atualização.
- **Responda** às perguntas do Suporte da Adobe ou da equipe de Infraestrutura em Nuvem.
- **Confirme** se as integrações, extensões, personalizações, trabalhos cron, filas e outras funções específicas do cliente estão funcionando conforme o esperado.
- **Validar** fluxos de trabalho críticos para os negócios, como check-out, exibições de catálogo, pesquisa, logon e processamento de pedido.
- **Relatar** comportamento inesperado prontamente, enquanto o contexto de atualização e os logs ainda estão disponíveis.

>[!TIP]
>
>Para projetos Pro, as atualizações de serviço na produção também exigem programação antecipada e um processo de confirmação em duas partes com o suporte da Adobe. Consulte [Suporte a serviços profissionais](#pro-services-support).

### Modo de manutenção

**O modo de manutenção não substitui a disponibilidade do cliente.** O modo de manutenção bloqueia o acesso da loja, mas não valida serviços de aplicativos, integrações, filas, trabalhos cron, check-out ou outras funções específicas do cliente.

Se o trabalho planejado exigir o modo de manutenção, coordene o uso com o Suporte da Adobe e siga as instruções para essa atualização. Depois, confirme se a loja e os workflows críticos estão funcionando normalmente antes de considerar o trabalho como concluído.
