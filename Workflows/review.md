---
description: Code review focused on Swift/SwiftUI quality, performance, and architecture
---

# Review Workflow

When reviewing code, evaluate each of these areas and provide **specific, actionable feedback** with code examples for every issue found.

## 1. Architecture (MVVM Compliance)
- Is the code properly separated into Model, ViewModel, and View layers?
- Are ViewModels free of `import SwiftUI`? (They should only depend on Foundation/Combine/Swift).
- Are Views thin and declarative — no business logic, no direct API calls?
- Are dependencies injected via protocols, not hardcoded concrete types?

## 2. SwiftUI View Health
- Are views small (ideally < 80 lines)? If not, suggest extracting subviews.
- Is `body` a single, readable expression? Watch for deeply nested conditionals.
- Are the correct property wrappers used?
  - `@StateObject` for ownership (created here), `@ObservedObject` for injection (passed in).
  - `@State` for simple local UI state, `@Binding` for two-way child communication.
- Is `.task { }` used instead of `.onAppear` + `Task { }` for async on-appear work?

## 3. Concurrency & Threading
- Is `async/await` used consistently? Flag any remaining completion handler patterns.
- Are ViewModels marked with `@MainActor` to prevent off-main-thread UI updates?
- Are long-running tasks properly using `Task` with cancellation support?
- Check for **retain cycles**: closures inside `Task` or `sink` that capture `self` strongly.

## 4. Performance
- Are there unnecessary re-renders? Check for `@Published` properties that fire too often.
- Are images loaded lazily? (`AsyncImage` or a caching library, not synchronous loading).
- Are expensive computations cached or moved out of the view's `body`?
- Are lists using `LazyVStack` / `LazyHStack` instead of `VStack` / `HStack` for large datasets?

## 5. Error Handling & Edge Cases
- Are all `async` calls wrapped in `do/catch` with meaningful error handling?
- Are loading, empty, and error states handled for every async view?
- Are optionals unwrapped safely? Flag any force-unwraps (`!`) that aren't justified.

## 6. Code Quality
- Is naming clear and consistent? (`camelCase` for properties/functions, `PascalCase` for types).
- Are there any dead code, unused imports, or commented-out blocks?
- Is the code simple? Flag overengineering — prefer straightforward solutions.
- Would a new developer understand this code without additional context?

## 7. Deliver the Review
- Group findings by severity: 🔴 **Critical** → 🟡 **Improvement** → 🟢 **Nitpick**.
- For each issue, provide:
  - What the problem is
  - Why it matters
  - A concrete code suggestion to fix it
- End with a **summary**: overall quality assessment and top 3 priorities to address.
