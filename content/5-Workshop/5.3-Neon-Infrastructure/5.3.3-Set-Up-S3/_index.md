---
title : "Initialize and Configure S3"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3.3. </b> "
---

### 5.3.3.1. Create 4 S3 Buckets for the System

Create buckets for the following purposes:

- `frontend`
- `media`
- `audio`
- `logs`

Steps:

1. Open the Amazon S3 Console.
2. Click **Create bucket**.
3. Enter a globally unique bucket name on AWS.
4. Select the same region as your VPC and RDS.
5. Keep **Block all public access** set to Enabled.
6. Click **Create bucket**.
7. Repeat for all 4 buckets.

![Figure 59.](/images/5-Workshop/5.3-Neon-Infracstructure/image059.png)
![Figure 61.](/images/5-Workshop/5.3-Neon-Infracstructure/image061.png)

### 5.3.3.2. Enable Versioning for Media and Audio Buckets

1. Select the `neonfoodmap-media-dev` bucket.
2. Go to the **Properties** tab.
3. Select **Bucket Versioning** → **Edit**.
4. Choose **Enable**.
5. Repeat the same steps for the `neonfoodmap-audio-dev` bucket.

![Figure 63.](/images/5-Workshop/5.3-Neon-Infracstructure/image063.png)
![Figure 65.](/images/5-Workshop/5.3-Neon-Infracstructure/image065.png)
![Figure 67.](/images/5-Workshop/5.3-Neon-Infracstructure/image067.png)
![Figure 69.](/images/5-Workshop/5.3-Neon-Infracstructure/image069.png)
![Figure 71.](/images/5-Workshop/5.3-Neon-Infracstructure/image071.png)
![Figure 75.](/images/5-Workshop/5.3-Neon-Infracstructure/image075.png)
![Figure 77.](/images/5-Workshop/5.3-Neon-Infracstructure/image077.png)
![Figure 78.](/images/5-Workshop/5.3-Neon-Infracstructure/image078.png)
![Figure 80.](/images/5-Workshop/5.3-Neon-Infracstructure/image080.png)
![Figure 82.](/images/5-Workshop/5.3-Neon-Infracstructure/image082.png)
![Figure 83.](/images/5-Workshop/5.3-Neon-Infracstructure/image083.png)
![Figure 85.](/images/5-Workshop/5.3-Neon-Infracstructure/image085.png)

### 5.3.3.3. Configure Lifecycle Rule for Storage Bucket

1. Select the bucket you want to apply the lifecycle rule to.
2. Go to **Management** → **Lifecycle rules**.
3. Click **Create lifecycle rule**.
4. Name it, for example: `Move-to-IA-after-90d`.
5. Select **Apply to all objects in the bucket**.
6. Choose to transition objects to `Standard-IA` after 90 days.

![Figure 87.](/images/5-Workshop/5.3-Neon-Infracstructure/image087.png)

### 5.3.3.4. Block Public Access and Enable ACL for Bucket

1. Select the bucket.
2. Go to the **Permissions** tab.
3. Select **Block public access** → **Edit**.
4. Enable all **Block all public access** options.
5. Save.
6. Go to **Object Ownership** → **Edit**.
7. Select `ACLs enabled`.
8. Select `Bucket owner preferred`.
9. Click **Save changes**.

![Figure 89.](/images/5-Workshop/5.3-Neon-Infracstructure/image089.png)
![Figure 91.](/images/5-Workshop/5.3-Neon-Infracstructure/image091.png)
![Figure 93.](/images/5-Workshop/5.3-Neon-Infracstructure/image093.png)
![Figure 95.](/images/5-Workshop/5.3-Neon-Infracstructure/image095.png)
![Figure 97.](/images/5-Workshop/5.3-Neon-Infracstructure/image097.png)
![Figure 98.](/images/5-Workshop/5.3-Neon-Infracstructure/image098.png)
