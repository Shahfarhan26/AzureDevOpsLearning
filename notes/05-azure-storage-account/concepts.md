# 05 — Azure Storage Account

> **Source:** Uday Academy — Azure DevOps Course (Lectures 3–6)  
> **Topics:** Data types, Storage Account, Blob Storage, Access Tiers, Lifecycle Management, Access Keys, SAS Tokens, Azure Infrastructure, Redundancy

---

## 1. Data — The New Oil

> **"Without data, there is no software application."**

Everything in a software app is driven by data — product listings, login credentials, images, booking history, payment info.

**Examples of data:** username/password, images, videos, audio files, text files, Excel files, .exe files — anything and everything in IT.

### Types of Data

| Type | Structure | Rules | Storage Format | Examples |
|---|---|---|---|---|
| **Structured** | Has strict structure | Must be followed exactly | Tables (rows & columns) | SQL databases, relational DBs |
| **Semi-Structured** | Has structure/syntax | Can deviate from it | JSON, Collections | JSON files, NoSQL (MongoDB) |
| **Unstructured** | No structure | No rules | Raw files | Images, videos, audio, .txt, .exe |

#### Structured Data — Example Table
```
| Employee Name | Age | Hobbies        |
|---------------|-----|----------------|
| Raj           | 27  | Music, Cricket |
| Jerry         | 54  | null           |  ← must keep null, can't skip column
```

#### Semi-Structured Data — Same Data in JSON
```json
{
  "employees": [
    { "name": "Raj", "age": 27, "hobbies": ["music", "cricket"] },
    { "name": "Jerry", "age": 54 }   ← hobbies skipped — allowed in JSON!
  ]
}
```

---

## 2. Azure Storage Account

> **Storage Account = Azure resource used to store unstructured data.**

Why start here? It's simpler to understand than a server — great for getting familiar with Azure.

### Services Inside a Storage Account

When you create a storage account, Azure creates 4 service types:

| Service | Azure Name | Best For |
|---|---|---|
| **Blob Service** | Containers | Unstructured data (images, videos, files) ✅ |
| **Queue Service** | Queues | Semi-structured (message queuing) |
| **Table Service** | Tables | Structured + semi-structured |
| **File Service** | File Shares | File sharing across VMs |

> **Our focus: Blob Service → implemented in Azure as "Containers"**

### Blob
**BLOB = Binary Large Object** — any file uploaded to a storage account is called a blob.

### Storage Account Hierarchy
```
Storage Account
    └── Container  (like a folder)
            └── Blob  (your actual file)
```
You cannot upload a file without first creating a container.

### Storage Account Naming Rules
- **3–24 characters**, lowercase letters and numbers only
- No uppercase, no special characters
- **Name must be globally unique** (not just within your subscription — across all of Azure worldwide)

> Like an Instagram username — if someone already took it, you must choose something else.

### Creating a Storage Account (Steps)
1. Search "Storage Account" in Azure Portal
2. Click **Create**
3. Select **Subscription** + **Resource Group**
4. Give a **globally unique name**
5. Select a **Region**
6. Keep other defaults → **Review + Create**

---

## 3. Working with Blobs

### Uploading Files
1. Go to Storage Account → **Containers**
2. Create a container (give it a name)
3. Click **Upload** inside the container → browse and upload files

### Making a Blob Public (accessible via URL)
By default blobs are **private** (not accessible via URL). To expose:
1. Go to Storage Account → **Configuration**
2. Enable **"Allow Blob anonymous access"**
3. Go to the container → **Change access level** → set to **Blob**
4. Now the URL works publicly

### Blob URL Format
```
https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>
```

---

## 4. Storage Account Pricing Factors

Two factors affect your monthly bill:

| Factor | Description |
|---|---|
| **Storage Used** | How much data (GB) stored per month |
| **Transactions** | Number of read/write/delete operations on blobs |

> More storage = more cost. More transactions = more cost.

---

## 5. Access Tiers

Access tiers let you **optimize cost** based on how frequently you access data.

### The Three Tiers

| Tier | Best For | Storage Cost | Transaction Cost |
|---|---|---|---|
| **Hot** | Frequently accessed data | Highest | Lowest |
| **Cool** | Less frequently accessed | Medium | Medium |
| **Archive** | Backup / rarely accessed | Lowest | Highest |

> **Trend:** Moving from Hot → Cool → Archive: Storage cost ↓, Transaction cost ↑

### Example Cost Analysis (10,000 GB, 1 lakh transactions/month)

| Tier | Storage Cost (₹/GB/month) | Transaction Cost (₹/transaction) | Total |
|---|---|---|---|
| Hot | ₹1.5 → ₹15,000 | ₹0.5 → ₹50,000 | **₹65,000** |
| Cool | ₹0.8 → ₹8,000 | ₹1.0 → ₹1,00,000 | **₹1,08,000** |
| Archive | ₹0.08 → ₹800 | ₹500 → **₹5 crore** | ❌ Not viable |

> Hot is best for **high transaction volume**. Archive only makes sense when you'll barely access the data.

### Online vs Offline Tiers
| Category | Tiers | Can Read/Write? |
|---|---|---|
| **Online** | Hot, Cool | ✅ Yes, immediately |
| **Offline** | Archive | ❌ No — must rehydrate first |

### Archive & Rehydration
When a blob is in **Archive** tier, you **cannot read, write, or download** it.

To access it again → **Rehydration** = change tier from Archive back to an online tier (Hot or Cool).

| Rehydration Priority | Time Taken |
|---|---|
| **Standard** | ~24 hours (up to 4–6 hours sometimes) |
| **High** | ~1–2 hours (costs more) |

### Setting Access Tiers
- Access tier is set at the **blob level** (not container level)
- Default tier for new uploads = storage account's default (usually Hot)
- The tag **"Hot (inferred)"** means the blob inherited the default tier from the storage account

---

## 6. Lifecycle Management

**Goal:** Automatically change access tiers (and delete) blobs based on conditions — to save cost without manual work.

> Like a school rule: "After 30 days on notice board, move it to the archive room. After 1 year, shred it."

### Hierarchy
```
Lifecycle Management Policy
    └── Rule (can have multiple)
            └── Conditions + Actions (if X days → do Y)
```

### Common Conditions (reference points)
| Reference | Meaning |
|---|---|
| **Creation date** | Days since blob was created |
| **Last modified date** | Days since last write/update |
| **Last accessed date** | Days since anyone read the blob (must enable access tracking first) |

### Example: Bookmyshow Movie Poster Policy
```
If last modified > 30 days  → change to Cool
If last modified > 180 days → change to Archive
If last modified > 3650 days (10 years) → Delete blob
```

### Example Calculation
- Blob created: Jan 1, last modified: Mar 1
- Rule: "If not modified for 30 days → move to Cool"
- Moved to Cool on: **April 1** (30 days after last modification)
- Rule: "If not modified for 180 days → Archive"
- Archive on: **September 1** (180 days after Mar 1)

> **Important:** Moving to Cool does NOT auto-revert to Hot if file is modified. New modification just resets the "last modified date" clock.

### Filtering Rules to Specific Blobs
By default rules apply to all blobs. You can scope rules by:
- **Container name** — e.g., `telugu/` applies only to Telugu container
- **Blob prefix** — e.g., `telugu/a` applies only to blobs in Telugu container starting with "a"

**Example:** Rule with filter `telugu/a` applies to:
- ✅ `telugu/a.png`
- ✅ `telugu/a4.png`
- ❌ `telugu/b2.png` (doesn't start with 'a')
- ❌ `hindi/a.png` (wrong container)

---

## 7. Accessing Storage Account

### Method 1: Azure Portal (Username + Password)
Log into portal.azure.com → navigate to storage account. Access granted via your Azure credentials.

### Method 2: Azure Storage Explorer (Access Keys)

**Access Keys** = Master keys to your storage account. Created automatically by Azure (always 2 keys).

| Real Life | Azure Equivalent |
|---|---|
| Almirah (safe) | Storage Account |
| Valuables inside | Your data (blobs) |
| Lock key | Access Key |
| Address of almirah | Storage Account details |
| Key + Address note | **Connection String** |

**Connection String = Access Key + Storage Account Details**  
Used to connect to the storage account from external tools.

#### Access Key Facts
- Azure creates **2 access keys** by default (like spare keys)
- Both work equally — use either one
- They are like **master/root passwords** — full permissions to do anything
- Can be **rotated** (regenerated) if compromised — old key becomes invalid
- Find them: Storage Account → **Access Keys**

#### Connecting via Storage Explorer
1. Download **Azure Storage Explorer** (free software)
2. Click `+` → Connect to a resource → Storage Account
3. Paste your **Connection String**
4. You can now upload, download, edit, delete files directly

**Advantage:** Edit files with local apps (Notepad, image editors), then auto-sync changes back to Azure.

---

## 8. SAS Tokens (Shared Access Signature)

### Problem with Access Keys
Access Keys are **root passwords** — full permissions, no restrictions. If someone gets it, they can do anything. Also, you can't track who did what.

### Solution: SAS Token
> SAS = Shared Access Signature — a key with **configurable, limited permissions**.

You control:
| Parameter | Example |
|---|---|
| **Permissions** | Read only, Read+Write, no Delete |
| **Validity period** | Valid from 9 AM to 5 PM today only |
| **Allowed IP addresses** | Only requests from IP `192.168.x.x` are allowed |
| **Scope** | Only a specific container, not the whole storage account |

### How to Generate a SAS Token
1. Storage Account → **Shared Access Signature**
2. Select permissions (e.g., Read + List only)
3. Set start and expiry date/time
4. Optionally add allowed IP range
5. Click **Generate SAS and Connection String**
6. Copy the SAS token or Connection String

### Using a SAS Token in Storage Explorer
1. Click `+` → Connect → Storage Account → paste SAS connection string
2. You can access only what the token allows — write operations will fail if you only have Read permission

### Container-Level SAS
You can also generate a SAS scoped to **just one container** — the person can only see that container's files, nothing else.

---

## 9. Azure Infrastructure

### Hierarchy (from largest to smallest)

```
Geography
    └── Region
            └── Availability Zone
                    └── Data Center
                            └── Servers
```

### Geography
- One or more **regions** grouped by country/continent boundary
- Examples: United States (9 regions), India (3 regions), China, Australia

### Region
- A group of **data centers** in a specific physical location
- You **select the region** when creating any Azure resource
- Examples: East US 2 (Virginia), South India (Chennai), Central India (Pune), South Central India (Hyderabad — coming soon)

### Data Center
- A **secured facility** housing thousands of servers
- Controlled temperature, highest security
- We don't choose which data center — Azure manages this

### Availability Zones
- **Logical grouping** of data centers within a region
- Typically **3 zones** per region (Zone 1, Zone 2, Zone 3)
- **Not all regions support Availability Zones** (e.g., South India doesn't; Central India has 3 zones)
- Purpose: If one zone's data center fails, others still serve your data

### Region Pairs
Two regions are a **Region Pair** if:
1. Both are in the **same geography**
2. They are at least **300 miles (~500 km) apart** physically

Purpose: Store backup data in a physically distant region — protection against regional disasters (earthquakes, floods, etc.)

Examples:
- East US 2 ↔ Central US
- Australia East ↔ Australia Southeast

---

## 10. Data Redundancy (Replication Types)

**Goal:** Never lose data. Store multiple copies in smart locations.

### The Degree Certificate Analogy
- Take **multiple copies** of your certificate
- Store them in **different locations** (not all in one place)
- More distance between copies = more protection

### Redundancy Types

#### LRS — Locally Redundant Storage
- **3 copies**, all in **1 data center**
- ⚠️ Vulnerable: if the data center fails, all copies are gone
- Cheapest option

#### ZRS — Zone Redundant Storage
- **3 copies**, one per **availability zone** in the same region
- ✅ Tolerant to single data center failure
- ⚠️ Vulnerable: regional disaster (earthquake) can impact all zones

#### GRS — Geo-Redundant Storage
- **6 copies total**: 3 in **primary region** (LRS style) + 3 in **secondary region** (paired region)
- ✅ Tolerant to regional disasters
- ⚠️ Secondary region is **read-only by default** (you can't access it unless primary fails)

#### RA-GRS — Read-Access Geo-Redundant Storage
- Same as GRS but secondary region is also **readable anytime**

#### GZRS — Geo-Zone Redundant Storage
- **6 copies**: 3 in primary region spread across **zones** (ZRS style) + 3 in secondary region
- Maximum protection

#### RA-GZRS
- GZRS + read access to secondary region

### Summary Table

| Type | Copies | Locations | Protection Against |
|---|---|---|---|
| LRS | 3 | 1 data center | Hardware failure |
| ZRS | 3 | 3 zones, 1 region | Zone/data center failure |
| GRS | 6 | 2 regions | Regional disaster |
| RA-GRS | 6 | 2 regions | Regional disaster + readable secondary |
| GZRS | 6 | 3 zones (primary) + 1 region (secondary) | Maximum coverage |

### SLA (Service Level Agreement)
More redundancy = higher SLA (availability guarantee).

| Type | SLA |
|---|---|
| LRS | 99.9% |
| ZRS | 99.99% (four nines) |
| GRS/GZRS | 99.9999%+ (six+ nines) |

> **99.9% means ~1 min 26 seconds of downtime per day at most.**

Setting redundancy: Storage Account → **Redundancy** → choose your replication type.

---

## 📝 Key Takeaways

- **Data types:** Structured (SQL tables), Semi-structured (JSON/NoSQL), Unstructured (images/files)
- **Storage Account** = Azure resource for storing unstructured data, accessed via **Containers (Blob Service)**
- **Blob** = any file stored in a container (Binary Large Object)
- **Access Tiers:** Hot (frequent) → Cool (infrequent) → Archive (backup/never accessed)
- **Lifecycle Management** = Auto-change tiers based on age/last-modified rules → cost savings
- **Access Keys** = master keys (full access) — use cautiously
- **SAS Tokens** = limited, time-bound, IP-restricted access — preferred for sharing
- **Azure Infrastructure:** Geography → Region → Availability Zone → Data Center → Server
- **Region Pairs** = same geography + 300+ miles apart — used for disaster recovery
- **Redundancy Types:** LRS → ZRS → GRS → GZRS (increasing protection and cost)
