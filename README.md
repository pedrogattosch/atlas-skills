<p align="center">
  <img src="atlas-skills-icon.png" alt="Atlas Skills" width="160">
</p>

<h1 align="center">Atlas Skills</h1>

Coleção de skills para conduzir uma entrega de software do início da tarefa à publicação.

## Skills

| Skill | Uso |
| --- | --- |
| `atlas-issue` | Abre uma issue no board Development do GitLab, com as labels de coluna, área, complexidade e prioridade. |
| `atlas-start` | Escreve o plano da tarefa e cria a branch e uma worktree com o `PLAN.md`. |
| `atlas-spec` | Cria ou atualiza uma especificação funcional persistente em Markdown. |
| `atlas-implement` | Implementa uma tarefa coesa, respeitando o spec e o plano existentes. |
| `atlas-review` | Revisa um diff por bugs, regressões, riscos e lacunas de validação. |
| `atlas-commit` | Cria commits no padrão Conventional Commits, sem pedir aprovação. |
| `atlas-mr` | Envia a branch e abre o MR no GitLab com o template padrão. |
| `atlas-pr` | Envia a branch e abre o PR no GitHub com o template padrão. |
| `atlas-finish` | Remove a worktree e a branch local depois do merge, no GitHub ou no GitLab. |

## Fluxo de trabalho

```text
atlas-issue (opcional) → atlas-start → atlas-spec (opcional) → atlas-implement → atlas-review → atlas-commit → atlas-mr ou atlas-pr → atlas-finish
```

1. Quando a tarefa ainda não tiver issue, abrir uma com `/atlas-issue "descrição da issue"`. A skill mostra a issue e só a cria depois do ok no chat.
2. No repositório principal, rodar `/atlas-start "descrição da tarefa"`, ou `/atlas-start #<número da issue>` quando houver issue, com a descrição opcional como complemento. Com issue, a skill lê a issue no GitLab ou no GitHub e a usa como base do plano. A skill cria a branch, a worktree e o `PLAN.md`, sem mostrar o plano no chat, copia os arquivos `.env` não versionados e instala as dependências do projeto na worktree. Sem issue, a branch é `<tipo>/<slug>` e a worktree fica em `../<repo>-<slug>`; com issue, a branch é `<tipo>/<número da issue>-<slug>` e a worktree fica em `../<repo>-<número da issue>`.
3. Revisar o `PLAN.md` e ajustar o que for preciso direto no arquivo. No trabalho, copiar o conteúdo para a issue.
4. Abrir uma nova sessão na worktree: `cd <pasta da worktree> && claude`.
5. Quando a entrega precisar de requisitos documentados, pedir um spec com `atlas-spec`.
6. Pedir a implementação. Acionar o `atlas-implement` vale como aprovação do `PLAN.md`; ele segue a versão atual do arquivo e o spec.
7. Pedir uma revisão das mudanças com `atlas-review` e corrigir os achados P0 e P1 antes de seguir.
8. Rodar `/atlas-commit`. Os commits são criados automaticamente, separados por assunto.
9. Rodar `/atlas-mr` no GitLab ou `/atlas-pr` no GitHub (ambos aceitam `--draft` e `--target <branch>`; sem `--target`, o destino é a branch em que os MRs ou PRs recentes foram mesclados, como `dev`). A skill mostra título, destino e descrição e só envia a branch e abre o MR ou PR depois do ok no chat.
10. Para ajustes pedidos no MR ou PR, repetir os passos 6 a 9; a skill apenas envia os commits novos ao MR ou PR aberto.
11. Depois do merge, no repositório principal, rodar `/atlas-finish <branch>` para remover a worktree, a branch local e o `PLAN.md`.

Em repositórios pessoais sem revisão, como este, é possível commitar direto na branch padrão com `/atlas-commit --main` e enviar com `git push`, sem `atlas-start`, MR ou PR. Sem `--main`, o `atlas-commit` se recusa a commitar na branch padrão.
