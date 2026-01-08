---
source-git-commit: 8cbda8ca194c5e5865073c9eb08e061cfecb5ace
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 1%

---
# Adobe Commerce na infraestrutura em nuvem

Este site contém a documentação mais recente para o desenvolvedor do Commerce na Infraestrutura em nuvem.

- [Guia da Infraestrutura do Commerce na Nuvem](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/overview)
- [Introdução ao Commerce](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/overview) na infraestrutura em nuvem

## Código de conduta do Adobe Open Source

Este projeto adotou o [Código de conduta do Adobe Open Source](code-of-conduct.md). Para obter mais informações, consulte o artigo [Contribuição](contributing.md).

## Sobre suas contribuições para o conteúdo do Adobe

Consulte o [Guia do colaborador do Adobe Docs](https://experienceleague.adobe.com/en/docs/contributor/contributor-guide/introduction).

A forma como você contribui depende de quem você é e do tipo de alterações com as quais deseja contribuir:

### Pequenas alterações

Se você estiver contribuindo com pequenas atualizações, visite o artigo e clique na área de feedback que aparece na parte inferior do artigo, clique em **Opções de feedback detalhadas** e em **Sugerir uma edição** para ir para o arquivo de origem do Markdown no GitHub. Use a interface do GitHub para fazer suas atualizações.

Pequenas correções ou esclarecimentos que você envia para documentação e exemplos de código neste repositório são cobertos pelos termos de uso da Adobe.

### Grandes alterações ou novos artigos de membros da comunidade

Se você fizer parte da comunidade da Adobe e quiser criar um novo artigo ou enviar grandes alterações, use a guia Problemas no repositório Git para enviar um problema e iniciar uma conversa com a equipe de documentação. Depois de concordar com um plano, você precisará trabalhar com um funcionário para ajudá-lo a inserir o novo conteúdo por meio de uma combinação de trabalho nos repositórios públicos e privados.

### Grandes mudanças de funcionários da Adobe

Se você for um autor técnico, gerente de programa ou desenvolvedor da equipe de produtos de uma solução da Adobe Experience Cloud e seu trabalho for contribuir com ou criar artigos técnicos, deverá usar o repositório privado em `https://git.corp.adobe.com/AdobeDocs`.

## Ferramentas e configuração

Os colaboradores da comunidade podem usar a interface do usuário do GitHub para a edição básica ou bifurcar o repositório para fazer grandes contribuições.

Consulte o [Guia do colaborador do Adobe Docs](https://experienceleague.adobe.com/en/docs/contributor/contributor-guide/introduction) para obter mais detalhes.

## Como usar marcação para formatar seu tópico

Todos os artigos neste repositório usam GitHub flavored markdown. Se não estiver familiarizado com a marcação, consulte:

- [Noções básicas do Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [Cheatsheet de markdown para impressão](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## Modelos

Para alguns tópicos, usamos arquivos de dados e modelos para gerar conteúdo publicado. Os casos de uso para essa abordagem incluem:

- Publicação de grandes conjuntos de conteúdo gerado de forma programática
- Fornecer uma única fonte da verdade para clientes em vários sistemas que exigem formatos de arquivo legíveis por máquina, como YAML, para integração (por exemplo, ferramentas de CLI da nuvem, configurações de serviço)

Os exemplos de conteúdo de modelo incluem, entre outros, os seguintes:

- [Referência da CLI da nuvem](help/templated/cloud-cli-ref.md)
- [Pacotes na nuvem](help/templated/cloud-packages.md)
- [Referência de ferramentas ECE](help/templated/ece-tools.md)
- [Extensões PHP para nuvem](help/templated/php-extensions-cloud.md)

### Gerar conteúdo em modelo

Em geral, a maioria dos autores só precisa adicionar uma versão de lançamento às tabelas de disponibilidade de produtos e requisitos do sistema. A manutenção de todos os outros conteúdos em modelos é automatizada ou gerenciada por um membro dedicado da equipe. Estas instruções são destinadas para a maioria dos escritores.

>**OBSERVAÇÃO:**
>
>- A geração de conteúdo de modelo requer o trabalho na linha de comando em um terminal.
>- É necessário ter o Ruby instalado para executar o script de renderização. Consulte [_jekyll/.ruby-version](_jekyll/.ruby-version) para obter a versão necessária.

Consulte o seguinte para obter uma descrição da estrutura do arquivo para conteúdo de modelos:

- `_jekyll` — Contém tópicos de modelo e ativos necessários
- `_jekyll/_data` — Contém os formatos de arquivo legíveis por máquina usados para renderizar modelos
- `_jekyll/templated` — Contém arquivos de modelo baseados em HTML que usam a linguagem de modelo Liquid
- `help/_includes/templated` — Contém a saída gerada para conteúdo de modelo no formato de arquivo `.md` para que possa ser publicada nos tópicos do Experience League; o script de renderização grava automaticamente a saída gerada nesse diretório para você

Para atualizar o conteúdo do modelo:

1. No editor de texto, abra um arquivo de dados no diretório `_jekyll/_data`. Por exemplo:

   - [Referência da CLI da nuvem](help/templated/cloud-cli-ref.md): `_jekyll/_data/cloud-cli-ref.yaml`
   - [Pacotes na nuvem](help/templated/cloud-packages.md): `_jekyll/_data/cloud-packages.yaml`
   - [Referência de ferramentas ECE](help/templated/ece-tools.md): `_jekyll/_data/ece-tools.yaml`

2. Use a estrutura YAML existente para criar entradas.

3. Navegue até o diretório `_jekyll`.

   ```bash
   cd _jekyll
   ```

4. Gere conteúdo de modelo e grave a saída no diretório `help/_includes/templated`.

   ```bash
   bundle exec rake render
   ```

   >**OBSERVAÇÃO:** você deve executar o script a partir do diretório `_jekyll`. Se esta for a primeira vez que você executa o script, instale primeiro as dependências de Ruby com o comando `bundle install`. As tarefas e dependências principais do rake (Jekyll, Rake, otimização de imagem) são fornecidas pela gem `adobe-comdox-exl-rake-tasks` para melhorar a manutenção em repositórios de documentação do Adobe Commerce. Tarefas personalizadas específicas para este repositório são implementadas no `Rakefile`.

5. Volte para o diretório `root`.

   ```bash
   cd ..
   ```

6. Verifique se os `help/_includes/templated` arquivos esperados foram modificados.

   ```bash
   git status
   ```

   Você deve ver uma saída semelhante à seguinte:

   ```bash
   modified:   _data/cloud-cli-ref.yaml
   modified:   help/_includes/templated/cloud-cli-ref.md
   ```

7. Envie suas alterações.

   ```bash
   git add .
   git commit -m "descriptive message of the intended commit"
   git push
   ```

Consulte a documentação do Jekyll para obter mais detalhes sobre [Arquivos de dados](https://jekyllrb.com/docs/datafiles), [Filtros líquidos](https://jekyllrb.com/docs/liquid/filters/) e outros recursos.

## Tarefas do rake disponíveis

Este repositório usa tarefas do rake fornecidas pela gem `adobe-comdox-exl-rake-tasks`. Para ver todas as tarefas disponíveis, execute:

```bash
cd _jekyll
bundle exec rake --tasks
```

## Ganchos de pré-confirmação para otimização de imagem

Esse repositório inclui ganchos de pré-confirmação automatizados que otimizam imagens antes da confirmação. **Todos os colaboradores devem habilitar esses ganchos** para garantir uma otimização de imagem consistente e um tamanho de repositório reduzido.

### Configuração rápida

Após clonar o repositório, execute:

```bash
.githooks/setup-hooks.sh
```

### O que os ganchos fazem

- Detectar automaticamente arquivos de imagem preparados (PNG, JPG, JPEG, GIF, SVG)
- Executar `image_optim` para compactar e otimizar imagens
- Transferir imagens otimizadas automaticamente
- Garantir que todas as imagens confirmadas estejam corretamente otimizadas

### Benefícios

- Tamanho reduzido do repositório
- Carregamentos de página mais rápidos para a documentação
- Qualidade de imagem consistente em todos os colaboradores
- Não é necessária otimização manual

Para obter instruções detalhadas de instalação, solução de problemas e configuração, consulte [`.githooks/README.md`](.githooks/README.md).
