# Views & Composition

## Extraction Rules

- Extract a subview when `body` exceeds ~80–100 lines
- Extract when a subtree has its own distinct state or purpose
- Prefer `@ViewBuilder` functions over stored subview properties for conditional content

```swift
// Good: named, reusable pieces
struct ProfileHeader: View {
    let user: User
    var body: some View { ... }
}

// @ViewBuilder for conditional pieces
@ViewBuilder
private var loadingOverlay: some View {
    if isLoading {
        ProgressView()
            .frame(maxWidth: .infinity, maxHeight: .infinity)
            .background(.ultraThinMaterial)
    }
}
```

## Modifiers Order (Convention)

1. Layout (`.frame`, `.padding`, `.offset`)
2. Style (`.background`, `.foregroundStyle`, `.font`)
3. Behavior (`.onTapGesture`, `.onAppear`, `.task`)
4. Accessibility (`.accessibilityLabel`, `.accessibilityHidden`)

## Animations

```swift
// Explicit animation on state change
Button("Toggle") {
    withAnimation(.spring(duration: 0.3)) {
        isExpanded.toggle()
    }
}

// Animation modifier (reacts to any change in value)
contentView
    .animation(.easeInOut, value: opacity)

// Respect Reduce Motion (see accessibility.md)
```

## Common Mistakes

- Using `EquatableView` / `.equatable()` without measuring — it adds overhead
- Putting `@State` in a subview that should be owned by a ViewModel
- Using `id(_:)` to force re-creation when you should just update state
