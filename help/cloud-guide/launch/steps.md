---
title: Etapas de inicialização
description: Saiba como concluir a inicialização do site.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Etapas de inicialização

Depois de testar e concluir a lista de verificação de inicialização, você pode iniciar as etapas finais do lançamento. Essas etapas incluem a entrada de tíquetes, a transferência de acesso ao site e, finalmente, o teste da loja ao vivo.

A equipe de suporte do Adobe trabalha com você durante o processo, verificando o status e ajudando se ocorrerem dúvidas ou problemas. Todos os problemas devem ser rastreados com tíquetes para melhor capturar o que aconteceu e como foi resolvido. Quando você começa iterações contínuas implantando atualizações no armazenamento iniciado, pode haver problemas semelhantes novamente. Esses tíquetes podem ajudar a identificar os problemas e ajustar as tarefas de implantação.

## Entre em contato com o Adobe para iniciar seu site

Entre em contato com o suporte da Adobe Commerce e atualize todos os tíquetes de lançamento (publicação) do site com a data e a hora planejadas para mudar e iniciar sua loja.

## Alternar o DNS para o novo site

O valor alterado de Tempo de vida útil é importante para verificar o domínio alterado. Ao modificar os registros A e CNAME, a atualização utiliza o tempo configurado TTL para atualizar corretamente. Para obter detalhes sobre as configurações de DNS, consulte [Configurações de DNS](checklist.md#update-dns-configuration-with-production-settings).

### Para alterar para o novo site:

1. Acesse o serviço DNS.

1. Atualize os registros A e CNAME dos domínios e nomes de host.

1. Aguarde o tempo de TTL passar e reinicie o navegador da Web.

1. Acesse sua loja usando o domínio da loja.

## Testar o armazenamento ativo

Conclua alguns testes UAT na loja ao vivo para confirmar se tudo está carregando e as ações estão concluídas corretamente. Para obter uma lista de testes, consulte [Implantação de teste](../test/staging-and-production.md#complete-uat-testing).

## Pós-lançamento

O Adobe verifica e monitora o site para garantir que todos os serviços e acessos estejam em verde. Permanecemos disponíveis conforme necessário para verificar se todos os registros do sistema, serviços, armazenamento em cache e funções estão funcionando conforme você e seus clientes precisam.

Se ocorrer algum problema, crie e rastreie os problemas com o suporte. Inclua o máximo de informações possível, incluindo data/hora, recurso específico com um problema, sintomas e comportamentos estranhos, extensões e muito mais. Investigamos os registros, o problema e trabalhamos com você para resolvê-lo o mais rápido possível.
