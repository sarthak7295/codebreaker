# Accessibility

## VoiceOver

```swift
// Icon-only buttons MUST have a label
Button("Add Item", systemImage: "plus") { addItem() }

// Custom accessibility label when SF Symbol name is read aloud
Image(systemName: "heart.fill")
    .accessibilityLabel("Favorite")

// Group related elements
VStack {
    Text(title)
    Text(subtitle)
}
.accessibilityElement(children: .combine)

// Hide decorative images
Image("background")
    .accessibilityHidden(true)
```

## Dynamic Type

```swift
// Use built-in text styles — they scale automatically
Text("Heading").font(.headline)
Text("Body").font(.body)

// Custom fonts must also scale
Text("Custom")
    .font(.custom("Georgia", size: 17, relativeTo: .body))

// Test: Settings → Accessibility → Display & Text Size → Larger Text
```

## Reduce Motion

```swift
@Environment(\.accessibilityReduceMotion) private var reduceMotion

var body: some View {
    content
        .animation(reduceMotion ? .none : .spring(), value: isExpanded)
}
```

## Checklist

- [ ] All interactive elements have an accessibility label
- [ ] All text uses Dynamic Type-compatible font styles
- [ ] Animations respect `accessibilityReduceMotion`
- [ ] Minimum tap target: 44×44 pt
- [ ] Color alone is not the only way to convey information
- [ ] Tested with VoiceOver on device
