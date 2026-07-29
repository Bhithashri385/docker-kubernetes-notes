# Chapter 1 - Docker & Kubernetes Fundamentals

## Docker & Kubernetes Basics

### Introduction

Before learning Kubernetes, it's important to understand the relationship between **Images, Containers, Pods, Nodes, and Clusters**. These are the building blocks of containerized applications.

***

## 1. What is a Server?

A **server** is simply a computer that is always running and is used to host applications.

Unlike our personal computers, servers are designed to:

* Run applications 24/7
* Handle many users at the same time
* Have higher CPU, memory, and storage capacity

**Example:**

When you open Instagram or YouTube, your request goes to a server where the application is running.

```
User
  │
  ▼
Internet
  │
  ▼
Server
  │
  ▼
Application
```

> **Note:** In Kubernetes, a server is called a **Node**.

***

## 2. What is an Image?

An **Image** is a **blueprint** or **template** of an application.

It contains everything required to run the application:

* Application code
* Runtime (Python, Java, Node.js, etc.)
* Libraries
* Dependencies
* Configuration
* Startup command

An image **cannot run by itself**.

#### Analogy

A recipe tells you how to make a cake, but the recipe itself is not edible.

**Recipe = Image**

```
Image
│
├── Application Code
├── Runtime
├── Libraries
├── Dependencies
└── Start Command
```

***

## 3. What is a Container?

A **Container** is a **running instance of an Image**.

When an image is executed, it becomes a container.

```
Image
   │
   ▼
Container
```

Multiple containers can be created from the same image.

```
Image

├── Container 1
├── Container 2
└── Container 3
```

#### Analogy

* Recipe = Image
* Baked Cake = Container

***

## Why Do We Need Containers?

Without containers, all applications are installed directly on the server.

Example:

```
Server
│
├── Notes App
├── Banking App
├── Shopping App
└── Music App
```

Problems:

* Different applications may require different software versions.
* Dependencies can conflict.
* One application can affect another.

Containers solve this problem by isolating applications.

```
Server
│
├── Container
│     └── Notes App
│
├── Container
│     └── Banking App
│
└── Container
      └── Shopping App
```

Each container has its own:

* Runtime
* Libraries
* Dependencies
* Environment

This isolation prevents conflicts.

***

## 4. What is Docker?

**Docker** is a platform used to create, package, and run containers.

Docker takes an image and starts it as a container.

```
Server
│
└── Docker
      │
      ├── Container 1
      ├── Container 2
      └── Container 3
```

Simply put:

* Image → Blueprint
* Docker → Runs the blueprint
* Container → Running application

***

## 5. What is a Pod?

A **Pod** is the **smallest deployable unit in Kubernetes**.

Kubernetes does **not** manage containers directly.

Instead, it places one or more containers inside a Pod.

```
Pod
│
└── Container
```

Sometimes a pod contains multiple related containers.

Example:

```
Pod
│
├── Main Application
└── Logging Container
```

Containers inside a pod share:

* Network
* Storage
* IP Address

#### Analogy

Think of a Pod as a **Lunch Box**.

The lunch box may contain:

* Rice
* Curry
* Spoon

Everything travels together.

Similarly, a pod groups related containers together.

***

## 6. What is a Node?

A **Node** is a server that runs Pods.

Node = Server

```
Node
│
├── Pod
│     └── Container
│
├── Pod
│     └── Container
│
└── Pod
      └── Container
```

***

## 7. What is a Cluster?

A **Cluster** is a collection of multiple Nodes (servers) working together.

```
Cluster
│
├── Node 1
├── Node 2
├── Node 3
└── Node 4
```

Instead of relying on a single server, applications are distributed across many servers.

Advantages:

* High Availability
* Better Performance
* Easy Scaling
* Fault Tolerance

***

## 8. What is Kubernetes?

**Kubernetes (K8s)** is an open-source platform that manages containers running inside Pods across a Cluster.

Kubernetes automatically:

* Creates Pods
* Deletes Pods
* Restarts failed Pods
* Scales applications up or down
* Schedules Pods on Nodes
* Moves Pods if a Node fails

```
              Kubernetes
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     Node 1      Node 2      Node 3
        │           │           │
      Pods        Pods        Pods
        │           │           │
   Containers  Containers  Containers
```

Kubernetes acts like the **manager** of the entire cluster.

***

## Complete Architecture

```
Developer
    │
    ▼
Build Image
    │
    ▼
Docker runs the Image
    │
    ▼
Container
    │
    ▼
Pod
    │
    ▼
Node (Server)
    │
    ▼
Cluster (Multiple Nodes)
    │
    ▼
Kubernetes manages the Cluster
```

***

## Real-World Example

Imagine you develop a **Notes Application**.

1. Write the application code.
2. Create an Image containing the code and dependencies.
3. Docker runs the Image as a Container.
4. Kubernetes places the Container inside a Pod.
5. The Pod runs on a Node (Server).
6. Multiple Nodes together form a Cluster.
7. Kubernetes manages the entire Cluster.

***

## Summary

| Component      | Meaning                                                                   |
| -------------- | ------------------------------------------------------------------------- |
| **Server**     | A computer that hosts applications.                                       |
| **Node**       | A Server in Kubernetes.                                                   |
| **Image**      | Blueprint containing the application and everything required to run it.   |
| **Container**  | A running instance of an Image.                                           |
| **Docker**     | Platform used to build and run containers.                                |
| **Pod**        | Smallest deployable unit in Kubernetes containing one or more containers. |
| **Cluster**    | A collection of multiple Nodes (servers).                                 |
| **Kubernetes** | Platform that manages Pods across the Cluster.                            |

***

## Memory Trick

🍰 **Recipe** → **Image**

🎂 **Cake** → **Container**

🍱 **Lunch Box** → **Pod**

🏢 **Building** → **Node (Server)**

🏙️ **Apartment Complex** → **Cluster**

👨‍💼 **Building Manager** → **Kubernetes**

***

### Final Flow (Remember This)

```
Developer
    ↓
Image
    ↓
Container
    ↓
Pod
    ↓
Node (Server)
    ↓
Cluster
    ↓
Kubernetes
```

This is the fundamental architecture of Docker and Kubernetes and serves as the foundation for understanding container orchestration.
