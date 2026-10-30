# Esquema de Integración Eulalia V2

## Documento técnico de integración

**Idioma:** español  
**Alcance:** prueba de concepto técnica  
**Red:** Cardano Preview  
**Versión:** V2

## Resumen ejecutivo

V2 integra los clientes web y móvil de Eulalia, el backend de aplicación .NET, Hyperledger Identus para identidad autosoberana (SSI), PostgreSQL para el estado operativo y un backend dedicado de transacciones Cardano. El diseño mantiene los datos personales y las credenciales fuera de la cadena. Cardano recibe únicamente identificadores de evento acotados y compromisos SHA-256, mientras Identus gestiona las conexiones y el intercambio de credenciales.

Este documento describe el límite de integración implementado y su evidencia. No es un diseño de custodia para producción, un paquete de certificación electoral ni una afirmación de que una transacción Cardano se haya confirmado sin credenciales Preview válidas, una billetera financiada y acceso de red.

## 1. Mapa de componentes

| Componente | Responsabilidad | Repositorio |
|---|---|---|
| Aplicación móvil Eulalia | Cliente Flutter para ciudadanía e interacción SSI | [eulalia-app](https://github.com/democracyonchain/eulalia-app) |
| Frontend Eulalia | Interfaz React/Vite para organizaciones y operación | [eulalia-frontend](https://github.com/democracyonchain/eulalia-frontend) |
| Backend Eulalia | API .NET 8, autorización JWT, orquestación SSI, PostgreSQL y ciclo de afiliación | [eulalia-backend](https://github.com/democracyonchain/eulalia-backend) |
| Servicio Identus | Cloud Agent, DID, conexiones e intercambio de credenciales | [eulalia-atala-identus](https://github.com/democracyonchain/eulalia-atala-identus) |
| Backend Cardano | Servicio TypeScript/Fastify que valida compromisos, firma metadata acotada y publica en Cardano Preview | [eulalia-backend-cardano](https://github.com/democracyonchain/eulalia-backend-cardano) |
| PostgreSQL | Estado operativo durable de la integración .NET/Identus | Administrado por el despliegue del backend; no es almacenamiento on-chain |
| Blockfrost | Proveedor Preview y consulta de transacciones | [Documentación de Blockfrost](https://docs.blockfrost.io/) |

El código y el material de despliegue de Identus se mantienen en [eulalia-atala-identus](https://github.com/democracyonchain/eulalia-atala-identus). Ninguna otra URL de repositorio Identus es normativa para este documento V2.

```text
App Flutter / frontend React
          |
          | HTTPS + JWT
          v
Backend .NET Eulalia <----> PostgreSQL
          |
          | Cliente Identus y webhook
          v
Cloud Agent Identus <----> Conexión SSI e intercambio de credenciales

Backend .NET Eulalia
          |
          | bearer token + recordHash sin datos personales
          v
Backend Cardano (Fastify)
          |
          | Lucid + clave de firma del servidor
          v
Blockfrost Cardano Preview ----> metadata de transacción
```

El backend .NET es el límite del sistema de negocio. Nunca envía un identificador ciudadano a Cardano. Para una afiliación o anulación genera o recibe un `recordHash` hexadecimal de 64 caracteres, guarda el resultado Cardano junto con la afiliación y conserva la transición de negocio aunque Cardano no esté disponible. El backend Cardano es el límite de transacciones y no expone su clave privada.

## 2. Flujo SSI

### 2.1 Invitación y conexión

1. Un cliente autenticado solicita una invitación mediante el backend Eulalia.
2. El backend busca una invitación activa para la ciudadanía y la devuelve si ya existe.
3. Un reintento marca como fallidas las emisiones obsoletas antes de solicitar una nueva invitación a Identus.
4. El backend guarda en PostgreSQL el identificador y la URL de invitación. La URL se entrega al cliente para completar la conexión.
5. Identus envía un webhook de conexión al backend.

### 2.2 Oferta de credencial

El handler del webhook acepta el sobre actual de Identus y la forma histórica soportada. Extrae identificador de conexión, etiqueta y estado. Los estados pendientes, de solicitud recibida y de respuesta recibida no crean una oferta. Cuando la conexión alcanza el estado elegible de respuesta enviada, el backend reclama la emisión antes de llamar a Identus, crea una única oferta con el DID emisor y el esquema configurados, y guarda el identificador del registro y la URL de oferta.

Los webhooks repetidos son idempotentes: una misma emisión no puede crear ofertas duplicadas. La falta de configuración del esquema o una oferta fallida marca la emisión como fallida sin exponer secretos del proveedor.

La evidencia API relevante incluye `POST /api/SSI/invitation/{cedula}`, `GET /api/SSI/status/{cedula}` y `POST /api/SSI/webhook`. El identificador ciudadano permanece dentro del límite operativo protegido y no se incluye en las peticiones Cardano.

## 3. Flujo Cardano

### 3.1 Registro de eventos

El backend .NET llama al backend Cardano con un payload sin datos personales:

```json
{
  "eventId": "affiliation-123",
  "affiliationId": "123",
  "organizationId": "7",
  "recordHash": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
}
```

La anulación usa la misma forma y se envía a `/api/transactions/cancellations`. Las afiliaciones usan `/api/transactions/affiliations`. Todos los endpoints de transacción requieren `Authorization: Bearer <INTERNAL_API_TOKEN>`; `/health` es público.

El backend Cardano ejecuta esta secuencia:

1. Rechaza campos desconocidos, identificadores no acotados y compromisos que no sean SHA-256.
2. Devuelve el registro existente cuando `eventId` ya fue procesado.
3. Persiste `pending` antes de contactar al proveedor.
4. Construye metadata acotada con tipo de evento, ID de evento, ID de afiliación, ID de organización, compromiso, versión de esquema y timestamp.
5. Firma con la billetera CNE de Preview del servidor y publica mediante Blockfrost.
6. Persiste `submitted` con `txHash`, o `failed` con un código de error seguro.

`GET /api/transactions/:eventId` lee el estado guardado. `POST /api/transactions/:eventId/refresh` consulta el proveedor y puede mover un registro en curso a `confirmed`, `pending` o `failed`.

### 3.2 Privacidad y compromisos

`recordHash` es un compromiso SHA-256 hexadecimal de exactamente 64 caracteres hexadecimales. La API Cardano no debe recibir `cedula`, nombres, biometría, claims de credenciales sin transformar ni datos personales reversibles. El límite de seguridad de metadata es de 1.600 bytes UTF-8. La metadata es un puntero de integridad y auditoría, no un registro público de identidad.

El compromiso por sí solo no vuelve anónimos datos débiles o predecibles. Para producción se necesita una estrategia documentada de canonicalización y sal, gestión de claves, controles de acceso y revisión de privacidad. La implementación actual demuestra el límite y valida el formato del compromiso; no afirma unlinkability formal.

## 4. Estados, idempotencia y fallos

| Estado | Significado |
|---|---|
| `pending` | Aceptado y persistido antes del proveedor |
| `submitted` | El proveedor devolvió un hash de transacción |
| `confirmed` | La consulta al proveedor encontró la transacción |
| `failed` | Falló el envío o la consulta; se guarda un código seguro |
| `not_found` | Estado reservado para una consulta sin transacción conocida |

El servicio de transacciones mantiene un mapa de operaciones en curso para agrupar peticiones concurrentes con el mismo `eventId`. La consulta persistente es la segunda barrera de idempotencia después de reiniciar el proceso. Un evento repetido devuelve el registro original y nunca publica una segunda transacción. El servicio .NET de afiliaciones guarda por separado IDs de evento, compromisos, hashes de transacción y estados Cardano para afiliación y anulación. Un fallo Cardano no revierte la anulación de negocio; se registra para conciliación.

## 5. Configuración sin secretos

La configuración Cardano requerida se entrega mediante variables de entorno o un almacén protegido de secretos:

| Variable | Propósito |
|---|---|
| `BLOCKFROST_PROJECT_ID` | Credencial del proveedor Preview |
| `CNE_ADDRESS` | Dirección de la billetera CNE Preview |
| `CNE_PRIVATE_KEY` | Clave de firma del servidor; nunca registrar ni devolver |
| `INTERNAL_API_TOKEN` | Autenticación entre .NET y Cardano |
| `CARDANO_NETWORK` | Debe permanecer en `Preview` para esta demo |
| `METADATA_LABEL` | Etiqueta numérica de metadata, por defecto `1967` |
| `METADATA_SCHEMA_VERSION` | Versión de metadata, por defecto `1.0.0` |
| `DATA_FILE` | Ruta de persistencia JSON de demostración |

`.env.example` contiene solo placeholders. No se deben versionar `.env`, archivos de billetera, claves privadas, IDs de proveedor, bearer tokens, credenciales sin transformar ni datos personales. Preview y Mainnet deben usar credenciales completamente separadas. La firma en producción debería usar custodia con Vault o HSM, preferentemente con controles institucionales multifirma.

## 6. Evidencia y limitaciones

El repositorio contiene evidencia de integración para la API .NET, salud e infraestructura de Identus, material frontend/móvil y pruebas en [integration-validation](https://github.com/democracyonchain/eulalia-documents/tree/main/integration-validation). La evidencia local documentada incluye Swagger del backend, salud del Cloud Agent y estado de contenedores Identus. Los reportes de pruebas distinguen la agregación histórica de los resultados actuales reproducibles.

Limitaciones que deben permanecer explícitas:

- Cardano funciona solo en Preview y no es custodia de producción.
- La persistencia JSON es una demo, no una base transaccional durable.
- Una prueba unitaria o de integración exitosa no demuestra una transacción on-chain real.
- El envío end-to-end requiere acceso válido a Blockfrost Preview, billetera financiada y conectividad.
- Faltan endurecimientos de producción para autenticidad de webhooks, límites, autorización operativa, reintentos, conciliación, auditoría y rotación de claves.
- La prueba de concepto no es certificación electoral ni auditoría de seguridad.

## 7. Recomendaciones

1. Sustituir la persistencia JSON por un almacén transaccional con restricción única sobre `eventId`.
2. Añadir verificación firmada de webhooks, protección contra replay, límites, auditoría estructurada y reintentos acotados.
3. Definir y versionar el procedimiento canónico de entrada y derivación del compromiso antes de depender de conciliación entre sistemas.
4. Trasladar la firma a custodia Vault/HSM, separar roles operativos, rotar credenciales y documentar recuperación ante incidentes.
5. Añadir un worker de conciliación que reintente fallos seguros e informe diferencias entre PostgreSQL y Cardano.
6. Mantener PII y claims de credenciales fuera de la cadena y someter los flujos a revisión de privacidad y modelo de amenazas.

## Referencias

- [Documentos Eulalia](https://github.com/democracyonchain/eulalia-documents)
- [Repositorio de Hyperledger Identus](https://github.com/democracyonchain/eulalia-atala-identus)
- [Redes de prueba Cardano](https://docs.cardano.org/cardano-testnets/overview)
- [Portal de desarrolladores Cardano](https://developers.cardano.org/)
- [Lucid Evolution](https://github.com/Anastasia-Labs/lucid-evolution)
- [Documentación API Blockfrost](https://docs.blockfrost.io/)
