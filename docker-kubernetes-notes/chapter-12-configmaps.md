# Chapter 12: ConfigMaps

### Goal

Understand how Kubernetes keeps **application configuration separate from the Container Image**.

#### By the end of this chapter, you will understand:

* What is a ConfigMap?
* Why do we need ConfigMaps?
* How does a Pod use a ConfigMap?
* Environment Variables
* ConfigMap as a File
* ConfigMap vs Secret

***

## Overview

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



***

## Step 1: What is Configuration?

An application needs some settings to run.

Example:

```
APP_MODE=production
PORT=8080
DATABASE_HOST=prod-db
```

These settings are called **configuration**.

They are different from the actual application code.

***

## Step 2: Why Do We Need ConfigMaps?

Suppose configuration is inside the Container Image:

```
Application
     +
Configuration
     ↓
Container Image
```

Now imagine the database changes:

```
Old: database-old
New: database-new
```

If the configuration is inside the image, we may need to:

```
Change Configuration
        ↓
Rebuild Image
        ↓
Push Image
        ↓
Deploy Again
```

This is unnecessary.

Instead, Kubernetes separates them:

```
Container Image
      +
  ConfigMap
      ↓
     Pod
      ↓
Application
```

***

## Step 3: What is a ConfigMap?

A **ConfigMap** is a Kubernetes object used to store **non-sensitive configuration data**.

Example:

```
ConfigMap
│
├── APP_MODE = production
├── PORT = 8080
└── DATABASE_HOST = prod-db
```

The configuration is kept **outside the Container Image**.

***

## Step 4: Creating a ConfigMap

Example:

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_MODE: production
  PORT: "8080"
```

Here:

```
app-config
    │
    ├── APP_MODE = production
    └── PORT = 8080
```

***

## Step 5: ConfigMap → Pod

A Pod can use values stored inside a ConfigMap.

```
ConfigMap
     │
     │ Configuration
     ▼
    Pod
     │
     ▼
Application
```

There are two common ways to use a ConfigMap.

***

## Step 6: ConfigMap as Environment Variables

Example:

```yaml
env:
- name: APP_MODE
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_MODE
```

Flow:

```
ConfigMap
    ↓
Environment Variable
    ↓
Application
```

***

## Step 7: ConfigMap as a File

A ConfigMap can also be mounted into a Pod as a file.

```
ConfigMap
    ↓
Volume
    ↓
Pod
    ↓
Configuration File
    ↓
Application
```

The application can then read the configuration from the file.

***

## 🌐 Real-World Example: Shopping App

Imagine you have an online shopping application.

You build the application once:

```
shopping-app:v1
```

#### Development

```
APP_MODE=development
DATABASE_HOST=dev-db
PORT=8080
```

#### Production

```
APP_MODE=production
DATABASE_HOST=prod-db
PORT=8080
```

The **application image stays the same**.

Only the ConfigMap changes:

```
              SAME IMAGE
                  │
          shopping-app:v1
             /          \
            ↓            ↓
     Development      Production
         │                │
         ↓                ↓
    Dev Config        Prod Config
```

This is the main advantage of ConfigMaps.

> **Same Image + Different Configuration**

***

## Step 8: ConfigMap vs Secret

ConfigMap is for **non-sensitive data**:

```
APP_MODE
PORT
DATABASE_HOST
LOG_LEVEL
```

Secret is for **sensitive data**:

```
Password
API Key
Token
Private Key
```

```
ConfigMap → Non-sensitive information

Secret → Sensitive information
```

We will learn **Secrets in Chapter 13**.

***

## Key Terms

#### ConfigMap

Stores non-sensitive application configuration.

#### Environment Variable

A configuration value provided to an application through its environment.

#### Volume

A way to make ConfigMap data available as files inside a Pod.

***

## 📝 Key Takeaways

* ConfigMaps store **non-sensitive configuration**.
* Configuration is kept separate from the Container Image.
* The same Image can be used with different configurations.
* Pods can use ConfigMaps as **environment variables** or **files**.
* Configuration can change without rebuilding the Image.
* Sensitive information should be stored in **Secrets**.

***

## ⚡ Quick Revision

✅ **Container Image** → Contains the application

✅ **ConfigMap** → Contains configuration

✅ **Environment Variable** → One way to use ConfigMap data

✅ **File / Volume** → Another way to use ConfigMap data

✅ **Secret** → Used for sensitive information

***

## 🎯 Interview Questions

#### 1. What is a ConfigMap?

A Kubernetes object used to store non-sensitive configuration data separately from the application image.

#### 2. Why do we use ConfigMaps?

To change application configuration without rebuilding the Container Image.

#### 3. How can a Pod use a ConfigMap?

Through environment variables or mounted files.

#### 4. Can ConfigMaps store passwords?

No. Sensitive information should be stored using Kubernetes Secrets.

#### 5. What is the biggest advantage of ConfigMaps?

The **same Container Image can be used with different configurations**.
