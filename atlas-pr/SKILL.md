---
name: atlas-pr
description: Envia a branch atual e abre um pull request no GitHub com título e descrição no padrão do projeto, após aprovação. Use com /atlas-pr.
argument-hint: "[--draft]"
disable-model-invocation: true
---

# Atlas PR

Publicar a branch atual e abrir um pull request no GitHub. Não alterar código.

Opções do usuário: $ARGUMENTS

## Regras obrigatórias

- Escrever título e descrição em português, sem texto promocional, genérico ou emojis.
- Escrever o título no formato de commit do `atlas-commit`, resumindo a branch inteira. Se houver um único commit, usar o assunto dele.
- Preencher a descrição com `.github/pull_request_template.md` do repositório, quando existir; senão, usar o [template](template.md) desta skill.
- Ao preencher o template: em Issue relacionada, usar `Closes #<número>` quando houver issue, identificada pela branch no formato `<tipo>/<número da issue>-<slug>`, pelo `PLAN.md` ou pelos commits, e remover a seção quando não houver; remover Evidências quando não houver o que mostrar; marcar no Checklist apenas o que foi verificado.
- Escrever listas sem linha em branco entre os itens.
- Usar como fonte `PLAN.md` (quando existir), as mensagens de commit e o diff. Não inventar mudanças, testes ou resultados.
- Não registrar dúvidas ou decisões pendentes na descrição. Quando faltar informação, listar as perguntas no terminal e só escrever o PR depois que todas forem respondidas.
- Não incluir atribuição ao Claude ou a ferramentas de IA no título ou na descrição, como "Generated with Claude Code".
- Nunca enviar para a branch padrão. Nunca usar `--force`; usar `--force-with-lease` somente se o usuário pedir explicitamente.

## Passos

1. Rodar `git remote get-url origin`. Se o remoto for do GitLab, sugerir `/atlas-mr` e parar.
2. Rodar `git branch --show-current`. Se for a branch padrão, parar.
3. Rodar `git status --short`. Se houver mudanças não commitadas além de `PLAN.md`, sugerir `/atlas-commit` e perguntar se deve continuar mesmo assim.
4. Rodar `git fetch origin` e descobrir a branch padrão com `git symbolic-ref --short refs/remotes/origin/HEAD`.
5. Rodar `git log --oneline <padrão remota>..HEAD` e `git diff --stat <padrão remota>...HEAD`. Se não houver commits, parar.
6. Rodar `gh pr view --json url,state,title`. Se já houver PR aberto, oferecer apenas `git push`, mostrar o link e parar. Se o `gh` falhar por autenticação, orientar `gh auth login` e parar.
7. Se houver dúvidas ou decisões pendentes, listá-las no terminal e esperar todas as respostas.
8. Escrever título e descrição e mostrá-los no formato da aprovação. Esperar o ok.
9. Rodar `git push -u origin HEAD` e `gh pr create --base <padrão> --head <branch> --title "<título>" --body-file <arquivo temporário> [--draft]`. Se o push for rejeitado porque o remoto divergiu, parar e explicar.
10. Encerrar com o formato da saída.

## Formato da aprovação

```markdown
## Pull request

- Origem: `<branch>`
- Destino: `<branch padrão>`
- Título: `<título>`

<descrição preenchida>

Posso enviar a branch e abrir o PR?
```

## Formato da saída

```markdown
## PR aberto

- Link: <url do PR>
- Commits enviados: <quantidade>
```
