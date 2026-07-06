
# Amazon S3 Cheat Sheet

> [!IMPORTANT]
> **Goal:** 5-minute revision before interviews or real-world projects.

---

# 📦 What is Amazon S3?

Amazon S3 (Simple Storage Service) is AWS's **Object Storage** service.

### Common DevOps Use Cases

- Static Website Hosting
- Terraform Remote State
- Store Logs & Backups
- Store Application Artifacts
- Media Storage
- Disaster Recovery

> [!TIP]
> S3 = Object Storage | EBS = Block Storage | EFS = File Storage

---

# 📂 Bucket vs Object

```text
Bucket
├── resume.pdf
├── image.png
└── terraform.tfstate
```

| Component | Purpose |
|-----------|---------|
| Bucket | Container that stores objects |
| Object | Actual file stored inside the bucket |

---

# 💾 Storage Classes

| Storage Class | Use Case |
|---------------|----------|
| Standard | Frequently accessed data |
| Standard-IA | Rarely accessed but immediate retrieval |
| One Zone-IA | Non-critical data in one AZ |
| Glacier Instant Retrieval | Archive with instant access |
| Glacier Flexible Retrieval | Long-term archive |
| Glacier Deep Archive | Very old archive |

> [!TIP]
> Standard → Active | Standard-IA → Rare | Glacier → Archive | Deep Archive → Very Old Archive

---

# 🕒 Versioning

```text
Version 1
    ↓
Version 2
    ↓
Delete Marker
```

- Protects against accidental deletion.
- Preserves previous versions.
- Delete creates a **Delete Marker**, not permanent deletion.

---

# 🌐 Static Website Hosting

Requirements

- Enable Static Website Hosting
- Disable Block Public Access
- Add Bucket Policy
- Upload website files

| Feature | Purpose |
|---------|---------|
| Block Public Access | Removes AWS restriction |
| Bucket Policy | Grants public read permission |

---

# 🔐 Server Side Encryption

| Type | Description |
|------|-------------|
| SSE-S3 | AWS manages encryption keys |
| SSE-KMS | AWS KMS manages keys and permissions |

> [!TIP]
> SSE-KMS replication requires enabling KMS replication and selecting a destination KMS key.

---

# 🔒 Object Lock

| Mode | Can Admin Delete? |
|------|-------------------|
| Governance | Yes (with permission) |
| Compliance | No |

> [!IMPORTANT]
> Object Lock works only when Retention or Legal Hold is configured.

---

# 🔄 Lifecycle Rules

```text
30 Days → Standard-IA
90 Days → Glacier
365 Days → Delete
```

Automates object transition and deletion.

---

# 🌍 Replication

Purpose

- Disaster Recovery
- Cross-Region Backup
- Compliance

```text
Source Bucket
      ↓
Replication
      ↓
Destination Bucket
```

---

# 🌐 CORS (Concept)

Allows browsers to access S3 objects from another origin.

We'll revisit this with CloudFront.

---

# 🎯 Access Points (Concept)

Provides different access policies for different applications.

We'll revisit this after CloudFront.

---

# 💻 Common DevOps Use Cases

- Terraform Remote State
- Static Website Hosting
- Store Build Artifacts
- Logs
- Backups

---

# 📝 Quick Revision

- Bucket = Container
- Object = File
- Versioning = Multiple versions
- Delete = Delete Marker
- Lifecycle = Automate transitions
- Replication = Copy objects
- SSE-S3 = AWS managed keys
- SSE-KMS = KMS managed keys
- Object Lock = Prevent delete/overwrite
- CORS = Cross-origin browser access
- Access Points = Simplified access management
