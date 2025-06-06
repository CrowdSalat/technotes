# Spring security

## add custom authentication

- implement AuthenticationProvider in you own class
- and register it via
    - 

- nice example: https://github.com/dewantrie/springboot-custom-authentication-provider/blob/master/src/main/java/com/example/demo/config/SecurityConfig.java


- Authentication
- AuthenticationProvider
- AuthenticationManager
- UserDetailsService
- UserDetails
- SecurityFilterChain

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
