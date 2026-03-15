# Document processing Kafka demo

**Repos:**  
Root (README, JARs, docker-compose): [kafkademo](https://github.com/Avoudou/kafkademo)  
Projects: [docUpload](https://github.com/Avoudou/kafkademo-docUpload) · [docManager](https://github.com/Avoudou/kafkademo-docManager) · [docProcessor](https://github.com/Avoudou/kafkademo-docProcessor) · [dashboard](https://github.com/Avoudou/kafkademo-dashboard)


**1. Run infrastructure**

```bash
docker-compose up -d
```

(Mongo on 27017, Kafka on 9092.)

**2. Run the four JARs** (each in its own terminal, from this folder)

| JAR | Port |
|-----|------|
| docapi.jar | 8080 |
| doc-manager.jar | 8081 |
| docprocessor.jar | 8082 |
| kafka-dashboard.jar | 8083 |

```bash
java -jar docapi.jar
java -jar doc-manager.jar
java -jar docprocessor.jar
java -jar kafka-dashboard.jar
```
