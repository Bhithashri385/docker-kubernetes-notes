# Chapter 2: Container Lifecycle

Introduction

In Chapter 1, we learned about the fundamental components of containerization:

* Server (Node)
* Image
* Container
* Pod
* Cluster
* Kubernetes

Now, let's understand **how an application travels from a developer's computer to users**. This complete journey is called the **Container Lifecycle**.

Understanding this lifecycle is essential because every containerized application follows these steps.

***

## The Complete Container Lifecycle

The lifecycle of a containerized application can be represented as:

```
Developer
     │
     ▼
Write Application Code
     │
     ▼
Build Image
     │
     ▼
Push Image to Registry
     │
     ▼
Pull Image from Registry
     │
     ▼
Run Container
     │
     ▼
Container inside Pod
     │
     ▼
Pod runs on Node
     │
     ▼
Users access the Application
```

Each step has a specific purpose, which we will understand below.

***

## Step 1: Write the Application

Everything begins with writing the application.

For example, if we are creating a Notes application, our project may contain:

```
notes-app/
│
├── app.py
├── requirements.txt
├── templates/
└── static/
```

At this stage, we only have the **source code**.

However, source code alone is **not enough** to run the application on another computer.

***

## The "Works on My Machine" Problem

Imagine you developed your application on your laptop.

Your laptop already has:

* Python installed
* Required libraries
* Environment variables
* Dependencies

If you send only the source code to another developer or a server, the application may fail because those dependencies are missing.

This common issue is known as the **"It works on my machine" problem.**

***

## Step 2: Build an Image

To solve this problem, we package everything the application needs into an **Image**.

An Image contains:

* Application source code
* Runtime (Python, Java, Node.js, etc.)
* Required libraries
* Dependencies
* Configuration
* Startup command

Example:

```
Image
│
├── Application Code
├── Runtime
├── Libraries
├── Dependencies
└── Startup Command
```

#### Why do we build an Image?

Building an image ensures that the application behaves **exactly the same** on every machine.

Whether the image runs on:

* A developer's laptop
* A testing server
* A production server
* A cloud platform

the application will behave consistently.

***

## Step 3: Push the Image to a Container Registry

Once the image is built, it needs to be stored in a central location so that other systems can access it.

This location is called a **Container Registry**.

A Container Registry stores container images, similar to how GitHub stores source code.

Examples of Container Registries include:

* Docker Hub
* Quay.io
* Red Hat Registry

Workflow:

```
Developer
     │
Build Image
     │
     ▼
Container Registry
```

The registry does **not** run containers.

Its responsibility is only to store and distribute images.

***

## Step 4: Pull the Image

Before an application can run, the target machine (Node) must first download the required image from the registry.

This process is called **Pulling an Image**.

```
Container Registry
        │
        ▼
Node downloads Image
```

Once downloaded, the image is available locally on that Node.

***

## Step 5: Run the Image as a Container

An Image is only a blueprint.

When the image starts executing, it becomes a **Container**.

```
Image
   │
   ▼
Container
```

Remember:

* **Image** → Blueprint
* **Container** → Running application

A single image can be used to create multiple containers.

***

## Step 6: Kubernetes Creates a Pod

Kubernetes does not manage containers directly.

Instead, it places one or more containers inside a **Pod**, which is the smallest deployable unit in Kubernetes.

```
Pod
│
└── Container
```

A Pod may contain:

* One container (most common)
* Multiple closely related containers

Containers inside the same Pod share:

* Network
* Storage
* IP Address

***

## Step 7: Pod Runs on a Node

Every Pod must run on a **Node**.

A Node is simply a server within a Kubernetes cluster.

```
Node
│
└── Pod
      │
      └── Container
```

Kubernetes decides which Node should host each Pod.

***

## Step 8: Users Access the Application

Once the Pod is running successfully, users can access the application.

```
Browser
    │
Internet
    │
Node
    │
Pod
    │
Container
    │
Application
```

At this stage, the application is fully deployed and ready to serve requests.

***

## Understanding the Container Registry

A **Container Registry** is a repository that stores container images.

Think of it as a library for images.

| Platform         | Stores           |
| ---------------- | ---------------- |
| GitHub           | Source Code      |
| Docker Hub       | Container Images |
| Quay.io          | Container Images |
| Red Hat Registry | Container Images |

A registry:

* Stores images
* Allows images to be shared
* Allows Nodes to download (pull) images

A registry **does not** execute or run applications.

***

***

## Complete Architecture

```
Developer
     │
     ▼
Write Application
     │
     ▼
Build Image
     │
     ▼
Push Image
(Container Registry)
     │
     ▼
Node Pulls Image
     │
     ▼
Run Container
     │
     ▼
Container inside Pod
     │
     ▼
Pod runs on Node
     │
     ▼
Users access the Application
```

***

## Key Takeaways

* Source code alone cannot guarantee that an application will run on every machine.
* An Image packages the application along with everything required to run it.
* A Container Registry stores images for sharing and deployment.
* Pulling an image means downloading it from a registry.
* Running an image creates a Container.
* Kubernetes places Containers inside Pods.
* Pods run on Nodes.
* Users access the application after the Pod is successfully running.

***

## Quick Revision

✔ Source Code → Application files created by the developer.

✔ Image → Packaged application with all required dependencies.

✔ Container Registry → Repository that stores container images.

✔ Pull → Download an image from a registry.

✔ Container → Running instance of an image.

✔ Pod → Smallest deployable unit in Kubernetes.

✔ Node → Server that runs Pods.

***

## Interview Questions

#### 1. What is the Container Lifecycle?

The Container Lifecycle is the complete journey of an application from writing the source code to running it as a container inside a Kubernetes Pod.

***

#### 2. Why do we build an Image?

To package the application and all its dependencies so it runs consistently on any machine.

***

#### 3. What is the purpose of a Container Registry?

A Container Registry stores container images and allows them to be shared and downloaded by different systems.

***

#### 4. What is the difference between an Image and a Container?

* **Image:** A blueprint or packaged application.
* **Container:** A running instance of an Image.

***

#### 5. Does Kubernetes run Containers directly?

No. Kubernetes manages **Pods**, and Pods contain one or more Containers.
