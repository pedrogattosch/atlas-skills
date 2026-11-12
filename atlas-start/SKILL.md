---
name: atlas-start
description: Começa uma tarefa nova em um projeto, escrevendo o plano, criando a branch e uma worktree separada. Use com /atlas-start [#<número da issue>] ["descrição da tarefa"].
argument-hint: "[#<número da issue>] [\"descrição da tarefa\"]"
disable-model-invocation: true
---

# Atlas Start

Preparar uma tarefa nova: transformar a necessidade em um plano curto e verificável, criar a branch e uma worktree separada com o plano salvo em `PLAN.md`. Não implementar código.

Tarefa descrita pelo usuário: $ARGUMENTS

## Contexto do projeto

Consultar as fontes necessárias nesta ordem:

1. a issue, quando informada;
2. `README.md`;
3. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
4. código relacionado;
5. testes relacionados.

## Regras obrigatórias

- Escrever em português, sem texto promocional, genérico ou emojis.
- Executar o fluxo a partir do repositório principal, que é a primeira entrada de `git worktree list`.
- Considerar que a tarefa tem issue quando os argumentos informarem o número dela, como `#123`. Usar apenas o número informado; não deduzir nem inventar um.
- Quando houver issue, usar título, descrição e comentários dela como necessidade da tarefa; a descrição passada no comando complementa a issue e prevalece em caso de divergência. Não alterar, comentar ou fechar a issue.
- Nomear a branch como `<tipo>/<slug>`, ou `<tipo>/<número da issue>-<slug>` quando houver issue, com tipo `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `build`, `ci`, `chore` ou `style` e slug em kebab-case, sem acentos, com até cerca de 40 caracteres.
- Criar a worktree em uma pasta irmã do repositório: `../<repo>-<slug>`, ou `../<repo>-<número da issue>` quando houver issue, onde `<repo>` é o nome da pasta do repositório principal.
- Não mostrar o plano no chat nem esperar aprovação. O usuário revisa e ajusta o `PLAN.md` diretamente; rodar o `atlas-implement` vale como aprovação.
- Nunca versionar `PLAN.md`.
- Não copiar arquivos locais não versionados, como `.env`, sem o ok do usuário.

## Regras do plano

- Criar título curto, específico e orientado à mudança, preferencialmente iniciado por verbo no infinitivo.
- Em Objetivo, descrever em um parágrafo curto o resultado esperado, o problema, a motivação e o comportamento atual, sem antecipar a implementação: sem lista de arquivos, funções ou números de linha. Citar um detalhe técnico só quando for um risco que a implementação não descobriria sozinha.
- Não inventar funcionalidades, causas, requisitos, regras de negócio ou escopo. Registrar informação essencial ausente em Decisões pendentes.
- Quando houver um spec relacionado, tratá-lo como fonte de verdade para escopo e comportamento; uma instrução explícita mais recente do usuário prevalece. Não alterar o spec. Registrar em Decisões pendentes qualquer divergência entre spec e issue.
- Não incluir etapas de implementação; a implementação é decidida pelo `atlas-implement` a partir do código.
- Em Regras de negócio, registrar as regras que a entrega cria, altera ou remove, uma por linha, escritas como resultado observável que sirva de validação, sem etapas técnicas e sem comandos de teste ou build. Quando houver ambiguidade não bloqueante, registrar a menor suposição segura como regra marcada com "(suposição)".
- Manter o plano curto e focado em uma entrega coesa, sem repetir informação entre seções, ampliar o escopo ou adicionar detalhamento artificial.
- Registrar em Decisões pendentes apenas dúvidas que dependem do usuário, não escolhas já feitas.

## Formato do plano

```markdown
# <título>

Branch: `<branch>`

## Objetivo

<parágrafo curto com o resultado esperado, o problema, a motivação e o comportamento atual>

## Regras de negócio

| ID | Regra |
| --- | --- |
| R001 | <primeira regra verificável> |
| R002 | <segunda regra verificável> |

## Decisões pendentes

Nenhuma decisão pendente.
```

## Passos

1. Se não houver número de issue nem descrição da tarefa, pedir um dos dois e parar.
2. Rodar `git rev-parse --show-toplevel`, `git worktree list` e `git status --short`. Se houver mudanças não commitadas, avisar que elas não vão para a worktree nova e perguntar se deve continuar.
3. Rodar `git fetch origin` e descobrir a branch padrão com `git symbolic-ref --short refs/remotes/origin/HEAD`. Se falhar, usar `origin/main`.
4. Se houver issue, identificar a plataforma por `git remote get-url origin` e ler a issue com `glab issue view <número> --comments` no GitLab ou `gh issue view <número> --comments` no GitHub. Se a issue não existir, ou se o `glab` ou o `gh` não estiver instalado ou falhar por autenticação, informar o erro, orientar a instalação ou `glab auth login`/`gh auth login` e parar.
5. Escolher o nome da branch e da pasta da worktree. Confirmar que a branch não existe localmente (`git branch --list <branch>`) nem no remoto (`git ls-remote --heads origin <branch>`) e que a pasta da worktree ainda não existe.
6. Ler o contexto do projeto e escrever o plano.
7. Criar a worktree com `git worktree add <pasta da worktree> -b <branch> <branch padrão remota>`.
8. Gravar o plano em `PLAN.md` na raiz da worktree e garantir que `PLAN.md` está em `$(git rev-parse --git-common-dir)/info/exclude`, adicionando a linha se faltar.
9. Se o projeto depender de arquivos não versionados para rodar, perguntar se devem ser copiados para a worktree.
10. Encerrar com o formato da saída.

## Formato da saída

```markdown
- Branch: `<branch>`
- Worktree: `<caminho da worktree>`
```
