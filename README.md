# eulalia-documents

---

## (EN) English Version

Repository containing technical documentation, test evidence, and coverage reports for the Eulalia system.

### Directory Structure

```
eulalia-documents/
├── integration-validation/         # Integration evidence (logs + screenshots) / Evidencia de integracion
├── technical-document/
│   ├── Technical_Document_Of_The_Integration_Scheme (EN).pdf
│   └── Technical_Document_Of_The_Integration_Scheme (ES).pdf
├── user-interface/
│   ├── User_Interface_Design (EN).pdf
│   └── User_Interface_Design (ES).pdf
├── tests/
│   ├── README.md
│   ├── TestIntegration/
│   │   ├── ReportTestIntegration(EN).pdf
│   │   ├── ReportTestIntegration(ES).pdf
│   │   ├── TestPlan(EN).pdf
│   │   ├── TestPlan(ES).pdf
│   │   ├── TestReport(EN).pdf
│   │   └── TestReport(ES).pdf
│   └── coverage-report/
│       └── index.htm
```

### Test Results Summary (April 19, 2026)

| Component | Tests | Passed | Failed | Success Rate |
|-----------|-------|--------|--------|---------------|
| Backend .NET | 132 | 130 | 2 | 98.5% |
| Frontend Web (React + Vitest) | 3 | 3 | 0 | 100% |
| Flutter Mobile | 1 | 1 | 0 | 100% |
| API Integration | 6 | 5 | 1 | 83.3% |
| SSI E2E Flow | 5 | 5 | 0 | 100% |
| **TOTAL** | **147** | **144** | **3** | **98.0%** |

### Test Environment

| Component | Version | Port | Status |
|-----------|---------|------|--------|
| Backend .NET | 8.0 | 5219 | ✅ Running |
| Cloud Agent (Identus) | 1.39.0 | 8080 | ✅ Running |
| PostgreSQL Local | 13+ | 5432 | ✅ Running |
| PostgreSQL Docker | 13 | 5433 | ✅ Running |

### Notes

- All test reports are available in both English (EN) and Spanish (ES)
- PDFs generated using markdown-pdf
- Backend test coverage: 12.4% line coverage, 25% branch coverage

---

## (ES) Versión en Español

Repositorio que contiene la documentación técnica, evidencia de pruebas y reportes de cobertura del sistema Eulalia.

### Estructura de Directorios

```
eulalia-documents/
├── integration-validation/         # Evidencia de integracion (logs + capturas)
├── technical-document/
│   ├── Technical_Document_Of_The_Integration_Scheme (EN).pdf
│   └── Technical_Document_Of_The_Integration_Scheme (ES).pdf
├── user-interface/
│   ├── User_Interface_Design (EN).pdf
│   └── User_Interface_Design (ES).pdf
├── tests/
│   ├── README.md
│   ├── TestIntegration/
│   │   ├── ReportTestIntegration(EN).pdf
│   │   ├── ReportTestIntegration(ES).pdf
│   │   ├── TestPlan(EN).pdf
│   │   ├── TestPlan(ES).pdf
│   │   ├── TestReport(EN).pdf
│   │   └── TestReport(ES).pdf
│   └── coverage-report/
│       └── index.htm
```

### Resumen de Resultados de Pruebas (19 Abril 2026)

| Componente | Pruebas | Pasadas | Fallidas | Tasa de Éxito |
|-----------|---------|---------|----------|---------------|
| Backend .NET | 132 | 130 | 2 | 98.5% |
| Frontend Web (React + Vitest) | 3 | 3 | 0 | 100% |
| Flutter Mobile | 1 | 1 | 0 | 100% |
| Integración API | 6 | 5 | 1 | 83.3% |
| Flujo SSI E2E | 5 | 5 | 0 | 100% |
| **TOTAL** | **147** | **144** | **3** | **98.0%** |

### Entorno de Pruebas

| Componente | Versión | Puerto | Estado |
|-----------|---------|--------|--------|
| Backend .NET | 8.0 | 5219 | ✅ Corriendo |
| Cloud Agent (Identus) | 1.39.0 | 8080 | ✅ Corriendo |
| PostgreSQL Local | 13+ | 5432 | ✅ Corriendo |
| PostgreSQL Docker | 13 | 5433 | ✅ Corriendo |

### Notas

- Todos los reportes de pruebas están disponibles en inglés (EN) y español (ES)
- PDFs generados usando markdown-pdf
- Cobertura de pruebas backend: 12.4% cobertura de líneas, 25% cobertura de ramas
