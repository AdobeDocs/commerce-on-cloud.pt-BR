---
title: Dados de exemplo
description: Saiba como instalar dados de amostra com o Adobe Commerce na infraestrutura em nuvem.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Dados de exemplo

Se você precisar de alguns dados de exemplo ao desenvolver sua loja, poderá instalar dados de amostra. Esses dados simulam uma loja ativa do Adobe Commerce com clientes, produtos e outros dados. Esses dados de amostra funcionam melhor com uma nova instalação do Adobe Commerce no modelo de infraestrutura em nuvem.

Como prática recomendada, instale dados de amostra em ambientes de desenvolvimento e integração. Se você usar dados de amostra no Armazenamento temporário ou na Produção, será necessário [remover](#reset-or-uninstall-sample-data) as informações e os produtos antes de entrar em produção.

## Instalar dados de amostra

Para instalar dados de amostra:

1. Na estação de trabalho local, altere para o diretório do projeto.

1. Na raiz, insira o seguinte comando para adicionar dados de amostra:

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. Aguarde até que os componentes sejam atualizados.

1. Confirme e envie as alterações:

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Aguarde a implantação do projeto.

1. Verifique se a instalação foi bem-sucedida acessando a página inicial da loja no ambiente de integração. Você pode localizar o link da URL para a loja através de [!DNL Cloud Console].

1. Tirar um instantâneo do seu ambiente:

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

Você pode começar a testar seu desenvolvimento com dados em tempo real!

## Redefinir ou desinstalar dados de exemplo

Você pode redefinir ou remover dados de amostra seguindo o mesmo procedimento usado para instalar os dados de amostra:

- Excluir dados de exemplo: `./bin/magento sampledata:remove`
- Redefinir dados de exemplo: `./bin/magento sampledata:reset`
