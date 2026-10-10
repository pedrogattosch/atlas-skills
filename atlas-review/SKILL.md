---
name: atlas-review
description: Revisa alterações de código ou documentação em busca de bugs, regressões, riscos de segurança, falhas de validação e cobertura ausente. Use quando o usuário pedir revisão de um diff, pull request, merge request, patch ou mudanças locais, ou um título e uma descrição de PR ou MR. Não altera arquivos.
---

# Atlas Review

Revisar as alterações solicitadas com foco em problemas concretos que possam afetar comportamento, segurança, dados, compatibilidade ou manutenção. Quando solicitado, redigir um PR ou MR com base nas mudanças da branch atual. Não alterar arquivos, criar commits, abrir pull requests ou merge requests ou reescrever o código; os commits são feitos pelo `/atlas-commit` e a publicação pelo `/atlas-mr` (GitHub) ou `/atlas-pr` (GitLab).

## Contexto do projeto

Quando houver um projeto disponível, consultar as fontes necessárias nesta ordem:

1. `README.md`;
2. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
3. diff ou arquivos alterados;
4. código e testes relacionados.

## Regras obrigatórias

- Escrever todas as respostas em português, com ortografia e acentuação corretas, inclusive títulos e descrições de PR ou MR. Preservar apenas termos técnicos, nomes de arquivos e os tipos do padrão Conventional Commits. Não usar texto promocional, genérico ou emojis.
- Entender o comportamento existente e o objetivo da mudança antes de apontar um problema.
- Quando houver um spec relacionado, comparar as alterações com seus critérios de aceite, escopo e restrições.
- Reportar somente achados concretos, introduzidos pelas alterações ou diretamente afetados por elas.
- Priorizar bugs, regressões, perda ou corrupção de dados, vulnerabilidades, violações de contrato, casos de borda, compatibilidade e validação ausente para comportamento novo ou modificado.
- Não listar preferências de estilo, sugestões especulativas, refatorações amplas ou problemas não relacionados ao escopo.
- Verificar os chamadores e consumidores relevantes quando a mudança afetar uma interface, contrato ou comportamento compartilhado.
- Conferir se os testes existentes cobrem o comportamento alterado e reportar a lacuna apenas quando ela puder permitir regressão relevante.
- Não assumir que uma alteração está errada só porque é pequena, incomum ou diferente do padrão; apontar o efeito observável e as condições que o produzem.
- Não inventar cenários, requisitos ou regras de negócio. Quando uma conclusão depender de informação indisponível, declarar a incerteza em vez de registrar um achado.
- Não propor correção fora do escopo do achado.

## Formato da saída

```markdown
## Achados

- [P<prioridade>] <arquivo>:<linha> — <problema e impacto>. <cenário que o reproduz ou explica>.

## Resumo

<N> achado(s): <quantidade por prioridade>.
```

Prioridades:

- `P0`: bloqueia produção, causa perda de dados, vulnerabilidade crítica ou indisponibilidade ampla.
- `P1`: bug relevante, regressão provável ou risco de segurança que deve ser corrigido antes da aprovação.
- `P2`: problema real com impacto limitado, corrigir em seguida.
- `P3`: melhoria de robustez com impacto baixo e comprovável.

Em uma revisão sem achados, a seção Achados contém apenas `Nenhum achado. Aprovar.` e a seção Resumo é omitida.

## Modelo de PR ou MR

Quando o usuário pedir para escrever um PR ou MR, identificar a branch padrão do repositório e compará-la com a branch atual a partir do ancestral comum. Usar o diff e, quando úteis, os commits da branch para descrever apenas as mudanças presentes na comparação. Não incluir achados de revisão, salvo se também forem solicitados.

Escrever o título no formato de commit do `atlas-commit`, resumindo a branch inteira. Preencher a descrição com o template do repositório, quando existir (`.github/pull_request_template.md` no GitHub ou `.gitlab/merge_request_templates/Default.md` no GitLab); senão, usar o template em `../atlas-mr/template.md`, seguindo as mesmas regras de preenchimento do `atlas-mr`.

Não inventar mudanças, testes ou resultados. Se a branch padrão não puder ser identificada ou a comparação não estiver disponível, informar o impedimento em vez de escrever uma descrição presumida.

Usar este formato:

```markdown
## Título

<tipo>(<escopo>): <resumo>

## Descrição

<template preenchido>
```
