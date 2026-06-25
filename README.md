# Vectros SDK for Java

[![Maven Central](https://img.shields.io/maven-central/v/ai.vectros/vectros-sdk)](https://central.sonatype.com/artifact/ai.vectros/vectros-sdk)
[![license](https://img.shields.io/badge/license-Apache--2.0-blue)](https://www.apache.org/licenses/LICENSE-2.0)

The official Java client for the [Vectros API](https://vectros.ai) — hybrid
search, document ingestion, structured records, and grounded inference for your
application.

## Installation

Maven:

```xml
<dependency>
    <groupId>ai.vectros</groupId>
    <artifactId>vectros-sdk</artifactId>
    <version>0.29.8</version>
</dependency>
```

Gradle:

```groovy
implementation("ai.vectros:vectros-sdk:0.29.8")
```

Requires Java 11+.

## Quick start

```java
import ai.vectros.VectrosApiClient;
import ai.vectros.resources.search.requests.SearchRequest;
import ai.vectros.types.DocumentRequest;
import ai.vectros.types.RecordRequest;
import java.util.Map;

VectrosApiClient client = VectrosApiClient
    .builder()
    .token(System.getenv("VECTROS_API_KEY")) // sk_live_... or sk_test_...
    .build();

// Hybrid (keyword + semantic) search over your indexed content
var results = client.search().content(
    SearchRequest.builder()
        .query("patient intake form diabetes")
        .build());

// Ingest a document — extracted, chunked, and indexed for search + RAG
var doc = client.documents().ingestDocument(
    DocumentRequest.builder()
        .title("Patient Intake Form — Jane Doe")
        .build());

// Write a structured record against one of your schemas
var record = client.records().createRecord(
    RecordRequest.builder()
        .typeName("intake_form")
        .schemaId("6ba7b810-9dad-11d1-80b4-00c04fd430c8")
        .payload(Map.of("first_name", "Jane", "email", "jane@example.com"))
        .build());
```

> Request/response types live under `ai.vectros.types` and the per-resource
> `ai.vectros.resources.*` packages; your IDE's auto-import resolves them.

## Authentication

The SDK sends whatever credential you pass in the `Authorization: Bearer <token>`
header. Two credential types are accepted:

| Type | Prefix | Lifetime | Use from |
|------|--------|----------|----------|
| **API key** | `sk_live_*` / `sk_test_*` | Long-lived | **Server only** — full tenant access |
| **Scoped token** | `st_*` | Short-lived | **Server or browser** — narrowed scope, auto-expiring |

Keep API keys server-side only. For untrusted runtimes, mint a short-lived
scoped token on your backend and pass it via `.token(...)`. See the
[authentication guide](https://docs.vectros.ai) for the full pattern.

## What you can do

- **Hybrid search & RAG** — `client.search()`, `client.inference()` — vector +
  keyword search and grounded document Q&A over your indexed corpus.
- **Documents & folders** — `client.documents()`, `client.folders()` — ingest,
  organize, retrieve, and look documents up by field.
- **Structured records** — `client.records()` — create, read, update (full and
  partial), delete, and look records up by indexed field.
- **Schemas** — `client.schemas()` — define and evolve record/document schemas.
- **Identity & access** — `client.identity()`, `client.auth()` — manage clients,
  organizations, and users; mint and revoke scoped credentials.

## Full API reference

Every method, parameter, and type is documented in
[`reference.md`](./reference.md).

## Documentation

- **Guides & reference:** [docs.vectros.ai](https://docs.vectros.ai)
- **Product:** [vectros.ai](https://vectros.ai)

## License

[Apache License 2.0](./LICENSE).
