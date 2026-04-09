# Design & Human Interface Guidelines

## Spacing & Layout

- Use semantic spacing: `.padding()` without arguments uses the system default (16pt)
- Minimum interactive tap target: 44×44 pt (use `.contentShape(Rectangle())` to expand hit area)
- Use `Spacer()` sparingly — prefer alignment and padding

## Typography

```swift
// Always use system text styles for automatic scaling
Text("Title").font(.title)
Text("Body").font(.body)
Text("Caption").font(.caption)

// SF Symbols align with text baseline automatically
Label("Favorites", systemImage: "heart")
```

## Color & Dark Mode

```swift
// Use semantic colors — they adapt to light/dark automatically
Color.primary
Color.secondary
Color.background       // custom: define in Assets.xcassets

// Avoid hard-coded colors for text/backgrounds
Text("Hello").foregroundStyle(.primary)  // not .foregroundStyle(.black)
```

## Controls

```swift
// Buttons: always label icon-only buttons
Button("Delete", role: .destructive) { delete() }
Button("Add Item", systemImage: "plus") { add() }

// Use role: .destructive for irreversible actions
// Use role: .cancel for cancel actions in confirmations
```

## Sheets & Dialogs

```swift
// Confirmation dialog for destructive actions
.confirmationDialog("Delete this item?", isPresented: $showConfirm, titleVisibility: .visible) {
    Button("Delete", role: .destructive) { delete() }
    Button("Cancel", role: .cancel) {}
}

// Alert for errors
.alert("Something went wrong", isPresented: $showError) {
    Button("OK", role: .cancel) {}
} message: {
    Text(errorMessage)
}
```
