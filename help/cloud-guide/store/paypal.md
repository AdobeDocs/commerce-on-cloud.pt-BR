---
title: Configurar métodos de pagamento do PayPal
description: Configurar métodos de pagamento do PayPal para Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Checkout, Payments
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Configurar métodos de pagamento do PayPal

O Adobe Commerce na infraestrutura em nuvem fornece uma ferramenta de integração para configurar contas de finalização do PayPal Express diretamente por meio do administrador. Essa ferramenta está disponível para ECE 2.1.8 e posterior. Para oferecer melhor suporte ao teste de métodos de pagamento do PayPal, você pode habilitar e configurar sua conta de Finalização expressa do PayPal para contas de sandbox ou produção.

Você pode configurar a sandbox ou a conta de produção em cada ambiente:

* Para ambientes de integração e preparo, defina credenciais da sandbox.
* Para seu ambiente de produção, defina credenciais de sandbox para o teste inicial e substitua por credenciais de produção em tempo real para um armazenamento iniciado.

## Conta do PayPal

Embora seja melhor usar uma conta de comerciante do PayPal preparada e configurada, você pode criar uma conta ou atualizar uma conta pessoal por meio do Administrador.

[!DNL PayPal onboarding] dá suporte à conexão com as seguintes contas:

* Conta Comercial do PayPal
* Conta pessoal do PayPal, convertendo em uma conta comercial. Se você tiver uma conta pessoal existente do PayPal, poderá fazer logon com essas credenciais e atualizar esta conta para uma conta comercial conforme conclui a sincronização.

Se você não tiver uma conta existente do PayPal, crie uma. Digite um endereço de email para uma nova conta. Se uma conta do PayPal correspondente não for encontrada, siga as instruções para criar uma conta do PayPal Business. Ou você pode criar uma conta diretamente pelo [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

### Limitações do PayPal

O PayPal oferece suporte à conexão do PayPal Express Checkout para países em todo o mundo, exceto para as seguintes limitações:

* Índia e Japão (futuras atualizações do PayPal podem oferecer suporte a essas contas)
* Israel

Para o Brasil, você deve ter uma conta comercial existente do PayPal para se conectar. Você não pode converter uma conta pessoal do PayPal existente para o Brasil durante esse processo. Se precisar de uma conta, [crie uma conta comercial do PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

## Configurar a finalização do PayPal Express

Para configurar o Check-out do PayPal Express:

1. Acesse o Administrador do ambiente.
1. Na navegação do lado esquerdo, selecione **Lojas** > **Configuração** e **Vendas** > **Métodos de Pagamento**.
1. Para PayPal, selecione **Configurar**. Os campos de configuração são exibidos em seções expansíveis para Check-out expresso, Anunciar crédito do PayPal e Configurações básicas e avançadas.
1. Conecte sua conta do PayPal. Até que a conta seja conectada, as opções para habilitar serão desabilitadas. Para obter detalhes sobre contas disponíveis e com suporte para conexão e limitações, consulte [conta do PayPal](#paypal-account).

   * Para conectar sua conta do PayPal ao vivo, clique em Conectar-se ao PayPal e siga as instruções. Qualquer compra feita por um cliente usando um PayPal ao vivo é completa e cobra ativamente os clientes em uma loja ao vivo.
   * Para conectar sua conta de sandbox para testes, clique em Credenciais da sandbox e siga os prompts. Qualquer compra feita por um cliente usando um PayPal de sandbox concluída sem cobrar ativamente os clientes.

1. Defina as configurações de Check-out expresso para autenticar e usar a API do PayPal:

   * **Email Associado à Conta de Comerciante do PayPal** (Opcional) insira o endereço de email associado à sua conta de comerciante do PayPal. Esse email diferencia maiúsculas e minúsculas.
   * **Métodos de autenticação de API** como Assinatura de API ou Certificado de API.
   * Nome de usuário da API, Senha e Assinatura capturados de sua conta do PayPal.
   * **Modo de sandbox** selecione Sim ou Não para indicar se as credenciais inseridas são para sandbox. Se você tiver informado credenciais de produção, selecione Não.
   * **API Usa Proxy** selecione Sim ou Não para definir se o sistema usa um servidor proxy para estabelecer uma conexão entre o Adobe Commerce e o sistema de pagamento PayPal. Se Sim, insira o host e a porta do proxy.

1. Para obter informações detalhadas e etapas para configurar sua conta, consulte [Finalização expressa do PayPal](https://experienceleague.adobe.com/pt-br/docs/commerce-admin/stores-sales/payments/paypal/paypal-express-checkout), começando com a Etapa 2, Concluir as configurações necessárias.

Com a conta configurada e autenticada, você pode ativar e desativar as opções de pagamento do PayPal em Configurações obrigatórias do PayPal:

* **Habilitar esta Solução** exibe o método de pagamento do PayPal para clientes através do site.
* **Habilitar Experiência de Check-out em Contexto**
* **Habilitar Crédito do PayPal** permite que os clientes obtenham financiamento de crédito do PayPal sem custos adicionais. O PayPal paga o pedido antecipadamente, processando todos os reembolsos do crédito diretamente com o cliente.

## Variáveis do PayPal

Ao usar a ferramenta de integração do PayPal com Adobe Commerce na infraestrutura em nuvem, adicione a seguinte variável à seção `variables:env` do arquivo `magento.app.yaml`.

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

Se você estiver atualizando para a versão 2.2, da 2.1.8 ou posterior, ainda precisará adicionar essa variável.
