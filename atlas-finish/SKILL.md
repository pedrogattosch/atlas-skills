---
name: atlas-finish
description: Limpa uma tarefa já mergeada no GitHub ou no GitLab, removendo a worktree, apagando a branch local e atualizando a branch padrão. Use com /atlas-finish [branch].
argument-hint: "[nome da branch]"
disable-model-invocation: true
---

# Atlas Finish

Encerrar uma tarefa cujo MR (GitHub) ou PR (GitLab) já foi mergeado. Não alterar código.

Branch informada (opcional): $ARGUMENTS

## Regras obrigatórias

- Não remover a worktree onde a sessão está rodando.
- Não descartar uma branch sem MR ou PR mergeado, salvo pedido explícito do usuário.
- Não remover nada com mudanças não commitadas ou commits não enviados.
- Usar `--force` em `git worktree remove` ou `git branch -D` somente com novo ok do usuário.

## Passos

1. Rodar `git worktree list --porcelain`. A primeira entrada é o repositório principal. Sem branch no argumento, usar a branch da worktree atual e descobrir o caminho da worktree dessa branch.
2. Se o diretório atual estiver dentro da worktree a ser removida, parar e orientar: `cd <principal> && claude` e rodar `/atlas-finish <branch>` lá.
3. Identificar a plataforma por `git remote get-url origin`. No GitHub, rodar `gh pr view <branch> --json state,mergedAt,url` e conferir o estado `MERGED`; no GitLab, rodar `glab mr view <branch> --output json` e conferir o estado `merged`. Se não estiver mergeado, mostrar o estado e parar.
4. Rodar `git -C <worktree> status --porcelain` e `git -C <worktree> log --oneline @{u}..HEAD`. Se houver qualquer pendência, mostrar e parar.
5. Mostrar o que será removido (pasta da worktree e branch local) e esperar o ok.
6. Rodar `git worktree remove <worktree>`, `git branch -d <branch>` e `git fetch --prune origin`. Se `branch -d` recusar por "not fully merged" com o MR ou PR mergeado, explicar que isso acontece com squash merge e usar `git branch -D <branch>` após o ok.
7. Se o repositório principal estiver na branch padrão, rodar `git -C <principal> pull --ff-only`.
8. Encerrar com o formato da saída, usando `git worktree list` para listar as worktrees restantes.

## Formato da saída

```markdown
## Tarefa finalizada

- MR/PR: <url do MR ou PR>
- Worktree removida: `<caminho da worktree>`
- Branch removida: `<branch>`
- Branch padrão: <atualizada ou motivo de não atualizar>
- Worktrees restantes: <lista>
```
