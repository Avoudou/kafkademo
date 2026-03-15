# Document processing Kafka demo

Four Spring Boot apps (upload, manager, processor, dashboard) over Apache Kafka and MongoDB.

**Repos:**  
Root (README, JARs, docker-compose): [kafkademo](https://github.com/Avoudou/kafkademo)  
Projects: [docUpload](https://github.com/Avoudou/kafkademo-docUpload) · [docManager](https://github.com/Avoudou/kafkademo-docManager) · [docProcessor](https://github.com/Avoudou/kafkademo-docProcessor) · [dashboard](https://github.com/Avoudou/kafkademo-dashboard)

**Prerequisites:** Docker Desktop, Java 21. The four JARs are built from the project repos above.

**1. Run infrastructure**

```bash
docker-compose up -d
```

(Mongo on 27017, Kafka on 9092.) To stop: `docker-compose down`.

**2. Run the four JARs** (each in its own terminal, from this folder)

| JAR | Port |
|-----|------|
| docapi.jar | 8080 |
| doc-manager.jar | 8081 |
| docprocessor.jar | 8082 |
| kafka-dashboard.jar | 8083 |

```bash
java -jar docapi.jar
```

```bash
java -jar doc-manager.jar
```

```bash
java -jar docprocessor.jar
```

```bash
java -jar kafka-dashboard.jar
```

**3. Open in browser:** Upload UI — http://localhost:8080 · Processor dashboard (track processes) — http://localhost:8082 · Flow dashboard (message flow across topics) — http://localhost:8083. The manager (doc-manager.jar) runs on 8081 and has no UI; it is a backend service only.  

The upload API accepts **Excel spreadsheets only** (`.xlsx`, e.g. from Google Sheets export). On the upload page, in the **TEST** section: set **Count** (default 10), click **Test multiple uploads** to create that many test uploads and drive the pipeline, or **Clear database** to reset stored data.

**Flow:** You upload a document (or trigger test uploads); the upload API sends a Kafka message to a topic. The manager consumes it and publishes an appropriate message for processing. The processor is notified, runs an async mock process (3–10 s), then publishes to a common completion topic. The processor has its own dashboard to track processes; the root dashboard shows message flow across all topics.
