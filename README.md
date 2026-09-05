# ms-barriodigital-notify

Microservicio encargado del procesamiento asíncrono de notificaciones de **BarrioDigital**.

## Descripción

Este servicio permite desacoplar el envío de notificaciones del flujo transaccional de los trámites.

El usuario no necesita esperar a que una notificación sea enviada para que la operación principal finalice.

## Responsabilidades

- Consumir tareas desde RabbitMQ.
- Enviar correos electrónicos.
- Procesar notificaciones push.
- Notificar admisiones.
- Notificar visitas.
- Notificar resoluciones.
- Notificar rechazos.
- Gestionar errores de procesamiento.
- Evitar procesamiento duplicado.

## Stack tecnológico

- Java
- Spring Boot
- Spring AMQP
- Maven
- RabbitMQ
- Spring Boot Actuator
- Docker

## Puerto local

```text
8084
```

El puerto se utilizará principalmente para endpoints técnicos como health checks.

## Persistencia

Inicialmente:

```text
Sin base de datos
```

Si posteriormente se requiere registrar historial de entrega, deberá evaluarse antes de incorporar persistencia propia.

## RabbitMQ

Cola principal:

```text
q.cmd.email
```

DLQ:

```text
q.cmd.email.dlq
```

Routing keys consideradas:

```text
email.send
email.*
```

Exchange:

```text
cmd.direct
cmd.topic
```

Dead Letter Exchange:

```text
cmd.dead.dlx
```

## Ejemplo de mensaje

```json
{
  "eventId": "uuid",
  "type": "SEND_EMAIL",
  "timestamp": "2026-09-05T13:30:00Z",
  "traceId": "trace-id",
  "correlationId": "REQ-2026-000001",
  "version": 1,
  "payload": {
    "recipient": "usuario@example.com",
    "template": "REQUEST_ADMITTED"
  }
}
```

## Buenas prácticas

- ACK explícito.
- NACK en caso de error.
- Dead Letter Queue.
- Reintentos controlados.
- Procesamiento idempotente.
- `eventId`.
- `traceId`.
- `correlationId`.
- Logs estructurados.

## API pública

No se expondrán inicialmente endpoints públicos de negocio.

Endpoints técnicos futuros:

```http
GET /actuator/health
GET /actuator/metrics
```

## Observabilidad

Métricas relevantes:

- mensajes consumidos;
- mensajes exitosos;
- mensajes fallidos;
- reintentos;
- tasa de DLQ;
- tiempo de procesamiento.

Herramientas:

- Spring Boot Actuator.
- Amazon CloudWatch.

## Variables de entorno

```env
SERVER_PORT=8084

RABBITMQ_HOST=
RABBITMQ_PORT=5672
RABBITMQ_USER=
RABBITMQ_PASSWORD=

MAIL_HOST=
MAIL_PORT=
MAIL_USERNAME=
MAIL_PASSWORD=

EUREKA_SERVER_URL=
```

## Contratos

Los mensajes RabbitMQ serán documentados mediante AsyncAPI en:

```text
barriodigital-contracts
```

## Estructura esperada

```text
src/
├── main/
│   ├── java/
│   │   └── cl/duoc/barriodigital/notify/
│   │       ├── config/
│   │       ├── consumer/
│   │       ├── dto/
│   │       ├── exception/
│   │       ├── service/
│   │       └── util/
│   └── resources/
│       └── application.yml
└── test/
```

## Docker

```bash
docker build -t barriodigital/notify:1.0.0 .
```

## CI/CD

GitHub Actions será utilizado para build, testing, análisis y generación de imagen Docker.

## Estrategia Git

```text
main
develop
feature/*
fix/*
```

## Estado

🚧 Proyecto en etapa inicial de diseño y construcción.
