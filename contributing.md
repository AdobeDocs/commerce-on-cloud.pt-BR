---
source-git-commit: 305df8acb8c41369b34e2c20d9c7df6824222931
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---
# Contribuição

Obrigado por colaborar!

A seguir, há um conjunto de orientações que devem ser seguidas ao contribuir para esse projeto.

## Código de conduta

Este projeto adere ao [código de conduta](code-of-conduct.md) da Adobe. Ao participar,
espera-se que você mantenha esse código. Denuncie comportamento inaceitável para
[Grp-opensourceoffice@adobe.com](mailto:Grp-opensourceoffice@adobe.com).

## Documentação do guia do colaborador

Consulte o [Guia do Colaborador](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=pt-BR).

## Você tem uma pergunta?

Comece registrando um problema. Os executores existentes neste projeto trabalham para alcançar
consenso sobre a direção do projeto e soluções de problemas nos threads de problemas
(quando apropriado).

## Contrato de licença de colaborador

Todas as contribuições de terceiros a este projeto devem ser acompanhadas por um contribuidor assinado
contrato de licença. Dessa forma, a Adobe tem permissão para redistribuir suas contribuições
como parte do projeto. [Assine nosso CLA](https://opensource.adobe.com/cla.html). Você
é necessário enviar apenas um Adobe CLA por vez. Se já tiver enviado anteriormente,
você está pronto para ir!

## Revisões de código

Todos os envios devem vir na forma de solicitações de pull e precisam ser revisados
pelos responsáveis pelo projeto. Leia a [documentação de solicitação de pull do GitHub](https://docs.github.com/articles/about-pull-requests/)
para obter mais informações sobre envio de pull requests.

Por último, siga o [modelo de solicitação de pull](PULL_REQUEST_TEMPLATE.md) quando
enviando um pull request!

## Do colaborador para o confirmador

Adoramos contribuições vindas de nossa comunidade! Se você quiser ir além como colaborador
e se tornar um executor com acesso total à gravação e voz ativa no projeto, você deve
para o projeto. Os responsáveis existentes empregam uma indicação interna
processo que deve alcançar consenso lento (silêncio é aprovação) antes dos convites
são emitidos. Se você se sentir qualificado e quiser se envolver mais,
fique à vontade para entrar em contato com os responsáveis existentes e conversar sobre isso.

## Problemas de segurança

Problemas de segurança não devem ser relatados neste rastreador de problemas. Em vez disso, [registre um problema junto aos nossos especialistas em segurança](https://helpx.adobe.com/br/security/alertus.html)

## Destaques das novidades

Se as alterações introduzirem novos tópicos, atualizações significativas ou correções que precisam ser destacadas, você poderá adicionar uma breve descrição à [seção Novidades](https://experienceleague.adobe.com/pt-br/docs/commerce-cloud-service/user-guide/overview#whats-new) diretamente do corpo da solicitação de pull.

Para adicionar um destaque de Novidades:

1. Inclua a tag `whatsnew` com a descrição apropriada no corpo da solicitação de pull no final. A descrição deve fornecer contexto sobre a alteração e um link para o(s) tópico(s) de destino. Use o seguinte formato (as aspas de bloco de código são somente para representação e não as inclua no corpo da solicitação de pull):

   ```text
   whatsnew
   Short description of the change in the [target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html).
   ```

   ou, se houver vários tópicos:

   ```text
   whatsnew
   Short description of the changes in the [first target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html), [second target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-target-topic.html), and [third target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/third-target-topic.html).
   ```

   você também pode usar listas para vários destaques:

   ```text
   whatsnew
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

   ```text
   whatsnew
   The following changes were made to the documentation:
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

1. Adicione rótulos compatíveis que indiquem o tipo de alteração. Os rótulos compatíveis incluem rótulos para cada tipo de alteração, como:

   - `new-topic` - para novos tópicos
   - `major-update` - para atualizações importantes que podem incluir alterações significativas no conteúdo, estrutura ou funcionalidade
   - `technical` - para alterações técnicas que não são consideradas atualizações importantes, mas ainda exigem atenção

**Importante:**

1. A parte `whatsnew` deve começar na tag `whatsnew` e estar no final do corpo da solicitação de pull.
1. As descrições das alterações devem incluir os links de trabalho. Certifique-se de que os links estejam corretos e levem aos tópicos pretendidos. Se o tópico for novo, verifique se os links estão funcionando após mesclar a solicitação de pull e publicar o novo tópico. Não há problema em corrigir os links após a mesclagem da solicitação de pull.

Por exemplo, pesquise nas solicitações de pull fechadas no repositório para ver como os realces existentes estão formatados e compare-os com a [seção Novidades](https://experienceleague.adobe.com/pt-br/docs/commerce-cloud-service/user-guide/overview#whats-new) para ver como eles aparecem na documentação.
