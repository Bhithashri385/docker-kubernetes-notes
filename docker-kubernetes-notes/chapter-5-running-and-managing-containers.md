# Chapter 5: Running & Managing Containers

### &#x20;Goal

Understand how to create, run, inspect, stop, and remove Containers using Podman.

#### By the end of this chapter, you will understand:

* What happens when an Image is run?
* What is a Container?
* How does `podman run` work?
* How do we view running Containers?
* How do we stop and remove Containers?
* What is the difference between an Image and a Container?

***

## Before We Start...

In the previous chapter, we successfully built an Image.

```
notes-app (Image)
```

The Image is stored inside the **Local Image Store**.

But...

👉 **Can users use it?**

No.

An Image is only a **blueprint**.

To make the application run, we must create a **Container**.

The answer is:

```bash
podman run
```

Let's understand what actually happens internally.

***

## Overview

<div data-with-frame="true"><figure><img src="../.gitbook/assets/ChatGPT Image Jul 30, 2026, 11_51_28 AM.png" alt=""><figcaption></figcaption></figure></div>

***

## Step 1: The Image

Suppose we already have this Image:

```
notes-app
```

This Image contains:

* Application Code
* Runtime
* Libraries
* Dependencies
* Startup Command

***

## Step 2: Running the Image

Execute:

```bash
podman run notes-app
```

Podman now creates a **Container** from the Image.

```
Container Image
        │
        ▼
   podman run
        │
        ▼
Container Created
        │
        ▼
Application Starts
```

The application is now running.

***

## Step 3: What Happens Internally?

When you execute:

```bash
podman run notes-app
```

Podman performs the following steps:

1. Checks whether the Image exists locally.
2. Creates a new Container.
3. Adds a Writable Layer.
4. Starts the application.
5. Keeps the Container running.

```
Container Image
        │
        ▼
Create Container
        │
        ▼
Add Writable Layer
        │
        ▼
Start Application
        │
        ▼
Running Container
```

***

## Step 4: Image vs Container

This is one of the most important Docker & Kubernetes concepts.

| Image                       | Container                  |
| --------------------------- | -------------------------- |
| Blueprint                   | Running Instance           |
| Read-only                   | Writable                   |
| Stored on Disk              | Uses Disk + RAM            |
| Cannot Execute              | Can Execute                |
| Created using Containerfile | Created using `podman run` |

#### One Image → Multiple Containers

```
          Image
             │
     ┌───────┼────────┐
     ▼       ▼        ▼
Container1 Container2 Container3
```

A single Image can create multiple independent Containers.

***

## Step 5: Viewing Running Containers

To view running Containers:

```bash
podman ps
```

Example:

```
CONTAINER ID     IMAGE       STATUS

3ab91...         notes-app   Up 2 minutes
```

Only **running Containers** are displayed.

***

## Step 6: Viewing All Containers

Stopped Containers are hidden by default.

To display every Container:

```bash
podman ps -a
```

Example:

```
CONTAINER ID     IMAGE        STATUS

3ab91...         notes-app    Up

9dc21...         ubuntu       Exited
```

This command displays:

* Running Containers
* Stopped Containers

***

## Step 7: Stopping a Container

To stop a running Container:

```bash
podman stop <container-id>
```

Example:

```bash
podman stop 3ab91
```

The application stops running.

However,

> **The Container is NOT deleted.**

It still exists on your computer.

***

## Step 8: Starting a Stopped Container

Instead of creating a new Container, you can restart an existing one.

```bash
podman start <container-id>
```

Example:

```bash
podman start 3ab91
```

The Container starts running again.

***

## Step 9: Removing a Container

To permanently delete a Container:

```bash
podman rm <container-id>
```

Example:

```bash
podman rm 3ab91
```

> **Note:**

Removing a Container **does not delete the Image**.

The Image still remains in the Local Image Store.

***

## Step 10: Viewing Container Logs

Applications continuously generate logs.

To view them:

```bash
podman logs <container-id>
```

Example:

```bash
podman logs notes-app
```

Logs help developers troubleshoot errors.

***

## Step 11: Accessing a Running Container

Sometimes you need to work inside the Container.

Use:

```bash
podman exec -it <container-id> bash
```

Example:

```bash
podman exec -it notes-app bash
```

This opens a Linux terminal inside the running Container.

***

## Understanding `podman run`

Command:

```bash
podman run notes-app
```

| Part        | Meaning                        |
| ----------- | ------------------------------ |
| `podman`    | Starts Podman                  |
| `run`       | Creates and starts a Container |
| `notes-app` | Image Name                     |

***

## Internal Working

```
Container Image
       │
       ▼
podman run
       │
Creates Container
       │
Adds Writable Layer
       │
Starts Application
       │
Running Container
```

***

## Important Commands

| Command        | Purpose                          |
| -------------- | -------------------------------- |
| `podman run`   | Create and Start a Container     |
| `podman ps`    | Show Running Containers          |
| `podman ps -a` | Show All Containers              |
| `podman stop`  | Stop a Container                 |
| `podman start` | Restart a Container              |
| `podman rm`    | Remove a Container               |
| `podman logs`  | View Container Logs              |
| `podman exec`  | Open Terminal inside a Container |

***

## Key Terms

### Writable Layer

When a Container starts, Podman creates a small **Writable Layer** above the Image.

All new files, logs and changes are stored here.

The original Image remains unchanged.

***

### Container ID

Every Container receives a unique ID.

Example:

```
3ab91ed2f7a
```

This ID is used in most Podman commands.

***

## &#x20;Key Takeaways

* An Image is only a blueprint.
* Running an Image creates a Container.
* One Image can create multiple Containers.
* Containers have a Writable Layer.
* `podman ps` shows running Containers.
* `podman ps -a` shows all Containers.
* Stopping a Container does not remove it.
* Removing a Container does not remove the Image.

***

## Quick Revision

✅ `podman run` → Create & Start a Container

✅ `podman ps` → Running Containers

✅ `podman ps -a` → All Containers

✅ `podman stop` → Stop Container

✅ `podman start` → Restart Container

✅ `podman rm` → Remove Container

✅ `podman logs` → View Logs

✅ `podman exec` → Access Container Terminal

***

## 🎯 Interview Questions

#### 1. What is the difference between an Image and a Container?

An Image is a read-only blueprint, while a Container is a running instance of that Image.

***

#### 2. What happens when `podman run` is executed?

Podman creates a new Container from the Image, adds a Writable Layer, and starts the application.

***

#### 3. Does stopping a Container remove it?

No.

The Container still exists and can be started again using `podman start`.

***

#### 4. Does removing a Container delete its Image?

No.

The Image remains stored inside the Local Image Store.

***

#### 5. Can one Image create multiple Containers?

Yes.

A single Image can be used to create multiple independent Containers.
