---
title: Teste de preparo e produção
description: Saiba como testar em ambientes de preparo e produção.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Teste de preparo e produção

Após uma migração bem-sucedida de código, arquivos e dados para armazenamento temporário ou produção, use os URLs de ambiente para testar seus sites e armazenamentos. A seguir estão informações sobre a verificação de logs, o teste de configurações do Fastly, o teste de aceitação do usuário (UAT) e muito mais.

{{second-staging}}

## Arquivos de log

Se você encontrar erros na implantação ou outros problemas ao testar o, verifique os arquivos de log. Arquivos de log localizados no diretório `var/log`.

O log de implantação está em `/var/log/platform/<prodject-ID>/deploy.log`. O valor de `<project-ID>` depende da ID do projeto e se o ambiente é de Preparo ou de Produção. Por exemplo, com a ID de projeto `yw1unoukjcawe`, o usuário de Preparo é `yw1unoukjcawe_stg` e o usuário de Produção é `yw1unoukjcawe`.

Ao acessar logs em ambientes de produção ou de preparo, use o SSH para fazer logon em cada um dos três nós para localizar os logs. Ou você pode usar o [gerenciamento de logs do New Relic](../monitor/log-management.md) para exibir e consultar dados de log agregados de todos os nós. Consulte [Exibir logs](log-locations.md#application-logs).

## Verifique a base de código

Verifique se a base de código foi implantada corretamente nos ambientes de preparo e produção. Os ambientes devem ter bases de código idênticas.

## Verificar definições de configuração

Verifique as definições de configuração por meio do painel Admin, incluindo o URL base, URL de administração base, configurações de vários sites e muito mais. Se você precisar fazer alterações adicionais, conclua as edições na ramificação Git local e envie por push para a ramificação `master` em Integração, Preparo e Produção.

## Verificar armazenamento em cache rápido

[A configuração do Fastly](../cdn/fastly-configuration.md) requer atenção aos detalhes: usar as credenciais corretas de ID do Fastly Service e do token da API do Fastly, carregar o código VCL do Fastly, atualizar a configuração do DNS e aplicar os certificados SSL/TLS aos seus ambientes. Após concluir essas tarefas de configuração, você pode verificar o armazenamento em cache rápido em ambientes de preparo e produção.

**Para verificar a configuração do serviço Fastly**:

1. Faça logon no Administrador para Preparo e Produção usando a URL com `/admin` ou a [URL de Administrador atualizada](../environment/variables-admin.md#admin-url).

1. Navegue até **Lojas** > **Configurações** > **Configuração** > **Avançadas** > **Sistema**. Role e clique em **Cache de Página Inteira**.

1. Verifique se o valor de **Aplicativo de cache** está definido como _Fastly CDN_.

1. Teste as credenciais do Fastly.

   - Clique em **Fastly Configuration**.

   - Verifique os valores das credenciais de token da ID de serviço do Fastly e da API do Fastly. Consulte [Obter credenciais do Fastly](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials).

   - Clique em **Testar credenciais**.

   >[!WARNING]
   >
   >Verifique se você inseriu a ID do serviço Fastly e o token da API nos ambientes de Preparo e Produção. As credenciais do Fastly são criadas e mapeadas por ambiente de serviço. Se você inserir credenciais de armazenamento temporário no ambiente de Produção, não será possível fazer upload dos trechos de VCL, o armazenamento em cache não funcionará corretamente e a configuração de armazenamento em cache apontará para o servidor e as lojas errados.

**Para verificar o comportamento de cache do Fastly**:

1. Verifique os cabeçalhos usando o utilitário de linha de comando `dig` para obter informações sobre a configuração do site.

   Você pode usar qualquer URL com o comando `dig`. Os exemplos a seguir usam Pro URLs:

   - Estágios: `dig https://mcstaging.<your-domain>.com`
   - Produção: `dig https://mcprod.<your-domain>.com`

   Para ver testes `dig` adicionais, consulte [Testes do Fastly antes de alterar o DNS](https://docs.fastly.com/en/guides/working-with-domains).

1. Use `cURL` para verificar as informações do cabeçalho de resposta.

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   Consulte [Verificar cabeçalhos de resposta](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) para obter detalhes sobre como verificar os cabeçalhos.

1. Depois que você estiver online, use o `cURL` para verificar seu site online.

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## Concluir o teste UAT

Concluir o Teste de Aceitação do Usuário (UAT) em Preparo e Produção. Os testes a seguir são uma lista rápida de possíveis tarefas e áreas para testar como Comerciante e Cliente. Sua lista pode ser mais longa e incluir testes adicionais para módulos personalizados, extensões e integrações de terceiros. Ao testar, use desktops, laptops e dispositivos móveis.

Se encontrar problemas, salve as etapas de reprodução, as mensagens de erro, as capturas de tela estranhas e os links. Use essas informações para investigar e corrigir problemas no código e nas configurações do ambiente de integração ou nas configurações do ambiente.

<table>
<tr>
<td style="width:150px">Gerenciamento de usuários</td>
<td>
<ul>
<li>Criar e editar contas de clientes, verificar emails</li>
<li>Criar funções de Administrador para comerciantes</li>
<li>Criar contas de comerciante com funções específicas</li>
<li>Testar acesso à conta do comerciante por função</li>
</ul>
</td>
</tr>
<tr>
<td>Catálogos e produtos</td>
<td>
<ul>
<li>Criar um catálogo com produtos associados</li>
<li>Crie produtos para sua loja, incluindo todos os tipos de produtos: simples, configurável, incluído</li>
<li>Adicionar imagens do produto, amostras, vídeos e outras opções de mídia</li>
<li>Configurar preço, descontos e regras de preços </li>
<li>Configurar recursos avançados, incluindo intervalos de preços, produtos em destaque, datas de disponibilidade</li>
<li>Modificar o estoque e verificar se os valores corretos são exibidos e alterados por aumento e compra concluída</li>
</ul>
</td>
</tr>
<tr>
<td>Carrinhos e check-out</td>
<td>
<ul>
<li>Procure produtos e selecione as opções de filtragem</li>
<li>Adicionar produtos ao carrinho a partir de resultados de pesquisa, páginas de categoria, páginas de produtos</li>
<li>Testar todos os tipos de produtos</li>
<li>Visualizar o carrinho e modificar o conteúdo removendo ou alterando valores </li>
<li>Passar pelo checkout para verificar os valores do pedido em relação ao carrinho e às informações do produto</li>
<li>Verificar se o imposto é calculado corretamente para o carrinho</li>
<li>Conclua uma compra com opções diferentes: adicione um cupom, selecione entrega, especifique informações sobre entrega e faturamento e informações sobre pagamento</li>
<li>Verificar gateways e opções de pagamento durante a finalização da compra</li>
<li>Verificar notificações na tela, pedidos listados na conta do cliente e notificações por email</li>
<li>Teste o check-out do convidado e do cliente</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>Criar um pedido para um cliente</li>
<li>Procurar e exibir pedidos</li>
<li>Modificar um pedido adicionando e removendo produtos, alterando quantias, modificando informações sobre entrega e faturamento</li>
<li>Lidar com um reembolso</li>
<li>Cancelar um pedido</li>
<li>Aplicar códigos de cupom e descontos</li>
</ul>
</td>
</tr>
<tr>
<td>Conteúdo do site</td>
<td>
<ul>
<li>Marque todos os temas e ativos carregados corretamente</li>
<li>Verifique se o CSS é exibido corretamente, incluindo tamanhos de mídia responsivos</li>
<li>Verifique os Termos e condições, a política de reembolso e outras informações sobre a política</li>
<li>Verifique informações de contato, links e muito mais sobre sua empresa</li>
<li>Pesquise produtos e conteúdo, verifique a filtragem de resultados</li>
<li>Verificar o bloco de rodapé e os principais blocos de navegação</li>
<li>Teste as páginas 404 e de manutenção</li>
</ul>
</td>
</tr>
<tr>
<td>Extensões</td>
<td>
<ul>
<li>Verificar todas as configurações de extensão, especialmente para qualquer módulo de tributação, remessa e pagamento (exemplo: ordem enviada para o depósito e sistema de gerenciamento financeiro)</li>
<li>Testar todas as interações de módulo personalizadas e extensão instalada</li>
<li>Verifique dados para quaisquer interações que devem ser concluídas (pagamentos, pedidos, notificações por email)</li>
<li>Verificar as configurações por ambiente para suas extensões</li>
<li>Verificar dependências entre módulos e extensões funciona</li>
<li>Verificar todas as ações como comerciante e cliente</li>
</ul>
</td>
</tr>
<tr>
<td>Integrações de terceiros</td>
<td>
<ul>
<li>Verifique se os dados são salvos corretamente no Adobe Commerce e exportados, enviados ou acessíveis pelo serviço de terceiros (exemplo: os pedidos são exibidos no sistema de gerenciamento de pedidos de terceiros)</li>
<li>Verificar configurações e interações por integração</li>
<li>Realizar testes de ida e volta originados na Adobe Commerce e em seu serviço de terceiros</li>
<li>Verifique se a autenticação foi concluída</li>
<li>Verifique se há problemas registrados para atualizar integrações de código ou mensagens de erro nos painéis de controle</li>
</ul>
</td>
</tr>
<tr>
<td>Teste de back-end</td>
<td>
<ul>
<li>Testar e limpar o cache </li>
<li>Executar reindexações e verificar resultados</li>
<li>Verifique os trabalhos cron, verifique se há erros cron_schedule</li>
<li>Verifique e verifique se há problemas de script de shell</li>
<li>Verifique se há problemas registrados: logs de aplicativo, logs PHP, logs MySQL, logs de email</li>
</ul>
</td>
</tr>
</table>

## Teste de carga e estresse

Antes de iniciar, é melhor executar testes abrangentes de tráfego e desempenho nos ambientes de preparo e produção. Considere testes de desempenho para seus processos de front-end e back-end.

Antes de começar a testar, insira um tíquete com suporte, avisando os ambientes que você está testando, quais ferramentas está usando e o intervalo de tempo. Atualize o ticket com resultados e informações para acompanhar o desempenho. Ao concluir os testes, adicione os resultados atualizados e a observação no tíquete, onde o teste é concluído com data e hora.

Revise as opções do [Kit de Ferramentas de Desempenho](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) como parte de seu processo de preparação de pré-lançamento.

Para obter melhores resultados, use as seguintes ferramentas:

- [Teste de desempenho de aplicativo](../environment/variables-post-deploy.md#ttfb_tested_pages) — teste o desempenho de aplicativo configurando a variável de ambiente `TTFB_TESTED_PAGES` para testar o tempo de resposta do site.
- [Cerco](https://www.joedog.org/siege-home/)—Modelagem de tráfego e software de teste para levar sua loja ao limite. Acesse seu site com um número configurável de clientes simulados. O Cerco é compatível com autenticação básica, cookies, HTTP, HTTPS e protocolos FTP.
- [Jmeter](https://jmeter.apache.org)—Excelente teste de carga para ajudar a medir o desempenho para tráfego pontiagudo, como para vendas de flash. Crie testes personalizados para executar no site.
- [New Relic](../monitor/new-relic-service.md) (fornecido) — Ajuda a localizar processos e áreas do site que causam desempenho lento com o tempo rastreado gasto por ação, como transmissão de dados, consultas, Redis e muito mais.
- [WebPageTest](https://www.webpagetest.org) e [PKingdom](https://www.pingdom.com)—A análise em tempo real do tempo de carregamento das páginas do site com locais de origem diferentes. A PKingdom pode exigir uma taxa. WebPageTest é uma ferramenta gratuita.

## Teste funcional

Você pode usar a Estrutura de teste funcional (MFTF) do Magento para concluir o teste funcional do Adobe Commerce no ambiente do Cloud Docker. Consulte [Teste de aplicativo](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) no _Guia do Cloud Docker for Commerce_.

## Configurar a ferramenta Verificação de segurança

Há uma Ferramenta de Verificação de Segurança gratuita para seus sites. Para adicionar seus sites e executar a ferramenta, consulte [Ferramenta de Verificação de Segurança](../launch/overview.md#set-up-the-security-scan-tool).
