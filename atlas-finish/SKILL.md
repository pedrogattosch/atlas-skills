---
name: atlas-finish
description: Limpa uma ou mais tarefas já mergeadas no GitHub ou no GitLab, removendo a worktree, apagando a branch local e atualizando a branch padrão. Use com /atlas-finish [branch, worktree ou issue ...].
argument-hint: "[branch, worktree ou número da issue ...]"
disable-model-invocation: true
---

# Atlas Finish

Encerrar uma tarefa cujo MR (GitLab) ou PR (GitHub) já foi mergeado. Não alterar código.

Alvos informados (opcional): $ARGUMENTS

Cada alvo pode ser o nome da branch, a pasta ou o caminho da worktree, ou o número da issue (`332`, `#332`). Vários alvos são tratados um a um.

## Regras obrigatórias

- Não remover a worktree onde a sessão está rodando.
- Não descartar uma branch sem MR ou PR mergeado, salvo pedido explícito do usuário.
- Não remover nada com mudanças não commitadas ou commits não enviados.
- Usar `--force` em `git worktree remove` ou `git branch -D` somente com novo ok do usuário.

## Passos

1. Rodar `git worktree list --porcelain`. A primeira entrada é o repositório principal. Resolver cada alvo para o par worktree e branch:
   - branch: a worktree que está nela;
   - worktree (pasta ou caminho): a branch dela;
   - issue: a branch `<tipo>/<número>-*` ou a worktree `<repo>-<número>`.
   Sem alvo, usar a worktree atual. Se um alvo não resolver ou resolver para mais de um par, mostrar as opções e perguntar.
2. Se o diretório atual estiver dentro de uma worktree a ser removida, parar e orientar: `cd <principal> && claude` e rodar `/atlas-finish <alvo>` lá.
3. Identificar a plataforma por `git remote get-url origin`. No GitHub, rodar `gh pr list --head <branch> --state all --json number,state,url`; no GitLab, `glab mr list --source-branch <branch> --all --output json` (o `mr view` quebra quando a branch tem mais de um MR). Vale o MR ou PR mergeado. Sem nenhum mergeado, rodar `git fetch origin` e `git cherry origin/<branch padrão do MR> <branch>`: se todas as linhas vierem com `-`, o conteúdo já está na branch padrão por outro MR ou PR; mostrar isso e tratar como mergeado só com o ok do usuário. Caso contrário, mostrar o estado e parar esse alvo.
4. Rodar `git -C <worktree> status --porcelain` e `git -C <worktree> log --oneline @{u}..HEAD`. Se houver qualquer pendência, mostrar e parar.
5. Pedir o ok exatamente no formato da confirmação, sem mais nenhum texto antes ou depois, e esperar a resposta.
6. Para cada alvo, rodar `git worktree remove <worktree>` e `git branch -d <branch>`; no fim, `git fetch --prune origin`. Se `branch -d` recusar por "not fully merged" com o MR ou PR mergeado, explicar que isso acontece com squash merge e usar `git branch -D <branch>` após o ok.
7. Se o repositório principal estiver na branch padrão, rodar `git -C <principal> pull --ff-only`.
8. Encerrar com o formato da conclusão. Só acrescentar algo se um passo falhou ou ficou sem fazer (por exemplo, o `pull --ff-only` recusado), em uma linha.

## Formato da confirmação

Um par de linhas de worktree e branch por alvo:

```markdown
Não há mudanças pendentes.

- Worktree: <caminho da worktree>
- Branch local: <branch>

Posso remover?
```

## Formato da conclusão

```markdown
Tarefa finalizada.
```
