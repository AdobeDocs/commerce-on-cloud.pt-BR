---
source-git-commit: f75dbbc9726ff21a648b1bb3d8f06cb45a5d6b08
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---
# Arquivos da biblioteca Rake

Esse diretório contém definições de tarefas do rake organizadas por funcionalidade. O Rake carrega automaticamente todos os `.rake` arquivos neste diretório.

## Organização do arquivo

### `adobe-docs-tasks.rake`

Contém requisitos comuns, funcionalidade compartilhada e tarefas sem namespace para tarefas do Adobe Commerce em Experience League documentation repository rake:

- `whatsnew` - Gerar dados para resumo de notícias (padrão: desde a última atualização)
- `render` - Renderizar arquivos de modelo e manter inclusões

### `includes.rake`

Contém tarefas de gerenciamento de inclusão organizadas no namespace `:includes`:

- `includes:maintain_relationships` - Descobrir e manter relações de inclusão nos arquivos de Markdown
- `includes:maintain_timestamps` - Adicionar/atualizar carimbos de data/hora com base nas alterações do arquivo de inclusão
- `includes:maintain_all` - Executar ambas as operações em sequência
- `includes:unused` - Localizar arquivos de inclusão não utilizados

### `images.rake`

Contém tarefas de gerenciamento de imagens organizadas no namespace `:images`:

- `images:optimize` - Otimizar imagens em arquivos não confirmados modificados
- `images:unused` - Localizar imagens não usadas no projeto

## Como funciona

O Rake descobre e carrega automaticamente quaisquer arquivos `.rake` no diretório `rakelib/`. Isso nos permite:

1. **Organizar tarefas por funcionalidade** - Agrupar tarefas relacionadas
2. **Manter o Rakefile principal limpo** - Focar nas tarefas principais do projeto
3. **Adicione facilmente novos grupos de tarefas** - Basta criar novos `.rake` arquivos
4. **Manter separação de interesses** - Cada arquivo lida com um domínio específico

## Adicionando novos grupos de tarefas

Para adicionar um novo grupo de tarefas relacionadas:

1. Criar um novo arquivo `.rake` neste diretório
2. Usar um namespace descritivo (por exemplo, `namespace :images` para tarefas relacionadas à imagem)
3. Definir suas tarefas no namespace
4. O Rake os carregará automaticamente

## Exemplo de estrutura

```ruby
namespace :your_namespace do
  desc 'Description of what this task does'
  task :task_name do
    # Task implementation
  end
end
```

## Uso

As tarefas estão disponíveis imediatamente após a criação:

```bash
rake your_namespace:task_name
```

## Benefícios

- **Modularidade**: cada arquivo se concentra em uma área específica
- **Capacidade de manutenção**: mais fácil encontrar e modificar grupos de tarefas específicos
- **Escalabilidade**: pode adicionar muitos grupos de tarefas sem desorganizar o Rakefile principal
- **Desenvolvimento de Equipe**: Diferentes desenvolvedores podem trabalhar em diferentes grupos de tarefas
