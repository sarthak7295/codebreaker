# Navigation

## NavigationStack (type-safe)

```swift
enum Route: Hashable {
    case detail(Item)
    case settings
    case profile(userID: String)
}

@Observable
class Router {
    var path = NavigationPath()
    func push(_ route: Route) { path.append(route) }
    func pop() { path.removeLast() }
    func popToRoot() { path.removeLast(path.count) }
}

struct AppView: View {
    @State private var router = Router()

    var body: some View {
        NavigationStack(path: $router.path) {
            HomeView()
                .navigationDestination(for: Route.self) { route in
                    switch route {
                    case .detail(let item): DetailView(item: item)
                    case .settings:         SettingsView()
                    case .profile(let id):  ProfileView(userID: id)
                    }
                }
        }
        .environment(router)
    }
}
```

## Sheets & Modals

Prefer ViewModel-driven presentation — parent owns an optional child ViewModel:

```swift
@Observable
class HomeViewModel {
    var detailViewModel: DetailViewModel?

    func showDetail(for item: Item) {
        detailViewModel = DetailViewModel(item: item)
    }
}

struct HomeView: View {
    @State private var vm = HomeViewModel()

    var body: some View {
        List { ... }
            .sheet(item: $vm.detailViewModel) { childVM in
                DetailView(vm: childVM)
            }
    }
}
```

## Rules

- Use `NavigationStack` — never `NavigationView`
- Use `.topBarLeading` / `.topBarTrailing` — not `.navigationBarLeading/Trailing`
- Avoid deep `navigationDestination` nesting — consolidate at the stack root
- For split-view iPad layouts, use `NavigationSplitView`
