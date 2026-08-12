# Chapter 14: OpenShift Basics

Goal

Understand what **OpenShift** is, how it is related to Kubernetes, and how an application is deployed and accessed using OpenShift.

#### By the end of this chapter, you will understand:

* What is OpenShift?
* OpenShift vs Kubernetes
* OpenShift Projects
* Routes
* `oc` CLI
* OpenShift application deployment flow

***

## Step 1: What is OpenShift?

**OpenShift is a Kubernetes-based application platform developed by Red Hat.**

It uses Kubernetes as its foundation and adds additional tools and features for developers and administrators.

```
                 OpenShift
                     │
        ┌────────────┴────────────┐
        ↓                         ↓
   Kubernetes                OpenShift
   Features                  Features
        │                         │
        └────────────┬────────────┘
                     ↓
              Your Application
```

Think:

> **Kubernetes = Core orchestration**

> **OpenShift = Kubernetes + Additional platform features**

***

## Step 2: Kubernetes vs OpenShift

You already learned Kubernetes concepts:

```
Pods
Deployments
Services
ConfigMaps
Secrets
Nodes
Clusters
```

OpenShift uses these Kubernetes concepts and adds features such as:

```
Projects
Routes
ImageStreams
Builds
Web Console
oc CLI
Additional security features
```

So:

```
Kubernetes
     +
OpenShift Features
     ↓
OpenShift
```

OpenShift does **not replace Kubernetes**.

It is built around Kubernetes.

***

## Step 3: OpenShift Projects

An OpenShift **Project** is a workspace used to organize application resources.

For example:

```
OpenShift Cluster
│
├── Project: shopping-app
│     ├── Pods
│     ├── Services
│     └── Deployments
│
├── Project: banking-app
│     ├── Pods
│     ├── Services
│     └── Deployments
│
└── Project: testing
      ├── Pods
      └── Services
```

Projects help organize and separate applications and resources.

***

## Step 4: Routes

You already learned about Kubernetes **Services**.

A Service provides a stable way to communicate with Pods.

But users outside the cluster need a way to access the application.

OpenShift provides **Routes** for external access.

```
🌍 Internet
     ↓
   Route
     ↓
  Service
     ↓
   Pods
     ↓
Application
```

For example:

```
https://shop.example.com
          ↓
        Route
          ↓
       Service
          ↓
         Pods
```

Remember:

**Route → External access**

**Service → Stable access to Pods**

***

## Step 5: OpenShift Web Console

OpenShift provides a **Web Console** that allows developers and administrators to manage applications visually.

You can view and manage things such as:

```
Projects
Pods
Deployments
Services
Routes
Images
Applications
```

Instead of using only the terminal, you can use:

```
Web Console
     OR
OpenShift CLI
```

***

## Step 6: What is `oc`?

You already learned:

```
kubectl
```

which is used to interact with Kubernetes.

OpenShift provides:

```
oc
```

which is the **OpenShift command-line tool**.

Example:

```bash
oc get pods
```

Think:

```
Kubernetes → kubectl

OpenShift → oc
```

***

## Step 7: OpenShift Application Deployment Flow

Let's use an Instagram-like application as an example.

```
Source Code
     ↓
Build
     ↓
Container Image
     ↓
Deployment
     ↓
Pods
     ↓
Service
     ↓
Route
     ↓
🌍 Users
```

Let's understand the flow.

***

### 1. Source Code

Developers write the application.

```
Application Code
     ↓
app.py
HTML
CSS
JavaScript
etc.
```

***

### 2. Build

The source code is converted into a **Container Image**.

```
Source Code
     ↓
Containerfile
     ↓
Build
     ↓
Container Image
```

Example:

```
instagram:v1
```

Remember:

> **Image = Packaged application ready to run**

***

### 3. Deployment

A Deployment tells Kubernetes/OpenShift how the application should run.

```
Deployment
     ↓
Pods
     ↓
Application
```

If a Pod crashes:

```
Pod ❌
  ↓
Deployment notices
  ↓
New Pod ✅
```

***

### 4. Pods

The application runs inside Pods.

For example:

```
Deployment
    │
    ├── Pod 1
    ├── Pod 2
    └── Pod 3
```

Multiple Pods can run the same application.

***

### 5. Service

The Service provides a stable way to communicate with the Pods.

```
             Service
            /   |   \
           ↓    ↓    ↓
         Pod 1 Pod 2 Pod 3
```

The Service can send traffic to the available Pods.

***

### 6. Route

Finally, users outside the OpenShift cluster need access.

The Route provides external access:

```
🌍 User
   ↓
 Route
   ↓
Service
   ↓
 Pods
   ↓
Application
```

***

## 📱 Real-World Example: Instagram

Imagine Instagram is deployed on OpenShift.

```
             Instagram
                  │
            Source Code
                  ↓
                Build
                  ↓
          Container Image
                  ↓
             Deployment
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      Pod 1     Pod 2     Pod 3
        └─────────┼─────────┘
                  ↓
               Service
                  ↓
                Route
                  ↓
            🌍 Instagram
               Users
```

If one Pod crashes:

```
Pod 1 ❌
   ↓
Deployment
   ↓
Replacement Pod ✅
```

Users can continue accessing the application through the Service and Route.

***

## 🧠 Complete Picture

```
                OpenShift
                    │
                    ↓
              Source Code
                    │
                    ↓
                  Build
                    │
                    ↓
             Container Image
                    │
                    ↓
               Deployment
                    │
                    ↓
                  Pods
              ┌─────┼─────┐
              ↓     ↓     ↓
            Pod 1 Pod 2 Pod 3
              └─────┼─────┘
                    ↓
                 Service
                    ↓
                  Route
                    ↓
               🌍 Users
```

***

## 🔑 Key Terms

#### OpenShift

A Kubernetes-based application platform from Red Hat with additional developer, operational, and security features.

#### Project

A workspace used to organize and manage resources in OpenShift.

#### Route

Provides a way for users outside the cluster to access an application.

#### `oc`

The OpenShift command-line tool.

#### Web Console

A graphical interface for managing OpenShift resources.

***

## 📝 Key Takeaways

* OpenShift is built around **Kubernetes**.
* Kubernetes provides the core container orchestration.
* OpenShift adds additional developer and management features.
* **Projects** organize resources and applications.
* **Services** provide stable communication with Pods.
* **Routes** provide external access to applications.
* `oc` is the OpenShift CLI.
* OpenShift also provides a Web Console.
* Applications can follow the flow:

```
Code → Build → Image → Deployment → Pods → Service → Route → Users
```

***

## ⚡ Quick Revision

✅ **Kubernetes** → Core container orchestration

✅ **OpenShift** → Kubernetes-based platform + additional features

✅ **Project** → Organizes application resources

✅ **Deployment** → Manages application Pods

✅ **Service** → Provides stable access to Pods

✅ **Route** → Provides external access

✅ **`oc`** → OpenShift CLI

✅ **Web Console** → Graphical OpenShift management

***

## 🎯 Interview Questions

#### 1. What is OpenShift?

OpenShift is a Kubernetes-based application platform from Red Hat that adds additional tools and features for developing, deploying, and managing applications.

#### 2. Is OpenShift different from Kubernetes?

Yes. OpenShift is a separate platform, but it is built around Kubernetes and uses Kubernetes as its foundation.

#### 3. What is an OpenShift Project?

A Project is a workspace used to organize and manage application resources.

#### 4. What is a Route?

A Route provides external access to an application running inside an OpenShift cluster.

#### 5. What is `oc`?

`oc` is the command-line tool used to interact with OpenShift.

#### 6. What is the basic OpenShift application flow?

```
Source Code
     ↓
Build
     ↓
Container Image
     ↓
Deployment
     ↓
Pods
     ↓
Service
     ↓
Route
     ↓
Users
```
