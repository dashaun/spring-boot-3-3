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

Create an empty directory:

```bash
mkdir -p demos/sbom-actuator
cd demos/sbom-actuator
```

In an empty directory download the starter:

```bash
curl -G https://start.spring.io/starter.tgz \
  -d dependencies=web,actuator \
  -d type-gradle-project \
  -d javaVersion=21 | tar -xzvf -
```

Edit [build.gradle](../../demos/sbom-actuator/build.gradle) to add the CycloneDX gradle plugin:

```groovy
plugins {
  id 'org.cyclonedx.bom' version '1.8.2'
}
```

Edit the [application.properties](../../demos/sbom-actuator/src/main/resources/application.properties):

```properties
management.endpoints.web.exposure.include=health,sbom
```

Perform the gradle build

```bash
sdk use java 21.0.3-ms
./gradlew build
```

Start the Spring Boot app

```bash
java -jar build/libs/demo-0.0.1-SNAPSHOT.jar
```

Discover available SBOMs via the actuator endpoint http://localhost:8080/actuator/sbom

```bash
curl -i http://localhost:8080/actuator/sbom
```

Should return a json response with the list of SBOM ids.

```json
HTTP/1.1 200 
Content-Type: application/vnd.spring-boot.actuator.v3+json
Transfer-Encoding: chunked
Date: Wed, 19 Jun 2024 16:02:05 GMT

{"ids":["application"]}
```

View the SBOM via the actuator endpoint http://localhost:8080/actuator/sbom/application

```bash
curl -i http://localhost:8080/actuator/sbom/application
```

Use the osv.dev cli to scan the generated SBOM for vulnerabilities.

```bash
osv scan build/reports/application.cdx.json
```
