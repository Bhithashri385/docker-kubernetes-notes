# Chapter 9: Deployments

### Goal

Understand how Kubernetes Deployments manage Pods, keep applications running, and make scaling easy.

#### By the end of this chapter, you will understand:

* What is a Deployment?
* Why do we need Deployments?
* What is a ReplicaSet?
* What is Scaling?
* What are Rolling Updates?
* What is Rollback?

***

## Before We Start...

In the previous chapter, we learned how Kubernetes creates Pods.

Suppose we create one Pod.

```
Pod
```

Everything works perfectly.

But suddenly the Pod crashes.

What now?

Do we manually create another Pod?

**No.**

Kubernetes uses a **Deployment** to automatically create a new Pod.

***

## Overview

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

***

## Step 1: What is a Deployment?

A **Deployment** is a Kubernetes object that manages Pods.

It ensures that the required number of Pods are always running.

```
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
    Pods
```

Think of a Deployment as a **manager**.

It continuously checks whether your application is healthy.

***

## Step 2: Why Do We Need a Deployment?

Imagine your application has only one Pod.

```
Pod
↓
Crash ❌
```

Your application becomes unavailable.

With a Deployment:

```
Deployment
↓
Pod Crashes
↓
New Pod Created    
```

Your application stays available.

***

## Step 3: What is a ReplicaSet?

A **ReplicaSet** ensures that the desired number of Pods are running.

Example:

Desired Pods = 3

```
ReplicaSet
│
├── Pod 1
├── Pod 2
└── Pod 3
```

If one Pod crashes,

ReplicaSet immediately creates another Pod.

***

## Step 4: Scaling

Suppose your website becomes popular.

One Pod is no longer enough.

You increase the replicas.

Before:

```
Deployment
↓
1 Pod
```

After scaling:

```
Deployment
↓
3 Pods
```

Kubernetes automatically creates the additional Pods.

***

## Step 5: Rolling Updates

Suppose your application changes from:

Version 1 → Version 2

Instead of stopping everything,

Kubernetes replaces Pods **one at a time**.

```
Pod v1
↓
Pod v2
↓
Pod v2
↓
Pod v2
```

Users continue using the application without downtime.

***

## Step 6: Rollback

Suppose Version 2 has a bug.

You can return to Version 1.

```
Version 1
↓
Update
↓
Version 2 ❌
↓
Rollback
↓
Version 1 ✅
```

Rollback makes updates safe.

***

## Internal Working

```
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Creates Pods
      │
      ▼
Pods Running
```

If a Pod crashes:

```
Pod Deleted
↓
ReplicaSet Detects
↓
New Pod Created
```

***

## Key Terms

### Deployment

Manages Pods and keeps the application running.

***

### ReplicaSet

Ensures the required number of Pods are always running.

***

### Scaling

Increasing or decreasing the number of Pods.

***

### Rolling Update

Updating Pods one by one without downtime.

***

### Rollback

Returning to a previous working version.

***

## &#x20;Key Takeaways

* Deployment manages Pods.
* ReplicaSet maintains the required number of Pods.
* Scaling increases or decreases Pods.
* Rolling Updates avoid downtime.
* Rollback restores a previous version if an update fails.

***

## &#x20;Quick Revision

✅ Deployment → Manages Pods

✅ ReplicaSet → Maintains Pod Count

✅ Scaling → More or Fewer Pods

✅ Rolling Update → Update Without Downtime

✅ Rollback → Return to Previous Version

***

## &#x20;Interview Questions

#### 1. What is a Deployment?

A Deployment is a Kubernetes object that manages Pods and ensures the desired number of Pods are running.

***

#### 2. What is a ReplicaSet?

A ReplicaSet ensures that the required number of Pod replicas are always running.

***

#### 3. Why do we use Deployments instead of creating Pods directly?

Deployments provide automatic recovery, scaling, updates, and rollback, making applications easier to manage.

***

#### 4. What is Scaling?

Scaling is increasing or decreasing the number of Pod replicas based on application needs.

***

#### 5. What is a Rolling Update?

A Rolling Update replaces old Pods with new Pods gradually, allowing updates with little or no downtime.

***

#### 6. What is Rollback?

Rollback restores the application to a previous stable version if a new deployment fails.
