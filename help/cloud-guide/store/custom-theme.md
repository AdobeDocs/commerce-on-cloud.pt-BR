---
title: Tema personalizado
description: Saiba como instalar um tema personalizado com o Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Themes
exl-id: 3ae4b0d5-9179-42c4-bb07-8ec09bd057d0
TQID: https://experienceleague.adobe.com/rk-VP6z1tQSY-HMU-dD9hv6O9Wpesbp1o5KQMYapCJE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 296
ht-degree: 0%

---

# Tema personalizado

Você pode instalar um ou vários temas para usar em uma ou todas as lojas e sites do projeto. Os temas incluem vários arquivos estáticos, incluindo imagens, fontes, CSS, JavaScript, PHP e muito mais para projetar completamente suas lojas. Você pode adicionar o tema extraindo seu código para o sistema de arquivos ou usando o Composer.

## Instalar um tema manualmente

Para instalar um tema manualmente, você deve ter o código do tema em um arquivo compactado ou em uma estrutura de diretório semelhante à seguinte:

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**Para instalar um tema manualmente**:

1. Copie o código do tema em `<Project root dir>/app/design/frontend` para um tema de vitrine ou `<Project root dir>/app/design/adminhtml` para um tema de Administrador. Verifique se o diretório de nível superior é `<VendorName>`; caso contrário, o tema não será instalado corretamente.

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. Confirme o tema copiado no local correto.

   * Tema da loja: `ls <project-root>/app/design/frontend`
   * Tema do administrador: `ls <project-root>/app/design/adminhtml`

   A seguir, há uma amostra:

   Exemplo de Adobe Commerce de tema

1. Adicionar e confirmar arquivos.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. Envie os arquivos para a ramificação.

   ```bash
   git push origin <branch name>
   ```

1. Aguarde a conclusão da implantação.
1. Faça logon no Administrador.
1. Clique em **Conteúdo** > Design > **Temas**.

   O tema é exibido no painel direito.

## Instalar um tema usando o Composer

Instalar um tema usando o Composer é o mesmo que instalar qualquer outra extensão usando o Composer. Consulte [Instalar, gerenciar e atualizar módulos](extensions.md) para obter detalhes.

Para instalar um tema usando o Composer:

1. Compre o tema da Commerce Marketplace.
1. Obtenha o nome do Compositor do tema.
1. Altere para o diretório raiz do Adobe Commerce e digite o comando:

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   Por exemplo,

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. Aguarde até que as dependências sejam atualizadas.
1. Digite os seguintes comandos:

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. Faça logon no Administrador.
1. Clique em **Conteúdo** > Design > **Temas**.

   O tema é exibido no painel direito.

## Vários temas

Ao usar vários temas, como temas diferentes por localidade, revise a variável de ambiente `SCD_MATRIX` para personalizar a implantação do tema. Consulte os estágios [compilação](../environment/variables-build.md#scd_matrix) ou [implantação](../environment/variables-deploy.md#scd_matrix) na [configuração de ambiente](../environment/configure-env-yaml.md).
