---
name: atlas-plan
description: Planeja issues complexas e redige issues novas antes da implementação. Use para análise inicial, próximos passos ou preparação para uma implementação complexa.
---

# Planejar

Transformar uma necessidade em um plano técnico simples, objetivo e executável. Não implementar código, não alterar arquivos do projeto e não fazer commit.

## Contexto do projeto

Quando houver um projeto disponível, consultar as fontes necessárias nesta ordem:

1. `README.md`;
2. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
3. código relacionado;
4. testes relacionados.

## Regras obrigatórias

- Escrever em português, sem texto promocional, genérico ou emojis.
- Criar título curto, específico e orientado à mudança, preferencialmente iniciado por verbo no infinitivo.
- Nas seções objetivo e contexto, descrever apenas o resultado esperado, o problema, a motivação e as informações necessárias para orientar a entrega, sem antecipar a implementação.
- Não inventar funcionalidades, causas, requisitos, regras de negócio ou escopo. Se faltar informação essencial, indicar objetivamente o que falta antes de redigir.
- Quando houver um spec relacionado, tratá-lo como fonte de verdade para escopo e comportamento; uma instrução explícita mais recente do usuário prevalece. Não alterar o spec durante o planejamento.
- Não citar testes, commits, merge requests ou branches.
- Inspecionar o projeto quando necessário para compreender arquitetura, padrões, convenções e impacto da mudança.
- Respeitar a estrutura documentada e as convenções existentes.
- Criar etapas técnicas detalhadas o suficiente para permitir a implementação sem redesenhar a solução.
- Converter os comportamentos esperados em validações verificáveis, expressas como resultados observáveis, e não como etapas técnicas.
- Não propor arquitetura nova ou refatoração ampla sem necessidade concreta.
- Registrar a menor suposição segura quando houver ambiguidade não bloqueante.
- Manter o plano focado em uma entrega coesa, sem ampliar o escopo ou adicionar detalhamento artificial.
- Não implementar código nem alterar arquivos do projeto durante o planejamento.
- Registrar apenas dúvidas relevantes em Decisões pendentes. Quando não houver dúvidas, escrever “Nenhuma decisão pendente.”.
- Ao receber alterações no planejamento, retornar o documento completo atualizado.

### Formato obrigatório

## Título

## Objetivo

<resultado esperado da implementação>

## Contexto

<problema, motivação, comportamento atual e informações necessárias para orientar a entrega>

## Etapas da implementação

1. Primeiro passo técnico.
2. Segundo passo técnico.

## Validações

| ID | Validação |
|---|---|
| V001 | Primeira validação verificável. |
| V002 | Segunda validação verificável. |

## Decisões pendentes

Nenhuma decisão pendente.
