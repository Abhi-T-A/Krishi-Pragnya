# Project Memory & Context

**Project Identity:** KrishiPragnya  
**Problem Statement:** PS-3 — Crop Disease Detection for Small Farmers  
**Document Purpose:** Durable context, verified environment facts, architectural decisions, and current progress tracker.  
**Last Updated:** October 2026  

---

## 1. Verified Environment & Paths

- **Project Root Directory:** `D:\Krishi-Pragnya`
- **Mobile Application Directory:** `D:\Krishi-Pragnya\mobile`
- **Documentation Directory:** `D:\Krishi-Pragnya\docs`
- **Flutter SDK:** Version `3.47.7` (Channel stable, installed at `C:\Users\Abhi T A\Downloads\flutter_windows_3.47.7-stable\flutter`)
- **Dart SDK:** Version `3.13.5`
- **Android SDK:** Version `36.1.0` (Target Android 8.0+ / API 26+)
- **Host OS:** Windows 11 (x64)
- **Active Devices Verified:** Chrome (Web), Edge (Web), Windows Desktop.

---

## 2. Technology Stack & Architectural Principles

| Layer | Selected Technology | Architectural Rationale |
|:---|:---|:---|
| **Mobile Client** | Flutter / Dart | Android-first, high performance on budget hardware, cross-platform capable. |
| **Backend** | Python 3.12 + FastAPI | High throughput async REST API with native Python ML/CV ecosystem integration. |
| **Backend Architecture** | Modular Monolith | Keeps operational overhead low; avoids distributed microservices complexity for early phases. |
| **Database** | PostgreSQL | Relational transactional integrity, foreign key cascading, and multi-run report history. |
| **Object Storage** | Private Object Storage (S3-compatible / Supabase Storage) | Secure, private bucket for photographs with signed, time-limited upload/download URLs. |
| **Offline Storage** | SQLite (on mobile device) | Outbox pattern with idempotent client UUIDs for resilient rural operation. |
| **AI Inference** | PyTorch + OpenCV | Server-side image validation, gatekeeper leaf filtering, and disease classification. |
| **Advisory Engine** | Audited Deterministic Rule Base | Eliminates chemical hallucination risks by tying all treatments strictly to UAS Bangalore / ICAR packages of practice. |

---

## 3. Current Milestone & Verified Progress

- **Current Milestone:** **Phase 1 — Documentation & Project Structure (COMPLETED)**
- **Verified Completed Items:**
  1. Base Flutter project scaffolding created in `mobile/` using `flutter create mobile`.
  2. Windows runner include path and compatibility configuration established (`.clangd`, `.vscode/c_cpp_properties.json`).
  3. Default Flutter counter starter application restored in `mobile/lib/main.dart` and `mobile/test/widget_test.dart`.
  4. Verified `flutter analyze` passes with **0 issues**.
  5. Verified `flutter test` passes with **All tests passed**.
  6. Created comprehensive, mutually consistent documentation in `docs/`:
     - `PRD.md`: Full product requirements and target metrics.
     - `ARCHITECTURE.md`: System components, API contracts, and Mermaid sequence/ER diagrams.
     - `RULES.md`: Mandatory engineering and agricultural safety guidelines.
     - `DESIGN.md`: UI/UX design specifications and rural accessibility constraints.
     - `TASKS.md`: Phased development roadmap with explicit completion tracking.
     - `MEMORY.md`: This project context and decision registry.

---

## 4. Architectural Decision Records (ADRs)

1. **ADR-01: Modular Monolith over Microservices**
   - *Decision:* Deploy backend as a unified modular monolith.
   - *Rationale:* Eliminates inter-service networking, distributed tracing, and multi-container deployment overhead while keeping clear internal code boundaries.

2. **ADR-02: Client-Side Pre-Flight Blur & WebP Compression**
   - *Decision:* Downsample and compress camera captures to $< 250\text{ KB}$ WebP locally on the device prior to network transmission.
   - *Rationale:* Rural 2G/EDGE networks fail on 8–15MB camera JPEGs $> 80\%$ of the time.

3. **ADR-03: Two-Stage AI Inference with Gatekeeper**
   - *Decision:* Validate leaf presence (Stage 1) before running deep disease classification (Stage 2), with temperature-scaled uncertainty thresholding.
   - *Rationale:* Prevents out-of-distribution hallucinations (e.g., tractors or non-plants classified as diseased leaves).

4. **ADR-04: Strict Deterministic Chemical Advisory Engine**
   - *Decision:* Forbid generative LLMs from inventing pesticide dosages or active ingredients. All chemical recommendations are looked up from audited tables.
   - *Rationale:* Indian *Insecticides Act, 1968* compliance and farmer crop safety.

5. **ADR-05: Idempotent SQLite Outbox Pattern**
   - *Decision:* Client generates a UUIDv4 `client_request_id` per draft. Server enforces `UNIQUE(user_id, client_request_id)`.
   - *Rationale:* Guarantees zero duplicate report creations or billing when mobile connections drop mid-upload.

---

## 5. Unresolved Decisions & Pending Setup Checks

1. **Cloud Database Provider:** Confirmation between managed Supabase PostgreSQL cloud vs. local Dockerized PostgreSQL for local development.
2. **Crop Dataset Selection:** Finalization of the specific open/field crop disease dataset (e.g., PlantVillage subset vs. Indian field datasets) to be benchmarked in Phase 6.
3. **SMS / Push Fallback Provider:** Finalization of DLT-registered SMS provider for Karnataka rural farmers lacking steady data connections.

---

## 6. Next Authorized Step

- **Awaiting User Instruction:** Do NOT proceed to Phase 2 (UI Foundation) or any application feature implementation without explicit instruction from the user.

---

## 7. Rules for Updating This Memory File

1. Update this document whenever a key architectural decision is made or changed.
2. Mark milestones as completed **only** when verified via automated tests or concrete artifacts.
3. Keep environment versions, paths, and constraints synchronized with active reality.
4. **Never write credentials, passwords, JWT tokens, or API secrets into this file.**
