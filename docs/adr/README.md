# Architecture decision records

[DECISIONS.md](../../DECISIONS.md) is the decision register (D01–D18). These initial records are proposed planning baselines or spike candidates, not validated production decisions.

| ADR | Subject |
| --- | --- |
| [0001](0001-greenfield-desktop.md) | Greenfield native desktop |
| [0002](0002-native-collaboration.md) | Native collaboration and home cells |
| [0003](0003-realtime-outbox.md) | Realtime transport and durable delivery |
| [0004](0004-plugin-kernel.md) | Plugin-first, trusted kernel |
| [0005](0005-object-storage.md) | Attachments and object storage |
| [0006](0006-devbox-workbench.md) | DevBox Workbench and session execution |
| [0007](0007-device-broker.md) | Attended Computer Use |
| [0008](0008-deployment.md) | Portable cell deployment |
| [0009](0009-native-onboarding.md) | Native onboarding through Dewee SSO |
| [0010](0010-cross-workspace.md) | Shared contexts across workspace boundaries |

Before implementing a spike-dependent or irreversible decision, record status (proposed/accepted/superseded), context, constraints, alternatives, consequences, security boundary, test evidence, compatibility, migration, rollback and review owner. Link the plan task and requirement. Record why complexity is warranted and what evidence would change the decision. Never overwrite accepted decisions without a superseding record.
