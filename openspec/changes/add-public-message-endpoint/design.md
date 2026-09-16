## Context

Projeto Spring Boot 4.1.1 / Java 21 com `spring-boot-starter-security` e `webmvc`, hoje sem nenhuma config nem controller. Ver `proposal.md - Why`. Restricao didatica: evoluir em passos minimos respeitando SRP para facilitar os proximos passos (users, PasswordEncoder, JWT).

## Goals / Non-Goals

**Goals:**
- Explicitar o `SecurityFilterChain` em vez de depender do default implicito.
- Separar decisao de seguranca (`config`) de exposicao HTTP (`controller`).
- Manter comportamento facil de testar com `curl` e `MockMvc`.

**Non-Goals:**
- Nao definir modelo de usuario, banco, roles ou JWT.
- Nao customizar CSRF, sessao, CORS ou tratamento de erro alem do padrao.
- Nao introduzir DTOs ou camadas de servico para um retorno estatico.

## Decisions

- **1 classe `SecurityConfig` com `@Configuration` + `@EnableWebSecurity` expondo `@Bean SecurityFilterChain`**
  Rationale: ponto unico de decisao de seguranca (SRP, DIP — controller nao conhece seguranca). Alternativa considerada: multiplos FilterChains com `@Order` — descartada por ser complexidade prematura para 2 regras.
- **Sintaxe lambda `authorizeHttpRequests(auth -> auth.requestMatchers(GET, "/api/message").permitAll().anyRequest().authenticated())` + `formLogin`/`httpBasic` defaults**
  Rationale: sintaxe atual suportada no Boot 4.x, legivel para aprendizado. Alternativa: desabilitar tudo e deixar stateless — descartada porque adicionaria conceitos (csrf, session) antes da hora.
- **`controller/MyController` com `@RestController` + `@GetMapping("/api/message")` retornando `String`**
  Rationale: menor superficie para o passo 1; OCP — novos endpoints entram sem tocar na config existente, so adicionando matchers. Alternativa: retornar `ResponseEntity`/`record` JSON — adiado para quando houver contrato real.
- **Pacotes `...config` e `...controller`**
  Rationale: prepara crescimento (`service`, `security/jwt`) sem reorganizacao. Alternativa: tudo no pacote raiz — descartada por violar SRP na organizacao.

## Risks / Trade-offs

- [Risco] Manter `formLogin` ativo pode confundir quem testa via `curl` esperando so 401 JSON → Mitigacao: documentar nos tasks como testar com `-u` (httpBasic) e via browser.
- [Risco] `anyRequest().authenticated()` parece amplo demais → Mitigacao: e intencional como default-deny didatico; proximo passo refina por metodo/rotait com testes.
- [Trade-off] Retornar `String` pura nao e padrao REST final → Aceito para passo 1; evolucao para JSON nao quebra a regra de seguranca.

## Migration Plan

- Mudanca aditiva, sem migracao: nenhuma API previa contratada.
- Rollback: remover `SecurityConfig` volta ao default trancado; remover `MyController` remove o endpoint publico.
- Validacao: `GET /api/message` → 200 anonimo; `GET /qualquer-outra` → 401/redirect anonimo.

## Open Questions

- Nenhuma bloqueante. Questao deferivel: no passo 2 preferir `httpBasic` puro + csrf disabled para API stateless ou manter `formLogin` para aprendizado via browser?
