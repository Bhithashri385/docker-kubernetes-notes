# Chapter 13: Kubernetes Secrets

### Goal

Understand how Kubernetes stores and provides **sensitive information** such as passwords, API keys, and tokens.

#### By the end of this chapter, you will understand:

* What is a Secret?
* Why do we need Secrets?
* ConfigMap vs Secret
* How to create a Secret
* How a Pod uses a Secret
* Secret as an Environment Variable
* Secret as a File

***

## Overview

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

***

## Step 1: Why Do We Need Secrets?

Imagine our application needs a database password:

```
DATABASE_PASSWORD=myPassword123
```

We should NOT put this directly inside the application code or Container Image.

Instead:

```
Application Image
       +
    Secret
       ↓
      Pod
       ↓
  Application
```

This keeps sensitive information separate from the application.

***

## Step 2: What is a Secret?

A **Secret** is a Kubernetes object used to store **sensitive information**.

Examples:

```
Database Password
API Key
Access Token
Username
Private Key
```

Example:

```
Secret
│
├── DB_USERNAME = admin
└── DB_PASSWORD = ********
```

***

## Step 3: ConfigMap vs Secret

| ConfigMap            | Secret                |
| -------------------- | --------------------- |
| Normal configuration | Sensitive information |
| PORT                 | Password              |
| APP\_MODE            | API Key               |
| DATABASE\_HOST       | Token                 |
| LOG\_LEVEL           | Private Key           |

Remember:

```
ConfigMap → Non-sensitive

Secret → Sensitive
```

***

## Step 4: Creating a Secret

Example:

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: db-secret

type: Opaque

stringData:
  username: admin
  password: myPassword123
```

Here:

```
db-secret
    │
    ├── username = admin
    └── password = myPassword123
```

> ⚠️ Never put real passwords or API keys into Git.

***

## Step 5: Secret → Pod

A Pod can use values stored inside a Secret.

```
Secret
   │
   │ Sensitive Data
   ▼
  Pod
   │
   ▼
Application
```

There are two common ways to use a Secret:

1. Environment Variables
2. Files

***

## Step 6: Secret as an Environment Variable

Example:

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

Flow:

```
Secret
   ↓
DB_PASSWORD
   ↓
Environment Variable
   ↓
Application
```

The application can access the value through the environment variable.

***

## Step 7: Secret as a File

A Secret can also be mounted into a Pod as a file.

```
Secret
   ↓
Volume
   ↓
Pod
   ↓
Secret File
   ↓
Application
```

For example:

```
/etc/secrets/password
```

The application can read the Secret from the file.

***

## 🌐 Step 8: Real-World Example

Imagine an online shopping application.

The application needs to connect to its database.

#### Application Image

```
shopping-app:v1
```

The Image contains:

```
Application Code
```

But it does NOT contain:

```
Database Password ❌
```

Instead, Kubernetes has:

```
db-secret

DB_USERNAME = admin
DB_PASSWORD = ********
```

Now:

```
             Kubernetes
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
 shopping-app:v1       db-secret
        │                   │
        └─────────┬─────────┘
                  ↓
                 Pod
                  ↓
             Application
                  ↓
              Database
```

If the database password changes, we update the Secret instead of rebuilding the application Image.

***

## 🔐 ConfigMap + Secret Together

A real application may need both.

```
                Application
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      ConfigMap              Secret
          │                     │
      PORT=8080          DB_PASSWORD=****
      APP_MODE=prod      API_KEY=****
      DB_HOST=prod-db
          │                     │
          └──────────┬──────────┘
                     ↓
                    Pod
```

So:

**ConfigMap → "How should my application run?"**

**Secret → "What sensitive information does my application need?"**

***

## 📝 Key Takeaways

* Secrets store **sensitive information**.
* Passwords and API keys should not be hardcoded into application code.
* Secrets keep sensitive configuration separate from the Container Image.
* Pods can use Secrets as **environment variables** or **files**.
* ConfigMaps are for normal configuration.
* Secrets are for sensitive information.
* Never commit real Secrets/passwords to Git.
* Kubernetes Secrets are not automatically fully secure; proper access control and encryption are also important.

***

## ⚡ Quick Revision

✅ **Secret** → Stores sensitive information

✅ **Password** → Use Secret

✅ **API Key** → Use Secret

✅ **ConfigMap** → Normal configuration

✅ **Environment Variable** → One way to provide Secret data to a Pod

✅ **File / Volume** → Another way to provide Secret data

***

## 🎯 Interview Questions

#### 1. What is a Kubernetes Secret?

A Kubernetes object used to store and provide sensitive information such as passwords, tokens, and API keys.

#### 2. Why shouldn't passwords be stored in a ConfigMap?

ConfigMaps are intended for non-sensitive configuration. Sensitive information should be handled using Secrets.

#### 3. How can a Pod use a Secret?

A Pod can use a Secret as an **environment variable** or as a **mounted file**.

#### 4. What is the difference between ConfigMap and Secret?

**ConfigMap → Non-sensitive configuration**

**Secret → Sensitive information**

#### 5. Should we commit Secrets to Git?

**No.** Real passwords, API keys, and tokens should never be committed to source control.

#### 6. Are Kubernetes Secrets automatically fully secure?

**No.** Proper RBAC, encryption at rest, and secure handling are also important.
