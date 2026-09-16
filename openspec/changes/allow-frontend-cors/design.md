## Context

`SecurityFilterChain` atual exige authexceto `GET /api/message` e nao configura `cors`, entao o Spring responde sem headers CORS. Frontend Nginx no Docker (`5500->80`) ja chama `http://localhost:9090/api/message`. Ver `proposal.md - Why`.

## Goals / Non-Goals

**Goals:**
- Habilitar CORS minimo para o frontend local sem mudar regras de `permitAll`/`authenticated`.
- Manter decisao de CORS na camada de config (SRP), fora do controller.

**Non-Goals:**
- Nao liberar `*`, multiplas origens, `PUT/POST/DELETE`, nem `allowCredentials=true`.
- Nao criar filtro CORS manual nem `@CrossOrigin` espalhado.

## Decisions

- **`.cors(withDefaults())` + `@Bean CorsConfigurationSource` restrito a `/api/**`**
  Rationale: integra com o FilterChain do Spring Security; sem `.cors()` o bean e ignorado. Alternativa `@CrossOrigin` no controller — descartada por acoplar transporte ao controller e dificultar evolucao global.
- **Origem exata `http://localhost:5500`, metodos `GET`, headers `*`, credentials `false`**
  Rationale: menor privilegio para o passo atual (GET publico). Alternativa `allowedOriginPatterns("*")` — descartada por abrir demais para aprendizado.
- **Path `/api/**` em vez de `/**`**
  Rationale: CORS so onde ha API; pagina do frontend e login padrao nao precisam.

## Risks / Trade-offs

- [Risco] Nova origem/porta no futuro exige editar a lista → Mitigacao: proximo passo extrai para propriedade (`app.cors.allowed-origins`).
- [Trade-off] `allowedHeaders("*")` e amplo → Aceito para GET simples; refinar quando houver `Authorization`/JWT.
