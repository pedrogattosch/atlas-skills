---
name: atlas-spec
description: Cria ou atualiza especificações funcionais persistentes em Markdown para uma entrega. Use quando o usuário pedir um spec, requisitos, critérios de aceite ou definição de escopo para uma mudança que deve ficar documentada. Não planeja a implementação nem altera código.
---

# Atlas Spec

Transformar uma necessidade em uma especificação funcional clara e persistente.
Quando o usuário solicitar a criação ou atualização de um spec, gravar o
documento em Markdown. Não escolher a solução técnica nem implementar código.

## Contexto do projeto

Quando houver um projeto disponível, consultar as fontes necessárias nesta
ordem:

1. `README.md`;
2. documentação e specs relacionados em `docs/`;
3. código relacionado, somente quando necessário para compreender o comportamento atual;
4. testes relacionados, somente quando esclarecem o comportamento atual.

## Regras obrigatórias

- Escrever em português, sem texto promocional, genérico ou emojis.
- Seguir a estrutura e a localização de documentação já existentes no projeto. Quando não houver convenção, usar `docs/specs/<nome-da-entrega>.md`.
- Antes de criar um arquivo, procurar um spec relacionado para atualizar em vez de duplicar a documentação.
- Descrever resultado, escopo, comportamento esperado, critérios de aceite, restrições e decisões pendentes.
- Separar explicitamente o que está fora do escopo.
- Converter comportamentos esperados em critérios de aceite verificáveis e observáveis.
- Não escolher arquivos, classes, endpoints, bibliotecas, arquitetura, estratégia de teste ou etapas de implementação.
- Não inventar requisitos, regras de negócio ou decisões. Registrar informações ausentes em `Decisões pendentes`.
- Quando receber alterações, atualizar o documento completo em vez de criar uma versão paralela.
- Não alterar código, testes, configuração, issues, branches ou commits.
- Quando o usuário pedir apenas explicação, análise ou um rascunho no chat, não criar arquivo até que ele peça para persistir o spec.

## Formato obrigatório

# Título

## Objetivo

<resultado esperado para a pessoa usuária ou negócio>

## Contexto

<problema atual, motivação e comportamento existente relevante>

## Escopo

<o que esta entrega inclui>

## Fora do escopo

<o que esta entrega não inclui>

## Comportamento esperado

<fluxos, regras e casos relevantes>

## Critérios de aceite

- Dado <contexto>, quando <ação>, então <resultado observável>.

## Restrições

<compatibilidade, segurança, desempenho ou decisões já tomadas>

## Decisões pendentes

Nenhuma decisão pendente.
