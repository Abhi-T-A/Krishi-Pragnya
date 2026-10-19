# Engineering Rules & Guidelines

**Project:** KrishiPragnya  
**Problem Statement:** PS-3 — Crop Disease Detection for Small Farmers  
**Document Version:** 1.0.0  
**Status:** Mandatory for all contributors and automated agents  

---

## 1. Core Engineering Principles

1. **Inspect Before Editing**
   - Always view and inspect existing files, dependencies, and configuration before modifying code.
   - Do not make assumptions about existing code or directory structures without reading them first.

2. **Explain the Purpose of Significant Changes**
   - Clearly document *why* a change is necessary, what architectural constraint it satisfies, and what trade-offs were made.
   - Do not make silent architectural pivots.

3. **Keep Changes Small, Focused, and Atomic**
   - Each task or pull request must address a single concern.
   - Avoid broad, sweeping refactors that touch unrelated parts of the codebase simultaneously.

4. **Never Overwrite or Delete Unrelated Files**
   - Strictly respect the project boundaries. Never modify, clean up, or delete files outside the current task scope.
   - Do not remove generated platform scaffolding (Android, iOS, web, Windows, macOS, Linux) without explicit instruction.

5. **Never Claim Tests Passed Unless Actually Executed**
   - Never fabricate or assume test results.
   - Every claim of test success must be backed by a verified execution of `flutter test`, `flutter analyze`, or `pytest`.

---

## 2. Code Quality & Standards

### 2.1 Dart & Flutter Conventions
- Adhere strictly to the official Dart Style Guide (`effective_dart`).
- Maintain zero analyzer warnings in `flutter analyze`.
- Use `const` constructors wherever possible to optimize widget rebuild performance.
- Avoid deprecated APIs (e.g., use `Color.withValues(alpha: ...)` instead of `Color.withOpacity(...)`).
- Manage states predictably; avoid embedding business logic directly into UI build methods.

### 2.2 Python & FastAPI Conventions
- Adhere to PEP 8 standards with type hints throughout (`pydantic` v2 and typed parameters).
- Keep API routers thin; delegate business logic to dedicated service classes.
- Use async/await syntax for non-blocking I/O operations (database queries, network requests, storage operations).
- Handle model inference cleanly without blocking the main event loop.

---

## 3. Architectural Separation & Organization

1. **Feature-Based Modular Organization**
   - Organize code around domain capabilities (`crop_reports`, `diagnosis`, `advisory`, `expert_reviews`, `offline_sync`), not technical file types.
   - Keep module boundaries explicit with clean internal interfaces.

2. **Layered Separation of Concerns**
   - **UI Layer:** Pure presentation, responsive layouts, user interaction, and accessibility.
   - **Business Logic Layer:** State management, rule processing, and orchestration.
   - **Data Layer:** API clients, SQLite DAOs, repository pattern, and serialization.
   - Never mix SQL queries, raw HTTP calls, or ML inference directly into UI components.

---

## 4. Agricultural Safety & Domain Guardrails

1. **Zero Hallucination of Agricultural Chemicals**
   - Chemical pesticide names, active ingredients, concentrations, dilution ratios, and Pre-Harvest Intervals (PHI) must **NEVER** be generated dynamically by an unconstrained LLM.
   - All chemical guidance must be retrieved deterministically from audited, verified knowledge bases (e.g., UAS Bangalore, ICAR, CIB&RC).

2. **Honest AI Uncertainty Presentation**
   - Never present an unverified, probabilistic AI prediction as a confirmed, definitive diagnosis.
   - Always state the epistemic uncertainty or confidence level in plain Kannada and English.
   - When a confidence score falls below calibrated thresholds, explicitly inform the farmer and route the case toward expert verification.

3. **Decouple Confidence from Severity**
   - Never treat high prediction confidence as high disease severity.
   - Severity is a biological quantification of leaf damage and pathogen progression, requiring its own evaluation criteria.

---

## 5. Security & Input Validation

1. **Zero Hardcoded Secrets**
   - Never commit API keys, database connection strings, JWT secrets, or storage credentials into version control.
   - Use environment variables (`.env`) validated at runtime by configuration schemas.

2. **Strict Input & Image Validation**
   - Always validate uploaded file formats (JPEG, PNG, WebP) and enforce strict size limits ($\le 10\text{ MB}$ raw upload limit, target $< 250\text{ KB}$ compressed client payload).
   - Validate dimensions and decode integrity before passing images to inference pipelines.

3. **Enforce Server-Side Authorization**
   - Never trust client-supplied user roles or claims.
   - Verify ownership of reports and authorizations for expert reviews strictly on the backend.

---

## 6. UI/UX & Rural Accessibility Rules

1. **Explicit State Representation**
   - Every interactive screen must explicitly handle all four primary UI states:
     - **Loading State:** Clear progress indication with supportive text.
     - **Success / Content State:** Legible, high-contrast display of results.
     - **Empty State:** Helpful, friendly instructions on what to do next.
     - **Error State:** Actionable, human-friendly error messages with retry options (never raw stack traces).

2. **Offline-First Resilience**
   - The application must remain responsive and informative when network connectivity is lost.
   - Never block the UI thread waiting for network timeouts.
   - Clearly communicate when the app is operating in offline mode and show pending upload status.

3. **Kannada-First Linguistic Quality**
   - Kannada text must be culturally natural, grammatically correct, and use established regional farming terms (e.g., "ಭತ್ತದ ಬೆಂಕಿ ರೋಗ" for Paddy Blast, "ದುಂಡಾಣು ಎಲೆ ಕರಕು ರೋಗ" for Bacterial Leaf Blight).
   - Never rely on raw, unreviewed machine translations for safety-critical advisory text.

---

## 7. Testing & Verification Requirements

- Write unit tests for all business logic, validation rules, and data parsers.
- Write widget smoke tests for all primary screens.
- Run static analysis (`flutter analyze` for mobile, `ruff` / `flake8` / `mypy` for backend) before committing.
- Run the full test suite (`flutter test` / `pytest`) and ensure all tests pass cleanly.
