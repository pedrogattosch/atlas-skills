---
name: atlas-issue
description: Abre uma issue no projeto Board da Equipe IA no GitLab e a adiciona ao board Development, com labels de coluna, área, complexidade e prioridade, após aprovação. Use com /atlas-issue "descrição da issue" [--coluna <nome>].
argument-hint: "\"descrição da issue\" [--coluna <nome>]"
disable-model-invocation: true
---

# Atlas Issue

Abrir uma issue a partir de uma necessidade descrita pelo usuário e adicioná-la ao board da equipe. Não alterar código, criar branches ou commits.

Necessidade e opções do usuário: $ARGUMENTS

## Board da equipe

- Projeto das issues: `gitlab.insidesistemas.com.br/x-23/ia-interna/board`, referenciado nos comandos do `glab` por `-R gitlab.insidesistemas.com.br/x-23/ia-interna/board`. As issues ficam sempre nesse projeto, independentemente do repositório em que a sessão estiver.
- Board: `Development`, em https://gitlab.insidesistemas.com.br/x-23/ia-interna/board/-/boards.
- Colunas, definidas pela label da lista: `📋 A fazer`, `🚧 Em desenvolvimento`, `⏸️ Stand-by` e `🧪 Em validação`. Sem `--coluna`, usar `📋 A fazer`. Aceitar o nome da coluna sem o emoji e sem diferenciar maiúsculas.

## Labels

Toda issue recebe exatamente uma label de cada grupo:

- Coluna: a label da coluna escolhida.
- Área: uma label de produto ou área já existente no projeto, como `Dashboard Inside`, `IA Gestor`, `IA HUB`, `NLU`, `Paperclip`, `Agenda Inside`, `Precifica` ou `n8n`. Escolher pela descrição e pelo repositório atual; se não for possível identificar com segurança, perguntar na aprovação.
- Complexidade: `Complexidade 1 - Baixa`, `Complexidade 2 - Média` ou `Complexidade 3 - Alta`.
- Prioridade: `Prioridade 1 - Baixa`, `Prioridade 2 - Média` ou `Prioridade 3 -  Alta` (com dois espaços antes de `Alta`, como está no GitLab).

Quando o usuário não informar complexidade ou prioridade, sugerir um valor e deixar explícito na aprovação que é uma sugestão. Antes de criar, conferir os nomes com `glab label list -R <projeto> --per-page 100`; nunca criar labels nem usar as labels antigas de coluna (`Em Execução`, `Stand By`, `Pendente de Publicação`).

## Regras obrigatórias

- Escrever título e descrição em português, com ortografia e acentuação corretas, sem texto promocional, genérico ou emojis.
- Criar título curto e específico, iniciado por verbo no infinitivo (`Corrigir`, `Criar`, `Integrar`), sem prefixo do Conventional Commits.
- Preencher a descrição com o [template](template.md) desta skill: seções `Contexto`, `O que fazer?` e `Critérios de pronto`, com itens em `* ` sem linha em branco entre eles e cada parágrafo em uma linha, sem hard wrap.
- Em `O que fazer?`, descrever as entregas esperadas sem escolher arquivos, classes, bibliotecas ou arquitetura. Em `Critérios de pronto`, escrever resultados observáveis e verificáveis.
- Não inventar requisitos, causas, regras de negócio ou passos de reprodução.
- Não registrar dúvidas ou decisões pendentes na issue. Quando faltar informação, listar as perguntas no terminal e só escrever a issue depois que todas forem respondidas.
- Não citar caminhos de arquivos locais ou fora do git que quem lê a issue no GitLab não consegue abrir.
- Atribuir a issue ao usuário autenticado no `glab`, salvo se ele indicar outra pessoa.
- Antes de criar, procurar issues abertas parecidas. Se houver uma provável duplicata, mostrá-la e perguntar se deve continuar.
- Não incluir atribuição ao Claude ou a ferramentas de IA no título ou na descrição, como "Generated with Claude Code".
- Não alterar, comentar ou fechar outras issues.

## Passos

1. Se não houver descrição da necessidade, pedir e parar.
2. Rodar `glab api user --hostname gitlab.insidesistemas.com.br` para obter o usuário autenticado. Se o `glab` não estiver instalado ou falhar por autenticação, orientar a instalação ou `glab auth login --hostname gitlab.insidesistemas.com.br` e parar.
3. Quando a sessão estiver em um repositório, ler `README.md` e documentação relacionada em `docs/` se ajudar a entender a necessidade e a área.
4. Procurar duplicatas com `glab issue list -R <projeto> --search "<termos>"`.
5. Conferir as labels do projeto e escolher coluna, área, complexidade e prioridade.
6. Se houver dúvidas ou decisões pendentes, listá-las no terminal e esperar todas as respostas.
7. Escrever título e descrição e mostrá-los no formato da aprovação. Esperar o ok e aplicar os ajustes pedidos.
8. Gravar a descrição em um arquivo temporário e rodar `glab issue create -R <projeto> --title "<título>" --description-file <arquivo temporário> --label "<coluna>,<área>,<complexidade>,<prioridade>" --assignee <usuário> --yes`.
9. Rodar `glab issue view <número> -R <projeto> --output json` e conferir se as quatro labels e o responsável foram aplicados. Se algo faltar, corrigir com `glab issue update`, sem criar a issue de novo.
10. Encerrar com o formato da saída.

## Formato da aprovação

```markdown
## Issue

- Título: `<título>`
- Coluna: <coluna>
- Área: <label de área>
- Complexidade: <label> <"(sugestão)" quando não informada>
- Prioridade: <label> <"(sugestão)" quando não informada>
- Responsável: <usuário>

<descrição preenchida>

Posso criar a issue no board?
```

## Formato da saída

```markdown
## Issue criada

- Link: <url da issue>
- Número: #<número>
- Board: Development — <coluna>
- Labels: <labels aplicadas>
- Responsável: <usuário>
```
