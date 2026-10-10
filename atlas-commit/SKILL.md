---
name: atlas-commit
description: Cria commits automaticamente no padrão Conventional Commits a partir das mudanças atuais, separando por assunto. Use com /atlas-commit.
argument-hint: "[--main] [dica opcional sobre a mudança]"
disable-model-invocation: true
---

# Atlas Commit

Criar commits coesos a partir das mudanças atuais, sem pedir aprovação. Parar apenas nos casos de risco descritos abaixo. Não enviar alterações.

Opções e dica do usuário (opcional): $ARGUMENTS

## Regras obrigatórias

- Nunca commitar na branch padrão (main/master), exceto quando o usuário passar `--main`. A opção vale apenas para essa execução.
- Um assunto por commit. Mudanças de assuntos diferentes viram commits separados, cada um com sua lista de arquivos.
- Adicionar arquivos pelo nome; nunca usar `git add -A` ou `git add .`.
- Nunca commitar `PLAN.md`, `.env`, chaves, tokens, arquivos de credenciais ou arquivos gerados que não deveriam estar no repositório. Se algum aparecer entre as mudanças, deixá-lo fora e avisar.
- Se uma mudança parecer não relacionada à tarefa ou houver dúvida sobre incluí-la, deixá-la fora e avisar.
- Não incluir atribuição ao Claude ou a ferramentas de IA na mensagem, como `Co-Authored-By: Claude` ou "Generated with Claude Code".
- Nunca usar `--no-verify`.
- Não fazer push; em uma branch de tarefa, isso é feito pelo `/atlas-mr` (GitHub) ou `/atlas-pr` (GitLab); com `--main`, o push fica com o usuário.

## Mensagem de commit

Usar o padrão Conventional Commits, em português:

```text
<tipo>(<escopo opcional>): <resumo>

<corpo opcional: por que a mudança foi feita>
```

- Tipos: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `build`, `ci`, `chore`, `style`.
- Resumo com verbo no presente (`adiciona`, `corrige`, `remove`), minúsculo, sem ponto final, com até 72 caracteres.
- Escopo com o módulo ou área afetada (`auth`, `api`, `ui`). Omitir se não houver um claro.
- Corpo somente quando o motivo não for óbvio pelo resumo, com linhas de até cerca de 72 colunas.
- Mudança incompatível: `!` depois do tipo (`feat(api)!: ...`) e rodapé `BREAKING CHANGE: <explicação>`.

## Passos

1. Rodar `git status --short`, `git diff` e `git diff --staged`. Se não houver mudanças, avisar e parar.
2. Rodar `git branch --show-current`. Se for a branch padrão sem `--main`, parar e sugerir `/atlas-start` ou `/atlas-commit --main`. Desconsiderar `--main` ao usar o restante dos argumentos como dica.
3. Agrupar as mudanças por assunto e escrever a mensagem de cada commit.
4. Para cada commit, rodar `git add <arquivos>` e `git commit -F <arquivo temporário com a mensagem>`.
5. Se um hook falhar, mostrar o erro. Se a correção for simples e fizer parte da mudança, corrigir e tentar de novo; senão, parar e explicar.
6. Encerrar com o formato da saída.

## Formato da saída

```markdown
## Commits

- Branch: `<branch>`
- `<hash>` <tipo>(<escopo>): <resumo>

## Fora dos commits

<arquivos deixados de fora e o motivo, ou "Nenhum arquivo deixado de fora.">

Próximo passo: <`/atlas-mr` ou `/atlas-pr` em uma branch de tarefa; `git push` quando usado `--main`>
```
