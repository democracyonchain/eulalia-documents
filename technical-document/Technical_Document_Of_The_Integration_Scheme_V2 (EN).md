# Eulalia V2 Integration Scheme

## Technical integration document

**Language:** English  
**Scope:** technical proof of concept  
**Network:** Cardano Preview  
**Version:** V2

## Executive summary

V2 integrates Eulalia's web and mobile clients, the .NET application backend, Hyperledger Identus for self-sovereign identity (SSI), PostgreSQL for operational state, and a dedicated Cardano transaction backend. The design keeps personal data and credentials off-chain. Cardano receives only bounded event identifiers and SHA-256 commitments, while Identus manages connection and credential-exchange state.

This document describes the implemented integration boundary and its evidence. It is not a production custody design, an electoral certification package, or an assertion that a Cardano transaction succeeded without valid Preview credentials, a funded wallet, and network access.

## 1. Components and responsibilities

| Component | Responsibility | Repository |
|---|---|---|
| Eulalia mobile app | Citizen-facing Flutter client and SSI interaction | [eulalia-app](https://github.com/democracyonchain/eulalia-app) |
| Eulalia frontend | React/Vite web interface for organizations and operations | [eulalia-frontend](https://github.com/democracyonchain/eulalia-frontend) |
| Eulalia backend | .NET 8 API, JWT authorization, SSI orchestration, PostgreSQL persistence, and affiliation lifecycle | [eulalia-backend](https://github.com/democracyonchain/eulalia-backend) |
| Identus service | Cloud Agent, DID/connection and credential exchange infrastructure | [eulalia-atala-identus](https://github.com/democracyonchain/eulalia-atala-identus) |
| Cardano backend | TypeScript/Fastify service that validates commitments, signs bounded metadata, and submits to Cardano Preview | [eulalia-backend-cardano](https://github.com/democracyonchain/eulalia-backend-cardano) |
| PostgreSQL | Durable operational state for the .NET/Identus integration | Managed by the backend deployment; not an on-chain data store |
| Blockfrost | Cardano Preview provider and transaction lookup boundary | [Blockfrost docs](https://docs.blockfrost.io/) |

The Identus integration source and deployment material are maintained at [eulalia-atala-identus](https://github.com/democracyonchain/eulalia-atala-identus). No other Identus repository URL is normative for this V2 document.

## 2. Integration map

```text
Flutter app / React frontend
          |
          | HTTPS + JWT
          v
Eulalia .NET backend <----> PostgreSQL
          |
          | Identus client and webhook
          v
Identus Cloud Agent <----> SSI connection and credential exchange

Eulalia .NET backend
          |
          | bearer token + privacy-safe recordHash
          v
Cardano backend (Fastify)
          |
          | Lucid + server-side signing key
          v
Blockfrost Cardano Preview ----> transaction metadata
```

The .NET backend is the business-system boundary. It never sends a citizen identifier to Cardano. For an affiliation or cancellation it derives or receives a 64-character hexadecimal `recordHash`, stores the Cardano result with the affiliation, and preserves the business transition even if Cardano is unavailable. The standalone Cardano backend is the transaction boundary and does not expose its private key.

## 3. SSI flow

### 3.1 Invitation and connection

1. An authenticated client requests an invitation through the Eulalia backend.
2. The backend checks for an active invitation for the citizen and returns it when one already exists.
3. A retry marks obsolete issuance state as failed before requesting a new Identus invitation.
4. The backend stores the invitation identifier and URL in PostgreSQL. The URL is returned to the client for the connection step.
5. Identus sends a connection webhook to the backend.

### 3.2 Credential offer

The webhook handler accepts the current Identus envelope and the supported legacy event shape. It extracts the connection identifier, label, and state. Pending, request-received, and response-received states do not create an offer. When the connection reaches the eligible response-sent state, the backend claims the issuance before calling Identus, creates one credential offer using the configured issuer DID and schema, and persists the record identifier and offer URL.

Repeated webhook events are idempotent: the same issuance cannot create duplicate offers. Missing schema configuration or a failed offer marks the issuance as failed without exposing provider secrets.

Relevant API evidence includes `POST /api/SSI/invitation/{cedula}`, `GET /api/SSI/status/{cedula}`, and `POST /api/SSI/webhook`. The citizen identifier remains inside the protected operational boundary and is not included in Cardano requests.

## 4. Cardano flow

### 4.1 Event submission

The .NET backend calls the Cardano backend with a privacy-safe payload:

```json
{
  "eventId": "affiliation-123",
  "affiliationId": "123",
  "organizationId": "7",
  "recordHash": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
}
```

The cancellation endpoint uses the same shape and is submitted to `/api/transactions/cancellations`. Affiliation events use `/api/transactions/affiliations`. All transaction endpoints require `Authorization: Bearer <INTERNAL_API_TOKEN>`; `/health` is public.

The Cardano backend performs the following sequence:

1. Reject unknown fields, unbounded identifiers, and non-SHA-256 commitments.
2. Return the existing record when `eventId` has already been processed.
3. Persist `pending` before contacting the provider.
4. Build bounded metadata containing only event type, event ID, affiliation ID, organization ID, commitment, schema version, and timestamp.
5. Sign with the server-side CNE Preview wallet and submit through Blockfrost.
6. Persist `submitted` with `txHash`, or persist `failed` with a safe error code.

`GET /api/transactions/:eventId` reads stored state. `POST /api/transactions/:eventId/refresh` checks the provider and can move an in-flight record to `confirmed`, `pending`, or `failed`.

### 4.2 Privacy and commitments

`recordHash` is a SHA-256 hexadecimal commitment with exactly 64 hexadecimal characters. The Cardano API must not receive `cedula`, names, biometrics, raw credential claims, or reversible personal data. The metadata safety bound is 1,600 UTF-8 bytes. Metadata is an integrity and audit pointer, not a public identity record.

The commitment scheme does not make weak or guessable source data anonymous by itself. Production use would require a documented canonicalization and salting strategy, key management, access controls, and a privacy review. The current implementation demonstrates the boundary and validates the commitment format; it does not claim formal unlinkability.

## 5. State, idempotency, and failure behavior

| State | Meaning |
|---|---|
| `pending` | Accepted and persisted before provider submission |
| `submitted` | Provider returned a transaction hash |
| `confirmed` | Provider lookup found the transaction |
| `failed` | Submission or lookup failed; safe error code is stored |
| `not_found` | Reserved status for a provider lookup with no known transaction |

The transaction service keeps an in-flight map to coalesce concurrent requests with the same `eventId`. Persistent lookup is the second idempotency barrier after process restart. A repeated event returns the original record and never submits a second transaction. The .NET affiliation service separately persists Cardano event IDs, hashes, transaction hashes, and statuses for both affiliation and cancellation. A Cardano failure does not roll back the business cancellation; it is recorded for reconciliation.

## 6. Configuration and secret handling

Required Cardano configuration is supplied through environment variables or a protected deployment secret store:

| Variable | Purpose |
|---|---|
| `BLOCKFROST_PROJECT_ID` | Preview provider credential |
| `CNE_ADDRESS` | CNE Preview wallet address |
| `CNE_PRIVATE_KEY` | Server-side signing key; never log or return it |
| `INTERNAL_API_TOKEN` | .NET-to-Cardano service authentication |
| `CARDANO_NETWORK` | Must remain `Preview` for this demo |
| `METADATA_LABEL` | Numeric transaction metadata label, default `1967` |
| `METADATA_SCHEMA_VERSION` | Metadata schema version, default `1.0.0` |
| `DATA_FILE` | Demo JSON persistence path |

`.env.example` contains placeholders only. Do not commit `.env`, wallet files, private keys, provider IDs, bearer tokens, raw credentials, or personal data. Preview and Mainnet credentials must be completely separate. Production signing should use Vault or HSM-backed custody, preferably with institutional multisignature controls.

## 7. Evidence and limitations

The repository contains integration evidence for the .NET API, Identus health and infrastructure, frontend/mobile material, and tests under [integration-validation](https://github.com/democracyonchain/eulalia-documents/tree/main/integration-validation). The documented local evidence includes Backend Swagger, Identus Cloud Agent health, and container status. Test reports distinguish historical aggregation from current reproducible results.

Known limitations are intentional and must remain visible:

- Cardano support is Preview-only and is not production custody.
- JSON transaction persistence is a demo store, not a durable transactional database.
- A successful unit or integration test does not prove a live on-chain transaction.
- End-to-end submission requires valid Blockfrost Preview access, a funded wallet, and network connectivity.
- Webhook authenticity, rate limiting, operational authorization, retries, reconciliation jobs, audit retention, and key rotation need production hardening.
- The technical proof of concept is not electoral certification or a security audit.

## 8. Recommendations

1. Replace JSON persistence with a transactional store and a unique constraint on `eventId`.
2. Add signed webhook verification, replay protection, rate limits, structured audit events, and bounded retries.
3. Define and version the canonical input and commitment derivation procedure before relying on cross-system reconciliation.
4. Move signing to Vault/HSM custody, separate operational roles, rotate credentials, and document incident recovery.
5. Add a reconciliation worker that retries safe failures and reports mismatches between PostgreSQL and Cardano.
6. Keep raw PII and credential claims off-chain and subject the final data flows to a privacy and threat-model review.

## References

- [Eulalia documents](https://github.com/democracyonchain/eulalia-documents)
- [Hyperledger Identus repository](https://github.com/democracyonchain/eulalia-atala-identus)
- [Cardano testnets](https://docs.cardano.org/cardano-testnets/overview)
- [Cardano developer portal](https://developers.cardano.org/)
- [Lucid Evolution](https://github.com/Anastasia-Labs/lucid-evolution)
- [Blockfrost API documentation](https://docs.blockfrost.io/)
