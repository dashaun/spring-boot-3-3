## Support for Base64 resources in property (and YAML) files

---

### Before 3.3

Values that are multiple lines can be cumbersome to configure in yaml or properties files.

```yaml
spring:
  ssl:
    bundle:
      pem:
        mybundle:
          keystore:
            certificate: |
                -----BEGIN CERTIFICATE-----
                MIIFaDCCBFCgAwIBAgISESHkvZFwK9Qz0KsXD3x8p44aMA0GCSqGSIb3DQEBCwUA
                VQQDDBcqLmF3cy10ZXN0LnByb2dyZXNzLmNvbTCCASIwDQYJKoZIhvcNAQEBBQAD
                ggEPADCCAQoCggEBAMGPTyynn77hqcYnjWsMwOZDzdhVFY93s2OJntMbuKTHn39B
                ...
                bml6YXRpb252YWxzaGEyZzIuY3JsMIGgBggrBgEFBQcBAQSBkzCBkDBNBggrBgEF
                BQcwAoZBaHR0cDovL3NlY3VyZS5nbG9iYWxzaWduLmNvbS9jYWNlcnQvZ3Nvcmdh
                bml6YXRpb252YWxzaGEyZzJyMS5jcnQwPwYIKwYBBQUHMAGGM2h0dHA6Ly9vY3Nw
                lffygD5IymCSuuDim4qB/9bh7oi37h
                -----END CERTIFICATE-----
```

---

### But / Because

Base64 encoding the value flattens the value into one line vs multiple.

Improves usability in diffing tools if storing configuration in git.

---

### Therefore

Resources can now be loaded as Base64 encoded text values using a base64: prefix. This can be useful with resources such as SSL certificates.

---

### Demo?


Base64 Resources


```yaml
spring:
  ssl:
    bundle:
      pem:
        mybundle:
          keystore:
            certificate: "base64:LS0tLS1CRUdJTi..."
            private-key: "base64:QmFnIEF0dHJpYn..."
```