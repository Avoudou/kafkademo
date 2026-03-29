# Document processing Kafka demo

Four Spring Boot apps (upload, manager, processor, dashboard) over Apache Kafka and MongoDB.

**Repos:**  
Root (README, JARs, docker-compose): [kafkademo](https://github.com/Avoudou/kafkademo)  
Projects: [docUpload](https://github.com/Avoudou/kafkademo-docUpload) · [docManager](https://github.com/Avoudou/kafkademo-docManager) · [docProcessor](https://github.com/Avoudou/kafkademo-docProcessor) · [dashboard](https://github.com/Avoudou/kafkademo-dashboard)

**Prerequisites:** Docker Desktop. The four JARs are built from the project repos above.

**Run everything with one command:**

```bash
docker-compose up --build
```

This starts Kafka, MongoDB, and all four services in containers. To stop: `docker-compose down`.

| Service | Port |
|---------|------|
| docapi (upload) | 8080 |
| doc-manager | 8081 |
| docprocessor | 8082 |
| kafka-dashboard | 8083 |

**Open in browser:** Upload UI — http://localhost:8080 · The manager (doc-manager.jar) runs on http://localhost:8081, just display the service health - backend only. · Processor dashboard (tracks backend mock processes  with random delay) — http://localhost:8082 · Flow dashboard (message flow across topics) — http://localhost:8083. 

The upload API accepts **Excel spreadsheets only** (`.xlsx`, e.g. from Google Sheets export). On the upload page, in the **TEST** section: set **Count** (default 10), click **Test multiple uploads** to create that many random test documents and upload them, or **Clear database** to reset stored data.

**Flow:** You upload a document (or trigger test uploads); the upload API sends a Kafka message to a topic. The manager consumes it and publishes an appropriate message for processing. The processor is notified, runs an async mock process (3–10 s), then publishes to a common completion topic. The processor has its own dashboard to track processes; the root dashboard shows message flow across all topics.
