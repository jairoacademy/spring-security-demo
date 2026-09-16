## Purpose

Estabelece o comportamento baseline de seguranca HTTP da aplicacao para aprendizado progressivo: um endpoint publico de verificacao e protecao por autenticacao para todo o resto.

## ADDED Requirements

### Requirement: Endpoint publico de verificacao
O sistema SHALL expor `GET /api/message` sem exigir autenticacao, retornando corpo de texto `"It works"` com status 200.

#### Scenario: Acesso anonimo ao endpoint publico
- **WHEN** um cliente anonimo faz `GET /api/message`
- **THEN** o sistema responde 200 com corpo `"It works"`

### Requirement: Protecao das demais rotas
O sistema SHALL exigir autenticacao para qualquer requisicao diferente de `GET /api/message`, negando acesso anonimo.

#### Scenario: Acesso anonimo a rota protegida
- **WHEN** um cliente anonimo acessa qualquer rota diferente de `/api/message`
- **THEN** o sistema responde 401 ou redireciona para login, sem expor conteudo

#### Scenario: Acesso autenticado a rota protegida
- **WHEN** um cliente autenticado acessa uma rota protegida
- **THEN** o sistema permite o processamento normal da requisicao
