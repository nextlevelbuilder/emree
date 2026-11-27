# ADR 0005: Attachments and object storage

Status: proposed baseline; D08,D12.

Use private R2/S3 compatible store with authoritative PG metadata and separate scan/transform workers; local/S3 provider for self-hosted. Alternative DB blobs or WebSocket binary media rejected for bandwidth and lifecycle isolation. Do not repurpose active runtime filesystem backend. Short scoped upload grants, verify actual size/type/hash/quota, no read until scan ready; fresh download ACL, short URL or authenticated proxy; residency/retention policy. A05 prove quarantine, unauthorized read and retry cleanup; provider outage disables new attachments without breaking messaging.
