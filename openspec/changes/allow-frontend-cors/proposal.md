## Why

O frontend Docker em `http://localhost:5500` chama `GET http://localhost:9090/api/message` e o browser bloqueia com `No 'Access-Control-Allow-Origin' header`, mesmo com o backend retornando 200. Sem CORS liberado para essa origem, o passo de aprendizado com frontend separado fica travado.

## What Changes

- Habilita `cors` no `SecurityFilterChain` (`http.cors(withDefaults())`).
- Adiciona `@Bean CorsConfigurationSource` restrito a `/api/**`: origem `http://localhost:5500`, metodos `GET`, headers `*`, sem credenciais.
- Mantem `GET /api/message` publico e demais rotas autenticadas (sem mudanca nas regras de autorizacao).
- Mantem `MyController` intacto (regra de transporte fica na config, SRP).

## Capabilities

### New Capabilities
- `frontend-cors`: liberacao de CORS para o frontend local consumir a API — headers observaveis em respostas simples e preflight.

### Modified Capabilities
- (nenhuma — `security-baseline` ainda nao foi arquivada para `openspec/specs/`, entao nao ha capability principal para modificar)

## Impact

- Codigo: apenas `config/SecurityConfig.java` (imports `java.util.List`, `web.cors.*`, `.cors()` + bean).
- Comportamento observavel: respostas de `/api/**` passam a incluir `Access-Control-Allow-Origin: http://localhost:5500`; preflight `OPTIONS` passa a responder 200 com `Access-Control-Allow-Methods: GET`.
- Sem BREAKING para `curl`/testes sem `Origin`; origens diferentes de `5500` continuam sem header CORS.
