---
title: Otimização rápida de imagens
description: Saiba como otimizar a entrega de imagens e simplificar o gerenciamento de imagens para o site do Adobe Commerce, habilitando e configurando a Otimização rápida de imagens.
feature: Cloud, Configuration, Media
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# Otimização rápida de imagens

O Fastly image otimization (Fastly IO) fornece manipulação e otimização de imagem em tempo real para acelerar a entrega de imagens e simplificar a manutenção de conjuntos de fontes de imagem para aplicativos web responsivos. Uma vez configurado, o Fastly IO fornece os seguintes recursos de otimização de imagem:

- Forçar conversão com perdas
- Otimização profunda de imagem
- Taxas de pixels adaptáveis
- Suporte para formatos comuns de imagem: PNG, JPEG, GIF e WebP

Antes de ativar e configurar a opção Fastly IO, você deve configurar o serviço Fastly e configurar a blindagem do Origin.

Com base nas suas configurações, o trecho Fastly Image Otimization (Fastly IO) insere o código VCL para executar a otimização de imagem que acelera a entrega da imagem do produto na loja. Há três etapas para configurar o Fastly IO: Habilitar, Configurar e Verificar.

## Habilitar Fastly IO

Ative a otimização de imagem do Fastly (Fastly IO) no painel Admin fazendo upload do trecho do Fastly IO VCL. O snippet fornece as instruções de configuração do Fastly para processar todas as imagens por meio de otimizadores de imagem, usando as configurações padrão.

**Pré-requisitos:**

- Instalar ou atualizar para o módulo Fastly versão 1.2.62 ou posterior
- [Configuração do escudo Fastly Origin e back-end](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)

**Para habilitar o Fastly IO**:

1. Faça logon como administrador no painel [Administrador](../../get-started/onboarding.md#access-your-admin-panel) local.

1. Selecione **Lojas** > **Configurações** > **Configuração** > **Avançadas** > **Sistema**.

1. No painel direito, expanda **Cache de Página Inteira**.

1. Selecione **Configuração do Fastly** > **Otimização de Imagem** para especificar as definições de configuração.

1. No campo _Fastly IO_, selecione **Enable/Disable**.

1. Carregue o trecho Fastly IO:

   - Selecione **Opções de configuração de E/S padrão** para abrir a página Opções de configuração padrão de otimização de imagem.
   - Selecione **Carregar** para carregar o trecho de VCL para o servidor.

## Configurar o Fastly IO

Revise e atualize as definições de configuração de E/S padrão para otimização de imagem, conforme necessário. Por exemplo, você pode querer alterar os níveis de qualidade WebP e JPEG para formatos com perdas, ou alterar o formato para veicular imagens JPEG para _Progressivo_ ou _Linha de Base_. Além disso, você pode usar o Fastly IO para obter recursos de otimização de imagem mais granulares, como:

- Forçar conversão com perdas
- Otimização profunda de imagem
- Taxas de pixels adaptáveis

**Para atualizar o Fastly IO**:

1. Na página _Configuração do Fastly_ no campo _Opções de configuração de E/S Padrão_, selecione **Configurar**.

   ![Exibir as configurações do Fastly IO](../../assets/cdn/fastly-io-default-config.png)

1. Revise e atualize as definições de configuração do Fastly IO na página _Opções de configuração padrão de otimização de imagem_:

   ![Revisar a configuração do Fastly IO](../../assets/cdn/fastly-io-config-options.png)

   - **WebP Automático?**—deixe a configuração padrão (`Yes`) para converter imagens para o formato WebP nos navegadores que suportam esse formato. Se você alterar a configuração para **Não**, o Fastly usará o tipo de arquivo de imagem, em vez de converter a imagem para o formato WebP.

   - **Qualidade padrão de WebP (com perdas)**—deixe a configuração padrão (`85`) ou digite o nível de compactação para imagens formatadas em arquivo com perdas. Você pode especificar qualquer número inteiro de 1 a 100.

   - **Controles de formato de JPEG padrão** — mantenha a configuração padrão (`Auto`) ou selecione o tipo de JPEG a ser usado ao veicular uma imagem. Se o valor for definido como _Auto_, o Fastly entrega imagens com o tipo de saída correspondente ao tipo de entrada. Selecione _Linha de Base_ para exibir imagens linha por linha, começando pela parte superior esquerda e indo até a parte inferior direita. Selecione _Progressivo_ para exibir uma imagem borrada que ficará clara durante o carregamento.

   - **Qualidade de JPEG padrão**—deixe a configuração padrão (`85`) ou digite o nível de compactação para obter a qualidade dos formatos de arquivo com perdas. Especifique qualquer número inteiro de 1 a 100.

   - **Permitir upscaling?**—deixe a configuração padrão (`No`) ou selecione `Yes` para retornar imagens maiores que o arquivo de origem original para que elas possam caber nas dimensões solicitadas.

   - **Redimensionar filtro**—deixe a configuração padrão (`Lancsoz3`) ou selecione uma alternativa. Essa configuração especifica o filtro usado para fornecer uma imagem redimensionada. Dependendo do filtro selecionado, a imagem redimensionada pode ter um número de pixels maior ou menor.

      - `Lanczos3` (padrão) — Fornece a imagem de melhor qualidade. Ele aumenta a capacidade de detectar bordas e recursos lineares em uma imagem e usa a reamostragem de _[!DNL sinc]_&#x200B;para fornecer a melhor reconstrução possível.
      - `Lanczos2` — Usa o mesmo filtro que `Lancsoz3`, mas com uma aproximação menos precisa da função de reamostragem _[!DNL sinc]_.
      - `Bicubic` — Tem um efeito de nitidez natural ao tornar uma imagem menor.
      - `Bilinear` — Tem um efeito de suavização natural ao tornar uma imagem maior.
      - `Nearest` — Tem um efeito de pixelização natural ao redimensionar a arte de pixels.

1. Depois de especificar as definições de configuração de E/S para o serviço Fastly, selecione **Cancelar** para retornar às definições de configuração do Fastly.

1. No campo Configuração de otimização de imagem _Habilitar otimização de imagem profunda_, selecione **Sim** para ativar a otimização de imagem profunda.

   ![Habilitar a otimização de imagem profunda do Fastly IO](../../assets/cdn/fastly-io-deep-image-config.png)

   A otimização de imagem profunda está desativada por padrão. Quando esse recurso está ativado, o recurso de redimensionamento integrado no Adobe Commerce é desativado e o trabalho de redimensionamento é descarregado para o serviço Fastly IO. A otimização de imagem se aplica somente a imagens de produtos. As imagens do CMS não são redimensionadas. Consulte a [documentação do Fastly](#deep-image-optimization).

1. Depois de habilitar a otimização de imagens profundas, habilite o recurso [proporções de pixel adaptáveis](#adaptive-pixel-ratios) para gerar imagens otimizadas para uso em sites responsivos.

   ![Habilitar proporções de pixel adaptáveis do Fastly IO](../../assets/cdn/fastly-io-config-adaptive-pixel.png)

   - No campo _Habilitar proporções de pixel do dispositivo adaptável_, selecione **Sim**.
   - No campo _Taxas de pixels do dispositivo_, aceite a configuração padrão ou marque a caixa de seleção **Entrada do sistema** para remover a configuração. Em seguida, selecione a proporção desejada. Uma configuração de proporção de pixels de dispositivo mais alta oferece imagens maiores.

1. Selecione **Salvar Configuração**.

### Forçar conversão com perdas

Por padrão, o serviço Fastly IO força a conversão de formatos sem perdas, como PNG, BMP ou WEBP, para o formato JPEG/WEBP.

A vantagem de forçar conversões com perdas é que imagens menores são servidas.
Por exemplo, usando o formato JPEG ou WEBp em vez de PNG, o tamanho pode ser reduzido em 60 a 70%, dependendo do nível de qualidade especificado na configuração do Fastly IO.

Dependendo do nível de qualidade selecionado para otimização de imagem, você pode perceber diferenças visuais nas imagens. Por exemplo, o canal/transparências de Alpha são removidos e substituídos por um plano de fundo branco, a menos que você use a Otimização de imagem profunda, que usa a cor do plano de fundo do seu tema.

Se você desativar a conversão com perdas (`WebP Auto? = No`), o Fastly IO alterará apenas imagens JPEG para o formato WEBP para navegadores compatíveis. Nenhum outro tipo de imagem foi alterado. Por exemplo, se a imagem original for PNG, a saída do serviço Fastly IO será PNG.

### Otimização profunda de imagem

A otimização de imagem profunda está desativada por padrão. Habilitar essa opção desativa o redimensionamento interno do Adobe Commerce e o descarrega completamente no serviço Fastly IO.
Este recurso redimensiona apenas imagens do _produto_. As imagens do CMS não são redimensionadas.

A ativação da otimização de imagens profundas adiciona uma definição de cor de plano de fundo a cada imagem, conforme definido no seu tema. Como resultado, as imagens WebP são trocadas de WebP sem perdas para WebP com perdas. Uma das principais diferenças entre sem perda e com perda é que as perdas descartam o canal alfa de imagens PNG, que fornece imagens muito menores. No entanto, imagens com transparências podem parecer estranhas em páginas de produtos e campanhas que usam um plano de fundo diferente.

Por exemplo, o código a seguir representa a fonte original de uma imagem do tema Luma:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/cache/f073062f50e48eb0f0998593e568d857/m/b/mb02-gray-0.jpg"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Quando o recurso de otimização de imagem Fastly IO Deep está habilitado, o código fonte original da imagem é regravado conforme mostrado no exemplo a seguir:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

### Taxas de pixels adaptáveis

O recurso proporções de pixel adaptáveis é útil para otimizar imagens para aplicativos web progressivos. Ele permite que você forneça vários tamanhos e resoluções de imagem de um arquivo de origem de imagem, adicionando um `srcset` para cada imagem de produto.

Quando o recurso proporções de pixel adaptáveis está habilitado, o serviço Fastly IO fornece uma imagem de largura fixa que pode se adaptar a `device-pixel-ratios` variados.
Por exemplo, o serviço modifica a definição da imagem do produto conforme mostrado no exemplo a seguir:

```html
<img class="product-image-photo"
     srcset="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=2 2x,
  https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=3 3x"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Consulte `srcset` [suporte a navegador](https://caniuse.com/#feat=srcset) e [especificação](https://html.spec.whatwg.org/multipage/embedded-content.html#attr-img-srcset).

## Validar o Fastly IO

Depois de habilitar e configurar o Fastly IO, valide sua configuração executando testes de velocidade de página da Web com e sem o Fastly IO habilitado. Além disso, revise as imagens no armazenamento para verificar o tamanho e a aparência da imagem quanto a problemas.
