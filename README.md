# eulalia-documents

---

## (EN) English Version

Repository containing the Eulalia technical documentation, integration evidence, test evidence, milestone material, final reports, and coverage reports. The material describes a technical proof of concept, not a production or electoral certification package.

### Directory Structure

```
eulalia-documents/
├── integration-validation/         # Integration evidence: logs, outputs, and captured evidence
├── technical-document/
│   ├── Technical_Document_Of_The_Integration_Scheme (EN).pdf
│   ├── Technical_Document_Of_The_Integration_Scheme (ES).pdf
│   ├── Technical_Document_Of_The_Integration_Scheme_V2 (EN).pdf
│   └── Technical_Document_Of_The_Integration_Scheme_V2 (ES).pdf
├── user-interface/
│   ├── User_Interface_Design (EN).pdf
│   └── User_Interface_Design (ES).pdf
├── final-report/
│   ├── Final Project Report (EN|ES).pdf
│   ├── Project Completion Report (EN|ES).pdf
│   ├── Proof of Concept Results (EN|ES).pdf
│   └── Test Report (EN|ES).pdf
└── tests/
    ├── README.md
    ├── TestIntegration/             # Integration reports and plans
    ├── TestResults/                 # Raw coverage output
    └── coverage-report/             # HTML coverage report
```

The `Test Report (EN|ES).pdf` files in `final-report/` are the current test-report baseline and reconciliation. The reports distinguish historical evidence from reproducible current results.

### Architecture Documentation

The documented architecture includes:

| Component | Role | Repository |
|-----------|------|------------|
| `eulalia-app` | Flutter mobile application | [GitHub](https://github.com/democracyonchain/eulalia-app) |
| `eulalia-frontend` | React/Vite web frontend | [GitHub](https://github.com/democracyonchain/eulalia-frontend) |
| `eulalia-backend` | .NET 8 API, SSI orchestration, and PostgreSQL access | [GitHub](https://github.com/democracyonchain/eulalia-backend) |
| `eulalia-identus` | Identus Cloud Agent infrastructure and supporting services | [GitHub](https://github.com/democracyonchain/eulalia-atala-identus) |
| `eulalia-backend-cardano` | TypeScript/Fastify Cardano Preview transaction PoC | [GitHub](https://github.com/democracyonchain/eulalia-backend-cardano) |

The V2 integration documents describe interfaces, privacy boundaries, SSI flow, and Cardano affiliation/cancellation event recording. See the [English V2 document](technical-document/Technical_Document_Of_The_Integration_Scheme_V2%20(EN).md) or [Spanish V2 document](technical-document/Technical_Document_Of_The_Integration_Scheme_V2%20(ES).md).

### Test Results and Status

The historical April 19, 2026 aggregation recorded 147 tests, 144 passing, and a 98.0% aggregate success rate. That figure is retained as historical documentation only; it is not presented as the current approved result. The current baseline and reconciliation are the English and Spanish `Test Report` documents in `final-report/`. Consult those reports for current metrics, scope, coverage, and known failures.

The repository also contains historical coverage evidence. The backend README reports low overall coverage, approximately 12.4% line coverage and 25% branch coverage in the referenced report. These figures must not be interpreted as full-system coverage.

### References

- [Hyperledger Identus documentation](https://hyperledger-identus.github.io/docs/)
- [Cardano Preview testnet](https://docs.cardano.org/cardano-testnets/overview)
- [Blockfrost documentation](https://docs.blockfrost.io/)
- [Cardano developer portal](https://developers.cardano.org/)

---

## (ES) Versión en Español

Repositorio que contiene la documentación técnica de Eulalia, la evidencia de integración, la evidencia de pruebas, el material de hitos, los informes finales y los reportes de cobertura. El material describe una prueba de concepto técnica, no un paquete de producción ni de certificación electoral.

### Estructura de Directorios

```
eulalia-documents/
├── integration-validation/         # Evidencia de integración: logs, salidas y capturas
├── technical-document/
│   ├── Technical_Document_Of_The_Integration_Scheme (EN).pdf
│   ├── Technical_Document_Of_The_Integration_Scheme (ES).pdf
│   ├── Technical_Document_Of_The_Integration_Scheme_V2 (EN).pdf
│   └── Technical_Document_Of_The_Integration_Scheme_V2 (ES).pdf
├── user-interface/
│   ├── User_Interface_Design (EN).pdf
│   └── User_Interface_Design (ES).pdf
├── final-report/
│   ├── Final Project Report (EN|ES).pdf
│   ├── Project Completion Report (EN|ES).pdf
│   ├── Proof of Concept Results (EN|ES).pdf
│   └── Test Report (EN|ES).pdf
└── tests/
    ├── README.md
    ├── TestIntegration/             # Informes y planes de integración
    ├── TestResults/                 # Salida de cobertura sin procesar
    └── coverage-report/             # Reporte HTML de cobertura
```

Los archivos `Test Report (EN|ES).pdf` de `final-report/` son la línea base y reconciliación actual de pruebas. Los informes distinguen la evidencia histórica de los resultados actuales reproducibles.

### Documentación de la Arquitectura

La arquitectura documentada incluye:

| Componente | Responsabilidad | Repositorio |
|------------|-----------------|-------------|
| `eulalia-app` | Aplicación móvil Flutter | [GitHub](https://github.com/democracyonchain/eulalia-app) |
| `eulalia-frontend` | Frontend web React/Vite | [GitHub](https://github.com/democracyonchain/eulalia-frontend) |
| `eulalia-backend` | API .NET 8, orquestación SSI y acceso a PostgreSQL | [GitHub](https://github.com/democracyonchain/eulalia-backend) |
| `eulalia-identus` | Infraestructura de Identus Cloud Agent y servicios de apoyo | [GitHub](https://github.com/democracyonchain/eulalia-atala-identus) |
| `eulalia-backend-cardano` | PoC de transacciones Cardano Preview con TypeScript/Fastify | [GitHub](https://github.com/democracyonchain/eulalia-backend-cardano) |

Los documentos de integración V2 describen interfaces, límites de privacidad, flujo SSI y registro en Cardano de eventos de afiliación y cancelación. Consulte el [documento V2 en inglés](technical-document/Technical_Document_Of_The_Integration_Scheme_V2%20(EN).md) o el [documento V2 en español](technical-document/Technical_Document_Of_The_Integration_Scheme_V2%20(ES).md).

### Resultados y Estado de las Pruebas

La agregación histórica del 19 de abril de 2026 registró 147 pruebas, 144 exitosas y una tasa agregada de 98.0%. Esa cifra se conserva únicamente como documentación histórica y no se presenta como resultado actual aprobado. La línea base y reconciliación actual son los documentos `Test Report` en inglés y español de `final-report/`. Consulte esos informes para conocer las métricas actuales, el alcance, la cobertura y los fallos conocidos.

El repositorio también contiene evidencia histórica de cobertura. El README del backend informa una cobertura global baja, aproximadamente 12.4% de líneas y 25% de ramas en el reporte referenciado. Estas cifras no deben interpretarse como cobertura completa del sistema.

### Referencias

- [Documentación de Hyperledger Identus](https://hyperledger-identus.github.io/docs/)
- [Red de pruebas Cardano Preview](https://docs.cardano.org/cardano-testnets/overview)
- [Documentación de Blockfrost](https://docs.blockfrost.io/)
- [Portal para desarrolladores de Cardano](https://developers.cardano.org/)
