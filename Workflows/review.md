---
description: Code review focused on Swift/SwiftUI quality, performance, and architecture
---

# Review Workflow

When reviewing code, evaluate each of these areas and provide **specific, actionable feedback** with code examples for every issue found.

## 1. Architecture (MVVM Compliance)
- Is the code properly separated into Model, ViewModel, and View layers?
- Are ViewModels free of `import SwiftUI` whenever possible? (They should ideally depend only on Foundation/Combine/Swift.)
- Are Views thin and declarative — no business logic, no direct API calls?
- Are dependencies injected via protocols, not hardcoded concrete types?
- Does the ViewModel contain only presentation logic, with networking or persistence handled by services?

## 2. SwiftUI View Health
- Are views reasonably small and focused? If not, suggest extracting subviews.
- Is `body` a single, readable expression? Watch for deeply nested conditionals.
- Are the correct property wrappers used?
  - `@StateObject` for ownership (created here), `@ObservedObject` for injection (passed in).
  - `@State` for simple local UI state, `@Binding` for two-way child communication.
- Is `.task { }` used instead of `.onAppear` + `Task { }` for async on-appear work when appropriate?
- Ensure no heavy computations occur directly inside the `body` property.

## 3. Concurrency & Threading
- Is `async/await` used consistently? Flag any remaining completion handler patterns.
- Are ViewModels marked with `@MainActor` to prevent off-main-thread UI updates?
- Are long-running tasks properly using `Task` with cancellation support?
- Check for **retain cycles**: closures inside `Task` or `sink` that capture `self` strongly.

## 4. Performance
- Are there unnecessary re-renders? Check for `@Published` properties that fire too often.
- Are images loaded lazily? (`AsyncImage` or a caching approach, not synchronous loading).
- Are expensive computations cached or moved out of the view's `body`?
- For large datasets, ensure `List`, `LazyVStack`, or `LazyHStack` are used instead of non-lazy stacks.

## 5. Error Handling & Edge Cases
- Are all `async` calls wrapped in `do/catch` with meaningful error handling?
- Are loading, empty, and error states handled for every async view?
- Are optionals unwrapped safely? Flag any force-unwraps (`!`) that aren't justified.

## 6. Code Quality
- Is naming clear and consistent? (`camelCase` for properties/functions, `PascalCase` for types).
- Are there any dead code, unused imports, or commented-out blocks?
- Is the code simple? Flag overengineering — prefer straightforward solutions.
- Would a new developer understand this code without additional context?

## 7. Testability
- Can the ViewModel be tested without launching SwiftUI?
- Are services injected via protocols so they can be mocked?
- Are side effects (networking, storage) isolated from UI logic?

## 8. Feature Modularity
- Does the code belong in the current feature module?
- Are reusable UI elements extracted into shared components when appropriate?
- Is shared logic placed in Core or shared services instead of duplicated across features?

## 9. Deliver the Review
- Group findings by severity: 🔴 **Critical** → 🟡 **Improvement** → 🟢 **Nitpick**.
- For each issue, provide:
  - What the problem is
  - Why it matters
  - A concrete code suggestion to fix it
- End with a **summary**: overall quality assessment and top 3 priorities to address.