# Kafka Patterns

## Overview
Violet services use Apache Kafka for event-driven communication between microservices.

## Producer Configuration
```java
@Configuration
public class KafkaProducerConfig {
    @Bean
    public ProducerFactory<String, String> producerFactory() {
        Map<String, Object> config = new HashMap<>();
        config.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        config.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        config.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        return new DefaultKafkaProducerFactory<>(config);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
}
```

## Consumer Configuration
```java
@KafkaListener(topics = "${kafka.topic.name}", groupId = "${spring.kafka.consumer.group-id}")
public void listen(ConsumerRecord<String, String> record) {
    // Process message
}
```

## Topic Naming Convention
- Format: `{domain}.{entity}.{action}`
- Examples:
  - `orders.order.created`
  - `products.product.updated`
  - `merchants.merchant.connected`

## Message Format
Use JSON with snake_case field naming to match service conventions:
```json
{
  "event_type": "order.created",
  "event_id": "uuid",
  "timestamp": "2024-01-01T00:00:00Z",
  "payload": {
    "order_id": 12345,
    "app_id": 67890
  }
}
```

## Error Handling
- Use Dead Letter Topics (DLT) for failed messages
- Implement retry logic with exponential backoff
- Log failed messages with correlation IDs
