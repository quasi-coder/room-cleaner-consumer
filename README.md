# Room Cleaner Consumer

A Spring Boot application that acts as a RabbitMQ consumer, listening for room cleaning requests published to a queue and processing them by logging the room details.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Spring Boot | Application framework |
| Spring AMQP (RabbitMQ) | Message queue consumer |
| Jackson | JSON deserialization |
| SLF4J | Logging |
| Maven Wrapper | Build tool |

---

## Project Structure

```
room-cleaner-consumer/
└── src/main/java/com/dwiveddi/boot/
    ├── RoomCleanerConsumerApplication.java  # App entry + AMQP beans (Queue, Exchange, Binding)
    ├── RoomCleanerProcessor.java            # Message handler — deserializes Room JSON and logs it
    └── Room.java                            # Room model (id, name, number, info)
└── src/main/resources/
    └── application.properties              # Queue and exchange names
```

---

## How It Works

1. **Queue** — Binds to `room.cleaner` queue on the `quasi-rooms-exchange` topic exchange
2. **Listener** — `SimpleMessageListenerContainer` delegates to `RoomCleanerProcessor.receiveMessage()`
3. **Processing** — Deserializes the incoming JSON into a `Room` object and logs: `"Room ready for cleaning <roomNumber>"`

---

## Configuration

```properties
amqp.queue.name=room.cleaner
amqp.exchange.name=quasi-rooms-exchange
```

---

## Getting Started

1. **Start RabbitMQ**
   ```bash
   docker run -d -p 5672:5672 -p 15672:15672 rabbitmq:management
   ```

2. **Run the consumer**
   ```bash
   ./mvnw clean spring-boot:run
   ```

3. **Send a test message** using the `room-clr-app` producer or RabbitMQ management UI at `http://localhost:15672`

**Works together with:** `room-clr-app` (producer) and `room-web-app` (web interface)
