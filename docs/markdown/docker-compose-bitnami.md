## Docker Compose support for Bitnami Container Images

---

### Before 3.3

Docker Compose support was introduced in Spring Boot 3.1

---

### But / Because

Expanded images supported out of the box

---

### Therefore

---

Bitnami give developers 

- SLSA level 3 compliant build provenance
- SBOM metadata in multiple formats

---

### Demo?

---

Create an empty directory

```bash
mkdir -p demos/bitnami
cd demos/bitnami
```

---

In an empty directory download the starter

```bash
curl -G https://start.spring.io/starter.tgz \
  -d dependencies=web,docker-compose,data-jdbc,flyway,postgresql \
  -d type-gradle-project \
  -d javaVersion=21 | tar -xzvf -
```

---

`build.gradle` will contain additional dependencies

- `spring-boot-docker-compose` 
- `postgresql` 

---

`compose.yaml` docker compose config for postgres

By default this will use OSS postgres image

---

Run the application

```bash
sdk use java 21.0.3-ms
./gradlew bootRun
```

Spring boot docker compose will detect the `compose.yaml`

```log
.s.b.d.c.l.DockerComposeLifecycleManager : Using Docker Compose file '/workspaces/spring-boot-3-3/demos/bitnami/compose.yaml'
o.s.boot.docker.compose.core.DockerCli   :  postgres Pulling 
...
o.s.boot.docker.compose.core.DockerCli   :  postgres Pulled 
o.s.boot.docker.compose.core.DockerCli   :  Network bitnami_default  Creating
o.s.boot.docker.compose.core.DockerCli   :  Network bitnami_default  Created
o.s.boot.docker.compose.core.DockerCli   :  Container bitnami-postgres-1  Creating
```

---

Update `compose.yaml` to use the bitnami image 

```yaml
services:
  postgres:
    image: 'bitnami/postgresql:13.14.0-debian-11-r15'
...
```

---

Run the application

```
./gradlew bootRun
```

Notice the image changes to the Bitnami Postgres Image.

---

- Builds on prior work in `spring-boot-docker-compose` and service connections
- Bitnami in addition to official images for several of the supported technologies
  - Cassandra, Elasticsearch, MariaDB, MySQL, MongoDB, Neo4j, PostgreSQL, RabbitMQ, Redis