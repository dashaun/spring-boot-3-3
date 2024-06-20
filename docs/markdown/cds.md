## CDS Support for improved startup times and reduced memory consumption

---

### Before 3.3

Startup times have been improving

---

### But / Because

But we are still paying per ms in the cloud and latency is the new downtime

Not all of my workloads are ready or appropriate for native images

---

### Therefore

CDS is the new flag that should be enabled, to squeeze off a few more milliseconds from your startup time

It's already available in the Paketo Java Buildpack
(Friends don't let friends use Dockerfile)

---

### Quick startup time demo

Create an empty directory:

```bash
mkdir -p demos/cds
cd demos/cds
```

In an empty directory download the starter:

```bash
curl -G https://start.spring.io/starter.tgz \
  -d dependencies=web,actuator \
  -d type-gradle-project \
  -d javaVersion=21 | tar -xzvf -
```


Edit [build.gradle](../../demos/cds/build.gradle) to customize the `bootBuildImage` task:

```groovy
tasks.named('bootBuildImage') {
	environment["BP_JVM_CDS_ENABLED"] = "true"
	environment["BP_JVM_VERSION"] = "21"
}
```

Build the Image:

```bash
./gradlew bootImage
```

Run the image:

```bash
docker run -p 8080:8080 docker.io/library/demo:0.0.1-SNAPSHOT
```

Notice the inclusion of `Spring CDS Enabled` log line which will add the flag `-XX:SharedArchiveFile=application.jsa` to the `JAVA_TOOL_OPTIONS`:

```logs
Spring CDS Enabled, contributing -XX:SharedArchiveFile=application.jsa to JAVA_TOOL_OPTIONS
Picked up JAVA_TOOL_OPTIONS: -Djava.security.properties=/layers/paketo-buildpacks_bellsoft-liberica/java-security-properties/java-security.properties -XX:+ExitOnOutOfMemoryError -XX:MaxDirectMemorySize=10M -Xmx5305712K -XX:MaxMetaspaceSize=82603K -XX:ReservedCodeCacheSize=240M -Xss1M -XX:+UnlockDiagnosticVMOptions -XX:NativeMemoryTracking=summary -XX:+PrintNMTStatistics -XX:SharedArchiveFile=application.jsa -Dorg.springframework.cloud.bindings.boot.enable=true
```