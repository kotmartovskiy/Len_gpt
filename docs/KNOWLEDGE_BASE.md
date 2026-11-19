# Eng Knowledge Base

**Priority: P2 — important infrastructure, non-blocking for current Eng development.**

## Purpose

Create a user-controlled, provider-independent repository of accumulated knowledge so that loss of access to GitHub, Cortex, ChatGPT, another LLM provider, a VPS, or a network path does not destroy the ability to continue work.

This is a cross-project Eng capability. It is not the same thing as Eng Information Gateway: the Gateway acquires remote resources; the Knowledge Base preserves, organizes, and retrieves accumulated knowledge and project state.

## Ownership and authority

- The canonical working copy belongs on user-controlled local storage on Eng (currently Lenovo RD450X), not exclusively in a cloud service.
- GitHub is a version-control and collaboration mirror, not the only copy.
- Cortex is a semantic memory/retrieval service, not a complete archive and not the sole source of truth.
- Raw conversation exports and original attachments are source records. Summaries and curated notes are derivatives linked to those sources.
- Synchronization is not assumed. Every import/export/sync process must record direction, source, timestamp, status, and conflicts.
- Keep the knowledge archive portable: human-readable Markdown/text plus original files and documented metadata. Avoid making a proprietary database or a particular vector store mandatory for recovery.

## Information layers

1. **Source archive** — original conversation exports, attachments, imported files, repository snapshots, and service exports.
2. **Curated knowledge** — normalized technical notes, research, procedures, and facts with provenance.
3. **Project state** — current purpose, architecture, roadmap, status, open questions, risks, and next actions.
4. **Decision history** — decisions, rationale, alternatives considered, date, status, and superseding decisions.
5. **Indexes** — full-text indexes, embeddings, tags, and generated metadata. These are rebuildable and never replace source records.

Do not silently collapse these layers. A summary can be wrong or incomplete; retain the original material needed to audit or reconstruct it.

## Suggested initial layout

The exact paths may change during implementation, but the structure should remain understandable without a special application:

```text
knowledge/
├── README.md
├── INDEX.md
├── projects/
│   ├── eng/
│   ├── eng-information-gateway/
│   ├── ai-gateway/
│   └── phone-vault/
├── decisions/
├── knowledge/
│   ├── infrastructure/
│   ├── hardware/
│   ├── software/
│   ├── research/
│   └── procedures/
├── sources/
│   ├── conversations/
│   ├── cortex-exports/
│   ├── github-snapshots/
│   └── other-imports/
├── attachments/
└── manifests/
```

This is a logical layout, not a requirement to move all existing repositories into one monorepo. Project repositories may remain independent; the Knowledge Base should reference them and retain exportable snapshots where appropriate.

## Provenance and conflict handling

For each imported item, preserve as much as available:
- source service and source identifier/URL;
- original creation/update timestamp, clearly distinguished from import time;
- import timestamp and import tool/version;
- original filename and content type;
- SHA-256 of original bytes;
- project/topic tags and links to related decisions;
- transformation lineage for summaries, extracted text, or converted files.

Distinguish verified facts, user decisions, hypotheses, model-generated interpretations, and unresolved claims. If sources conflict, preserve both, mark the conflict, and record how it was resolved. Do not invent missing dates, source IDs, or certainty.

## Backup and recovery

Apply a practical 3-2-1-style strategy:
- canonical working copy on Eng local storage;
- another copy on a separate physical device;
- a further offline or otherwise failure-independent copy where feasible.

Backups of sensitive information must be encrypted. Recovery keys must be stored separately and safely. Backups must include raw exports, attachments, curated documents, decision records, manifests, database dumps where applicable, and the configuration needed to rebuild indexes. Indexes and embeddings may be regenerated from preserved source material.

Git commits and a second cloud provider are useful additional replicas but do not replace independent physical/offline backups. A backup job reporting success is not proof of recoverability: verify hashes and periodically perform an actual restore test.

Do not put API tokens, passwords, private keys, session cookies, or other secrets into the knowledge repository or ordinary exports. Keep secrets in an appropriate secret store and document only references and recovery procedures.

## Integration boundaries

- **Eng core:** owns local storage, indexing, access policy, project links, and recovery workflow.
- **GitHub:** provides code history and documentation collaboration; import/export must remain possible if GitHub becomes unavailable.
- **Cortex:** provides semantic recall; periodically export relevant records and retain source identifiers locally. Do not assume Cortex is a backup.
- **Conversation providers:** export raw conversations and attachments where supported; preserve them independently of provider accounts.
- **Eng Information Gateway:** exports fetched artifacts and provenance manifests into the Knowledge Base. Gateway failure must not make already stored knowledge inaccessible.
- **LLM providers:** may help classify, summarize, deduplicate, or detect contradictions, but the archive must remain readable and usable without any LLM.

No connector or provider outage should disable local read access. Synchronization adapters must be optional and independently restartable.

## Incremental delivery plan

1. Create the local directory/repository and a human-readable master index.
2. Copy current project documentation and decision records into the canonical local store.
3. Import existing GitHub documentation and Cortex records with provenance; retain raw exports.
4. Import conversation exports and attachments in bounded, verifiable batches.
5. Configure versioned backups to another physical device and an offline/independent destination.
6. Verify hashes and perform a restore test.
7. Only later add semantic search, automated deduplication, contradiction/staleness detection, and bidirectional sync.

## Priority and non-goals

Priority is **P2**: important for long-term continuity, but not a blocker for the first working Eng features or the Eng Information Gateway fetch/store MVP.

Do not start by building a large knowledge-management platform, complex graph database, or mandatory vector database. First establish a portable archive, stable project documents, provenance, and recoverable backups. Add automation only when it removes real manual work.

## Definition of done for the first useful milestone

- Current project documentation and decision records exist in the local canonical store.
- Each imported item can be traced to its source.
- The archive can be browsed as ordinary files without GitHub, Cortex, or an LLM.
- At least one independent backup exists.
- A restore test has been completed and recorded.
