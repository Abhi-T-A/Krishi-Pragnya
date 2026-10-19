# Product Requirements Document (PRD)

**Project:** KrishiPragnya  
**Problem Statement:** PS-3 — Crop Disease Detection for Small Farmers  
**Document Version:** 1.0.0  
**Status:** Approved for Project Setup & Planning Phase (Implementation Pending)  
**Last Updated:** October 2026  

---

## 1. Problem Statement

Smallholder farmers in India, particularly in Karnataka, face severe crop yield losses (often 20% to 40% annually) due to unmanaged or misidentified crop diseases and pest infestations.

### Core User Constraints
1. **Device Limitations:** Entry-level Android smartphones (Android 8.0–12.0, 2GB–3GB RAM, budget cameras with variable focus and dynamic range).
2. **Network Constraints:** Spotty, intermittent 2G/3G/4G connectivity in rural farming zones; high latency and frequent connection drops.
3. **Information & Language Barriers:** Technical agricultural terminology in English or complex scientific literature is inaccessible. Farmers require plain Kannada explanations supplemented with visual cues and audio narration.
4. **Economic & Safety Risks:** Incorrect chemical treatments (buying wrong fungicides/pesticides based on guesswork or unverified local store advice) waste limited financial resources, degrade soil health, and risk human toxicity without resolving the plant disease.
5. **Access to Agronomists:** Local Krishi Vigyan Kendras (KVKs) and extension officers are overburdened; rapid in-person field inspections are rarely available within the critical 24–48 hour disease progression window.

---

## 2. Product Vision & Measurable Goals

### 2.1 Product Vision
**KrishiPragnya** is an accessible, farmer-first crop health platform that empowers smallholder farmers to capture leaf photographs, receive rapid and reliable disease identification, understand the urgency/severity of the infection, access safe, evidence-based agricultural guidance in Kannada, and escalate uncertain or high-risk cases to qualified agricultural experts.

### 2.2 Measurable Target Goals (Post-Implementation Targets)
- **Diagnostic Turnaround:** Server-side disease classification and initial advisory response delivered within $< 5$ seconds under standard mobile connectivity ($> 500\text{ Kbps}$).
- **Bandwidth Efficiency:** Client-side image optimization keeping total payload per report under $250\text{ KB}$ (via WebP/JPEG compression), achieving $> 95\%$ upload success on intermittent rural connections.
- **Diagnostic Safety:** Zero hallucinated or unverified chemical prescriptions. $100\%$ of delivered chemical recommendations must map directly to audited, authorized agricultural guidelines (e.g., UAS Bangalore, ICAR, CIB&RC).
- **Language Inclusivity:** Full parity between Kannada and English across all diagnosis summaries, severity alerts, safety precautions, and audio playback.
- **Offline Reliability:** Zero data loss for field captures made offline. $100\%$ of offline drafts and cached advisories persist in local storage until synchronized.
- **Triage & Human Escalation:** $100\%$ of uncertain AI predictions (confidence score below calibrated operating threshold) or severe disease cases must offer a seamless escalation path to human agricultural experts.

---

## 3. Six Core Capabilities

| ID | Capability | Description | Technical Owner | Target Status |
|:---|:---|:---|:---|:---|
| **C1** | **AI Crop Disease Detection** | Multi-class image classification identifying disease symptoms on supported crops (e.g., Paddy, Tomato, Ragi) from photos taken by the farmer. Rejects non-plant images and flags uncertain predictions. | AI Inference Engine / FastAPI | Proposed |
| **C2** | **Disease Severity Assessment** | Estimates disease progression and urgency (Low, Moderate, High, or Unknown) based on symptomatic leaf coverage and disease pathology rather than raw classification confidence. | Severity Module / CV / Rule Engine | Proposed |
| **C3** | **Evidence-Based Treatment Guidance** | Safe, practical recommendations consisting of cultural/organic controls, approved chemical treatments with precise dilution ratios, Pre-Harvest Intervals (PHI), and safety precautions. | Advisory Engine / Knowledge Base | Proposed |
| **C4** | **Kannada & English Localization** | Native Kannada UI labels, disease common names (e.g., "ಭತ್ತದ ಬೆಂಕಿ ರೋಗ" for Paddy Blast), structured advice, and audio narration (Text-to-Speech / pre-recorded clips). | Mobile UI / Localization Service | Proposed |
| **C5** | **Offline & Low-Connectivity Support** | Local device persistence of reports, drafts, and historical advisories in SQLite. Outbox synchronization with idempotent retries when connectivity returns. | Mobile SQLite Outbox / Sync Gateway | Proposed |
| **C6** | **Agricultural Expert Verification** | Asynchronous workflow allowing farmers to request expert review for ambiguous or critical cases, with dedicated expert review capabilities and notification updates. | Expert Workflow Service / PostgreSQL | Proposed |

---

## 4. User Personas

### Persona 1: Smallholder Farmer (Basavaraj, Mandya District)
- **Profile:** Cultivates 2 acres of paddy and ragi. Owns a budget Android smartphone. Primary language is Kannada.
- **Need:** Notices brown spindle-shaped spots on paddy leaves. Needs to know immediately what the disease is, whether it will destroy his harvest, what exact treatment to purchase, and how much water to mix with it.
- **Key Pain Point:** Cannot read English chemical bottles and cannot afford to buy expensive, incorrect pesticides.

### Persona 2: Agricultural Extension Officer / KVK Scientist (Dr. Shwetha)
- **Profile:** Agricultural scientist at a regional research station. Monitors regional disease outbreaks.
- **Need:** Reviews ambiguous or severe disease reports flagged by the AI system, confirms or updates the diagnosis, attaches specific localized management notes, and issues early warnings.
- **Key Pain Point:** Lacks a centralized digital triage tool to review verified farmer field reports without traveling long distances.

---

## 5. Functional Requirements (FR)

### 5.1 Crop Selection & Image Capture (Proposed)
- **FR-1.1:** The app shall allow the farmer to select from supported crops (initial baseline: Paddy, Tomato, Ragi).
- **FR-1.2:** The app shall guide the farmer with on-screen visual prompts for camera positioning (proper distance, good lighting, single leaf focus).
- **FR-1.3:** The app shall perform client-side image validation (blur detection, minimum resolution) prior to initiating network upload.
- **FR-1.4:** The app shall compress images to a lightweight format (target: WebP/JPEG $\le 250\text{ KB}$, max dimension 1024px) before transmission.

### 5.2 AI Inference & Quality Filtering (Proposed)
- **FR-2.1:** The backend shall reject invalid, corrupt, or non-plant images with an explicit, farmer-friendly explanation.
- **FR-2.2:** The inference engine shall output disease classification probabilities along with an epistemic confidence score.
- **FR-2.3:** The system shall enforce calibrated confidence thresholds: predictions below threshold must be marked as `UNCERTAIN` and routed to the expert verification workflow.
- **FR-2.4:** The backend shall log the model version tag with every prediction for traceability.

### 5.3 Severity Assessment (Proposed)
- **FR-3.1:** The severity module shall evaluate visual symptom area and crop-specific pathology to assign a severity classification (`LOW`, `MODERATE`, `HIGH`, `UNKNOWN`).
- **FR-3.2:** Severity must be decoupled from prediction confidence (e.g., high confidence in early-stage disease equals Low severity).
- **FR-3.3:** High-severity conditions must display immediate containment warnings.

### 5.4 Agricultural Advisory & Safety (Proposed)
- **FR-4.1:** All advisories must be sourced exclusively from pre-audited agricultural packages of practice (e.g., UAS Bangalore, ICAR).
- **FR-4.2:** Advisories must present a structured three-tier response:
  1. *Immediate Action & Cultural Control:* Non-chemical sanitization (e.g., remove infected leaves, manage field water).
  2. *Chemical Control (when justified):* Approved active ingredient, standard dilution ratio (e.g., "0.6g per 1L water"), and commercial trade example.
  3. *Safety Precaution & Waiting Period:* Protective gear instructions and Pre-Harvest Interval (PHI) in days.
- **FR-4.3:** The system must never permit generative hallucination of chemical formulations or unapproved dosages.

### 5.5 Language & Accessibility (Proposed)
- **FR-5.1:** All screens, buttons, error messages, and advisories must be available in Kannada (`kn`) and English (`en`).
- **FR-5.2:** Farmers must be able to switch language at any time without losing current state.
- **FR-5.3:** The app shall provide audio playback for diagnosis results and advisory instructions.

### 5.6 Offline Resilience & Synchronization (Proposed)
- **FR-5.1:** If network connectivity is unavailable, captured photos and report drafts must be saved securely to the local SQLite database.
- **FR-5.2:** The app must maintain an Outbox queue with distinct synchronization statuses (`LOCAL_DRAFT`, `PENDING_UPLOAD`, `SYNCED`).
- **FR-5.3:** When connectivity is restored, the synchronization engine must use client-generated idempotency keys (UUIDv4) to guarantee zero duplicate report creations.
- **FR-5.4:** Farmers must be able to browse all past reports and cached advisories completely offline.

### 5.7 Expert Review & Case Management (Proposed)
- **FR-7.1:** Farmers can explicitly request human expert verification for any analyzed report.
- **FR-7.2:** Cases with `UNCERTAIN` AI predictions or `HIGH` severity must automatically suggest expert verification.
- **FR-7.3:** Authorized experts must have a review interface to inspect the image, view model predictions, update the diagnosis, and submit verified advice.
- **FR-7.4:** The status of the review (`REQUESTED`, `ASSIGNED`, `IN_REVIEW`, `VERIFIED`) must update the farmer's report asynchronously.

---

## 6. Non-Functional Requirements (NFR)

- **NFR-1 (Performance):** Total API response time for diagnosis under stable connection $\le 5.0\text{ seconds}$. Client-side blur check $\le 300\text{ ms}$.
- **NFR-2 (Reliability):** Zero report loss during network interruptions. Idempotent backend APIs reject duplicate submissions gracefully.
- **NFR-3 (Usability):** High-contrast UI elements, clear visual icons accompanying text, large touch targets ($\ge 48\times 48\text{ dp}$), readable fonts under bright daylight.
- **NFR-4 (Security):** Farmer phone numbers and images stored in private object storage with signed, time-limited access URLs. Backend role-based authorization for expert actions.
- **NFR-5 (Maintainability):** Modular monolith backend architecture with strict boundary separation between report management, inference, advisory rules, and expert reviews.

---

## 7. Scope & Boundaries

### 7.1 In-Scope (MVP Baseline)
- Android-first Flutter mobile application.
- Target crops for initial feasibility: **Paddy (ಭತ್ತ)**, **Tomato (ಟೊಮೇಟೊ)**, and **Finger Millet / Ragi (ರಾಗಿ)**.
- Pre-audited knowledge base for common diseases of the target crops in Karnataka.
- Offline SQLite storage for drafts and past reports.
- Backend REST API (FastAPI) with PostgreSQL data layer.
- Two-tier language support: Kannada and English.

### 7.2 Out-of-Scope (Future Enhancements)
- Autonomous chemical spraying drone integration.
- Commercial e-commerce pesticide sales or payment gateways.
- Multi-crop broadacre satellite remote sensing.
- Real-time video stream continuous disease diagnosis.
- Unconstrained conversational LLM chemical prescribing.

---

## 8. Assumptions & Technical Risks

| Assumption / Risk | Impact | Mitigation Strategy |
|:---|:---|:---|
| **Risk:** Training datasets (e.g. lab backgrounds) fail on complex real-field backgrounds. | High | Mandate field-collected evaluation datasets. Implement image quality checks and gatekeeper leaf detection before classification. |
| **Risk:** Lack of installed Kannada TTS engine on budget Android devices. | Medium | 3-tier audio fallback: device TTS $\rightarrow$ pre-recorded native audio clips for standard advisories $\rightarrow$ server-rendered fallback. |
| **Risk:** Weak or dropped cellular connectivity during upload. | High | Client-side image compression to $< 250\text{ KB}$ and SQLite Outbox queue with resumable/idempotent retry logic. |
| **Risk:** Unsafe pesticide recommendations lead to crop damage. | Critical | Strict deterministic advisory tables tied to official CIB&RC / State Agricultural University packages of practice. LLMs forbidden from generating dosages. |
| **Assumption:** Farmers can point their phone camera at a single symptomatic leaf with reasonable framing. | Medium | Provide clear visual bounding guide and live framing instructions in the camera interface. |

---

## 9. Dataset & Model Validation Requirements

1. **Validation Protocol:** Model performance must be evaluated on an independent, hold-out test set containing **in-field photographs** taken under varied natural lighting, shadow conditions, and background clutter.
2. **Acceptance Thresholds:**
   - Minimum Top-1 Precision on primary disease classes: $\ge 85\%$.
   - Out-of-Distribution (non-plant, random objects) rejection rate: $\ge 90\%$.
   - Zero tolerance for classifying non-crop images as healthy or diseased without uncertainty flagging.
3. **Model Governance:** Every deployed model artifact must have an immutable version tag (`model_version_id`), registered input resolution, preprocessing parameters, and class label mapping stored in the database registry.
