---
name: sui
description: Use when writing, refactoring, or reviewing SwiftUI, SwiftData, or Swift concurrency code for iOS apps that should follow project-specific style, modern Apple APIs, and strict Swift 6 rules
license: MIT
argument-hint: "[task or review focus]"
metadata:
  author: topscrech
  version: "1.0"
---

# SUI

Use this skill for SwiftUI implementation, refactoring, and review; apply these rules as project conventions, while still following any local `AGENTS.md` or user instructions that are more specific. Always use it when working with Swift/SwiftUI projects

## Workflow

1. Inspect the target project's deployment target, Swift language mode, dependencies, and local conventions before editing
1. Prefer existing app structure, naming, view modifiers, and feature folders over adding new patterns
1. Keep changes narrow and compile after significant implementation work
1. If reviewing, report only genuine issues with file and line references, then give the smallest practical fix

## SwiftUI Style

- Prefer SwiftUI-only solutions and avoid UIKit, AppKit, and Combine unless requested
- Prefer no dots at the end of sentences in user-facing copy and comments
- Do not introduce third-party frameworks without asking first
- If ScrechKit is present, remember it imports SwiftUI and prefer modifiers such as `.hapticOn()`, `.title()`, `.secondary()`, and `.title(.secondary)`
- SwiftUI imports Foundation, so do not add Foundation imports just for SwiftUI files
- Prefer adaptive `.font()` fonts or ScrechKit equivalents over fixed font sizes
- Do not force specific font sizes; prefer Dynamic Type
- Avoid hard-coded padding and stack spacing unless requested or already established locally
- Prefer static member lookup where possible, such as `.circle` rather than `Circle()` and `.borderedProminent` rather than `BorderedProminentButtonStyle()`
- Use `foregroundStyle()` instead of `foregroundColor()`
- Use `clipShape(.rect(cornerRadius:))` instead of `cornerRadius()`
- Do not use `fontWeight()` unless there is good reason; use `bold()` for bold text
- Avoid `AnyView` unless it is absolutely required
- Avoid computed properties for view decomposition; move UI into separate `View` structs
- Split long views into subviews, with each subview in its own file
- Break different types into different Swift files rather than placing multiple structs, classes, or enums in one file
- Follow feature-based folder structure and strict naming conventions
- Use `UpperCamelCase` for types and `lowerCamelCase` for values and functions
- SwiftUI view type names should usually end in `View`
- When defining enums, prefer concise single-line cases without associated values, for example `case cloud, game, bot`
- Prefer shorthand closure arguments in simple `ForEach` bodies, such as `ForEach(items) { ...$0... }`
- When making a `ForEach` from an enumerated sequence, do not convert it to an array first; prefer `ForEach(x.enumerated(), id: \.element.id)`

## Navigation And Tabs

- Always use `NavigationStack` instead of `NavigationView`
- Use `navigationDestination(for:)` to declare navigation destinations
- When targeting iOS 18 or later, use `Tab(title:image:value:content:)` or related `Tab(...)` initializers instead of `.tabItem`
- Apply `.navigationTitle(...)` to the root `TabView` or containing navigation view, not to individual child tab views
- Hide scroll indicators with `.scrollIndicators(.hidden)` rather than `showsIndicators: false`

## State And Data Flow

- For iOS 17 or later, prefer `@Observable` classes instead of `ObservableObject`
- Place view logic into view models or similar testable types
- Instead of dependency injection through subview initializers, pass view models to subviews using `@Environment` or `@EnvironmentObject`
- Do not use `Binding(get:set:)` unless there is no readable alternative
- Avoid `onTapGesture()` unless the tap location or tap count is needed; use `Button` for ordinary actions
- Prefer `Button("Title", systemImage: "symbol", action: action)` for image buttons and always include text
- Prefer `Button(_:systemImage:action:)` over `Button(action:label:)` when it fits
- Use the non-deprecated `onChange()` variants that accept either zero or two parameters, never the one-parameter variant

## SwiftData

If SwiftData is configured with CloudKit:

- Never use `@Attribute(.unique)`
- Model properties must have default values or be optional
- Relationships must be optional

## Swift And Foundation

- Swift 6 language mode and strict Swift concurrency should be assumed
- MainActor default isolation mode should be assumed, so functions and properties are `@MainActor` by default
- All API calls must use async/await
- Avoid old-style Grand Central Dispatch such as `DispatchQueue.main.async()`
- Use `Task.sleep(for:)` instead of `Task.sleep(nanoseconds:)`
- Avoid force unwraps and force try unless failure is unrecoverable
- Prefer Swift-native string APIs, such as `replacing("hello", with: "world")` instead of `replacingOccurrences(of:with:)`
- Prefer modern Foundation APIs such as `URL.documentsDirectory` and `appending(path:)`
- Filter user-facing text with `localizedStandardContains()` rather than `contains()`
- Never use C-style number formatting; prefer format styles such as `Text(abs(change), format: .number.precision(.fractionLength(2)))`

## Rendering And Layout APIs

- Do not use `UIScreen.main.bounds` for available size
- Avoid `GeometryReader` when newer APIs such as `containerRelativeFrame()` or `visualEffect()` work
- When rendering SwiftUI views, prefer `ImageRenderer` to `UIGraphicsImageRenderer`

## Build And Versioning

- Build after significant changes so compile errors can be fixed
- Build number `0` is acceptable; do not change it to `1`
- Do not commit secrets or environment-specific files

## Git

- When asked to push, commit all existing changes first
- When asked to check out a branch or revision, do it in the main project without creating copies
- Keep commit messages short and action-oriented, such as `fixed settings crash`
- PR descriptions should summarize user-visible impact and list affected platforms or schemes

## Output

Keep output direct and actionable. When making code changes, summarize changed files, important behavior changes, and the build or test command that was run
