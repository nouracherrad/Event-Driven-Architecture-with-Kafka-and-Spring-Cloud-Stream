

```markdown
# 🧩 Event-Driven Architecture with Kafka and Spring Cloud Stream

## 📖 Description du projet
Ce projet illustre la mise en place d’une **architecture pilotée par les événements (Event-Driven Architecture)** en utilisant **Apache Kafka** et **Spring Cloud Stream**.  
Il a été réalisé dans le cadre de l’**Activité Pratique n°1** du cours de Mohamed Youssfi, visant à comprendre la communication asynchrone entre microservices, le traitement temps réel et l’analyse de flux de données.

## ⚙️ Objectifs pédagogiques
- Comprendre le fonctionnement de **Kafka** et de **Zookeeper**.
- Créer un **Producer**, un **Consumer** et un **Supplier** Kafka avec Spring Cloud Stream.
- Mettre en œuvre un **pipeline de traitement de flux** avec Kafka Streams.
- Visualiser les résultats de l’analyse de flux en temps réel via une interface Web.

## 🧱 Architecture du projet
```

+-------------------------+
|  PageEventSupplier      |  --> Produit des événements (T3)
+-------------------------+
↓
+-------------------------+
|  KStreamFunction        |  --> Filtre, groupe et compte les événements (T4)
+-------------------------+
↓
+-------------------------+
|  PageEventConsumer      |  --> Consomme et affiche les événements filtrés
+-------------------------+
↓
+-------------------------+
|  PageEventController    |  --> Expose les endpoints REST et analytics
+-------------------------+
↓
+-------------------------+
|  HTML + Smoothie.js     |  --> Visualisation temps réel
+-------------------------+

````

## 🧩 Technologies utilisées
- **Java 17**
- **Spring Boot 3.x**
- **Spring Cloud Stream**
- **Apache Kafka**
- **Kafka Streams API**
- **Docker / Docker Compose**
- **HTML5 / JavaScript (Smoothie.js)**

## 🐳 Démarrage avec Docker
1. Créer et lancer les conteneurs Kafka et Zookeeper :
```bash
docker-compose up -d
````

2. Vérifier que les conteneurs sont en cours d’exécution :

```bash
docker ps
```

Tu devrais voir :

* `bdcc-zookeeper`
* `bdcc-kafla-broker`

## 🚀 Exécution du projet Spring Boot

1. Ouvre le projet dans **IntelliJ IDEA**.
2. Lance l’application Spring Boot (`KafkaSpringCloudStreamApplication`).
3. Accède aux endpoints :

   * **Générer un événement :**

     ```
     http://localhost:8080/publish?name=Page1&topic=T3
     ```
   * **Visualiser les analyses en temps réel :**
     Ouvre le fichier **analytics.html** dans ton navigateur pour voir le graphe dynamique.

## 📡 Configuration Kafka Streams

Dans `application.properties` :

```properties
spring.application.name=kafka-spring-cloud-stream
server.port=8080

spring.cloud.stream.bindings.pageEventSupplier-out-0.destination=T3
spring.cloud.stream.bindings.kStreamFunction-in-0.destination=T3
spring.cloud.stream.bindings.kStreamFunction-out-0.destination=T4
spring.cloud.stream.bindings.pageEventConsumer-in-0.destination=T2
spring.cloud.function.definition=pageEventConsumer;pageEventSupplier;kStreamFunction
spring.cloud.stream.bindings.pageEventSupplier-out-0.producer.poller.fixed-delay=200
spring.cloud.stream.kafka.streams.binder.configuration.commit.interval.ms=1
```

## 📝 Explications du projet

### 1. Mise en place de l’environnement Kafka

* Installation et configuration de **Zookeeper** et **Kafka Broker**.
* Deux options utilisées :

  * **Localement**, via les binaires Kafka pour tester les commandes `kafka-console-producer` et `kafka-console-consumer`.
  * **Avec Docker**, en utilisant `docker-compose.yml`.

### 2. Création des services Kafka avec Spring Cloud Stream

* **Producer (`pageEventSupplier`)** : génère des événements `PageEvent` et les publie sur le topic `T3`.
* **Consumer (`pageEventConsumer`)** : consomme les événements depuis le topic `T2` et les affiche.
* **KStream Function (`kStreamFunction`)** : filtre, groupe et compte les événements par page sur des fenêtres de 5 secondes, puis les envoie sur le topic `T4`.

### 3. Contrôleur REST (`PageEventController`)

* Endpoint `/publish` : permet de pousser un événement sur un topic Kafka depuis le navigateur ou Postman.
* Endpoint `/analytics` : fournit les données d’analyse en **temps réel** via **SSE** (Server-Sent Events).
* Utilisation de **InteractiveQueryService** pour interroger le **state store** de Kafka Streams et récupérer les comptes par page.

### 4. Visualisation Web

* Fichier **`analytics.html`** utilisant **Smoothie.js** pour tracer un **graphe dynamique** des événements en temps réel.

### 5. Résultat final

Après avoir démarré les conteneurs Docker et l’application Spring Boot :

* Les événements sont publiés automatiquement par `pageEventSupplier`.
* Les événements filtrés et agrégés apparaissent dans la console via `pageEventConsumer`.
* Le graphe dynamique affiche en temps réel le nombre d’événements par page.

**📸 Screen du résultat final :**
*(À ajouter ici après capture de l’écran)*

```

---

Si tu veux, je peux te faire une **version améliorée avec badges GitHub, Maven, Docker et Java**, ce qui rendra ton README encore plus attractif pour GitHub.  

Veux‑tu que je fasse ça ?
```
