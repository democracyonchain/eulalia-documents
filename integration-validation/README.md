# Eulalia Integration Validation Evidence

## (EN) English

This folder contains evidence of the Eulalia VoterID system integration with ATALA PRISM (Hyperledger Identus).

### Folder Structure

```
integration-validation/
├── 01-api-ssi/         # SSI API endpoints evidence
├── 02-identus/         # Identus Cloud Agent evidence
├── 03-frontend/        # Frontend Web UI evidence
├── 04-mobile/          # Mobile App evidence
├── 05-tests/          # Test execution results
├── 06-backend/         # Backend Swagger/API evidence
└── README.md          # This file
```

### Components Running

| Component | URL/Port | Status |
|-----------|----------|--------|
| Backend .NET | http://localhost:5219 | ✅ Running |
| Frontend Web | http://localhost:5173 | ✅ Running |
| Identus Cloud Agent | http://localhost:8080 | ✅ Running |
| PostgreSQL (Identus) | localhost:5433 | ✅ Running |

### What User Needs to Capture

Since screenshots cannot be generated automatically, user must capture:

1. **01-api-ssi/**: Swagger UI screenshot at http://localhost:5219/swagger
2. **02-identus/**: Docker containers screenshot (`docker ps`)
3. **03-frontend/**: Login page screenshot at http://localhost:5173/login
4. **04-mobile/**: Flutter app screenshots (run `flutter run`)
5. **05-tests/**: Test results screenshot (run all tests)
6. **06-backend/**: Swagger evidence PDF (`backend.pdf` already captured)

---

## (ES) Español

Esta carpeta contiene evidencia de la integración del sistema Eulalia VoterID con ATALA PRISM (Hyperledger Identus).

### Estructura de Carpetas

```
integration-validation/
├── 01-api-ssi/         # Evidencia de endpoints API SSI
├── 02-identus/         # Evidencia del Cloud Agent Identus
├── 03-frontend/       # Evidencia de la UI del Frontend Web
├── 04-mobile/         # Evidencia de la App Móvil
├── 05-tests/          # Resultados de ejecución de pruebas
├── 06-backend/         # Evidencia Swagger/API del backend
└── README.md          # Este archivo
```

### Componentes Corriendo

| Componente | URL/Puerto | Estado |
|------------|------------|--------|
| Backend .NET | http://localhost:5219 | ✅ Corriendo |
| Frontend Web | http://localhost:5173 | Corriendo |
| Cloud Agent Identus | http://localhost:8080 | ✅ Corriendo |
| PostgreSQL (Identus) | localhost:5433 | ✅ Corriendo |

### Lo que el Usuario Debe Capturar

Como las capturas de pantalla no se pueden generar automáticamente, el usuario debe capturar:

1. **01-api-ssi/**: Captura de Swagger UI en http://localhost:5219/swagger
2. **02-identus/**: Captura de contenedores Docker (`docker ps`)
3. **03-frontend/**: Captura de página de login en http://localhost:5173/login
4. **04-mobile/**: Capturas de la app Flutter (ejecutar `flutter run`)
5. **05-tests/**: Captura de resultados de pruebas (ejecutar todas las pruebas)
6. **06-backend/**: Evidencia Swagger en PDF (`backend.pdf` ya capturado)

---

## Evidence Date / Fecha de Evidencia
2026-05-15
