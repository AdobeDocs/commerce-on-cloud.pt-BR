---
title: Recuperação de falha de componente
description: Saiba como fazer a recuperação se um componente não for implantado corretamente na infraestrutura em nuvem do Adobe Commerce.
feature: Cloud, Deploy
exl-id: f5e79366-5548-40dd-ac2a-56dc84c5d4e2
TQID: https://experienceleague.adobe.com/1krotAWilGcnp3OUMk-fl5iHFObGpWFUC8EpYSPjQfU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 0%

---

# Recuperação de falha de componente

Este tópico discute como recuperar se um componente não for implantado corretamente. Exemplos típicos incluem componentes que possuem dependências que não são atendidas pelo seu ambiente remoto, tais como versões incompatíveis do PHP.

Você pode se recuperar de uma implantação com falha de qualquer uma das seguintes maneiras:

- [Restaurar um backup](../storage/snapshots.md#restore-a-snapshot)
- Limpar projeto e código de alterações anteriores e reimplantar

## Limpeza, remoção e reimplantação

Para limpar da implantação anterior, identifique o componente que foi adicionado ou atualizado e remova-o. Primeiro, faça logon no ambiente remoto e limpe manualmente o conteúdo do diretório `var`. Em seguida, remova o componente do arquivo `composer.json` e reimplante o ambiente.

**Para limpar os `var` diretórios**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Use o SSH para fazer logon no ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Limpar os diretórios `var`.

   ```shell
   rm -rf var/*
   ```

1. Faça logout.

**Para remover o componente**:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Limpe o cache.

   ```bash
   composer clear-cache
   ```

1. Remover o componente do arquivo `composer.json`.

   ```bash
   composer remove <component-name>:<version>
   ```

   Se a mensagem a seguir for exibida, você não precisará fazer mais nada:

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Aguarde enquanto as dependências são atualizadas.

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Veja mais sobre como restaurar um ambiente sem um backup em [Restaurar um ambiente](../development/restore-environment.md).

{{stuck-deployment-tip}}
