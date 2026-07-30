# Chapter 4: Building Your First Container Image

🎯 Goal

Understand how **Podman** converts your application into a runnable **Container Image** using a **Containerfile**.

#### By the end of this chapter, you will understand:

* What is a Base Image?
* How does `podman build` work internally?
* What happens when an Image is built?
* What is the Local Image Store?
* How do we view built Images?

***

## Before We Start...

Suppose you have the following project:

```
notes-app/
│
├── app.py
├── requirements.txt
└── Containerfile
```

You have:

* Written your application ✅
* Created a Containerfile ✅

Now the question is...

> **How do we convert these files into a Container Image?**

The answer is:

```bash
podman build
```

But instead of memorizing the command, let's understand **what actually happens internally**.

***

## Overview

<figure><img src="../.gitbook/assets/ChatGPT Image Jul 30, 2026, 11_08_10 AM.png" alt=""><figcaption></figcaption></figure>

***

## Step 1: The Project Directory

Every application starts as a project folder.

```
notes-app/
│
├── app.py
├── requirements.txt
├── static/
├── templates/
└── Containerfile
```

This folder contains everything Podman needs to build the Image.

***

## Step 2: Reading the Containerfile

Podman first looks for the **Containerfile**.

Example:

```dockerfile
FROM python:3.12

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

The Containerfile is simply a **list of instructions** that tells Podman how to build the Image.

***

## Step 3: Running `podman build`

Execute:

```bash
podman build -t notes-app .
```

Podman starts reading the Containerfile **line by line**.

```
Containerfile
      │
      ▼
Read FROM
      ▼
Read WORKDIR
      ▼
Read COPY
      ▼
Read RUN
      ▼
Read CMD
```

#### Understanding the `.`

Notice the **`.`** at the end of the command.

```bash
podman build -t notes-app .
```

The `.` means:

> **Use the current folder as the Build Context.**

#### What is Build Context?

The **Build Context** is the folder whose files Podman can access while building the Image.

***

## Step 4: Download the Base Image

Suppose the first instruction is:

```dockerfile
FROM python:3.12
```

If this Base Image is **not available locally**, Podman downloads it from a Container Registry.

```
Container Registry
        │
        ▼
   python:3.12
        │
        ▼
 Local Machine
```

If the Base Image already exists on your machine, Podman simply reuses it.

***

## Step 5: Execute Each Instruction

Podman now executes every instruction one by one.

```
FROM
 ↓
WORKDIR
 ↓
COPY
 ↓
RUN
 ↓
CMD
```

Each instruction creates a **new Image Layer**.

***

## Step 6: Create the Final Image

After executing all instructions, Podman combines all the layers into one Container Image.

```
Layer 5  → CMD
Layer 4  → RUN
Layer 3  → COPY
Layer 2  → WORKDIR
Layer 1  → FROM
────────────────────
Final Container Image
```

Your Image is now ready to use.

***

## Step 7: Store the Image Locally

Where does the Image go?

The Image is stored in Podman's **Local Image Store**.

```
Your Computer
│
└── Podman Image Store
      │
      ├── python:3.12
      ├── ubuntu
      ├── nginx
      └── notes-app
```

> **Note:** Nothing is running yet.

The Image is simply stored on your computer, waiting to be used.

***

## Step 8: View Available Images

To list all Images stored on your computer:

```bash
podman images
```

Example Output:

```
REPOSITORY     TAG      IMAGE ID

python         3.12     8ab12...
ubuntu         latest   1bc45...
notes-app      latest   7de91...
```

This command displays all Images available in the Local Image Store.

***

## Understanding `podman build`

Command:

```bash
podman build -t notes-app .
```

| Part        | Meaning                           |
| ----------- | --------------------------------- |
| `podman`    | Runs the Podman tool              |
| `build`     | Builds a new Image                |
| `-t`        | Assigns a Tag (Image Name)        |
| `notes-app` | Name of the Image                 |
| `.`         | Build Context (Current Directory) |

***

## Internal Working

```
Project Folder
      │
      ▼
Containerfile
      │
      ▼
Podman
      │
Reads Instructions
      │
Creates Image Layers
      │
Combines Layers
      │
Creates Container Image
      │
Stores Image Locally
```

***

## Key Terms

### Build Context

The folder whose files Podman can access while building the Image.

Usually represented by:

```bash
.
```

***

### Tag

A human-readable name given to an Image.

Example:

```
notes-app:latest
```

* **notes-app** → Repository Name
* **latest** → Tag

***

### Local Image Store

The location on your computer where Podman stores downloaded and built Images.

***

## 📝 Key Takeaways

* Every application starts inside a project folder.
* Podman reads the Containerfile to build an Image.
* The `.` represents the Build Context.
* Podman downloads the Base Image only if it is not already available locally.
* Every Containerfile instruction creates a new Image Layer.
* All layers are combined to create the final Container Image.
* The Image is stored in Podman's Local Image Store.
* `podman images` displays all Images stored locally.

***

## ⚡ Quick Revision

✅ `podman build` → Builds a Container Image

✅ `-t` → Assigns a name (Tag) to the Image

✅ `.` → Current Directory (Build Context)

✅ Base Image → Starting point of every Image

✅ Local Image Store → Stores all Images on your computer

✅ `podman images` → Lists all locally available Images

***

## 🎯 Interview Questions

#### 1. What does `podman build` do?

It reads the Containerfile, executes each instruction, creates Image Layers, combines them, and builds a Container Image.

***

#### 2. What is Build Context?

The Build Context is the directory whose files Podman can access while building the Image. It is commonly represented using `.`.

***

#### 3. What does `-t` mean in `podman build -t my-app .`?

It assigns a **Tag (name)** to the Image.

***

#### 4. Where are Images stored after they are built?

Images are stored locally on the host machine inside Podman's **Local Image Store**, which is managed by Podman.
