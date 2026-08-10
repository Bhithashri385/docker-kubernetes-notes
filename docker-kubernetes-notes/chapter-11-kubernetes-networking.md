# Chapter 11: Kubernetes Networking

### Goal

Understand how Pods communicate with each other and how Kubernetes provides networking inside a Cluster.

#### By the end of this chapter, you will understand:

* Pod IP
* Pod-to-Pod Communication
* Services and Networking
* Kubernetes DNS
* CNI

***

## Before We Start...

Pods need to communicate with each other.

For example:

```
Frontend Pod
      ↓
Backend Pod
      ↓
Database Pod
```

But how does this communication happen?

Kubernetes Networking handles this.

## Overview:

<figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

***

## Step 1: Pod IP Address

Every Pod gets its own IP address.

Example:

```
Pod A
IP: 10.1.1.5

Pod B
IP: 10.1.1.6
```

Pods can communicate using these IP addresses.

```
Pod A
10.1.1.5
   │
   │ Request
   ▼
Pod B
10.1.1.6
```

***

## Step 2: Pod-to-Pod Communication

Pods can communicate with other Pods inside the Kubernetes Cluster.

Example:

```
Frontend Pod
      ↓
Backend Pod
      ↓
Database Pod
```

For example:

The Frontend may ask the Backend:

> "Give me the user's profile."

The Backend may ask the Database:

> "Give me this user's information."

This is called **Pod-to-Pod Communication**.

***

## Step 3: Why We Don't Directly Use Pod IPs

Pods are temporary.

Suppose:

```
Backend Pod
IP: 10.1.1.6
```

The Pod crashes.

ReplicaSet creates a new Pod:

```
New Backend Pod
IP: 10.1.1.15
```

The IP changed!

Therefore, applications should not depend directly on Pod IP addresses.

***

## Step 4: Service Provides a Stable Address

Instead of connecting directly to a Pod:

```
Frontend
    ↓
Backend Pod
```

we use a Service:

```
Frontend
    ↓
Backend Service
    ↓
Backend Pods
```

The Service provides a stable way to reach the Pods even when Pods are recreated.

***

## Step 5: Kubernetes DNS

We don't want applications to remember Service IP addresses.

For example:

```
10.96.0.20
```

Instead, we can use a Service name:

```
backend-service
```

Kubernetes DNS finds the Service IP.

```
Frontend Pod
      │
      │ backend-service
      ▼
Kubernetes DNS
      │
      │ Service IP
      ▼
Backend Service
      │
      ▼
Backend Pod
```

#### Remember:

**DNS → Finds the Service**

**Service → Sends traffic to a healthy Pod**

***

## Step 6: What is CNI?

**CNI = Container Network Interface**

CNI provides the networking system that allows Pods to communicate.

Think of CNI as the **road system** connecting different places.

```
Pod A
  │
  │
  └──── Pod Network ────┐
                        │
                      Pod B
```

Some examples of CNI plugins are:

* Calico
* Cilium
* Flannel

You don't need to memorize these now.

Just remember:

> **CNI provides networking for Pods.**

***

## &#x20;Complete Networking Flow

```
Frontend Pod
      │
      │ backend-service
      ▼
Kubernetes DNS
      │
      │ Finds Service
      ▼
Backend Service
      │
      │ Selects healthy Pod
      ▼
Backend Pod
```

The underlying Pod network is provided using **CNI**.

***

## Real-World Example

Imagine an application like Instagram.

```
Instagram
    │
    ▼
Frontend Pod
    │
    ▼
User Service
    │
    ▼
User Pod
    │
    ▼
Database Service
    │
    ▼
Database Pod
```

Different parts of the application run in different Pods and communicate through the Kubernetes network.

***

## Key Terms

#### Pod IP

The IP address assigned to a Pod.

#### Pod-to-Pod Communication

Communication between Pods inside the Cluster.

#### Service

Provides a stable way to access Pods.

#### DNS

Allows applications to find Services using names instead of IP addresses.

#### CNI

Provides the networking mechanism for Pods.

***

## Key Takeaways

* Every Pod gets an IP address.
* Pods can communicate with other Pods.
* Pod IPs can change when Pods are recreated.
* Services provide stable access to Pods.
* DNS helps applications find Services by name.
* CNI provides networking for Pods.

***

## Quick Revision

✅ **Pod IP** → Address of a Pod

✅ **Pod-to-Pod** → Pods communicate with each other

✅ **Service** → Stable access to Pods

✅ **DNS** → Finds Services using names

✅ **CNI** → Provides Pod networking

***

## Interview Questions

#### 1. Do Pods have IP addresses?

Yes. Kubernetes assigns an IP address to every Pod.

#### 2. Why shouldn't we depend directly on Pod IPs?

Because Pods are temporary and their IP addresses can change.

#### 3. Why do we use a Service?

A Service provides a stable way to access Pods.

#### 4. What does DNS do in Kubernetes?

DNS allows applications to find Services using names instead of remembering IP addresses.

#### 5. What is CNI?

CNI (Container Network Interface) provides networking for Pods.
