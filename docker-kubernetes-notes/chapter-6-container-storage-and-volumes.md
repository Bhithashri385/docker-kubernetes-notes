# Chapter 6: Container Storage & Volumes

### Goal

Understand how Containers store data and why Volumes are required.

#### By the end of this chapter, you will understand:

* Where Container data is stored
* What is a Writable Layer?
* Why Container data is temporary
* What are Volumes?
* Why do we need Volumes?

***

## Before We Start...

Suppose you run your application:

```bash
podman run notes-app
```

Your Container starts successfully.

Now imagine your application creates a file:

```
report.txt
```

Everything works perfectly.

But later you remove the Container.

```bash
podman rm notes-app
```

Now the question is...

> **What happens to report.txt?**

The answer is: It is deleted.

Why?

Let's understand.

***

## Overview

<figure><img src="../.gitbook/assets/ChatGPT Image Jul 30, 2026, 02_34_00 PM (1).png" alt=""><figcaption></figcaption></figure>

***

## Step 1: Images are Read-Only

Remember,

An Image is only a blueprint.

```
Container Image

Application
Python
Libraries
```

Images cannot be modified.

***

## Step 2: Podman Creates a Writable Layer

When a Container starts,

Podman adds a small **Writable Layer** above the Image.

```
Writable Layer
────────────────────
Image Layers
────────────────────
Base Image
```

This Writable Layer stores:

* New files
* Updated files
* Logs
* Temporary data

***

## Step 3: Where Does Data Go?

Suppose your application creates:

```
report.txt
```

It is stored inside the Writable Layer.

```
Writable Layer

report.txt
logs.txt
config.json
```

The original Image never changes.

***

## Step 4: What Happens When the Container is Removed?

Execute:

```bash
podman rm notes-app
```

The Writable Layer is deleted.

```
Container Deleted

↓

Writable Layer Deleted

↓

All Changes Lost
```

Everything stored inside that layer disappears.

***

## Step 5: The Problem

Imagine you have:

* MySQL Database
* PostgreSQL
* User Uploads
* Application Logs

If all data disappears every time a Container is removed,

your application becomes useless.

We need a permanent storage solution.

***

## Step 6: Introducing Volumes

A **Volume** is a storage location outside the Container.

Instead of storing files inside the Writable Layer,

the Container stores them inside the Volume.

<pre><code>  Container
      │
      ▼
<strong>   Volume
</strong>      │
      ▼
  Hard Disk
</code></pre>

Even if the Container is removed,

the Volume still exists.

***

## Step 7: Why Volumes are Important

Without Volume

<pre><code><strong>Container
</strong>    ↓
Delete Container
    ↓
Data Lost ❌
</code></pre>

With Volume

```
Container
   ↓
Volume
   ↓
Delete Container
   ↓
Volume Still Exists 
```

Your data remains safe.

***

## Step 8: Real-World Example

Suppose you run a MySQL Container.

Without Volumes:

```
Database
  ↓
Delete Container
  ↓
Entire Database Lost ❌
```

With Volumes:

```
Database
  ↓
Volume
  ↓
Delete Container
  ↓
Database Still Safe 
```

## Internal Working

```
  Image
   ↓
Container
   ↓
Creates Writable Layer
   ↓
Stores Temporary Data
   ↓
───────────────
OR
───────────────
Stores Data in Volume
   ↓
Permanent Storage
```

***

## Important Commands

Create a Volume

```bash
podman volume create mydata
```

View Volumes

```bash
podman volume ls
```

Remove a Volume

```bash
podman volume rm mydata
```

***

## Key Terms

### Writable Layer

Temporary storage created when a Container starts.

Deleted when the Container is removed.

***

### Volume

Permanent storage managed by Podman.

Data remains even after the Container is deleted.

***

## 📝 Key Takeaways

* Images are Read-only.
* Containers receive a Writable Layer.
* All temporary data is stored inside the Writable Layer.
* Removing a Container deletes the Writable Layer.
* Volumes provide permanent storage.
* Volumes are essential for databases and user data.

***

## ⚡ Quick Revision

✅ Image → Read-only

✅ Writable Layer → Temporary Storage

✅ Volume → Permanent Storage

✅ Container Removal → Deletes Writable Layer

✅ Volume → Keeps Data Safe

***

## 🎯 Interview Questions

#### 1. What is a Writable Layer?

A Writable Layer is a temporary storage layer added to a Container where all changes are stored.

***

#### 2. What happens to the Writable Layer when a Container is removed?

It is deleted along with all data stored inside it.

***

#### 3. Why do we need Volumes?

Volumes provide permanent storage that remains even after a Container is removed.

***

#### 4. Are Images modified when a Container writes data?

No.

Only the Container's Writable Layer is modified.
