---
description: 'Add messaging — Kafka, RabbitMQ, or SQS integration with producers, consumers, error handling, and DLQ'
mode: agent
---

# Add Messaging

Add asynchronous messaging to this Java application.

## Step 1: Scan Current State

1. Check for existing messaging dependencies (Kafka, RabbitMQ, ActiveMQ, SQS)
2. Check for `@KafkaListener`, `@RabbitListener`, or JMS annotations
3. Check for Spring Cloud Stream dependencies
4. Check application.yml for messaging configuration

## Step 2: Determine Messaging Needs

Ask the user (if not specified):
- What messages need to be sent/received?
- What broker? (Kafka for event streaming, RabbitMQ for task queues, SQS for AWS)
- Event-driven (pub-sub) or work queue (point-to-point)?

| Broker | Best For |
|---|---|
| **Kafka** | High-throughput event streaming, event sourcing, log aggregation |
| **RabbitMQ** | Task queues, routing, request-reply, lower throughput |
| **SQS/SNS** | AWS-native, serverless, managed |

## Step 3: Present Plan

```
MESSAGING SETUP PLAN
═════════════════════

Broker: [Kafka / RabbitMQ / SQS]
Pattern: [Pub-Sub / Point-to-Point / Both]

Phase 1 — Dependencies & Configuration
  [ ] Add messaging dependency (spring-kafka / spring-amqp / spring-cloud-aws-sqs)
  [ ] Configure broker connection in application.yml
  [ ] Add serialization config (JSON serializer/deserializer)

Phase 2 — Producer
  [ ] Create event/message DTOs
  [ ] Create Producer service
  [ ] Add topic/queue/exchange configuration
  [ ] Add error handling for failed sends

Phase 3 — Consumer
  [ ] Create Consumer class with @KafkaListener / @RabbitListener
  [ ] Add deserialization config
  [ ] Add error handling (retry, DLQ)
  [ ] Add idempotency handling (if needed)

Phase 4 — Error Handling & Resilience
  [ ] Configure Dead Letter Queue (DLQ) / Dead Letter Topic (DLT)
  [ ] Configure retry policy (max attempts, backoff)
  [ ] Add error logging and monitoring
  [ ] Add consumer lag monitoring (Kafka)

Phase 5 — Testing
  [ ] Add embedded Kafka / RabbitMQ for tests (or Testcontainers)
  [ ] Create producer tests
  [ ] Create consumer tests

Phase 6 — Docker Compose
  [ ] Add broker to docker-compose.yml for local development
```

**Wait for approval.**

## Step 4: Execute

Add incrementally — configuration first, then producer, then consumer, then error handling.
Build and test after each phase.
