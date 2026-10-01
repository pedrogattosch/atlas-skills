---
name: atlas-review
description: Revisa alterações de código ou documentação em busca de bugs, regressões, riscos de segurança, falhas de validação e cobertura ausente. Use quando o usuário pedir revisão de um diff, pull request, patch ou mudanças locais. Não altera arquivos.
---

# Atlas Review

Revisar as alterações solicitadas com foco em problemas concretos que possam
afetar comportamento, segurança, dados, compatibilidade ou manutenção. Não
alterar arquivos, criar commits ou reescrever o código.

## Contexto do projeto

Quando houver um projeto disponível, consultar as fontes necessárias nesta
ordem:

1. `README.md`;
2. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
3. diff ou arquivos alterados;
4. código e testes relacionados.

## Regras obrigatórias

- Escrever em português, sem texto promocional, genérico ou emojis.
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
- Se não houver achados, escrever exatamente: `Nenhum achado. Aprovar.`

## Formato obrigatório

Para cada achado, usar este formato:

`[P<prioridade>] <arquivo>:<linha> — <problema e impacto>. <cenário que o reproduz ou explica>.`

Prioridades:

- `P0`: bloqueia produção, causa perda de dados, vulnerabilidade crítica ou indisponibilidade ampla.
- `P1`: bug relevante, regressão provável ou risco de segurança que deve ser corrigido antes da aprovação.
- `P2`: problema real com impacto limitado, corrigir em seguida.
- `P3`: melhoria de robustez com impacto baixo e comprovável.

Após os achados, quando houver algum, finalizar com:

## Resumo

`<N> achado(s): <quantidade por prioridade>.`
