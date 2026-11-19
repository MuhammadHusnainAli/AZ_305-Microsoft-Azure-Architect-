# Module 3: Describe Azure storage services
Units: Introduction · Storage accounts · Storage redundancy · Storage services · Data migration options · File movement options · Assessment · Summary

```mermaid
flowchart LR
  SA[Storage account<br/>unique namespace] --> B[Blob]
  SA --> F[Files]
  SA --> Q[Queue]
  SA --> T[Table]
  SA -.-> D[Disks are managed separately]
```

## Storage accounts
- A unique namespace: `https://<account>.blob.core.windows.net` (blob), `.file.`, `.queue.`, `.table.`, `.dfs.` (Data Lake Gen2).
- Name: 3–24 characters, lowercase letters and numbers, **globally unique**.

| Account type | Supports | Redundancy |
|---|---|---|
| **Standard general-purpose v2** | Blob (incl. Data Lake), Queue, Table, Files | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS |
| **Premium block blobs** | Block/append blobs, low latency | LRS, ZRS |
| **Premium file shares** | Files (SMB + NFS) | LRS, ZRS |
| **Premium page blobs** | Page blobs only | LRS, ZRS |

## Redundancy (very frequently tested)
```mermaid
flowchart TB
  LRS["LRS: 3 copies, 1 datacenter<br/>11 nines"] --> ZRS["ZRS: 3 copies across 3 zones<br/>12 nines"]
  LRS --> GRS["GRS: LRS + async LRS in paired region<br/>16 nines"]
  ZRS --> GZRS["GZRS: ZRS + async LRS in paired region<br/>16 nines"]
  GRS --> RAGRS[RA-GRS: GRS + read access to secondary]
  GZRS --> RAGZRS[RA-GZRS: GZRS + read access to secondary]
```
- Primary region: **LRS** (survives a disk/rack failure) or **ZRS** (survives a datacenter/zone failure).
- Secondary region: **GRS/GZRS**. The secondary is **not readable** unless you choose **RA-** (read-access). After a failover the secondary becomes primary.
- Asynchronous replication means RPO is usually **< 15 minutes**.

## Storage services
| Service | Use |
|---|---|
| **Blob** | Unstructured objects: images, video, backups, logs, data lake. Access via HTTP/REST |
| **Azure Files** | Managed file shares over **SMB/NFS**, mount on Windows/Linux/macOS; Azure File Sync caches them on Windows Server |
| **Queue** | Messages up to 64 KB, millions of messages, async work between components |
| **Table** | NoSQL key/attribute store, schemaless |
| **Disks** | Block storage for VMs (managed disks) |

### Blob access tiers
| Tier | Use | Minimum retention |
|---|---|---|
| **Hot** | Frequently accessed | — |
| **Cool** | Infrequent access | 30 days |
| **Cold** | Rarely accessed, fast retrieval | 90 days |
| **Archive** | Offline; rehydration takes **hours** | 180 days |
- Only Hot, Cool and Cold can be set at **account level**; Archive is set **per blob**. Use **lifecycle management** policies to move blobs between tiers automatically.

## Data migration options
- **Azure Migrate**: the hub to discover, assess and migrate servers, databases, web apps and VDI.
- **Azure Data Box** family: physical devices for **offline** transfer (up to ~80 TB usable per Data Box) when bandwidth is limited, in either direction (import or export).

## File movement options
| Tool | Use |
|---|---|
| **AzCopy** | Command line copy/sync of blobs and files (one direction only for sync) |
| **Azure Storage Explorer** | GUI that uses AzCopy underneath |
| **Azure File Sync** | Syncs Windows file servers with Azure Files, with **cloud tiering** (keeps hot files locally) |

## Self-check
1. Survive the loss of a region **and** read data during the outage → **RA-GRS or RA-GZRS**
2. Survive a zone failure with data in a single region → **ZRS**
3. Move 200 TB with a slow internet link → **Azure Data Box**
4. Keep frequently used files on an on-premises file server and the rest in Azure → **Azure File Sync with cloud tiering**
