# 01 - API SSI Evidence

## Description / Descripción
Evidence of ATALA PRISM (Identus) SSI API integration with Eulalia Backend.

---

## (EN) English

### Endpoints Tested

| Endpoint | Method | Status | Evidence |
|----------|--------|--------|----------|
| `/api/Provincia` | GET | ✅ 200 | Public endpoint working |
| `/api/SSI/webhook` | POST | ✅ 200 | Webhook processed successfully |
| `/api/SSI/invitation/{cedula}` | POST | ⚠️ 401 | Requires JWT authentication |

### Files

- `ssi-webhook-response.json` - Webhook test response
- `screenshot-swagger-ssi.png` - **[TODO: User to capture]** Swagger UI screenshot

### How to Capture

1. Open: http://localhost:5219/swagger
2. Take screenshot showing SSI endpoints
3. Save as `screenshot-swagger-ssi.png` in this folder

---

## (ES) Español

### Endpoints Probados

| Endpoint | Método | Estado | Evidencia |
|----------|--------|--------|-----------|
| `/api/Provincia` | GET | ✅ 200 | Endpoint público funcionando |
| `/api/SSI/webhook` | POST | ✅ 200 | Webhook procesado exitosamente |
| `/api/SSI/invitation/{cedula}` | POST | ⚠️ 401 | Requiere autenticación JWT |

### Archivos

- `ssi-webhook-response.json` - Respuesta de prueba del webhook
- `screenshot-swagger-ssi.png` - **[POR HACER: Usuario]** Captura de Swagger UI

### Cómo Capturar

1. Abrir: http://localhost:5219/swagger
2. Tomar captura mostrando los endpoints SSI
3. Guardar como `screenshot-swagger-ssi.png` en esta carpeta

---

## Evidence Date / Fecha de Evidencia
2026-05-15