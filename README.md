# ms-barriodigital-notify

Microservicio encargado del procesamiento asíncrono de notificaciones y avisos de **BarrioDigital**.

## Responsabilidades

- Consumir mensajes desde RabbitMQ.
- Procesar notificaciones a vecinos.
- Procesar avisos relacionados con cuadrillas.
- Registrar en logs el resultado del procesamiento.
- Manejar errores básicos de mensajería.

## Stack tecnológico

- Java
- Spring Boot
- Spring AMQP
- Maven
- RabbitMQ
- Eureka Client
- Spring Boot Actuator
- Docker

## Puerto

```text
8084
```

## Persistencia

```text
Sin base de datos
```

## RabbitMQ

Colas consumidas:

```text
q.cmd.notification
q.cmd.crew
```

### Notificaciones

```text
Requests
   ↓
q.cmd.notification
   ↓
Notify
```

Ejemplo:

```json
{
  "requestId": "uuid",
  "type": "REQUEST_ADMITTED",
  "recipient": "usuario@correo.cl",
  "message": "Su trámite fue admitido.",
  "timestamp": "2026-09-05T14:00:00Z"
}
```

### Avisos a cuadrillas

```text
Requests
   ↓
q.cmd.crew
   ↓
Notify
```

Ejemplo:

```json
{
  "requestId": "uuid",
  "crewId": "uuid",
  "message": "Nueva visita asignada.",
  "scheduledAt": "2026-09-07T10:00:00Z",
  "timestamp": "2026-09-05T14:00:00Z"
}
```

## API pública

```text
No posee endpoints REST de negocio.
```

## Observabilidad

- Spring Boot Actuator.
- Logs.
- Métricas.
- Amazon CloudWatch.

## Estructura esperada

```text
src/
├── main/
│   ├── java/
│   │   └── cl/duoc/barriodigital/notify/
│   │       ├── config/
│   │       ├── consumer/
│   │       ├── dto/
│   │       ├── service/
│   │       └── exception/
│   └── resources/
│       └── application.yml
└── test/
```

## Contrato

```text
barriodigital-contracts/asyncapi/rabbitmq.asyncapi.yaml
```

## Ejecución

```bash
./mvnw spring-boot:run
```

## Docker

```bash
docker build -t barriodigital/notify:1.0.0 .
```

## Estrategia Git

```text
main
develop
feature/*
fix/*
```

## Estado

🚧 Proyecto en etapa inicial de diseño e implementación.
