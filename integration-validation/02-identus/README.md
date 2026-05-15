# 02 - Identus (Atala PRISM) Evidence

## Description / Descripción
Evidence of ATALA PRISM / Hyperledger Identus Cloud Agent running and integrated with Eulalia.

---

## (EN) English

### Identus Cloud Agent Status

| Component | Version | Port | Status |
|-----------|---------|------|--------|
| Cloud Agent | 1.39.0 | 8080 | ✅ Running |
| PostgreSQL | 13 | 5433 | ✅ Running |
| Prism Node | 2.5.0 | 5432 | ✅ Running |
| Vault | latest | 8200 | ✅ Running |
| APISIX | 2.15.0 | 8080 | ✅ Running |

### Health Check

```json
{
  "version": "1.39.0"
}
```

### Files

- `docker-containers.txt` - Docker container status
- `cloud-agent-health.json` - Cloud Agent health response
- `health.pdf` - Health endpoint captured as PDF
- `infra.pdf` - Docker Desktop containers view captured as PDF
- `screenshot-containers.png` - **[TODO: User to capture]** Docker containers screenshot
- `screenshot-health.png` - **[TODO: User to capture]** Health endpoint screenshot

### Reviewer Note

- `infra.pdf` shows the Identus stack running (`shared` group running).

### How to Capture

1. Run: `docker ps`
2. Take screenshot showing all containers
3. Save as `screenshot-containers.png`

---

## (ES) Español

### Estado del Cloud Agent de Identus

| Componente | Versión | Puerto | Estado |
|------------|---------|--------|--------|
| Cloud Agent | 1.39.0 | 8080 | ✅ Corriendo |
| PostgreSQL | 13 | 5433 | ✅ Corriendo |
| Prism Node | 2.5.0 | 5432 | ✅ Corriendo |
| Vault | latest | 8200 | ✅ Corriendo |
| APISIX | 2.15.0 | 8080 | ✅ Corriendo |

### Verificación de Salud

```json
{
  "version": "1.39.0"
}
```

### Archivos

- `docker-containers.txt` - Estado de contenedores Docker
- `cloud-agent-health.json` - Respuesta de salud del Cloud Agent
- `health.pdf` - Endpoint de salud capturado en PDF
- `infra.pdf` - Vista de contenedores en Docker Desktop (PDF)
- `screenshot-containers.png` - **[POR HACER: Usuario]** Captura de contenedores
- `screenshot-health.png` - **[POR HACER: Usuario]** Captura del endpoint de salud

### Nota Para Reviewer

- `infra.pdf` muestra el stack de Identus corriendo (grupo `shared` en ejecución).

### Cómo Capturar

1. Ejecutar: `docker ps`
2. Tomar captura mostrando todos los contenedores
3. Guardar como `screenshot-containers.png`

---

## Evidence Date / Fecha de Evidencia
2026-05-15
