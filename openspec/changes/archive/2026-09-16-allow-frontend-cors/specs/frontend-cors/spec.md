## Purpose

Permite que o frontend local em http://localhost:5500 consuma a API via browser, expondo os headers CORS necessarios sem relaxar a autorizacao existente.

## ADDED Requirements

### Requirement: Resposta simples inclui origem permitida
O sistema SHALL incluir `Access-Control-Allow-Origin: http://localhost:5500` em respostas `GET /api/message` quando a requisicao trouxer `Origin: http://localhost:5500`.

#### Scenario: Fetch cross-origin do frontend
- **WHEN** o browser em `http://localhost:5500` faz `GET /api/message`
- **THEN** o sistema responde 200 `"It works"` com `Access-Control-Allow-Origin: http://localhost:5500`

### Requirement: Preflight restrito a GET
O sistema SHALL responder preflight `OPTIONS /api/**` da origem permitida com `Access-Control-Allow-Methods` contendo `GET`.

#### Scenario: Preflight do browser
- **WHEN** o browser envia `OPTIONS /api/message` com `Origin: http://localhost:5500` e `Access-Control-Request-Method: GET`
- **THEN** o sistema responde 200 com `Access-Control-Allow-Origin: http://localhost:5500` e `Access-Control-Allow-Methods: GET`

### Requirement: Outras origens sem CORS
O sistema SHALL NOT expor `Access-Control-Allow-Origin` para origens diferentes de `http://localhost:5500`.

#### Scenario: Origem nao listada
- **WHEN** um cliente chama `GET /api/message` com `Origin: http://localhost:3000`
- **THEN** a resposta nao contem `Access-Control-Allow-Origin` para essa origem
