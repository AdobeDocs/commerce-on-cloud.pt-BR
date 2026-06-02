---
title: Bloqueio protetor
description: Saiba mais sobre o recurso de bloqueio de proteção do Adobe Commerce na infraestrutura em nuvem e como ele funciona para proteger seu site contra vulnerabilidades de segurança conhecidas.
feature: Cloud, Configuration, Security
topic: Security
exl-id: 4a470e75-0b42-4ab7-b3dc-9f50b63bea14
TQID: https://experienceleague.adobe.com/E0lyCu6cFEaHR0KoauRFmQi9QThzoGDmNetnzYv3hVg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 0%

---

# Bloqueio protetor

O Adobe Commerce na infraestrutura em nuvem tem um recurso de bloqueio de proteção que, em determinadas circunstâncias, restringe o acesso a sites com vulnerabilidades de segurança. Esse método de bloqueio parcial impede a exploração de vulnerabilidades de segurança conhecidas. Softwares desatualizados geralmente contêm explorações, portanto, é importante proteger-se contra essas explorações bloqueando parcialmente o acesso a esses sites.

## Como funciona o bloco protetor

A Adobe Commerce mantém um banco de dados de assinaturas de vulnerabilidades de segurança conhecidas em software de código aberto que são normalmente implantadas em infraestruturas em nuvem. A verificação de segurança analisa somente vulnerabilidades conhecidas em projetos de código aberto; ela não pode examinar as personalizações que você escrever.

A verificação de segurança é executada:

- Quando você envia o novo código para o Git
- Quando novas vulnerabilidades são adicionadas ao banco de dados

Se uma vulnerabilidade crítica for detectada em seu aplicativo, ele rejeitará o push do Git.

Há dois tipos de blocos que são executados:

1. **Concluir bloco**—para sites de desenvolvimento. A mensagem de erro que acompanha o `git push` fornece informações detalhadas sobre a vulnerabilidade.

1. **Bloqueio parcial**—para sites de produção, o que permite que o site permaneça online. Dependendo da natureza da vulnerabilidade, partes de uma solicitação, como uma sequência de consulta, cookies ou qualquer cabeçalho adicional, podem ser removidas das solicitações GET. Todas as outras solicitações podem ser totalmente bloqueadas, como logon, envio de formulário ou check-out do produto.

O desbloqueio é automatizado após a resolução do risco de segurança. O bloco é removido logo após a aplicação de uma atualização de segurança que remove a vulnerabilidade.

## Recusar o bloco protetor

O bloco de proteção existe para protegê-lo contra vulnerabilidades conhecidas no software implantado pela Adobe Commerce na infraestrutura em nuvem.

No entanto, você pode recusar o bloco protetor adicionando o seguinte a [`.magento.app.yaml`](../application/configure-app-yaml.md):

```yaml
   preflight:
      enabled: false
```

Você pode recusar explicitamente uma verificação específica, por exemplo:

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
