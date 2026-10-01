# Atlas Skills

Coleção de skills para conduzir uma entrega de software do escopo à revisão.

O Atlas separa o contrato funcional, o planejamento técnico, a implementação e
a revisão. Cada skill tem uma responsabilidade própria e não cria commits,
issues ou merge requests automaticamente.

## Skills

| Skill | Uso |
| --- | --- |
| `atlas-spec` | Cria ou atualiza uma especificação funcional persistente em Markdown. |
| `atlas-plan` | Redige o plano técnico de uma entrega. |
| `atlas-implement` | Implementa uma tarefa coesa, respeitando o spec e o plano existentes. |
| `atlas-review` | Revisa um diff por bugs, regressões, riscos e lacunas de validação. |

## Fluxo de trabalho

```text
atlas-spec
  ↓
atlas-plan
  ↓
atlas-implement
  ↓
atlas-review
```
