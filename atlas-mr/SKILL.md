---
name: atlas-mr
description: Envia a branch atual e abre um merge request no GitLab com título e descrição no padrão do projeto, só depois da aprovação do usuário no chat. Use com /atlas-mr [--draft] [--target <branch>] ou sempre que o usuário pedir para abrir um MR.
argument-hint: "[--draft] [--target <branch>]"
disable-model-invocation: false
---

# Atlas MR

Publicar a branch atual e abrir um merge request no GitLab. Não alterar código.

Opções do usuário: $ARGUMENTS

## Regras obrigatórias

- Nunca enviar a branch nem abrir o MR sem antes mostrar título, destino e descrição no formato da aprovação e receber o ok do usuário no chat. Um pedido como "abre o MR" é o pedido de preparar; o envio só acontece depois do ok.
- O destino é a branch de `--target`; sem ela, a branch em que os MRs recentes do repositório foram mesclados (como `dev`), e só então a branch padrão. Dizer o destino na aprovação.
- Escrever título e descrição em português, sem texto promocional, genérico ou emojis.
- Escrever o título no formato de commit do `atlas-commit`, resumindo a branch inteira. Se houver um único commit, usar o assunto dele.
- Preencher a descrição com `.gitlab/merge_request_templates/Default.md` do repositório, quando existir; senão, usar o [template](template.md) desta skill.
- Ao preencher o template: a Descrição é um parágrafo curto com o que muda e por quê; cada item de Mudanças é uma frase, agrupando o que é da mesma tela ou do mesmo recurso; Como testar é uma lista numerada com os passos e o resultado esperado. Só essas três seções: sem tipo de mudança, issue relacionada, evidências ou checklist.
- Só a primeira letra maiúscula, como no início de uma frase. Termos do domínio ficam em minúscula no meio do texto (notas, atividades, rede, grupo, implantador, financeiro, administrador); maiúscula só em siglas (CSM, API, MRR) e nomes de sistema ou produto (Oráculo, SenseData, Movidesk).
- Escrever listas sem linha em branco entre os itens.
- Usar como fonte `PLAN.md` (quando existir), as mensagens de commit e o diff. Não inventar mudanças, testes ou resultados.
- Não registrar dúvidas ou decisões pendentes na descrição. Quando faltar informação, listar as perguntas no terminal e só escrever o MR depois que todas forem respondidas.
- Não incluir atribuição ao Claude ou a ferramentas de IA no título ou na descrição, como "Generated with Claude Code".
- Nunca enviar para a branch padrão. Nunca usar `--force`; usar `--force-with-lease` somente se o usuário pedir explicitamente.

## Passos

1. Rodar `git remote get-url origin`. Se o remoto for do GitHub, sugerir `/atlas-pr` e parar.
2. Rodar `git branch --show-current`. Se for a branch padrão, parar.
3. Rodar `git status --short`. Se houver mudanças não commitadas além de `PLAN.md`, sugerir `/atlas-commit` e perguntar se deve continuar mesmo assim.
4. Rodar `git fetch origin` e descobrir a branch padrão com `git symbolic-ref --short refs/remotes/origin/HEAD`. Definir o destino: `--target`, senão a `target_branch` dos últimos MRs mesclados (`glab mr list --merged --per-page 5 --output json`), senão a padrão.
5. Rodar `git log --oneline origin/<destino>..HEAD` e `git diff --stat origin/<destino>...HEAD`. Se não houver commits, parar.
6. Rodar `glab mr view <branch> --output json`. Se já houver MR aberto, oferecer apenas `git push`, mostrar o link e parar. Se o `glab` não estiver instalado ou falhar por autenticação, orientar a instalação ou `glab auth login` e parar.
7. Se houver dúvidas ou decisões pendentes, listá-las no terminal e esperar todas as respostas.
7a. Quando o repositório tiver testes de tela (pasta `tests/e2e` ou instrução em `docs/TESTES.md`), rodá-los do jeito que o doc manda antes da aprovação e dizer o resultado nela. Com e2e falhando, parar e corrigir antes de abrir o MR; o CI roda os e2e e um MR aberto com eles quebrados volta.
8. Escrever título e descrição e mostrá-los no formato da aprovação. Encerrar a resposta nesse ponto e esperar o ok; aplicar os ajustes pedidos e mostrar de novo até o ok.
9. Rodar `git push -u origin HEAD` e `glab mr create --source-branch <branch> --target-branch <destino> --title "<título>" --description "$(cat <arquivo temporário>)" --yes [--draft]`. Se o push for rejeitado porque o remoto divergiu, parar e explicar.
10. Encerrar com o formato da saída.

## Formato da aprovação

```markdown
## Merge request

- Origem: `<branch>`
- Destino: `<destino>`
- Título: `<título>`
- Testes: <resultado da suíte e dos e2e, ou "o repositório não tem e2e">

<descrição preenchida>

Posso enviar a branch e abrir o MR?
```

## Formato da saída

```markdown
## MR aberto

- Link: <url do MR>
- Commits enviados: <quantidade>
```
