---
description: Design and implement a new feature end-to-end using MVVM
---

# Feature Workflow

This is the most important workflow. Follow it strictly to ship features fast without accumulating tech debt.

## 1. Understand the Requirements
- Clarify the user's goal. Ask questions if anything is ambiguous.
- Define the **minimum shippable version** — what's the simplest implementation that delivers value?
- List the screens, interactions, and data needed.

## 2. Plan the Architecture (BEFORE writing code)
Create a brief plan artifact covering:
- **Models**: What data structures are needed? Define as `struct` with `Codable` conformance.
- **ViewModels**: What state and logic does each screen need? Define as `@Observable` or `ObservableObject` class with `@Published` properties and `@MainActor`.
- **Views**: What SwiftUI views are needed? Keep each view small and focused (< 80 lines). Extract subviews early.
- **Services/Repositories**: What external data sources are involved? Define protocols for dependency injection.
- **File naming**: `[Feature]View.swift`, `[Feature]ViewModel.swift`, `[Feature]Model.swift`, `[Feature]Service.swift`.

## 3. Implement Bottom-Up
Build in this order to avoid forward references and enable testing:
1. **Models** — Data structures and DTOs
2. **Protocols** — Service interfaces for dependency injection
3. **Services** — API clients, persistence, concrete implementations
4. **ViewModels** — Business logic, state management, call services via protocol
5. **Views** — SwiftUI views, kept small and declarative

## 4. Follow These Swift/SwiftUI Rules
- Use `async/await` for all asynchronous work. No completion handlers.
- Use `@MainActor` on ViewModels to guarantee UI updates on the main thread.
- Use dependency injection via **protocol parameters** in ViewModel initializers.
- No storyboards. Ever.
- Prefer `NavigationStack` over deprecated `NavigationView`.
- Use `.task { }` modifier for on-appear async work instead of `.onAppear` + `Task { }`.

## 5. Verify the Implementation
- Build the project and confirm zero errors and zero warnings.
- Run on simulator to visually verify the feature works as expected.
- If the project has existing tests, run them to confirm nothing is broken.
- Walk the user through the implementation and ask for feedback.

## 6. Polish
- Add loading and error states to all async views.
- Ensure the feature works on different device sizes (iPhone SE → iPhone 16 Pro Max).
- Add meaningful accessibility labels where appropriate.
