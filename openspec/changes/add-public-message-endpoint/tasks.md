## 1. Configuracao de seguranca (SRP: so decide acesso)

- [x] 1.1 Criar `config/SecurityConfig` com `@Configuration` + `@EnableWebSecurity` e `@Bean SecurityFilterChain` usando `authorizeHttpRequests` com `requestMatchers(GET, "/api/message").permitAll()` e `anyRequest().authenticated()`, mantendo defaults, e verificar que a aplicacao sobe sem erro de contexto
- [x] 1.2 Revisar a config explicando cada linha (o que e FilterChain, matcher, permitAll vs authenticated) e verificar respondendo o que cada regra faz

## 2. Controller publico (SRP: so responde)

- [x] 2.1 Criar `controller/MyController` com `@RestController` e `@GetMapping("/api/message")` retornando `"It works"` e verificar `GET /api/message` anonimo retorna 200 `"It works"`
- [x] 2.2 Verificar que `GET /qualquer-outra-rota` anonimo retorna 401 ou redirect para login, provando o `anyRequest().authenticated()`

## 3. Validacao de aprendizado

- [x] 3.1 Testar manualmente com `curl` os dois casos (publico 200, resto 401) e verificar saidas documentadas
- [x] 3.2 Rodar `./mvnw test` (ao menos `contextLoads`) e verificar build verde, discutindo proximo passo (httpBasic puro vs formLogin, CSRF, testes MockMvc)
