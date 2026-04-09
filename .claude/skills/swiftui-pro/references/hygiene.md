# Code Hygiene

## Compilation

- Zero warnings policy — treat all Swift 6 warnings as errors
- No unused variables, imports, or `@State` properties
- Run `swift build` or `xcodebuild build` and confirm clean before marking done

## File Organization

- One type per file
- File name matches the primary type name (`UserViewModel.swift`)
- Group by feature, not by type (`Features/Profile/` not `ViewModels/`)
- Delete dead code — don't comment it out

## Naming

- Types: `UpperCamelCase`
- Functions, properties, variables: `lowerCamelCase`
- Boolean properties: `is`, `has`, `should` prefix (`isLoading`, `hasError`)
- Async functions: no `async` suffix needed — the call site context is enough
- Avoid abbreviations: `userID` not `uid`, `backgroundColor` not `bgColor`

## Comments

- Don't comment what the code says — comment *why* something non-obvious was done
- Mark intentional force-unwraps: `// Safe: guaranteed non-nil by X`
- Use `// MARK: -` to separate logical sections in longer files

## SwiftLint

If SwiftLint is configured, ensure no new violations are introduced:

```bash
swiftlint lint --quiet
```
