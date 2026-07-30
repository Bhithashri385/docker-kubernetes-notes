# Chapter 7: Pods

### Goal

Understand what a **Pod** is, why Kubernetes uses Pods instead of directly managing Containers, and how Pods work internally.

#### By the end of this chapter, you will understand:

* What is a Pod?
* Why are Pods needed?
* What is inside a Pod?
* What resources do Containers inside a Pod share?
* Single Container vs Multi-Container Pods
* Pod Lifecycle

***

## Before We Start...

So far, we have learned:

```
Application
      │
      ▼
Container Image
      │
      ▼
Container
```

Using Podman, we can easily run Containers.

But now imagine a company running **10,000 Containers**.

Can Kubernetes manage every Container individually?

**No.**

Instead, Kubernetes groups Containers into a **Pod**.

A Pod is the smallest unit that Kubernetes manages.

***

## Overview

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

***

## Step 1: What is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes.

Instead of managing Containers directly,

Kubernetes manages **Pods**.

```
Pod
 │
 └── Container
```

A Pod usually contains **one Container**.

However, it can also contain multiple Containers that work together.

***

## Step 2: Why Do We Need Pods?

Imagine your application also needs a logging service.

```
Application

Logging
```

Both should:

* Start together
* Stop together
* Share Storage
* Share Network

Instead of running separately,

Kubernetes places them inside a Pod.

```
┌─────────────────────┐
│        Pod          │
│                     │
│  App Container      │
│  Log Container      │
└─────────────────────┘
```

Now Kubernetes manages them as one unit.

***

## Step 3: What Do Containers Share?

Containers inside the same Pod share:

* Network
* IP Address
* Storage
* Lifecycle

```
              Pod
──────────────────────────
 Shared IP Address
 Shared Network
 Shared Storage
──────────────────────────
│                        │
▼                        ▼
Container A         Container B
```

Because they share the same network,

Containers communicate using:

```
localhost
```

Communication becomes extremely fast.

***

## Step 4: Single Container Pod

Most Pods contain only one Container.

Example:

```
Pod
│
└── Nginx Container
```

This is the most common architecture.

***

## Step 5: Multi-Container Pod

Sometimes multiple Containers must work together.

Example:

```
┌──────────────────────────┐
│          Pod             │
│                          │
│  Web Application         │
│  Logging Container       │
│  Monitoring Agent        │
└──────────────────────────┘
```

All Containers:

* Share Network
* Share Storage
* Start Together
* Stop Together

***

## Step 6: Pod Lifecycle

When Kubernetes creates a Pod:

```
Create Pod
      │
      ▼
Pull Image
      │
      ▼
Create Container
      │
      ▼
Start Application
      │
      ▼
Running Pod
```

If the Pod crashes,

Kubernetes automatically creates a new Pod.

***

## Step 7: Pod vs Container

| Pod                             | Container                 |
| ------------------------------- | ------------------------- |
| Managed by Kubernetes           | Runs the Application      |
| Contains one or more Containers | Contains Application Code |
| Has its own IP Address          | Shares Pod IP             |
| Smallest deployable unit        | Smallest runnable unit    |

***

## Internal Working

```
Application
      │
      ▼
Container
      │
      ▼
Pod
      │
      ▼
Worker Node
      │
      ▼
Kubernetes Cluster
```

Kubernetes always deploys **Pods**, not individual Containers.

***

## Important Facts

* A Pod gets one IP Address.
* Containers inside a Pod communicate using `localhost`.
* Pods are temporary.
* Kubernetes recreates Pods if they fail.
* Most Pods contain only one Container.

***

## Key Terms

### Pod

The smallest deployable unit in Kubernetes.

***

### Single Container Pod

A Pod containing one Container.

***

### Multi-Container Pod

A Pod containing multiple Containers that work together.

***

### Shared Network

All Containers inside a Pod share the same IP Address and network.

***

## 📝 Key Takeaways

* Kubernetes manages Pods, not Containers.
* A Pod contains one or more Containers.
* Containers inside a Pod share networking and storage.
* Pods are temporary.
* Kubernetes automatically recreates failed Pods.
* Most Pods contain only one Container.

***

## ⚡ Quick Revision

✅ Pod → Smallest Deployable Unit

✅ Pod → One or More Containers

✅ Shared IP Address

✅ Shared Network

✅ Shared Storage

✅ Kubernetes → Manages Pods

***

## Interview Questions

#### 1. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes that contains one or more Containers.

***

#### 2. Why does Kubernetes use Pods instead of Containers?

Pods provide shared networking, shared storage, and easier management for one or more Containers.

***

#### 3. Can a Pod contain multiple Containers?

Yes.

Containers that need to work together can be placed inside the same Pod.

***

#### 4. What resources are shared inside a Pod?

Containers share:

* Network
* IP Address
* Storage
* Lifecycle

***

#### 5. What happens if a Pod crashes?

Kubernetes automatically creates a new Pod to replace it.
