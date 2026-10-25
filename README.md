# FouzStack Router

> **Declarative, drift-aware navigation for Android. BackStack as logical authority, Activities as physical representation.**

[![Android](https://img.shields.io/badge/Android-API%2021%2B-green.svg)](https://developer.android.com)
[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.java.com)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Production--ready-success.svg)]()
[![Tests](https://img.shields.io/badge/Tests-Empirically%20verified-brightgreen.svg)]()

---

## 📖 Table of contents

1. [What is FouzStack Router?](#-what-is-fouzstack-router)
2. [Why another router?](#-why-another-router)
3. [Philosophy](#-philosophy)
4. [Installation](#-installation)
5. [Quick start](#-quick-start)
6. [Key features](#-key-features)
7. [Comparison](#-comparison)
8. [Examples](#-examples)
9. [Architecture](#-architecture)
10. [Explicit contracts](#-explicit-contracts)
11. [Project status](#-project-status)
12. [Roadmap](#-roadmap)
13. [Contributing](#-contributing)
14. [License](#-license)

---

## 🎯 What is FouzStack Router?

**FouzStack Router** is a navigation library for Android based on
Activities, designed for applications that need **extreme reliability**
in back stack coherence.

Unlike other routers, FouzStack Router:

- **Detects drift** between logical and physical state.
- **Distinguishes legitimate destroys from real drift** via `expectedDestroyKeys`.
- **Resolves Android's asynchronous timing** of `onActivityDestroyed` with a late-confirm buffer.
- **Never crashes** due to navigation failures — it degrades silently.
- **Documents its contracts** explicitly.

> **"Navigating is not pushing Activities. Navigating is reconciling the physical representation with the logical truth."**

---

## 🤔 Why another router?

Excellent alternatives exist: **Jetpack Navigation**, **Cicerone**, **Conductor**. Why FouzStack Router?

| Need | Jetpack Nav | Cicerone | FouzStack |
|---|---|---|---|
| **Activities only** (no Fragments) | ❌ | ✅ | ✅ |
| **Explicit drift detection** | ❌ | ⚠️ Partial | 🏆 **Complete** |
| **Late-destroy buffer** | ❌ | ❌ | ✅ |
| **100% Java-friendly API** | ❌ | ✅ | ✅ |
| **Persistence with real limits** | ⚠️ | ⚠️ | ✅ |
| **Native interceptors** | ❌ | ✅ | ✅ |
| **Contract documentation** | ⚠️ | ⚠️ | 🏆 **Complete** |
| **Declarative deep links** | ✅ | ✅ | ✅ |
| **No Fragment dependency** | ❌ | ✅ | ✅ |

**FouzStack Router** is designed for teams that:

- Work with **Activities + Java** (not Kotlin-first, not Compose).
- Value **reliability over development speed**.
- Need to **understand what's happening when something goes wrong** (drift detection).
- Prefer a **small, predictable API** over a large ecosystem.

---

## 🧭 Philosophy

Three invariants rule the entire design:

### 1. `BackStack` is the logical authority

The navigation state is a stack of `RouteEntry`. Nothing more. Android
may destroy, recreate, or reorder Activities. That **does not change**
the logical state.

### 2. Activities are physical representation

Android's physical back stack is an implementation detail. The Router
**never** delegates decisions to Android's back stack.

### 3. `RouterLifecycle` observes, does not decide

The lifecycle observer detects discrepancies but **never resolves them
automatically**. It notifies. The consumer decides.

**Corollary:** navigating is reconciling the physical representation
with the logical truth. It's not "pushing Activities".

---

## 📦 Installation

### Gradle (once published on JitPack)

```groovy
dependencies {
    implementation 'com.github.fouzstack:fouzstack-router:1.0.0'
}
