## SSL SNI support for embedded web servers

---

### Before 3.3

Difficult to configure SSL + SNI support for embedded web servers

---

### But / Because

For use cases when SSL termination must take place at the application server with SNI enabled

---

### Therefore

Expose property configuration to allow configuring mutliple bundles by hostname

---

### Demo

```yaml
server:
  port: 8443
  ssl:
    bundle: "web"
    server-name-bundles:
      - server-name: "alt1.example.com"
        bundle: "web-alt1"
      - server-name: "alt2.example.com"
        bundle: "web-alt2"
```