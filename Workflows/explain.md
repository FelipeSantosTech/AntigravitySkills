---
description: Explain Swift/SwiftUI code with architecture context and visual diagrams
---

# Explain Workflow

Use this workflow when the user asks you to explain code. The goal is clarity — make the user understand *why* the code exists, not just *what* it does.

## 1. Identify the Scope
- Is the user asking about a single file, a function, a view, or an entire feature?
- Determine the MVVM layer: **Model**, **ViewModel**, or **View**.

## 2. Provide a High-Level Summary
- Start with a **1-2 sentence plain-English summary** of what the code accomplishes.
- State its role in the app's architecture (e.g., "This is the ViewModel that manages the user's watchlist state").

## 3. Break Down the Components
For each meaningful block of code, explain:
- **Purpose**: What does this block do?
- **Inputs/Outputs**: What data flows in and out?
- **Dependencies**: What other types, protocols, or services does it rely on?

## 4. Highlight Swift/SwiftUI-Specific Patterns
Call out and briefly define any of these when used:
- **Property wrappers**: `@State`, `@Binding`, `@StateObject`, `@ObservedObject`, `@EnvironmentObject`, `@Published`, `@AppStorage`
- **Concurrency**: `async/await`, `Task`, `@MainActor`, `actor`
- **Protocols & Generics**: Protocol conformances, associated types, `some View`
- **SwiftUI modifiers**: `.task`, `.onAppear`, `.sheet`, `.navigationDestination`
- **Combine**: `$publisher`, `sink`, `assign`

## 5. Show the Data Flow
- Describe how data moves through the MVVM layers for this code.
- When explaining a feature that spans multiple files, include a **Mermaid diagram** showing the relationships:
```
View → ViewModel → Service/Repository → Model
```

## 6. Note Design Decisions & Trade-offs
- Why was this approach chosen over alternatives?
- Are there any trade-offs (e.g., simplicity vs. flexibility)?
- Flag anything that looks like it could be improved, but frame it as an observation, not a criticism.
