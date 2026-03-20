# ⚡ Azure Storage Account — Commands & Quick Reference

---

## Azure Portal Navigation (Quick Paths)

| Task | Portal Path |
|---|---|
| Find your Tenant ID | Search "Tenant Properties" |
| View Subscriptions | Search "Subscriptions" |
| Create Resource Group | Search "Resource Groups" → Create |
| Create Storage Account | Search "Storage Accounts" → Create |
| Access Containers (Blobs) | Storage Account → Containers |
| View Access Keys | Storage Account → Access Keys |
| Generate SAS Token | Storage Account → Shared Access Signature |
| Set Lifecycle Rules | Storage Account → Lifecycle Management |
| Change Redundancy | Storage Account → Redundancy |
| Set Default Access Tier | Storage Account → Configuration |

---

## Blob Operations (Portal)

```
Upload a blob:
  Storage Account → Containers → [your container] → Upload

Edit a text blob:
  Storage Account → Containers → [blob] → Edit

Change blob access tier:
  Storage Account → Containers → [blob] → Change Tier

Make container public:
  1. Storage Account → Configuration → Enable anonymous access
  2. Container → Change access level → Blob
```

---

## Access Tier Quick Reference

```
Hot    → Frequently accessed → Low transaction cost, High storage cost
Cool   → Less frequently     → Medium transaction, Medium storage cost
Archive → Backup only        → Lowest storage cost, Highest transaction cost
                              → OFFLINE — cannot read/write directly

To access Archive blob:
  Rehydrate → Change Tier → Hot or Cool
  Standard: ~24 hours | High Priority: ~1–2 hours
```

---

## Lifecycle Management Rule Setup

```
Policy → Add Rule → Name the rule
  → Condition: "If blob was last modified more than X days ago"
  → Action: "Move to Cool / Archive / Delete"

Add filter to scope rule:
  → Filter: telugu/        (all blobs in 'telugu' container)
  → Filter: telugu/a       (blobs in 'telugu' container starting with 'a')

Enable last-accessed tracking:
  Storage Account → Lifecycle Management → Enable access tracking
```

---

## Connection String Format

```
DefaultEndpointsProtocol=https;
AccountName=<your-storage-account-name>;
AccountKey=<your-access-key>;
EndpointSuffix=core.windows.net
```

---

## SAS Token Permissions Reference

| Permission | Symbol | Allows |
|---|---|---|
| Read | r | View/download blobs |
| Write | w | Upload/modify blobs |
| Delete | d | Delete blobs |
| List | l | List containers/blobs |
| Add | a | Add blobs |
| Create | c | Create blobs |

---

## Redundancy Types Quick Reference

```
LRS   = 3 copies, 1 data center
ZRS   = 3 copies, 3 zones (same region)
GRS   = 6 copies, 2 regions (LRS in each)
GZRS  = 6 copies, 2 regions (ZRS in primary)
RA-*  = Read Access to secondary region

Changing redundancy:
  Storage Account → Redundancy → Select type
```

---

## Azure Infrastructure Reference

```
Geography > Region > Availability Zone > Data Center > Server

Region Pair criteria:
  - Same geography
  - Min 300 miles (~500km) apart

Notable regions in India:
  - South India     → Chennai
  - Central India   → Pune
  - South Central   → Hyderabad (coming soon)
```

---

## Naming Constraints Cheat Sheet

| Resource | Constraint |
|---|---|
| Resource Group | Alphanumeric + `_`, `-`, `()` · Unique within subscription |
| Storage Account | 3–24 chars · Lowercase letters and numbers only · **Globally unique** |
| Container | Lowercase letters, numbers, hyphens · 3–63 chars |
