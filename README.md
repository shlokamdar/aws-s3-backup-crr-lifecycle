# S3 Backup Architecture with Lifecycle Management & Cross-Region Replication (AWS)

##  Project Overview
Designed and implemented a **robust S3-based backup architecture** to demonstrate AWS data durability, lifecycle management, and cross-region disaster recovery.

The solution uses **versioned and encrypted S3 buckets**, **tag-based cross-region replication**, and **lifecycle policies** to ensure data protection, compliance, and cost optimization.

---

## Architecture Overview

### Regions Used
- **Primary Region:** ap-south-1 (Mumbai)
- **Disaster Recovery Region:** us-east-1 (N. Virginia)

### Bucket Configuration

#### Primary Bucket (Source)
- **Name:** shloka-backup-origin
- **Region:** ap-south-1
- **Versioning:** Enabled
- **Encryption:** SSE-S3
- **Public Access:** Blocked

#### Destination Bucket (Replica)
- **Name:** shloka-backup-destination
- **Region:** us-east-1
- **Versioning:** Enabled
- **Encryption:** SSE-S3
- **Public Access:** Blocked

**Architecture Diagram:**  
![S3 Cross-Region Backup Architecture](Architecture-diagram.png)

---

## Data Flow & Replication Logic

1. Objects are uploaded to the primary S3 bucket
2. Only objects tagged with **env=prod** are selected for replication
3. Selected objects are replicated to the destination bucket in a different region
4. Lifecycle policies manage storage transitions and cleanup of old versions

---

## Security & Compliance Features

- Blocked all public access on both buckets
- Enabled versioning for object recovery and replication support
- Enforced server-side encryption (SSE-S3)
- Used AWS-managed IAM role for least-privilege replication access

---

## Lifecycle & Cost Optimization Strategy

### Lifecycle Rules Applied
- Transition objects to **INTELLIGENT_TIERING after 60 days**
- Expire **noncurrent object versions after 30 days**

### Benefits
- Optimized storage costs for infrequently accessed backups
- Maintained compliance with data retention policies
- Reduced long-term storage overhead

  Check Documentation with steps implementated - ![Documentation](S3-Step-by-Step_Implementation.pdf)

---

## Testing & Validation

### Replication Test
- Uploaded 3 objects with tags:
  - `v1index.html` → env=prod
  - `v2index.html` → env=prod
  - `v3index.html` → env=test

### Result
- Only prod-tagged objects replicated
- Test-tagged object correctly excluded

This validated correct **tag-based replication behavior**.

---

## Challenges & Solutions

| Challenge | Resolution |
|--------|-----------|
| Understanding tag-based replication behavior | Tested with mixed-tag objects to validate filtering |
| Lifecycle rule configuration complexity | Verified each action independently in console |

---

## What This Project Demonstrates

- Enterprise-grade S3 backup design
- Cross-region disaster recovery strategy
- Tag-based selective replication
- IAM role usage for secure replication
- Lifecycle-based cost optimization
- AWS durability and compliance concepts

---

## Future Improvements

- Add S3 Replication Metrics & CloudWatch alarms
- Enable S3 Object Lock for compliance workloads
- Manage infrastructure using Terraform
- Add lifecycle transitions to Glacier tiers
