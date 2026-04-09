# Data Flow & Property Wrappers

## Swift 6 Observation (@Observable)

```swift
// Correct
@Observable
class CounterViewModel {
    var count = 0
    func increment() { count += 1 }
}

struct CounterView: View {
    @State private var vm = CounterViewModel()  // owns it
    var body: some View {
        Button("\(vm.count)") { vm.increment() }
    }
}

// Passing down + binding
struct ChildView: View {
    @Bindable var vm: CounterViewModel  // binds to @Observable
    var body: some View {
        TextField("Count", value: $vm.count, format: .number)
    }
}
```

## Choosing the Right Wrapper

| Scenario | Wrapper |
|---|---|
| Local view state (value type) | `@State` |
| Local ViewModel (reference type, `@Observable`) | `@State` |
| Passed-in `@Observable` needing bindings | `@Bindable` |
| Read-only access to passed-in data | plain `let` |
| App-wide shared state | `@Environment` |
| SwiftData query | `@Query` |

## Avoid These Patterns

- `Binding(get:set:)` in view body — use `@State` + `onChange` instead
- Publishing side effects in `@Published` setters — use `.task` or explicit methods
- Reaching up to parent state — pass a binding or callback down instead
- Storing `@Bindable` in a parent when a `let` suffices

## Environment

```swift
// Define (Swift 6 @Entry macro)
extension EnvironmentValues {
    @Entry var theme: Theme = .default
}

// Inject
ContentView()
    .environment(\.theme, .dark)

// Consume
@Environment(\.theme) private var theme
```
