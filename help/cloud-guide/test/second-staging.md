---
title: Segundo ambiente de preparo
description: Saiba mais sobre os benefícios e usos de ter um segundo ambiente de preparo para testes paralelos, isolamento de problemas, controle de versão e muito mais.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Segundo ambiente de preparo

Na infraestrutura do Adobe Commerce Cloud, o ambiente de preparo garante testes e validação abrangentes em uma configuração que pode espelhar o ambiente de produção. Esse ambiente permite identificar e resolver bugs com segurança, ao mesmo tempo em que garante que qualquer novo recurso ou alteração seja perfeitamente integrado antes que afete seu site ativo.

Alguns projetos exigem um fluxo de trabalho de desenvolvimento mais sofisticado. Para atender a essa necessidade, o Adobe oferece um ambiente de preparo adicional como uma opção complementar à sua infraestrutura de nuvem. Ter dois ambientes de preparo é vantajoso para projetos complexos e colaboração simultânea entre equipes.

Ter um ambiente de preparo secundário oferece os seguintes benefícios:

- **Teste paralelo**: com dois ambientes de preparo, as equipes podem testar novos recursos e atualizações críticas simultaneamente sem interferir no desenvolvimento umas das outras. Por exemplo, você pode dedicar um ambiente ao teste de recursos, enquanto reserva o outro ambiente para testes de desempenho e estresse.

- **Isolamento de problemas**: quando surgem bugs ou problemas no ambiente de preparo primário, você pode replicá-los e diagnosticá-los no ambiente secundário sem interromper o progresso dos testes. Isso garante que a solução de problemas não interfira nos testes contínuos.

- **Controle de Versão**: gerencie versões diferentes do seu aplicativo com mais eficiência. O ambiente de preparo primário pode executar a build estável mais recente, enquanto o secundário testa recursos experimentais. Isso ajuda a gerenciar melhor os ciclos de lançamento e garante que as alterações experimentais não interrompam as builds estáveis.

- **Planejamento de implantação aprimorado**: você pode simular diferentes cenários de implantação. O ambiente de preparo principal pode imitar a configuração de produção atual, enquanto o ambiente secundário pode ser configurado para testar versões futuras.

- **Teste de aceitação do usuário (UAT)**: ao usar o ambiente primário para desenvolvimento contínuo, você pode dedicar o ambiente secundário ao UAT. Isso permite que os usuários ou clientes testem e forneçam feedback sobre novos recursos em uma configuração controlada, sem afetar o teste.

- **Teste de regressão**: você pode usar um ambiente para testes de encaminhamento e outro para testes de regressão, garantindo que novas alterações não interrompam a funcionalidade existente.

- **Treinamento e demonstrações**: use um ambiente para treinar novos membros da equipe ou demonstrar novos recursos às partes interessadas. Isso garante que as atividades de treinamento não interrompam os testes.

- **Configuração de Link Privado**: os ambientes Mestre e Integração não oferecem suporte à configuração de Link Privado. Se o fluxo de trabalho de desenvolvimento exigir ambientes adicionais conectados via Link privado para desenvolvimento, teste ou qualquer outra finalidade, considere adicionar um ambiente de preparo extra.

Ter dois ambientes de preparo pode melhorar muito a eficiência do fluxo de trabalho, o gerenciamento de riscos e a qualidade geral do aplicativo. Esses ambientes fornecem segurança e flexibilidade que um ambiente de preparo não seria capaz de oferecer.

>[!NOTE]
>
>Esta configuração é um complemento do. Os clientes que desejam um ambiente secundário devem entrar em contato com o representante de vendas para solicitar um. O representante de vendas pode fornecer informações sobre preços e o processo de provisionamento.

Se seu projeto já tiver um ambiente de preparo adicional ou se você estiver considerando atualizar seu projeto atual, considere as seguintes linhas do tempo e responsabilidades de provisionamento:

- O processo de provisionamento pode levar até duas semanas após você confirmar a solicitação com o representante de vendas da Adobe.

- Os novos ambientes de preparo só estão disponíveis na mesma região dos ambientes de preparo e produção atuais.

- Você deve atualizar o ambiente de Integração ou o ambiente Mestre e especificar em quais ambientes o ambiente de preparo secundário será baseado. O ambiente de preparo secundário não pode ser copiado dos ambientes de preparo ou produção.

- Depois de receber o novo ambiente de preparo, você pode incluí-lo no fluxo de desenvolvimento. Assim como em outros ambientes, as atualizações de código e banco de dados são de sua responsabilidade.

## Solicitar o ambiente

Para facilitar sua solicitação, siga as etapas:

1. Atualize sua ramificação.

   Atualize sua ramificação de Integração ou Mestre com o código e o banco de dados que deseja usar para o novo ambiente.

1. Entre em contato com seu representante de vendas.

   Entre em contato com o representante de vendas da Adobe e forneça a ramificação específica que você deseja usar como base para seu novo ambiente de preparo (Integração ou Principal).

1. Confirme os detalhes.

   Depois de confirmar os detalhes com seu representante de vendas, aguarde a confirmação deles de que a solicitação de provisionamento foi recebida e está sendo processada. Quando o processo de provisionamento estiver concluído, a equipe do Adobe enviará uma confirmação.

1. Implante em seu novo ambiente.

   Siga as [etapas de implantação padrão](../deploy/staging-production.md) para implantar seu código e banco de dados no novo ambiente de preparo.
