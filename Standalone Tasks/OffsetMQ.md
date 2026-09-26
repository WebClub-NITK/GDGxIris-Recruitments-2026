# Task ID: OffsetMQ

#### `Backend Engineering`, `Distributed Systems`, `Message Brokers`, `Event Streaming`

Mentor: Appaji Dheeraj

Difficulty: `Medium-Hard`

---

## Overview

Build **OffsetMQ**, a lightweight event-streaming platform inspired by Apache Kafka. Producers publish events to partitioned topics, while consumers read them independently at their own pace.

Unlike a basic WebSocket broadcaster, OffsetMQ must persist messages, preserve their order within a partition, track consumer progress, and support replay. The baseline uses a **single broker process**. A CLI and documented HTTP, TCP, or gRPC API are sufficient; a frontend is not required.

## Required Features

### 1. Topics, Partitions, and Producers

- Create and list topics containing one or more partitions.
- Assign every message a monotonically increasing offset within its partition.
- Preserve order within a partition; global ordering is not required.
- Support concurrent producers.
- Route messages with the same key to the same partition using a stable strategy.
- Distribute unkeyed messages using round-robin or another documented strategy.
- Acknowledge a message only after accepting it into durable storage.

Each message must contain its topic, payload, timestamp, optional key, partition, and offset.

### 2. Durable Storage

- Persist each partition as an append-only log.
- Recover acknowledged messages and offsets after a broker restart.
- Ensure a partially written final record cannot corrupt earlier records.
- Document the storage format and durability trade-offs.

Files, an embedded database, or another self-managed mechanism may be used. Existing brokers such as Kafka, RabbitMQ, NATS, Redis Streams, or Pulsar may not be used as the storage or delivery engine.

### 3. Consumers and Offsets

- Read a partition beginning at a supplied offset.
- Return messages in ascending offset order.
- Store committed consumer offsets durably.
- Resume from the last committed offset after a restart.
- Replay from the beginning, latest position, or a specific valid offset.

The baseline guarantee is **at-least-once delivery**. Candidates must document when duplicate processing can occur.

### 4. Failure Handling

- Validate topics, partitions, offsets, payload sizes, and malformed requests.
- Return clear errors without crashing the broker.
- Limit the number or total size of messages returned per request.
- Log broker startup, recovery, and failed writes.

### 5. Verification

Include a repeatable integration or stress-test program that:

- Publishes at least **1,000 messages** across three or more partitions.
- Verifies continuous offsets, per-partition ordering, and stable key routing.
- Commits a consumer offset, restarts the consumer, and resumes correctly.
- Restarts the broker and verifies that acknowledged messages remain available.
- Resets an offset and successfully replays older messages.

The test output must clearly report which guarantees passed or failed.

## Bonus Features

Implementing any **two** makes the task `Hard`:

1. **Consumer Groups** — Share partitions between consumers and rebalance when membership changes.
2. **Retention Policies** — Delete messages based on age or stored size without corrupting offsets.
3. **Log Compaction** — Retain only the latest value for each message key.
4. **Idempotent Producers** — Prevent retries from appending the same logical message twice.
5. **Dead-Letter Topics** — Store rejected messages with failure metadata.
6. **Schema Validation** — Associate a versioned schema with a topic.
7. **Multi-Broker Replication** — Replicate partitions and survive a broker failure.

Multi-broker consensus, exactly-once processing, and distributed transactions are not baseline requirements.

## Deliverables

1. **Source Code**
   - Broker implementation and producer/consumer clients or CLI commands.
   - Appropriate `.gitignore` and clear project structure.

2. **Tests**
   - Repeatable integration/stress-test program covering the required guarantees.

3. **Documentation**
   - Architecture and API/protocol.
   - Topic, partition, offset, and storage model.
   - Delivery guarantee, recovery behaviour, setup instructions, and known limitations.

4. **Demo Video**
   - Demonstrate publishing, consuming, offset commits, restart recovery, and replay.

## Evaluation Criteria

- Correct message ordering, offsets, partition routing, commits, and replay.
- Safe concurrent producers without corrupted logs or offsets.
- Reliable recovery after broker and consumer restarts.
- Bounded memory usage and clear error handling.
- Repeatable tests and clear documentation of trade-offs.

## Learning Outcomes

- Topics, partitions, offsets, and append-only logs.
- Durable event streaming versus transient pub/sub.
- At-least-once delivery, committed offsets, and replay.
- Testing concurrent systems through controlled failures.

## Resources

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Kafka Design: The Log](https://kafka.apache.org/documentation/#design_log)
- [Kafka Consumer Concepts](https://docs.confluent.io/platform/current/clients/consumer.html)
- [gRPC Documentation](https://grpc.io/docs/)

Start with one topic, one partition, one producer, and one consumer. Make the log durable before adding consumer groups or bonus features.
