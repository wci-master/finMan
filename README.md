# finMan — Personal Finance Manager (Android)

Last updated: 2025-09-17

## 🔖 Project Title & Description

finMan is a mobile Android application designed to help individuals take control of their personal finances with ease and clarity. The app provides tools to track income, manage expenses, set and monitor budgets, visualize cash flow, and progress toward financial goals — all in one place.

Who it's for:
- Individuals and families who want a lightweight, privacy-respecting finance tracker on their Android phone.
- People who prefer local-first storage with optional cloud sync.
- Power users who want integrations with bank CSV imports and AI-assisted insights.

Why it matters:
- Many users feel overwhelmed by budgeting and transaction tracking. finMan aims to reduce friction with a clean UI, helpful automation, and actionable insights so users can build healthier money habits.

Core features (MVP):
- Add and categorize income and expenses
- Recurring transactions (bills, salaries)
- Simple budgets per category with alerts
- Financial goals (savings targets) and progress tracking
- CSV import/export for bank statements
- Local-first data storage with optional secure cloud backup

Future / stretched features:
- Bank integrations (Plaid-like or OFX imports)
- AI-powered insights and suggestions (savings tips, anomaly detection)
- Multi-currency support and exchange rates
- Shared accounts / family mode

## 🛠️ Tech Stack

- Platform: Android
- Primary language: Kotlin (Kotlin Multiplatform considered for future iOS share)
- UI framework: Jetpack Compose
- Architecture: MVVM + Clean Architecture boundaries (UI, domain, data)
- Dependency injection: Hilt
- Persistence: Room (SQLite) for local-first storage; optional encrypted storage with SQLCipher or Jetpack Security
- Networking (for optional cloud sync): Retrofit + OkHttp
- Background work: WorkManager (for scheduled backups, recurring transactions)
- Coroutines + Flow for async and reactive streams
- JSON: kotlinx.serialization or Moshi (choose kotlinx.serialization for Kotlin-first)
- Build: Gradle (Kotlin DSL optionally)
- CI: GitHub Actions (lint, build, unit tests, instrumentation tests)
- Testing: JUnit, Robolectric (unit + some UI), AndroidX Test / Espresso for instrumentation UI tests
- Static analysis & formatting: Ktlint / Detekt / Android Lint
- Release distribution: Play Store (internal / closed testing tracks)

Developer tools:
- Android Studio (Arctic Fox or newer)
- Git + GitHub for source control
- Firebase (optional) for analytics, remote config, and Crashlytics (if privacy policy allows)

## 🧠 AI Integration Strategy

We will use AI tools throughout the project to speed development, improve quality, and generate helpful user-facing features. Below are structured plans for each AI use area.

1) Code generation (IDE / CLI agent)
- Purpose: scaffold features, generate boilerplate, propose implementation snippets, and accelerate API client generation.
- Workflow:
  - Use an AI-powered coding assistant integrated in the IDE (or a CLI agent) to scaffold new screens, ViewModels, and repository interfaces from high-level prompts.
  - Keep generated code minimal and review every AI suggestion. The agent will produce:
    - Jetpack Compose UI screen stubs from a component spec (title, inputs, lists, actions)
    - ViewModel methods with coroutine flows and sample state classes
    - Room entity and DAO scaffolding from a data model prompt
    - Retrofit interfaces / data mappers from API specs or OpenAPI documents
  - Example prompt for scaffolding a transaction list screen:
    "Generate a Jetpack Compose TransactionListScreen with a ViewModel exposing StateFlow<List<TransactionUi>> and events for add/edit/delete. Use Hilt for injection and a Repository interface. Keep UI stateless and place business logic in ViewModel."

2) Testing (AI-assisted test generation)
- Purpose: produce unit and instrumentation test scaffolds, edge-case suggestions, and property-based test ideas.
- Tools & approach:
  - Use the AI assistant to create unit tests for ViewModels and domain use-cases (JUnit + Mockito / MockK). Prompt the agent with function signatures and expected behavior.
  - For UI, ask the agent to generate Espresso or Compose UI tests verifying critical flows: add transaction, edit, budget exceed alert, and recurring transaction creation.
  - Use mutation testing (Stryker or similar where available) and have the AI suggest additional tests for uncovered branches.
  - Example test prompt: "Write JUnit tests for TransactionViewModel.addTransaction(Transaction) ensuring new transaction is saved, state updates, and error states are handled when repository throws an exception. Use MockK to mock the repository." 

3) Documentation (docstrings, inline comments, README maintenance)
- Purpose: keep docs consistent, searchable, and up-to-date while minimizing manual effort.
- Workflow:
  - Auto-generate initial KDoc comments for public classes and methods using the AI assistant.
  - When changing public APIs or adding features, run a small doc generation pass asking the assistant to generate or refresh KDocs and a short changelog entry.
  - Use AI to maintain the README feature list and high-level architecture diagrams (text + mermaid diagrams) from the codebase by feeding the assistant a file tree and key file contents.

4) Context-aware techniques (how we feed inputs to the AI)
- Purpose: make AI outputs accurate and reproducible by providing structured context.
- Inputs and how we'll provide them:
  - API specs: Provide OpenAPI / Swagger docs or example JSON request/response payloads. The agent will use this to generate Retrofit interfaces and data mappers.
  - File trees: Use the repository file tree (or a trimmed tree) and recent diffs to ensure generated code matches project conventions and existing modules. Prefer sending only relevant files (e.g., domain model, DAO, and existing ViewModel) to keep prompts focused.
  - Diffs: When refactoring, feed the AI a git diff or patch to explain the change surface area. This helps it produce migration guidance, tests, and updated docs.
  - Environment & toolchain: Provide the build.gradle, app module structure, and dependency versions when asking for build changes.

5) Privacy, security & guardrails
- Never send user data or real financial data to third-party AI services. When using real data for testing, generate synthetic anonymized samples.
- Use local or enterprise-hosted AI models when possible for code generation to avoid sending source code externally.
- Create a review step: every AI-generated change must pass a human review, static analysis, and CI checks before merging.

## Developer workflow examples (concrete prompts & flows)

- Scaffolding a new feature (Add Budget screen):
  1. Prompt: "Create a Jetpack Compose AddBudgetScreen with fields: name, amount, category dropdown, save/cancel buttons. Add a ViewModel using Hilt that exposes form state and a saveBudget() suspend function which calls BudgetRepository.save(Budget). Include KDoc comments."
  2. AI returns code skeletons for screen, ViewModel, entity, and DAO. Developer reviews and integrates.

- Generating tests for a critical use-case:
  1. Prompt: "Write JUnit tests for BudgetInteractor that verifies budget creation reduces available category balance and triggers an over-budget alert when applicable. Use MockK and coroutine test utils." 
  2. AI suggests edge cases and a test file. Developer runs tests, adjusts assertions, and finalizes.

## Project structure (recommended)

- /app — Android app module
  - /src/main/java/... — Kotlin source
  - /src/androidTest — instrumentation tests
  - /src/test — unit tests
- /core — shared domain logic (useful if KMP later)
- /data — Room DAOs, repository implementations, mappers
- /ui — Compose screens and UI components
- /docs — architecture notes, diagrams, RFCs

## Quality gates

- Local: ktlint, detekt, Gradle build & unit tests
- CI (GitHub Actions): build matrix (kotlin versions / sdk), lint, unit tests, instrumentation smoke (if runner available), and static analysis reports. PRs must pass checks and have at least one approving review before merge.

## Minimum viable timeline (example)
- Week 1: Project setup, CI, core data models, Room schema, basic Add/View transactions UI
- Week 2: Recurring transactions, budgets, local notifications, CSV import/export
- Week 3: Goals, simple analytics, tests, and polish
- Week 4+: Optional cloud sync, AI insights, bank integrations

## Notes & next steps
- Keep the repository local-first and privacy-centric. Use AI to accelerate routine developer tasks but keep human review in the loop.
- Next actions to start coding:
  1. Initialize Android Studio project (module + Gradle files)
  2. Add CI workflow and linters
  3. Scaffold core data models and Room schema