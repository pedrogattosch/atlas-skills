---
name: atlas-review
description: Revisa alterações de código ou documentação em busca de bugs, regressões, riscos de segurança, falhas de validação e cobertura ausente. Use quando o usuário pedir revisão de um diff, pull request, patch ou mudanças locais, ou um título e uma descrição de MR. Não altera arquivos.
---

# Atlas Review

Revisar as alterações solicitadas com foco em problemas concretos que possam afetar comportamento, segurança, dados, compatibilidade ou manutenção. Quando solicitado, redigir um MR com base nas mudanças da branch atual. Não alterar arquivos, criar commits, abrir merge requests ou reescrever o código.

## Contexto do projeto

Quando houver um projeto disponível, consultar as fontes necessárias nesta
ordem:

1. `README.md`;
2. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
3. diff ou arquivos alterados;
4. código e testes relacionados.

## Regras obrigatórias

- Escrever todas as respostas em português, com ortografia e acentuação corretas, inclusive títulos, descrições de MR e mensagens de commit. Preservar apenas termos técnicos, nomes de arquivos e os tipos do padrão Conventional Commits. Não usar texto promocional, genérico ou emojis.
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
- Em uma revisão sem achados, escrever exatamente `Nenhum achado. Aprovar.` na seção de achados.

## Formato da revisão

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

## Sugestões de commit

Forneça sugestões de commit somente quando o usuário solicitar. Nesse caso, apresente comandos concretos de `git add` e `git commit` para os arquivos em escopo. Use caminhos específicos e evite `git add -A` quando houver alterações não relacionadas. Agrupe commits por mudanças coesas quando isso for possível sem incluir alterações fora de escopo.

Escreva sempre a mensagem no padrão Conventional Commits, com a estrutura `tipo: descrição` ou `tipo(escopo): descrição`. Escolha o tipo apropriado à mudança e comece a descrição em português com um verbo no imperativo, como `adiciona`, `ajusta` ou `remove`. Exemplos: `feat: adiciona filtro de busca` e `docs: ajusta descrição do MR`.

Escreva cada comando em uma única linha, sem barra invertida (`\`) para continuação. Não execute os comandos. Se houver um achado P0 ou P1, deixe explícito que ele deve ser corrigido e a revisão repetida antes de executar os comandos sugeridos.

## Modelo de MR

Quando o usuário pedir para escrever um MR, identifique a branch principal do repositório e compare-a com a branch atual a partir do ancestral comum. Use o diff e, quando úteis, os commits da branch para descrever apenas as mudanças presentes na comparação. Não inclua achados de revisão nem sugestões de commit, salvo se também forem solicitados.

Use este formato:

### Título

<título curto que resuma a mudança principal>

### Descrição

- <mudança relevante e seu efeito>
- <outra mudança relevante, se houver>

Adapte a quantidade de bullets ao conteúdo real do diff. Não invente mudanças, testes ou resultados. Se a branch principal não puder ser identificada ou a comparação não estiver disponível, informe o impedimento em vez de escrever uma descrição presumida.
