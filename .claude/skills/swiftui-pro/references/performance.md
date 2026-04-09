# Performance

## Minimize View Re-renders

```swift
// Bad: whole list re-renders when any item changes
struct ListView: View {
    @State private var items: [Item]
    var body: some View {
        List(items) { item in
            HeavyRowView(item: item)  // re-init on every change
        }
    }
}

// Better: extract row so only changed rows re-render
struct ItemRow: View {
    let item: Item  // stable identity
    var body: some View { ... }
}
```

## Lazy Containers

- Use `LazyVStack` / `LazyHStack` for long lists not wrapped in `List`
- `List` is already lazy — don't wrap in another lazy container
- Use `LazyVGrid` for grid layouts with many items

## Images

```swift
// Resize before display to avoid large texture uploads
AsyncImage(url: url) { image in
    image
        .resizable()
        .scaledToFill()
        .frame(width: 60, height: 60)
        .clipped()
} placeholder: {
    ProgressView()
}
```

## Avoid

- `AnyView` — breaks view identity, prevents optimizations; use generics or `@ViewBuilder`
- Heavy work in `body` — move to ViewModel or `.task`
- Frequent `@State` updates from high-frequency sources (timers, sensors) — throttle them
- `GeometryReader` wrapping large subtrees — prefer `containerRelativeFrame()`
