# ADR 0008: Portable cell deployment

Status: preferred candidate, capacity/residency validation required; D12,D15.

Cloudflare edge and R2 where permitted; regional Go/Postgres/realtime/worker workloads on existing Dewee Linux infrastructure with Compose or Kubernetes. Keep home cell independently recoverable; on-prem may use local/private S3, no mandatory Cloudflare dependency. Alternatives all-in edge Durable Objects as canonical DB (portability mismatch) and premature distributed data stack (operational overhead) rejected. Confirm actual provider, latency, compliance, budgets and recovery before production. G02/C05 benchmark and rehearse restore; feature flags and staged rollout, no production deployment by this design.
