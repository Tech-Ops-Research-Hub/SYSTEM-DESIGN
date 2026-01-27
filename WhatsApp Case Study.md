# 📘 WhatsApp System Design — Mermaid Diagrams

---

## 1. High-Level Architecture

```mermaid
graph TD
    A[Mobile Client] --> B[API Gateway]
    B --> C[Auth Service]
    B --> D[Messaging Service]
    B --> E[Media Service]
    B --> F[Notification Service]

    D --> G[Message Queue]
    G --> H[Message Store]

    E --> I[Media Storage]
    F --> J[Push Notification Service]

    D --> K[Presence Service]
    K --> L[Redis Cache]
```

---

## 2. Message Sending Flow (Text Message)

```mermaid
sequenceDiagram
    participant U1 as Sender
    participant API as API Gateway
    participant MSG as Messaging Service
    participant MQ as Message Queue
    participant DB as Message Store
    participant U2 as Receiver

    U1->>API: Send Message
    API->>MSG: Validate & Route
    MSG->>MQ: Publish Message
    MQ->>DB: Persist Message
    MSG->>U2: Deliver Message
```

---

## 3. Message Delivery with Offline Support

```mermaid
sequenceDiagram
    participant User
    participant Server
    participant Queue
    participant Storage

    User->>Server: Send Message
    Server->>Queue: Push Message
    Queue->>Storage: Store Message

    alt Receiver Online
        Server->>User: Deliver Instantly
    else Receiver Offline
        Storage->>Server: Hold Message
        Server->>User: Deliver When Online
    end
```

---

## 4. Media Upload & Delivery Flow

```mermaid
sequenceDiagram
    participant User
    participant API
    participant MediaService
    participant ObjectStorage
    participant CDN

    User->>API: Upload Media
    API->>MediaService: Validate
    MediaService->>ObjectStorage: Store File
    ObjectStorage->>CDN: Distribute
    CDN->>User: Stream Media
```

---

## 5. Presence System (Online / Offline)

```mermaid
graph TD
    A[User App] --> B[Presence Service]
    B --> C[Redis]
    C --> D[Last Seen Data]

    B --> E[Contacts]
```

---

## 6. End-to-End Encryption Flow

```mermaid
sequenceDiagram
    participant Sender
    participant Server
    participant Receiver

    Sender->>Sender: Encrypt Message
    Sender->>Server: Send Encrypted Message
    Server->>Receiver: Forward Ciphertext
    Receiver->>Receiver: Decrypt Message
```

✔ Server never reads message content
✔ Keys exist only on user devices

---

## 7. Group Messaging Architecture

```mermaid
graph TD
    A[User] --> B[Group Service]
    B --> C[Group Metadata]
    B --> D[Message Fanout]
    D --> E[User 1]
    D --> F[User 2]
    D --> G[User 3]
```

---

## 8. Scalability Architecture

```mermaid
graph TD
    LB[Load Balancer]
    LB --> S1[App Server]
    LB --> S2[App Server]
    LB --> S3[App Server]

    S1 --> Cache[Redis]
    S2 --> Cache
    S3 --> Cache

    Cache --> DB[(Database)]
```

---

## 9. Failure Handling Design

```mermaid
graph TD
    A[Client] --> B[API]
    B -->|Failure| C[Retry Queue]
    C --> D[Fallback Service]
    D --> E[User Notification]
```

---

## 10. WhatsApp System Design Summary

| Component         | Purpose            |
| ----------------- | ------------------ |
| API Gateway       | Entry point        |
| Messaging Service | Message routing    |
| Queue             | Async processing   |
| Redis             | Presence + caching |
| Object Storage    | Media              |
| CDN               | Fast delivery      |
| Encryption        | Privacy            |
| Load Balancer     | Scaling            |

---

## 11. Key Design Principles Used

✔ Event-driven architecture
✔ Asynchronous processing
✔ Horizontal scalability
✔ Fault tolerance
✔ Low-latency delivery
✔ Strong encryption
✔ Geo-distributed systems

---

## 12. Interview-Ready Talking Points

* Why WhatsApp uses **message queues**
* How **fan-out** works for groups
* Why **Redis** is critical
* Why **CDN** is needed
* How WhatsApp achieves **low latency**
* How **end-to-end encryption** works
* Handling **millions of concurrent users**

---

## 13. Final Note

This architecture supports:

* Billions of messages/day
* Millions of concurrent users
* Near-zero latency delivery
* High fault tolerance
* Secure communication

---
