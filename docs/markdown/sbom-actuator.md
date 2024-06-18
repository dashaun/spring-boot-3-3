## Support for SBOMs with the new SBOM actuator endpoint

---
The new SBOM actuator endpoint allows you to access SBOM (Software Bill of Materials) for a Spring Boot application. The SBOM provides detailed information about the components used to build the artifact, including precise identification of dependencies in your application. A Spring Boot app can add the CycloneDX maven/gradle plugin, which uses information from the dependency management system. The SBOM actuator endpoint will serve the SBOM when the application runs.

### Before 3.3

---
Before Spring Boot 3.3, applications could generate an SBOM, but there was no out-of-the-box way to make it available at runtime.

### But / Because

---
Exposing the SBOM through an actuator endpoint allows one to discover all of the application dependencies similarly to observing other actuator endpoints.

### Therefore

---
Discovering applications that have exposed SBOM endpoints can allow the operations team to build a process that will crawl all SBOM actuators in production. The collected SBOMs could then be scanned with a tool such as [osv.dev](https://osv.dev/) to quickly identify running application vulnerabilities without scanning the entire artifact. If scanning the whole artifact (jar or image) is performed, the jar manifest will include the location of the SBOM so the scanner can include it in its findings.

### Demo?

- [Blog Post](https://spring.io/blog/2024/05/24/sbom-support-in-spring-boot-3-3)
- [osv.dev cli](https://github.com/google/osv-scanner/releases/tag/v1.7.4)

After following the blog above a user could perform a Gradle build.
```
./gradlew build
```

Use the osv.dev cli to scan the generated SBOM for vulnerabilities.
```
osv scan build/reports/application.cdx.json
```

Start the Spring Boot app

```
java -jar build/libs/demo-sbom-0.0.1-SNAPSHOT.jar
```

View the SBOM via the actuator endpoint http://localhost:8080/actuator/sbom/application

```
curl http://localhost:8080/actuator/sbom/application
```


