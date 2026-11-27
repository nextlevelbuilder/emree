# Infrastructure and operations

## Deploy where

Hosted: Cloudflare DNS/TLS/WAF/CDN and existing Dewee control plane/Workers for identity and cell discovery, private R2 for attachments where residency allows. Regional Linux compute (existing Dewee infrastructure or validated Kubernetes provider) hosts Go collaboration API, realtime gateway, workers, PostgreSQL, Valkey and optional JetStream. DevBox workers are separate compute pool, gVisor where approved. Choose Vietnam/Singapore or other regions only after real latency, capacity, legal and data-residency checks; no assumption about currently provisioned regions or capacity.

Self-hosted/dedicated: Docker Compose for small installations, K8s reference manifests for HA; PostgreSQL + realtime + cache + compatible object store under customer's control. No mandatory Cloudflare service. SQLite server edition only if upstream explicitly supports native collaboration and all durability/authorization requirements; not claim parity in initial release. Desktop SQLite independent of server edition.

## Home cells

Control plane maps workspace -> single home cell. Each cell has its own PG, queues, cache, backups, attachment namespace/policy and runtime capacity. Do not share database across cells. Add cells for capacity, isolate noisy tenants, dedicated placement for residency. Multi-region reads/active-active writes out of scope. Existing cross-cell agent coordination uses authorized peer grants. Future native shared rooms use owner-anchored authority and authenticated relay, separate from tenant placement; see [FEDERATION.md](FEDERATION.md). Migration requires freeze/fence writes, export/restore metadata and objects, verify hashes/cursors/membership, route cutover and rollback; do not edit workspace routing manually while writes can occur.

## Deployment units and scaling

| Unit | Start | Scale by |
| --- | --- | --- |
| Go collaboration API | Module in Dewee, split deployment when measured | p95 latency, requests, pool pressure |
| Realtime gateway | Centrifugo + Valkey PoC | sockets, memory, FD, outbound bandwidth, room fan-out |
| Agent runtime | existing Dewee deployment | concurrent runs, CPU, budget, queue wait |
| Collab workers | bounded PG outbox consumers | oldest job age, retries, throughput |
| Media worker | isolated scan/transform processes | queue age, CPU/RAM, storage egress |
| DevBox controller/session gateway | separate worker trust domain | live sessions, bytes, admitted box resources |
| Device relay | independently bounded connection pool | connected devices and command latency |

Do not introduce Kafka, service mesh, OpenSearch, JetStream or a separate marketplace backend before measured need. Preserve shared runtime workspace PVC semantics; media store is a separate object lifecycle. No ingress to employee machine; device broker connects outbound. Private networking, scoped workload identities, least privilege, TLS; no public Docker socket, DB or broker. Rate-limit at edge and application. Keep secret references in config, use secret manager, no plaintext env examples or production values in public repo.

## Availability and recovery

Proposed GA 99.9% for core collaboration API/realtime recovery over a rolling month; define incident exclusions explicitly. Model provider, device network and third-party compute separate SLOs. Start with PG HA and synchronous durability policy appropriate to accepted ACK definition, PITR/backups plus restore rehearsal. Proposed regional backup RPO <=15 min, RTO <=4h subject to owner/contract; distinguish from zero acknowledged-message loss in single replica/process fault tests. Cross-region disaster may lose data within agreed RPO unless stronger replication proven. Store signed release metadata separately, object versions/lifecycle, keys recoverable via authorized process. Document worker failure and DevBox volume locality: not magically portable across hosts.

## Operations

- Health/readiness probes; connection draining on deploy; WebSocket reconnect with jitter. Rolling deployment only with contract compatibility and reversible schema changes (expand/migrate/contract).
- OpenTelemetry trace correlation (request, event, room, run, approval, session); Prometheus/Grafana dashboards; bounded redacted logs, no message text by default. Metrics aggregate and label cardinality bounded; optional diagnostic collection opt-in.
- Alerts for DB saturation, outbox age, realtime disconnect/error rate, replay gaps, quota rejects, media backlog, lease failure and unauthorized access attempts. Runbooks for broker outage, PG failover, lost cell, revoked session, object scan outage, compromised plugin and client update rollback.
- Signed macOS notarized/Windows signed/Linux packages as applicable; signed updater metadata, verified hashes, staged rollout, rollback compatible with schema, enterprise mirror. Build matrix with actual target OS versions validated, no unsigned production installers.
- Budget tracking per workspace for agent runs, DevBox compute/disk, attachments and media egress; no unlimited anonymous work. Load-test before setting commercial caps.

## Capacity stages

Alpha: 1k simultaneous test sockets/cell; one small staging deployment, PG outbox and realtime candidate, R2/S3, synthetic workloads. Beta: benchmark 10k/cell; HA PG, realtime replicas, aggregate worker quotas; evaluate JetStream only if PG bottleneck proven. GA: SLO/security/restore evidence, on-prem reference deployment and operator docs. Multi-cell 100k target is a future benchmark, not a capacity claim.

## Provider seams

Versioned interfaces: RealtimeTransport, JobQueue, CacheStore, ObjectStore, MediaProcessor, SearchProvider, ComputerProvider, ExecutionProvider, optional VoiceVideoProvider. Only trusted operators choose infrastructure providers. Validate feature/capability compatibility; changing durable backend requires drain/migration/rollback, not blind hot reload. Cloudflare Durable Objects is a potential edge transport, not canonical room database. See [REALTIME.md](REALTIME.md) for data/traffic and [SECURITY.md](SECURITY.md) for gates.
