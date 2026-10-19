# Development Roadmap & Task Tracker

**Project:** KrishiPragnya  
**Problem Statement:** PS-3 — Crop Disease Detection for Small Farmers  
**Current Milestone:** Phase 1 — Documentation & Project Structure  
**Document Version:** 1.0.0  
**Last Updated:** October 2026  

---

## Progress Overview

| Phase | Phase Name | Status | Completion |
|:---|:---|:---:|:---:|
| **Phase 0** | Environment & Flutter Project Setup | **COMPLETED** | 100% |
| **Phase 1** | Documentation & Architecture Baseline | **COMPLETED** | 100% |
| **Phase 2** | UI Foundation & Design System | PENDING | 0% |
| **Phase 3** | Farmer-Facing Screens | PENDING | 0% |
| **Phase 4** | Camera Capture & Image Pre-Processing | PENDING | 0% |
| **Phase 5** | FastAPI Backend & PostgreSQL Data Layer | PENDING | 0% |
| **Phase 6** | Dataset Evaluation & AI Inference Pipeline | PENDING | 0% |
| **Phase 7** | Severity Assessment & Treatment Advisory | PENDING | 0% |
| **Phase 8** | Kannada Localization & Offline SQLite Outbox | PENDING | 0% |
| **Phase 9** | Agricultural Expert Verification Workflow | PENDING | 0% |
| **Phase 10** | End-to-End Testing, Security & Deployment | PENDING | 0% |

---

## Phase 0: Environment & Flutter Project Setup
- **Objective:** Verify Flutter SDK environment, project scaffolding, and baseline starter build.
- **Dependencies:** Flutter SDK 3.47.7, Dart 3.13.5, Android SDK.
- **Deliverables:** Working Flutter starter project in `mobile/`.
- **Status:** **COMPLETED (Verified)**

### Tasks
- [x] Verify Flutter environment and connected device capability (`flutter devices`).
- [x] Initialize Flutter mobile project in `mobile/` (`flutter create mobile`).
- [x] Configure Windows runner compilation flags and include paths.
- [x] Verify clean starter build with zero analyzer warnings (`flutter analyze` $\rightarrow$ 0 issues).
- [x] Verify smoke test execution (`flutter test` $\rightarrow$ All tests passed).

---

## Phase 1: Documentation & Architecture Baseline
- **Objective:** Author comprehensive, unambiguous project specifications before feature implementation.
- **Dependencies:** Phase 0 completion.
- **Deliverables:** Six core documentation files in `docs/` (`PRD.md`, `ARCHITECTURE.md`, `RULES.md`, `DESIGN.md`, `TASKS.md`, `MEMORY.md`).
- **Status:** **COMPLETED (Verified)**

### Tasks
- [x] Inspect existing workspace and establish `docs/` directory.
- [x] Author `PRD.md` with problem statement, personas, and functional/non-functional requirements.
- [x] Author `ARCHITECTURE.md` with component responsibilities, data models, and Mermaid diagrams.
- [x] Author `RULES.md` with engineering principles, safety rules, and coding standards.
- [x] Author `DESIGN.md` with visual system, palette, typography, and screen specifications.
- [x] Author `TASKS.md` with phased task breakdown, dependencies, and verification criteria.
- [x] Author `MEMORY.md` with environment snapshot, decision records, and project constraints.
- [x] Review all documents for internal consistency and absence of contradictions.

---

## Phase 2: UI Foundation & Design System
- **Objective:** Establish the Flutter theme, typography, color tokens, and reusable UI components.
- **Dependencies:** Phase 1 completion.
- **Deliverables:** Theme module, color tokens, reusable button/card widgets in `mobile/lib/core/theme/`.
- **Acceptance Criteria:** Theme renders correctly in both light/dark modes with WCAG AAA contrast for sunlight readability.
- **Status:** PENDING (Awaiting authorization)

### Tasks
- [ ] Define color palette constants (`PrimaryGreen`, `AccentGold`, `AlertRed`, `NeutralBackground`).
- [ ] Configure `ThemeData` with Material 3 and `NotoSansKannada` font integration.
- [ ] Create reusable component widgets:
  - [ ] `PrimaryButton` and `OutlinedAppButton`.
  - [ ] `StatusBadge` (Confidence, Severity, Sync status).
  - [ ] `AudioPlayerChip` (Text-to-speech audio trigger).
  - [ ] `ErrorDisplayCard` with retry actions.
- [ ] Run `flutter analyze` and verify zero warnings.

---

## Phase 3: Farmer-Facing Screens
- **Objective:** Build the core UI screen navigation flow without mocked or real backend dependencies.
- **Dependencies:** Phase 2 completion.
- **Deliverables:** Screen widgets in `mobile/lib/features/`.
- **Acceptance Criteria:** Farmer can navigate smoothly between Home, Language Selection, Report Details, and History.
- **Status:** PENDING

### Tasks
- [ ] Build Language Selection / Onboarding Screen.
- [ ] Build Home Screen with Crop Carousel (Paddy, Tomato, Ragi).
- [ ] Build Diagnosis & Severity Result Screen with placeholder data structures.
- [ ] Build Treatment Advisory Screen with structured accordion/tabs.
- [ ] Build Saved Reports & History Screen.
- [ ] Add widget tests for all new screens.

---

## Phase 4: Camera Capture & Image Pre-Processing
- **Objective:** Implement camera viewfinder, frame guidelines, blur validation, and client-side compression.
- **Dependencies:** Phase 3 completion, `camera` package or native channel.
- **Deliverables:** Camera controller, blur detector, and WebP compressor in `mobile/lib/features/camera/`.
- **Acceptance Criteria:** Images compressed to $< 250\text{ KB}$; blurry captures trigger prompt before upload.
- **Status:** PENDING

### Tasks
- [ ] Implement camera capture screen with leaf overlay guides.
- [ ] Implement client-side blur detection (Laplacian variance or edge analysis).
- [ ] Implement client-side image downsampling to max 1024px WebP format.
- [ ] Build image confirmation screen with "Retake" and "Continue" workflows.
- [ ] Verify memory stability on entry-level Android emulator/device during camera lifecycle.

---

## Phase 5: FastAPI Backend & PostgreSQL Data Layer
- **Objective:** Implement backend modular monolith with database migrations and report endpoints.
- **Dependencies:** Python 3.12+, FastAPI, PostgreSQL / Supabase.
- **Deliverables:** `backend/` directory with API gateway, services, database models, and migrations.
- **Acceptance Criteria:** `POST /api/v1/reports` creates reports idempotently and enforces `UNIQUE(user_id, client_request_id)`.
- **Status:** PENDING

### Tasks
- [ ] Initialize `backend/` modular monolith structure and virtual environment.
- [ ] Configure SQLAlchemy / SQLModel database connection and migration tooling.
- [ ] Create database tables: `users`, `crops`, `diseases`, `crop_reports`, `diagnosis_runs`, `predictions`, `severity_assessments`, `advisory_rules`, `report_advisories`, `expert_reviews`.
- [ ] Seed initial Karnataka crop catalog (Paddy, Tomato, Ragi).
- [ ] Implement report endpoints with idempotency handling.
- [ ] Implement presigned object storage ticket endpoint.
- [ ] Write integration tests for API endpoints using `pytest`.

---

## Phase 6: Dataset Evaluation & AI Inference Pipeline
- **Objective:** Evaluate real-field crop disease datasets, train/validate classifier, and integrate server-side inference.
- **Dependencies:** Phase 5 completion, PyTorch, OpenCV, verified field dataset.
- **Deliverables:** Model inference service with gatekeeper quality check in `backend/app/modules/inference/`.
- **Acceptance Criteria:** Minimum 85% Top-1 precision on supported classes; non-plant images rejected.
- **Status:** PENDING

### Tasks
- [ ] Document dataset sources, distribution, and in-field validation criteria.
- [ ] Implement OpenCV input validation (format verification, dimension checks, decode test).
- [ ] Integrate Stage 1 Gatekeeper model (detect valid crop leaf vs. non-plant input).
- [ ] Integrate Stage 2 Disease Classifier with PyTorch / TorchScript.
- [ ] Implement temperature scaling and confidence thresholding (`UNCERTAIN` classification).
- [ ] Register model version tag and metadata in `model_versions` table.
- [ ] Run benchmark inference tests and record latency metrics.

---

## Phase 7: Severity Assessment & Treatment Advisory
- **Objective:** Build lesion area quantification and deterministic CIB&RC-compliant agricultural knowledge base.
- **Dependencies:** Phase 6 completion.
- **Deliverables:** Severity evaluation module and audited advisory rule engine in `backend/`.
- **Acceptance Criteria:** Zero unverified chemical prescriptions; severity explicitly decoupled from confidence.
- **Status:** PENDING

### Tasks
- [ ] Implement lesion segmentation / area percentage estimation module.
- [ ] Map lesion coverage to severity categories (`LOW`, `MODERATE`, `HIGH`, `UNKNOWN`).
- [ ] Build audited Karnataka agricultural advisory dataset (UAS Bangalore Package of Practices).
- [ ] Implement advisory rule query engine with structured dosage, dilution, and PHI values.
- [ ] Implement frozen snapshot storage in `report_advisories` table.
- [ ] Write unit tests verifying strict advisory matching and safety constraints.

---

## Phase 8: Kannada Localization & Offline SQLite Outbox
- **Objective:** Add full Kannada localization, audio playback engine, and mobile SQLite Outbox synchronization.
- **Dependencies:** Phase 4 and Phase 7 completion.
- **Deliverables:** Localization arb/json assets, SQLite Outbox manager, and audio service in `mobile/`.
- **Acceptance Criteria:** Reports saved offline can be synced idempotently without duplicate record creation.
- **Status:** PENDING

### Tasks
- [ ] Bundle verified Kannada localized strings in mobile app.
- [ ] Implement 3-tier audio playback engine (Native TTS $\rightarrow$ pre-recorded clips $\rightarrow$ server fallback).
- [ ] Implement local SQLite database schema using `sqflite` or `drift`.
- [ ] Build Outbox queue manager (`LOCAL_DRAFT` $\rightarrow$ `PENDING_UPLOAD` $\rightarrow$ `SYNCED`).
- [ ] Implement network change listener and automated background synchronization.
- [ ] Test flaky/dropped cellular connectivity handling in mobile client.

---

## Phase 9: Agricultural Expert Verification Workflow
- **Objective:** Build human-in-the-loop expert triage workflow for uncertain or high-severity cases.
- **Dependencies:** Phase 5 and Phase 8 completion.
- **Deliverables:** Expert triage API, review dashboard, and notification triggers.
- **Acceptance Criteria:** Expert review updates report status asynchronously and triggers farmer notification.
- **Status:** PENDING

### Tasks
- [ ] Implement expert review management service (`expert_reviews` lifecycle).
- [ ] Build role-based authorization ensuring only verified experts can submit decisions.
- [ ] Build expert triage view (sort by urgency and wait time).
- [ ] Implement expert decision submission (confirm AI, change disease, attach custom note).
- [ ] Implement push notification hook (FCM / SMS fallback).

---

## Phase 10: End-to-End Testing, Security & Deployment
- **Objective:** Perform full end-to-end integration tests, security audit, and production deployment packaging.
- **Dependencies:** All previous phases completed.
- **Deliverables:** Docker containers, CI/CD pipeline, and audit report.
- **Acceptance Criteria:** End-to-end workflow verified from photo capture to advisory delivery and offline review.
- **Status:** PENDING

### Tasks
- [ ] Run end-to-end simulated farmer user journey test.
- [ ] Perform security review (token validation, RBAC, signed storage URLs).
- [ ] Containerize backend with multi-stage Docker build.
- [ ] Package release APK for Android testing.
- [ ] Document final field test validation results and operational metrics.
