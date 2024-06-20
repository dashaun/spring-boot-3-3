## Spring Security improvements

---

### Before 3.3

Spring Security version was 6.2.x

---

### But / Because

New features available in 6.3.x of Spring Security

---

### Therefore

Annotated Support for Securing Return Values for POJOs

```java
public class Order {

	@HasScope("payment:read")
	Payment getPayment() { ... }

}
```

---

`@HandleAuthorizationDenied` annotation

```java
public class Payment {
    @HandleAuthorizationDenied(handlerClass=Mask.class)
    @PreAuthorize("hasAuthority('card:read')")
    public String getCreditCardNumber() { ... }
}
```

---

`CompromisedPasswordChecker` bean support

```java
@Bean
public CompromisedPasswordChecker compromisedPasswordChecker() {
    return new HaveIBeenPwnedRestApiPasswordChecker();
}
```

---

Spring Boot

`JwtAuthenticationConverter` or `ReactiveJwtAuthenticationConverter` is now auto-configured when one of these properties is set:
```conf
spring.security.oauth2.resourceserver.jwt.authority-prefix
spring.security.oauth2.resourceserver.jwt.principal-claim-name
spring.security.oauth2.resourceserver.jwt.authorities-claim-name
```

---

### Demo

Watch for a meetup presention announcement focused exclusively on Spring Security 6.3.x 