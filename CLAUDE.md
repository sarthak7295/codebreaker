# Codebreaker — iOS App

## Quick Reference
- **Platform**: iOS 17+
- **Language**: Swift 6
- **UI Framework**: SwiftUI (no UIKit for new code)
- **Architecture**: MVVM with `@Observable`
- **Package Manager**: Swift Package Manager
- **Minimum Deployment**: iOS 17.0

---

## Build & Test Commands

```bash
# Build for simulator
xcodebuild -scheme Codebreaker \
  -destination 'platform=iOS Simulator,name=iPhone 16,OS=latest' \
  build

# Run tests
xcodebuild -scheme Codebreaker \
  -destination 'platform=iOS Simulator,name=iPhone 16,OS=latest' \
  test

# Clean
xcodebuild -scheme Codebreaker clean
```

> If XcodeBuildMCP is configured, prefer MCP tools over raw xcodebuild.

---

## Project Structure

```
Codebreaker/
├── App/                    # Entry point (@main), App struct
├── Features/               # One folder per feature
│   └── [Feature]/
│       ├── Views/
│       ├── ViewModels/
│       └── Models/
├── Core/                   # Shared utilities, services, extensions
│   ├── Extensions/
│   ├── Services/
│   └── Networking/
├── Resources/              # Assets.xcassets, Localizable.strings
└── Tests/
```

---

## Swift & SwiftUI Conventions

### Swift Style
- Swift 6 strict concurrency — treat all warnings as errors
- Prefer `@Observable` over `ObservableObject` (deprecated)
- Use `async/await` for all async work; avoid Combine for new code
- Prefer value types (`struct`) over reference types (`class`)
- Use `guard` for early exits; never force-unwrap (`!`) without comment justification
- Use typed errors conforming to `LocalizedError`
- One type per file — no multiple structs/classes/enums in a single `.swift` file

### SwiftUI Style
- Extract views when `body` exceeds ~80–100 lines
- `@State` for local view state only
- `@Environment` for dependency injection
- `@Bindable` for bindings to `@Observable` objects
- Use `NavigationStack` (not `NavigationView`)
- Use `.task` modifier for lifecycle-triggered async work
- Use `AsyncImage` for async image loading

### Deprecated APIs — Do Not Use
| Deprecated | Use Instead |
|---|---|
| `foregroundColor()` | `foregroundStyle()` |
| `cornerRadius()` | `clipShape(.rect(cornerRadius:))` |
| `ObservableObject` | `@Observable` macro |
| `NavigationView` | `NavigationStack` |
| `.navigationBarLeading/Trailing` | `.topBarLeading/Trailing` |
| `onChange(of:)` 1-param | 2-param `onChange` |
| `UIImpactFeedbackGenerator` | `.sensoryFeedback()` |
| Manual `EnvironmentKey` | `@Entry` macro |

### Navigation Pattern
Use `NavigationStack` with type-safe `Route` enums:

```swift
enum Route: Hashable {
    case detail(Item)
    case settings
}

NavigationStack(path: $router.path) {
    RootView()
        .navigationDestination(for: Route.self) { route in ... }
}
```

For modals: parent ViewModels own an optional child ViewModel. Sheet presents when non-nil; dismissal sets it to nil.

---

## Data & Persistence
- SwiftData for local persistence (not Core Data)
- Keychain for credentials and sensitive values
- `UserDefaults` only for non-sensitive preferences

---

## Testing
- Use **Swift Testing** framework (`@Test`, `@Suite`, `#expect()`) — not XCTest
- Unit tests for all ViewModels
- UI tests for critical flows only
- Target 80%+ coverage on business logic

---

## Hard Rules

- **Never modify `.pbxproj` directly** — create files with Claude Code, then add to Xcode manually
- No UIKit for new features
- No storyboards or `.xib` files
- No third-party frameworks without asking first
- No force-unwrap without an explanatory comment
- No monolithic views — extract at 100 lines
- No ignoring Swift 6 concurrency warnings
