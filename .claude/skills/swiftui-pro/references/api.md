# Deprecated API Reference

Replace these deprecated APIs with their modern equivalents.

| Deprecated | Modern Replacement |
|---|---|
| `foregroundColor()` | `foregroundStyle()` |
| `cornerRadius()` | `clipShape(.rect(cornerRadius:))` |
| `ObservableObject` + `@Published` | `@Observable` macro |
| `@StateObject` | `@State` (with `@Observable`) |
| `@ObservedObject` | direct property or `@Bindable` |
| `NavigationView` | `NavigationStack` or `NavigationSplitView` |
| `.navigationBarLeading/Trailing` | `.topBarLeading/Trailing` |
| `onChange(of:)` (1-param closure) | `onChange(of:) { old, new in }` or `onChange(of:) {}` |
| `UIImpactFeedbackGenerator` | `.sensoryFeedback()` modifier |
| Manual `EnvironmentKey` boilerplate | `@Entry` macro |
| `GeometryReader` for sizing | `containerRelativeFrame()` or `visualEffect()` |
| `showsIndicators:` on `ScrollView` | `.scrollIndicators(.hidden)` |
| `Text + Text` concatenation | String interpolation in a single `Text` |
| `AnyView` type erasure | Generics or `@ViewBuilder` |
| `preferredColorScheme()` on view | `.environment(\.colorScheme, .dark)` in previews |
| `UIApplication.shared.open()` | `@Environment(\.openURL)` |
| `.accentColor()` | `.tint()` |
