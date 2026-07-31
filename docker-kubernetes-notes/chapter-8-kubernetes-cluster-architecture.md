# Chapter 8: Kubernetes Cluster Architecture

### Goal

Understand how a Kubernetes Cluster works internally and how different components work together to manage applications.

#### By the end of this chapter, you will understand:

* What is a Kubernetes Cluster?
* What is the Control Plane?
* What is a Worker Node?
* What is the API Server?
* What is etcd?
* What is the Scheduler?
* What is kubelet?
* What is kube-proxy?

***

## Before We Start...

In the previous chapter, we learned that Kubernetes runs **Pods**.

Now imagine a company running:

* 500 Servers
* 10,000 Pods
* 100 Applications

Questions arise:

* Which server should run a Pod?
* What happens if a server crashes?
* How does Kubernetes know the current state of the cluster?

To solve these problems,

Kubernetes organizes everything into a **Cluster**.

***

## Overview

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

***

## Step 1: What is a Kubernetes Cluster?

A **Cluster** is a group of computers (called Nodes) working together.

Instead of using one server,

Kubernetes uses multiple servers.

```
          Kubernetes Cluster
        ┌──────────┬──────────┬
        │          │          │          
        ▼          ▼          ▼
     Node 1     Node 2     Node 3
```

This improves:

* Availability
* Scalability
* Reliability

***

## Step 2: Cluster Components

A Kubernetes Cluster has two main parts.

```
Kubernetes Cluster
│
├── Control Plane
│
└── Worker Nodes
```

***

## Step 3: Control Plane

The **Control Plane** is the brain of Kubernetes.

It decides:

* Where Pods should run
* When Pods should restart
* How the cluster should behave

It manages the entire cluster.

```
Control Plane
↓
Manages Everything
```

***

## Step 4: Worker Nodes

Worker Nodes are the machines where your applications actually run.

Each Worker Node contains:

* Pods
* kubelet
* kube-proxy
* Container Runtime

```
Worker Node
│
├── Pod
├── kubelet
├── kube-proxy
└── Container Runtime
```

***

## Step 5: API Server

The **API Server** is the entry point into Kubernetes.

Whenever you run:

```bash
kubectl apply
```

The request first goes to the API Server.

```
User
↓
kubectl
↓
API Server
```

Every Kubernetes component communicates through the API Server.

***

## Step 6: etcd

Kubernetes needs to remember everything.

For example:

* Which Pods are running?
* Which Nodes are available?
* Which Deployments exist?

This information is stored in **etcd**.

```
API Server
↓
etcd
↓
Cluster Data
```

Think of **etcd** as Kubernetes' database.

***

## Step 7: Scheduler

Suppose you create a new Pod.

Which Worker Node should run it?

The **Scheduler** decides.

It checks:

* Available CPU
* Available Memory
* Node Health

Then chooses the best Worker Node.

```
New Pod
↓
Scheduler
↓
Best Worker Node
```

***

## Step 8: kubelet

Every Worker Node runs a program called **kubelet**.

Its job is to:

* Receive instructions
* Create Pods
* Monitor Pods
* Report status

```
Control Plane
↓
kubelet
↓
Pods
```

***

## Step 9: kube-proxy

Pods need to communicate with each other.

The **kube-proxy** handles networking.

It routes traffic between Pods and Services.

```
Pod A
↓
kube-proxy
↓
Pod B
```

***

## Step 10: Complete Architecture

```
                 User
                   │
              kubectl
                   │
                   ▼
             API Server
                   │
     ┌─────────────┼─────────────┐
     ▼             ▼             ▼
 Scheduler       etcd     Control Plane
                   │
         -------------------------
         │                       │
         ▼                       ▼
   Worker Node 1          Worker Node 2
         │                       │
      kubelet                kubelet
         │                       │
        Pods                   Pods
```

***

## Internal Working

```
User
↓
kubectl
↓
API Server
↓
Scheduler
↓
Worker Node
↓
kubelet
↓
Pod Created
```

***

## Key Components

### Control Plane

The brain of Kubernetes.

***

### Worker Node

Runs Pods.

***

### API Server

Entry point of Kubernetes.

***

### etcd

Stores all cluster information.

***

### Scheduler

Selects the best Worker Node.

***

### kubelet

Creates and monitors Pods.

***

### kube-proxy

Handles Pod networking.

***

## Key Takeaways

* A Cluster contains multiple Nodes.
* Control Plane manages the Cluster.
* Worker Nodes run Pods.
* API Server is the entry point.
* etcd stores cluster data.
* Scheduler selects Worker Nodes.
* kubelet manages Pods.
* kube-proxy handles networking.

***

## Quick Revision

✅ Cluster → Collection of Nodes

✅ Control Plane → Brain

✅ Worker Node → Runs Pods

✅ API Server → Entry Point

✅ etcd → Cluster Database

✅ Scheduler → Selects Nodes

✅ kubelet → Manages Pods

✅ kube-proxy → Networking

***

## Interview Questions

#### 1. What is a Kubernetes Cluster?

A Kubernetes Cluster is a collection of Control Plane and Worker Nodes that work together to run containerized applications.

***

#### 2. What is the role of the API Server?

The API Server is the main communication gateway for all Kubernetes operations.

***

#### 3. What is etcd?

etcd is Kubernetes' key-value database that stores the cluster's configuration and state.

***

#### 4. What does the Scheduler do?

The Scheduler selects the most suitable Worker Node for new Pods based on available resources and scheduling policies.

***

#### 5. What is the difference between the Control Plane and a Worker Node?

The Control Plane manages the cluster and makes decisions, while Worker Nodes run the actual Pods and applications.
