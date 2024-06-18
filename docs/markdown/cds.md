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