# KrishiPragnya (ಕೃಷಿ ಪ್ರಜ್ಞಾ)

> **PS-3: Crop Disease Detection for Small Farmers**  
> AI-powered crop health diagnostics, severity quantification, and safe agricultural guidance in Kannada and English, engineered for low-connectivity rural environments.

---

## 🌾 Overview

Smallholder farmers in India, particularly in Karnataka, face severe annual crop yield losses (20%–40%) due to unmanaged diseases and pest attacks. **KrishiPragnya** is an accessible, offline-first mobile and cloud platform that allows farmers to photograph affected leaves, receive rapid and reliable disease identification, evaluate urgency through objective severity assessment, access audited treatment advice, and escalate ambiguous cases to agricultural scientists.

### Core Capabilities
1. **AI Crop Disease Detection:** Accurate multi-class image classification with out-of-distribution (OOD) rejection.
2. **Disease Severity Assessment:** Objective quantification of visible leaf symptoms and infection urgency.
3. **Evidence-Based Agricultural Advice:** Safe, practical guidance from audited packages of practice (CIB&RC / UAS Bangalore). Zero chemical dosage hallucinations.
4. **Kannada & English Localization:** Full dual-language interface, local terminology, and 3-tier audio playback (Text-to-Speech).
5. **Offline & Low-Connectivity Support:** SQLite Outbox queue with idempotent background synchronization.
6. **Agricultural Expert Verification:** Human-in-the-loop triage workflow for uncertain or high-risk cases.

---

## 📁 Repository Structure

```text
Krishi-Pragnya/
│
├── .vscode/          # IDE configuration & C++ embedder IntelliSense
│
├── docs/             # Core Project Documentation & Specifications
│   ├── PRD.md            # Product Requirements Document
│   ├── ARCHITECTURE.md   # System Architecture & API Contracts
│   ├── RULES.md          # Engineering Rules & Agricultural Safety Guardrails
│   ├── DESIGN.md         # UI/UX Design Specifications & Screen Layouts
│   ├── TASKS.md          # Phased Development Roadmap & Task Tracker
│   └── MEMORY.md         # Project Context & Architectural Decision Records
│
├── mobile/           # Flutter mobile application (Android-first, offline-capable)
│
├── frontend/         # Future web interface (Agricultural Expert Triage Portal)
│
├── backend/          # Python + FastAPI modular monolith backend API
│
├── database/         # Database schemas, migrations, and seed datasets
│
├── ml/               # Machine learning models, dataset tooling & inference pipelines
│
├── .gitignore        # Root Git ignore rules
└── README.md         # Project overview and navigation
```

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|:---|:---|:---|
| **Mobile Client** | Flutter / Dart | Android-first cross-platform application with offline SQLite persistence. |
| **Backend API** | Python 3.12 / FastAPI | High-performance modular monolith REST API. |
| **AI / Computer Vision** | PyTorch / OpenCV | Leaf validation, gatekeeper filtering, and disease classification. |
| **Primary Database** | PostgreSQL (Supabase) | Relational persistence for reports, predictions, rules, and reviews. |
| **Object Storage** | Private S3 / Supabase Storage | Encrypted storage for field photographs with signed, short-lived URLs. |
| **Offline Cache** | SQLite (on device) | Idempotent Outbox pattern for queueing field reports without network. |
| **Audio Engine** | Native TTS / Pre-recorded | 3-tier Kannada audio playback for rural accessibility. |

---

## 📚 Project Documentation

Detailed specifications and architectural decisions are documented in the [`docs/`](file:///d:/Krishi-Pragnya/docs) directory:

- 📄 [**PRD.md**](file:///d:/Krishi-Pragnya/docs/PRD.md) — Problem statement, target personas, functional requirements, and hold-out validation criteria.
- 📄 [**ARCHITECTURE.md**](file:///d:/Krishi-Pragnya/docs/ARCHITECTURE.md) — System component boundaries, Mermaid sequence diagrams, API contracts, and ER schemas.
- 📄 [**RULES.md**](file:///d:/Krishi-Pragnya/docs/RULES.md) — Mandatory engineering practices, zero chemical hallucination rules, and testing standards.
- 📄 [**DESIGN.md**](file:///d:/Krishi-Pragnya/docs/DESIGN.md) — Farmer-first UI/UX guidelines, sunlight-readable color palette, and screen designs.
- 📄 [**TASKS.md**](file:///d:/Krishi-Pragnya/docs/TASKS.md) — Phased development roadmap tracking progress across 11 milestones.
- 📄 [**MEMORY.md**](file:///d:/Krishi-Pragnya/docs/MEMORY.md) — Verified local environment, Architectural Decision Records (ADRs), and project context.

---

## 🚦 Current Status

- **Phase 0 (Environment Setup):** Verified (`Flutter 3.47.7`, `Dart 3.13.5`, `Android SDK 36.1.0`).
- **Phase 1 (Documentation Baseline):** Completed and verified.
- **Phase 2–10:** Pending authorization as per the roadmap in [docs/TASKS.md](file:///d:/Krishi-Pragnya/docs/TASKS.md).
