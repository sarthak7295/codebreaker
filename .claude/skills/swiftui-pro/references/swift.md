# Modern Swift

## Concurrency

```swift
// Prefer async/await over Combine or callbacks
func loadUser(id: String) async throws -> User {
    let (data, _) = try await URLSession.shared.data(from: url)
    return try JSONDecoder().decode(User.self, from: data)
}

// .task for view lifecycle
struct ProfileView: View {
    @State private var user: User?

    var body: some View {
        content
            .task {
                user = try? await loadUser(id: "123")
            }
    }
}

// actors for shared mutable state
actor UserCache {
    private var cache: [String: User] = [:]
    func user(for id: String) -> User? { cache[id] }
    func store(_ user: User) { cache[user.id] = user }
}
```

## Error Handling

```swift
// Typed errors
enum NetworkError: LocalizedError {
    case notFound
    case unauthorized

    var errorDescription: String? {
        switch self {
        case .notFound:     return "Resource not found."
        case .unauthorized: return "Please sign in again."
        }
    }
}
```

## Value Types & Immutability

- Prefer `struct` over `class`; use `class` only for shared mutable identity or `@Observable`
- Mark properties `let` unless mutation is required
- Avoid `mutating` functions on large structs (copy overhead) — consider actor or class

## Patterns to Avoid

- Force-unwrap (`!`) without a justification comment
- `try!` — use `try?` with a fallback or `try` with error propagation
- `DispatchQueue.main.async` — use `@MainActor` or `await MainActor.run`
- `NotificationCenter` for new code — use `@Observable` or `async` streams
