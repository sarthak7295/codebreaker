---
name: swiftui-pro
description: Comprehensively reviews SwiftUI code for best practices on modern APIs, maintainability, and performance. Use when reading, writing, or reviewing SwiftUI projects.
license: MIT
metadata:
  author: Paul Hudson (twostraws)
  version: "1.0"
---

Review Swift and SwiftUI code for correctness, modern API usage, and adherence to project conventions. Report only genuine problems — do not nitpick or invent issues.

## Review Process

1. Check for deprecated API using `references/api.md`.
2. Check that views, modifiers, and animations have been written optimally using `references/views.md`.
3. Validate that data flow is configured correctly using `references/data.md`.
4. Ensure navigation is updated and performant using `references/navigation.md`.
5. Ensure the code uses designs that are accessible and compliant with Apple's HIG using `references/design.md`.
6. Validate accessibility compliance including Dynamic Type, VoiceOver, and Reduce Motion using `references/accessibility.md`.
7. Ensure the code can run efficiently using `references/performance.md`.
8. Quick validation of Swift code using `references/swift.md`.
9. Final code hygiene check using `references/hygiene.md`.

If doing a partial review, load only the relevant reference files.

## Core Instructions

- Target iOS 17+ and Swift 6 or later, using modern Swift concurrency.
- Avoid UIKit unless explicitly requested.
- Do not introduce third-party frameworks without asking first.
- Break different types into separate Swift files — no multiple structs/classes/enums in one file.
- Use a consistent project structure with folder layout by app feature.

## Output Format

Organize findings by file. For each issue:

1. State the file and relevant line(s).
2. Name the rule being violated.
3. Show a brief before/after code fix.

Skip files with no issues. End with a prioritized summary of the most impactful changes.

### Example Output

#### ContentView.swift

**Line 12: Use `foregroundStyle()` instead of `foregroundColor()`.**

```swift
// Before
Text("Hello").foregroundColor(.red)

// After
Text("Hello").foregroundStyle(.red)
```

**Line 24: Icon-only button is inaccessible — add a label.**

```swift
// Before
Button(action: addItem) { Image(systemName: "plus") }

// After
Button("Add Item", systemImage: "plus", action: addItem)
```

#### Summary

1. **Accessibility (high):** Add label to button on line 24.
2. **Deprecated API (medium):** `foregroundColor()` → `foregroundStyle()` on line 12.

## References

- `references/api.md` — updating code to modern API, and the deprecated code it replaces
- `references/views.md` — view structure, composition, and animation
- `references/data.md` — data flow, shared state, and property wrappers
- `references/navigation.md` — NavigationStack/NavigationSplitView, alerts, sheets
- `references/design.md` — Apple Human Interface Guidelines
- `references/accessibility.md` — Dynamic Type, VoiceOver, Reduce Motion
- `references/performance.md` — optimizing SwiftUI for maximum performance
- `references/swift.md` — modern Swift code and Swift Concurrency
- `references/hygiene.md` — making code compile cleanly and stay maintainable
