## Why

Este projeto e um ambiente de aprendizado de Spring Security em pequenos passos. Hoje qualquer aplicacao sobe com o comportamento padrao do Spring (tudo trancado, senha gerada no log), sem nenhum endpoint publico documentado para experimentar. Precisamos de um baseline minimo e explicito: um endpoint publico para validar que a app funciona e uma regra clara de que todo o resto exige autenticacao.

## What Changes

- Adiciona `SecurityConfig` com um `@Bean SecurityFilterChain` explicito usando a sintaxe lambda `authorizeHttpRequests`.
- Libera `GET /api/message` com `permitAll` retornando `"It works"`.
- Exige autenticacao para qualquer outra requisicao (`anyRequest().authenticated()`).
- Mantem os mecanismos padrao (formLogin + httpBasic) neste passo para nao introduzir CSRF, sessao stateless ou JWT ainda.
- Adiciona `MyController` simples com `@RestController` e `@GetMapping("/api/message")`, sem logica de negocio.

## Capabilities

### New Capabilities
- `security-baseline`: baseline de seguranca HTTP — endpoint publico de verificacao e exigencia de autenticacao para o restante, com separacao entre configuracao de seguranca e controller (SRP).

### Modified Capabilities
- (nenhuma — projeto sem specs previas)

## Impact

- Codigo novo: `config/SecurityConfig.java`, `controller/MyController.java` sob `academy.jairo.springsecuritydemo`.
- Nenhuma mudanca de dependencia (ja usa `spring-boot-starter-security` + `webmvc` no Boot 4.1.1, Java 21).
- Comportamento: antes tudo bloqueado pelo default implicito; depois `/api/message` publico e resto autenticado de forma explicita e testavel.
- Sem BREAKING externo, pois nao ha API previa contratada.
