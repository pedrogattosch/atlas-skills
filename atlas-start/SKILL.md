---
name: atlas-start
description: Começa uma tarefa nova em um projeto, escrevendo o plano técnico, criando a branch e uma worktree separada. Use com /atlas-start "descrição da tarefa".
argument-hint: "\"descrição da tarefa\""
disable-model-invocation: true
---

# Atlas Start

Preparar uma tarefa nova: transformar a necessidade em um plano técnico simples e executável, criar a branch e uma worktree separada com o plano salvo em `PLAN.md`. Não implementar código.

Tarefa descrita pelo usuário: $ARGUMENTS

## Contexto do projeto

Consultar as fontes necessárias nesta ordem:

1. `README.md`;
2. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
3. código relacionado;
4. testes relacionados.

## Regras obrigatórias

- Escrever em português, sem texto promocional, genérico ou emojis.
- Executar o fluxo a partir do repositório principal, que é a primeira entrada de `git worktree list`.
- Nomear a branch como `<tipo>/<slug>`, com tipo `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `build`, `ci`, `chore` ou `style` e slug em kebab-case, sem acentos, com até cerca de 40 caracteres.
- Criar a worktree em uma pasta irmã do repositório: `../<repo>-<slug>`, onde `<repo>` é o nome da pasta do repositório principal.
- Não mostrar o plano no chat nem esperar aprovação. O usuário revisa e ajusta o `PLAN.md` diretamente; rodar o `atlas-implement` vale como aprovação.
- Nunca versionar `PLAN.md`.
- Não copiar arquivos locais não versionados, como `.env`, sem o ok do usuário.

## Regras do plano

- Criar título curto, específico e orientado à mudança, preferencialmente iniciado por verbo no infinitivo.
- Em Objetivo e Contexto, descrever apenas o resultado esperado, o problema, a motivação e as informações necessárias, sem antecipar a implementação.
- Não inventar funcionalidades, causas, requisitos, regras de negócio ou escopo. Registrar informação essencial ausente em Decisões pendentes.
- Quando houver um spec relacionado, tratá-lo como fonte de verdade para escopo e comportamento; uma instrução explícita mais recente do usuário prevalece. Não alterar o spec.
- Respeitar a arquitetura, os padrões e as convenções existentes. Não propor arquitetura nova ou refatoração ampla sem necessidade concreta.
- Criar etapas técnicas detalhadas o suficiente para permitir a implementação sem redesenhar a solução.
- Converter os comportamentos esperados em validações verificáveis, expressas como resultados observáveis, e não como etapas técnicas.
- Registrar a menor suposição segura quando houver ambiguidade não bloqueante.
- Manter o plano focado em uma entrega coesa, sem ampliar o escopo ou adicionar detalhamento artificial.
- Registrar apenas dúvidas relevantes em Decisões pendentes.

## Formato do plano

```markdown
# <título>

Branch: `<branch>`

## Objetivo

<resultado esperado da implementação>

## Contexto

<problema, motivação, comportamento atual e informações necessárias para orientar a entrega>

## Etapas da implementação

1. <primeiro passo técnico>
2. <segundo passo técnico>

## Validações

| ID | Validação |
| --- | --- |
| V001 | <primeira validação verificável> |
| V002 | <segunda validação verificável> |

## Decisões pendentes

Nenhuma decisão pendente.
```

## Passos

1. Se não houver descrição da tarefa, pedir uma e parar.
2. Rodar `git rev-parse --show-toplevel`, `git worktree list` e `git status --short`. Se houver mudanças não commitadas, avisar que elas não vão para a worktree nova e perguntar se deve continuar.
3. Rodar `git fetch origin` e descobrir a branch padrão com `git symbolic-ref --short refs/remotes/origin/HEAD`. Se falhar, usar `origin/main`.
4. Escolher o nome da branch e confirmar que ela não existe localmente (`git branch --list <branch>`) nem no remoto (`git ls-remote --heads origin <branch>`).
5. Ler o contexto do projeto e escrever o plano.
6. Criar a worktree com `git worktree add ../<repo>-<slug> -b <branch> <branch padrão remota>`.
7. Gravar o plano em `PLAN.md` na raiz da worktree e garantir que `PLAN.md` está em `$(git rev-parse --git-common-dir)/info/exclude`, adicionando a linha se faltar.
8. Se o projeto depender de arquivos não versionados para rodar, perguntar se devem ser copiados para a worktree.
9. Encerrar com o formato da saída.

## Formato da saída

```markdown
## Tarefa iniciada

- Branch: `<branch>`
- Worktree: `<caminho da worktree>`
- Plano: `<caminho da worktree>/PLAN.md`
- Arquivos locais copiados: <lista ou "nenhum">
- Decisões pendentes: <quantidade, ou "nenhuma">

Próximo passo: revisar o `PLAN.md`, abrir `cd <caminho da worktree> && claude` e pedir a implementação.
```
