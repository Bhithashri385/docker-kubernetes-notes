# Chapter 3: Podman & Containerfile

### Learning Objectives

By the end of this chapter, you will understand:

* What is Podman?
* Why do we need Podman?
* Docker vs Podman
* What is a Containerfile?
* How Podman builds an Image
* What are Image Layers?
* What are Rootless Containers?
* Basic Podman Architecture

***

## Introduction

In **Chapter 2**, we learned that an application follows this lifecycle:

```
Write Code
     │
     ▼
Build Image
     │
     ▼
Push Image
     │
     ▼
Pull Image
     │
     ▼
Run Container
```

But one important question still remains:

> **Who actually builds the Image?**

The answer is **Podman**.

Podman is the software responsible for building images, running containers, and managing them.

***

## What is Podman?

**Podman** is an open-source **Container Engine** developed by **Red Hat**.

It helps developers to:

* Build container images
* Run containers
* Manage containers
* Create Pods
* Push and pull images from registries

Simply put,

> **Podman is the software that manages the entire container lifecycle on your machine.**

***

## Why Do We Need Podman?

Without Podman (or Docker), your computer cannot understand how to convert your application into a container image.

Podman automates the entire process.

It can:

* Read a Containerfile
* Download required base images
* Install dependencies
* Build an Image
* Run that Image as a Container

***

## What Can Podman Do?

Podman provides many useful features.

✔ Build Images

✔ Run Containers

✔ Stop Containers

✔ Delete Containers

✔ Pull Images

✔ Push Images

✔ Create Pods

✔ Inspect Containers

✔ View Logs

***

## Podman Architecture

Unlike Docker, Podman does not require a continuously running background service.

The architecture is simple:

```
User
 │
 ▼
Podman
 │
 ▼
Linux Kernel
 │
 ▼
Containers
```

Podman communicates directly with the Linux kernel to create and manage containers.

***

Create a simple diagram later showing:

```
User
   │
Podman
   │
Linux Kernel
   │
Containers
```

***

## What is a Daemon?

A **Daemon** is a background process that continuously runs and waits for requests.

Example:

* Print Service
* Database Service
* Web Server

Docker uses a daemon called the **Docker Daemon**.

```
User
 │
 ▼
Docker CLI
 │
 ▼
Docker Daemon
 │
 ▼
Containers
```

Every Docker command first communicates with the Docker Daemon.

***

## Podman is Daemonless

Podman works differently.

It does **not** require an always-running daemon.

```
User
 │
 ▼
Podman
 │
 ▼
Container
```

Each Podman command executes independently.

#### Advantages

* Simpler Architecture
* Better Security
* Lower Memory Usage
* Easier Troubleshooting

***

## What are Rootless Containers?

Normally, Linux applications require **Root (Administrator)** permissions.

Running everything as Root is risky.

If a container is compromised, it could affect the entire system.

Podman allows containers to run as a **normal user**.

This feature is called **Rootless Containers**.

#### Benefits

* Better Security
* Reduced Risk
* No Root Privileges Required
* Ideal for Development

***

## Docker vs Podman

| Docker                                     | Podman                                  |
| ------------------------------------------ | --------------------------------------- |
| Uses Docker Daemon                         | Daemonless                              |
| Docker CLI communicates with Docker Daemon | Podman communicates directly with Linux |
| Popular in many environments               | Preferred by Red Hat and OpenShift      |
| Supports Rootless mode                     | Designed with Rootless usage in mind    |

> **Important:** Both Docker and Podman follow the **OCI (Open Container Initiative)** standards.

This means an image built using Docker usually works with Podman as well.

<figure><img src="../.gitbook/assets/ChatGPT Image Jul 30, 2026, 10_15_41 AM.png" alt=""><figcaption></figcaption></figure>

***

## What is a Containerfile?

A **Containerfile** is a text file that contains instructions for building an Image.

Think of it as the **recipe** for creating an Image.

Example:

```dockerfile
FROM python:3.12

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

Suggested diagram:

```
Containerfile
      │
      ▼
Podman
      │
      ▼
Image
```

***

## Understanding Each Instruction

### FROM

```dockerfile
FROM python:3.12
```

Specifies the **Base Image**.

Every image starts from another image.

***

### WORKDIR

```dockerfile
WORKDIR /app
```

Sets the working directory inside the container.

Equivalent to:

```bash
cd /app
```

***

### COPY

```dockerfile
COPY . .
```

Copies application files from your computer into the Image.

***

### RUN

```dockerfile
RUN pip install -r requirements.txt
```

Executes commands while the Image is being built.

Usually used for:

* Installing packages
* Updating software
* Creating directories

***

### CMD

```dockerfile
CMD ["python","app.py"]
```

Defines the command that will execute when the Container starts.

***

## How Podman Builds an Image

Suppose the Containerfile contains:

```dockerfile
FROM python:3.12
COPY . .
RUN pip install flask
CMD python app.py
```

Podman processes every instruction one by one.

```
Containerfile
      │
      ▼
Read FROM
      │
      ▼
Read COPY
      │
      ▼
Read RUN
      │
      ▼
Read CMD
      │
      ▼
Image Created
```

***

## Image Layers

One of the most important concepts in containers is **Layers**.

Every major instruction creates a new Layer.

```
Layer 4
CMD

──────────────

Layer 3
RUN

──────────────

Layer 2
COPY

──────────────

Layer 1
FROM
```

The final Image is created by stacking these layers.

***

### Why Layers are Important

Suppose you only change one Python file.

Without Layers:

Everything would need to be rebuilt.

With Layers:

Only the changed Layer is rebuilt.

This makes Image creation much faster.

***

## Internal Working

The complete Image build process is shown below.

```
Containerfile
        │
        ▼
Podman Reads Instructions
        │
        ▼
Creates Individual Layers
        │
        ▼
Combines Layers
        │
        ▼
Creates Image
        │
        ▼
Stores Image Locally
```

Later, this Image can be:

* Run as a Container
* Shared with others
* Uploaded to a Registry

***

## Complete Architecture

```
Developer
     │
Writes Containerfile
     │
     ▼
Podman
     │
Reads Instructions
     │
Creates Layers
     │
Creates Image
     │
Stores Image
     │
Runs Container
```

***

## Key Takeaways

* Podman is a Container Engine developed by Red Hat.
* Podman builds Images and runs Containers.
* Podman does not require a background daemon.
* Podman supports Rootless Containers.
* A Containerfile contains instructions for building an Image.
* Podman reads the Containerfile from top to bottom.
* Every instruction creates an Image Layer.
* Layers improve Image build performance.

***

## Quick Revision

✔ Podman → Container Engine

✔ Containerfile → Recipe for creating an Image

✔ Base Image → Starting point of an Image

✔ Layer → One step in Image creation

✔ Rootless → Runs Containers without Root privileges

✔ Daemonless → No continuously running background service

***

## Interview Questions

### 1. What is Podman?

Podman is an open-source Container Engine used to build, run, and manage containers and pods.

***

### 2. Why is Podman called daemonless?

Because it does not require a continuously running background daemon to manage containers.

***

### 3. What is a Containerfile?

A Containerfile is a text file containing instructions for building a container image.

***

### 4. What is a Base Image?

A Base Image is the initial image specified using the `FROM` instruction from which a new image is built.

***

### 5. What are Image Layers?

Image Layers are read-only layers created from each instruction in a Containerfile. They improve efficiency by allowing unchanged layers to be reused.

***

## Summary

```
Application Code
        │
        ▼
Containerfile
        │
        ▼
Podman
        │
        ▼
Build Image
        │
        ▼
Run Container
```



