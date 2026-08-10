# Chapter 10: Services

### Goal

Understand how Kubernetes Services provide a stable way to access Pods and expose applications inside and outside the cluster.

#### By the end of this chapter, you will understand:

* Why do we need Services?
* What is a Kubernetes Service?
* What is ClusterIP?
* What is NodePort?
* What is LoadBalancer?
* How Services communicate with Pods?

***

## Before We Start...

In the previous chapter, we learned that a Deployment creates Pods.

Example:

<pre><code>Deployment
      │
      ▼
ReplicaSet
      │
      ▼
<strong>    Pods
</strong></code></pre>

Suppose you have three Pods.

```
Pod 1
Pod 2
Pod 3
```

Everything works well.

But then one Pod crashes.

Kubernetes automatically creates a new Pod.

```
Pod 1 ❌
↓
Pod 4 ✅
```

Now imagine users were connected directly to **Pod 1**.

When Pod 1 disappears,

the application becomes unreachable.

So how do users always reach the correct Pod?

The answer is:

**Kubernetes Service.**

***

## Overview

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

***

## Step 1: What is a Service?

A **Service** provides a permanent network address for a group of Pods.

Instead of connecting to individual Pods, users connect to the Service.

```
User
   │
   ▼
Service
   │
   ▼
Pods
```

Even if Pods change,

the Service remains the same.

***

## Step 2: Why Do We Need Services?

Pods are temporary.

They can:

* Crash
* Restart
* Get replaced
* Receive new IP addresses

Without a Service:

```
User
↓
Pod
↓
Pod Deleted ❌
```

The connection is lost.

With a Service:

```
User
↓
Service
↓
New Pod 
```

Users never notice the Pod change.

***

## Step 3: ClusterIP

**ClusterIP** is the default Service type.

It allows communication **inside the Kubernetes Cluster**.

```
Pod A
↓
ClusterIP Service
↓
Pod B
```

External users **cannot** access a ClusterIP Service.

***

## Step 4: NodePort

Suppose you want to access your application from outside the cluster.

Use **NodePort**.

```
Internet
↓
Node IP:30080
↓
Service
↓
Pods
```

Kubernetes opens a port (usually between **30000–32767**) on every Worker Node.

Anyone who knows the node's IP and port can access the application.

***

## Step 5: LoadBalancer

In cloud environments (AWS, Azure, GCP),

you usually use a **LoadBalancer** Service.

```
Internet
↓
Cloud Load Balancer
↓
Service
↓
Pods
```

The cloud provider creates a public IP address.

Traffic is automatically distributed across healthy Pods.

***

## Step 6: How a Service Finds Pods

A Service uses **Labels** to identify which Pods belong to it.

Example:

```
Pod 1

Label:
app=nginx
```

```
Pod 2

Label:
app=nginx
```

The Service selects all Pods with:

```
app=nginx
```

If a new Pod has the same label,

the Service automatically includes it.

***

## Internal Working

```
User
   │
   ▼
Service
   │
   ▼
Select Pods using Labels
   │
   ▼
Forward Request
   │
   ▼
Running Pod
```

***

## Types of Services

| Service      | Purpose                                   |
| ------------ | ----------------------------------------- |
| ClusterIP    | Internal communication inside the cluster |
| NodePort     | External access using Node IP and Port    |
| LoadBalancer | External access using Cloud Load Balancer |

***

## Real-World Analogy

Imagine calling a company's customer care number.

```
Customer
↓
Customer Care Number
↓
Available Agent
```

You don't call a specific employee.

You call one number,

and the company connects you to any available agent.

A Kubernetes Service works the same way.

It routes requests to any healthy Pod.

***

## Key Terms

### Service

A stable network endpoint that provides access to one or more Pods.

***

### ClusterIP

Allows communication only within the Kubernetes Cluster.

***

### NodePort

Exposes the application through a port on every Worker Node.

***

### LoadBalancer

Creates an external load balancer to expose applications on cloud platforms.

***

### Labels

Key-value pairs attached to Pods.

Services use Labels to find the correct Pods.

***

## Key Takeaways

* Pods are temporary.
* Services provide a stable network address.
* Services use Labels to find Pods.
* ClusterIP is for internal communication.
* NodePort allows external access through a node.
* LoadBalancer exposes applications to the internet in cloud environments.

***

## Quick Revision

Service → Stable Network Endpoint

ClusterIP → Internal Access

NodePort → External Access via Node

LoadBalancer → Cloud-Based External Access

Labels → Used to Select Pods

***

## Interview Questions

#### 1. Why do we need a Kubernetes Service?

A Service provides a stable network endpoint for Pods, allowing applications to remain accessible even when Pods are recreated.

***

#### 2. What is the default Service type?

ClusterIP.

***

#### 3. What is the difference between ClusterIP and NodePort?

ClusterIP allows internal communication only, while NodePort allows external users to access the application through a node's IP address and port.

***

#### 4. What is a LoadBalancer Service?

A LoadBalancer Service creates an external load balancer (usually from a cloud provider) to expose applications to the internet.

***

#### 5. How does a Service know which Pods to send traffic to?

It uses Labels to identify and route traffic to matching Pods.
