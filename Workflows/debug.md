---
description: Systematic iOS debugging workflow for Swift/SwiftUI apps
---

# Debug Workflow

Follow these steps in order. Do NOT skip ahead — each step narrows the problem space.

## 1. Reproduce the Bug
- Ask the user to describe the exact steps to reproduce.
- Identify whether it happens on simulator, device, or both.
- Note the iOS version and device model if relevant.

## 2. Read the Error Output
- Check the **Xcode console** for error messages, warnings, and crash logs.
- Look for `Fatal error`, `EXC_BAD_ACCESS`, `Thread 1: signal SIGABRT`, or SwiftUI runtime warnings.
- If there's a crash log, identify the **faulting thread** and the **last meaningful frame** in the stack trace.

## 3. Isolate the Problem Layer (MVVM)
- **View layer**: Is the UI not rendering correctly? Check `@State`, `@Binding`, `@StateObject`, `@ObservedObject` usage. Verify the view body is not doing heavy computation.
- **ViewModel layer**: Is the logic wrong? Add `print()` statements or breakpoints inside the ViewModel methods. Check `@Published` properties are being set on the main actor.
- **Model / Service layer**: Is the data wrong? Inspect API responses, decoded models, and persistence layer (UserDefaults, CoreData, etc.).

## 4. Check Common SwiftUI Pitfalls
- **Retain cycles**: Look for `[weak self]` missing in closures, especially in Combine `sink` or `Task` blocks.
- **State lifecycle**: Ensure `@StateObject` is used for ownership and `@ObservedObject` for injection — never the reverse.
- **Main thread violations**: Verify UI updates happen on `@MainActor`. Check for `await` calls that resume on background threads.
- **Navigation bugs**: Check for duplicate `NavigationStack`/`NavigationView` nesting.
- **Preview crashes**: If only previews crash, check for missing mock data or environment objects.

## 5. Use Xcode Debugging Tools
- Set **breakpoints** at the suspected fault line and inspect variables.
- Use **`po`** in the debugger console to print object state.
- Use **Instruments** (Leaks, Time Profiler, Allocations) if the issue is performance or memory-related.
- Use **View Hierarchy Debugger** (Debug → View Debugging → Capture View Hierarchy) if the UI looks wrong.

## 6. Fix, Verify, and Explain
- Apply the minimal fix. Prefer simple, obvious fixes over clever ones.
- Verify the fix resolves the original reproduction steps.
- Explain **what caused the bug** and **why the fix works** so the user learns from it.
- If the fix reveals a pattern that could cause future bugs, suggest a preventive measure.
