---
source-git-commit: 3d3df68ab5e1781974348190b30372c0b8169d1d
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---
# Ganchos de pré-confirmação para otimização de imagem

Esse diretório contém ganchos de pré-confirmação que otimizam automaticamente as imagens antes de serem confirmadas no repositório.

## O que os ganchos fazem

- **Detectar automaticamente** arquivos de imagem preparados (`.png`, `.jpeg`, `.jpg`, `.gif`, `.svg`)
- **Execute`image_optim`** para compactar e otimizar imagens rasterizadas (`.png`, `.jpeg`, `.jpg`, `.gif`)
- **Repreparar imagens otimizadas** automaticamente
- **Verifique se todas as imagens rasterizadas confirmadas** estão corretamente otimizadas
- **Verificar SVGs** preparados em relação a um limite de tamanho e anular a confirmação se um SVG muito grande for referenciado a partir de qualquer arquivo em `help/` (caso contrário, apenas avisar)

## Benefícios

- Tamanho reduzido do repositório
- Carregamentos de página mais rápidos para a documentação
- Qualidade de imagem consistente em todos os colaboradores
- Não é necessária otimização manual

## Pré-requisitos

- Ruby 3.0 ou superior
- Pacote
- Git

## Configuração

### Configuração automática (recomendada)

```bash
.githooks/setup-hooks.sh
```

### Configuração manual

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### Concluir configuração do projeto

1. Clonar o repositório:

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. Ativar ganchos de pré-confirmação:

   ```bash
   .githooks/setup-hooks.sh
   ```

3. Instalar dependências do Jekyll:

   ```bash
   cd _jekyll
   bundle install
   ```

## Teste dos ganchos

1. Adicionar um arquivo de imagem ao repositório
2. Preparo: `git add <image-file>`
3. Tentar confirmar: `git commit -m 'test'`
4. O gancho deve otimizar automaticamente a imagem

### Saída esperada

```bash
Found 1 staged image(s). Running optimization...

Checking images ...
path/to/your/image.png    100.00%
Pre-commit image checks complete!
```

### Testes de unidade

A lógica de detecção de link SVG do gancho (que decide se um SVG superdimensionado é referenciado de `help/`) é coberta por testes de unidade que precisam apenas do pacote `minitest` do Ruby — sem gems ou configuração `_jekyll`:

```bash
ruby .githooks/test/svg_link_checker_test.rb
```

## Diretrizes de imagem

- **PNG**: usar para capturas de tela e elementos da interface do usuário (será otimizado automaticamente)
- **JPEG**: usar para fotografias (será otimizado automaticamente)
- **GIF**: usar para animações (será otimizado automaticamente)
- **SVG**: usar para ícones e elementos gráficos simples (não otimizado, mas verificado em relação a um limite de tamanho; a confirmação falhará somente se o SVG de tamanho maior for vinculado de `help/`)

Os ganchos de pré-confirmação otimizarão automaticamente as imagens `.png`, `.jpeg`/`.jpg` e `.gif` na confirmação e verificarão SVGs preparados em relação a um limite de tamanho (140 KB).

Se uma SVG em etapas exceder o limite e for referenciada a partir de um arquivo em `help/`, a confirmação será anulada. Se o SVG superdimensionado não for mencionado em nenhum lugar do `help/`, o gancho só imprimirá um aviso e a confirmação continuará. Converter SVGs grandes demais em PNG:

```bash
cd _jekyll
bundle exec rake images:svg_to_png path=../help/assets/image.svg
```

O caminho é relativo a `_jekyll`, portanto, as imagens em `help/` são referenciadas como `../help/...`.

## Otimização manual

Para otimização manual de imagens:

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## Configuração

Os ganchos usam o arquivo de configuração `_jekyll/.image_optim.yml` para personalizar as configurações de otimização:

- **PNG**: usa `advpng`, `optipng` e `pngquant`
- **JPEG**: Usa `jhead`, `jpegoptim` e `jpegtran`
- **GIF**: Usa `gifsicle`
- **SVG**: não otimizado (excluído de `image_optim` para preservar animações e gráficos vetoriais), mas verificado em relação a um limite de tamanho de 140 KB

## Solução de problemas

### Gancho não está em execução

- Verificar configuração de gancho: `git config core.hooksPath`
- Verifique se o arquivo de gancho é executável: `chmod +x .githooks/pre-commit`
- Verifique se você está no repositório correto com o diretório `_jekyll`

### Falhas de otimização

- Verificar se `bundle install` foi executado no diretório `_jekyll`
- Verifique se a gem `adobe-comdox-exl-rake-tasks` está instalada (fornece as tarefas do rake `images:optimize`, `images:check_size` e `images:svg_to_png` que o gancho executa)
- Revise o arquivo de configuração `.image_optim.yml`

### O SVG excede o limite de tamanho

- A confirmação será anulada se um SVG preparado exceder 140 KB e for referenciado de um arquivo em `help/` (caso contrário, o gancho só avisa e a confirmação continua)
- Converter o SVG em PNG: `cd _jekyll && bundle exec rake images:svg_to_png path=../help/assets/image.svg` (o caminho é relativo a `_jekyll`, portanto, as imagens em `help/` são referenciadas como `../help/...`)
- Em seguida, prepare o PNG no lugar do SVG e confirme novamente

### Problemas de desempenho

- Ajustar contagem de threads em `_jekyll/.image_optim.yml`
- Defina a variável de ambiente `DEBUG=1` para obter informações detalhadas sobre o erro

## Como funciona

1. **Acionador de pré-confirmação**: quando você executa o `git commit`, o gancho é executado automaticamente
2. **Detecção de imagem**: verifica arquivos preparados em busca de extensões de imagem
3. **Otimização**: executa `image_optim` em cada PNG, JPEG ou GIF preparado
4. **Repreparo**: adiciona automaticamente imagens otimizadas de volta à área de preparo
5. **Verificação de tamanho do SVG**: verifica cada SVG preparado em relação ao limite de tamanho de 140 KB
6. **Continuação da confirmação**: se a otimização for bem-sucedida e nenhum SVG superdimensionado for referenciado de `help/`, a confirmação continuará normalmente; caso contrário, a confirmação será anulada (um SVG superdimensionado não referenciado de `help/` aciona apenas um aviso)

## Formatos de imagem compatíveis

- **PNG** (`.png`) - Compactação sem perdas e com perdas
- **JPEG** (`.jpg`, `.jpeg`) - Compactação com perda com limpeza de metadados
- **GIF** (`.gif`) - Otimização de animação e estática
- **SVG** (`.svg`) - Não otimizado (confirme como está para preservar a qualidade), mas verificado em relação a um limite de tamanho de 140 KB; a confirmação será anulada se o limite for excedido e a SVG for referenciada a partir de `help/` (caso contrário, o gancho só avisa)

## Práticas recomendadas

1. **Testar o gancho**: tente confirmar uma imagem pequena primeiro para garantir que ela funcione
2. **Revisar alterações**: verifique a diferença do Git para ver os resultados da otimização
3. **Monitorar desempenho**: imagens grandes podem demorar para serem otimizadas
4. **Controle de versão**: ganchos são armazenados neste diretório `.githooks/`

## Suporte

Para problemas com os ganchos de pré-confirmação:

1. Verifique se há mensagens de erro na saída do gancho
2. Verifique se a configuração do `image_optim` está funcionando
3. Testar primeiro com as tarefas manuais do rake
4. Revise os registros e a configuração do gancho
5. Verifique a configuração do gancho: `git config core.hooksPath`
