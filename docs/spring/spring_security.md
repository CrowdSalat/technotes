# Spring security

## Configure exception for auth filter

Disable for:

- for actuator endpoints
- swagger-ui

```java 
@Configuration
@EnableWebSecurity
public class WebSecurityConfiguration extends AbstractSecurityWebApplicationInitializer {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
       return http
             .authorizeHttpRequests(authz ->
                   authz
                        // actuator
                         .requestMatchers(antMatcher("actuator/**")).permitAll()
                         //swagger
                         .requestMatchers(antMatcher("/swagger-ui/**")).permitAll()
                         .requestMatchers(antMatcher("/v3/api-docs/**")).permitAll()
             )
             // disable CSRF 
             .csrf(AbstractHttpConfigurer::disable)
             .build();
    }
}
 
```
