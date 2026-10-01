---
name: atlas-implement
description: Implementa tarefas de código ou documentação. Use para alterar código, criar ou atualizar documentação, ajustar comportamento, atender uma issue simples ou executar um plano aprovado.
---

# Implementar

Executar uma tarefa coesa, segura e alinhada ao projeto, seja de código ou documentação.

## Contexto do projeto

Antes de executar, consultar as fontes necessárias nesta ordem:

1. `README.md`;
2. arquivos relevantes em `docs/`, incluindo um spec relacionado quando existir;
3. código relacionado;
4. testes relacionados.

## Regras obrigatórias

- Executar somente a tarefa solicitada ou planejada. Quando houver um spec relacionado, considerá-lo a fonte de verdade para escopo e comportamento; uma instrução explícita mais recente do usuário prevalece. Se houver conflito entre spec e issue, informar a divergência antes de executar.
- Respeitar o planejamento existente e usar suas etapas como guia técnico, fazendo apenas os pequenos ajustes exigidos pelo código real.
- Inspecionar padrões, arquitetura, camadas, nomes e convenções existentes antes de implementar.
- Fazer a menor alteração necessária para atender completamente ao objetivo, sem formatação global, funcionalidades futuras, abstrações prematuras ou melhorias fora do escopo.
- Manter responsabilidades separadas, nomes claros e regras de negócio na camada responsável.
- Executar refatorações pequenas e seguras somente quando solicitadas e remover código morto apenas quando relacionado ao escopo.
- Usar comentários somente para detalhes técnicos não óbvios; não comentar código autoexplicativo, assinaturas, parâmetros, regras, decisões, tipos ou retornos já evidentes.
- Criar ou ajustar testes quando a mudança introduzir ou alterar comportamento observável, regra de negócio, validação, tratamento de erro, caso limite ou corrigir falha sem cobertura, desde que não haja cobertura adequada e exista infraestrutura disponível.
- Não criar testes para mudanças puramente estruturais, de texto, rótulo, configuração ou constante sem regra associada.
- Em documentação .md, manter cada parágrafo em uma linha, sem hard wrap, preservando idioma, títulos, nomes, organização e conteúdo de blocos de código.
- Usar informações do repositório quando necessárias para compreender a entrega e preservar mudanças não relacionadas do usuário.
- Não incluir em stage, commitar, enviar alterações, abrir merge request ou atualizar issue automaticamente.
- Atender todas as validações planejadas. Se alguma não puder ser atendida, informar qual ficou pendente e o motivo.
- Em correções posteriores, modificar somente o necessário e preservar o que já estiver correto.

## Formato obrigatório

## Resumo

<resumo curto da alteração e de qualquer divergência relevante>

## Validações

<testes relevantes executados ou a justificativa objetiva de não execução>
