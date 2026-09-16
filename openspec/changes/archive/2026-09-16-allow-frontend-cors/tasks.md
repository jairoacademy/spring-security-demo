## 1. Config CORS restrita (SRP: regra na config, nao no controller)

- [x] 1.1 Habilitar `.cors(withDefaults())` e criar `@Bean CorsConfigurationSource` restrito a `/api/**` (origem `http://localhost:5500`, metodos `GET`) e verificar `curl -H "Origin: http://localhost:5500" localhost:9090/api/message` retorna 200 com `Access-Control-Allow-Origin: http://localhost:5500`
- [x] 1.2 Verificar preflight `OPTIONS` com `Origin` + `Access-Control-Request-Method: GET` retorna 200 com `Allow-Methods: GET` e origem diferente (ex: `:3000`) nao recebe header CORS

## 2. Regressao

- [x] 2.1 Rodar `./mvnw test` e verificar `BUILD SUCCESS`, e retestar botao em `http://localhost:5500` retornando `It works` sem erro de CORS
