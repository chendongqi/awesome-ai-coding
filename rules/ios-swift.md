# iOS/Swift Development Rules

## Architecture (CRITICAL)

ALWAYS use MVVM or Clean Architecture:

```swift
// CORRECT: MVVM pattern
class UserViewModel: ObservableObject {
    @Published var user: User?
    @Published var isLoading = false
    @Published var error: Error?
    
    private let userService: UserServiceProtocol
    
    init(userService: UserServiceProtocol) {
        self.userService = userService
    }
    
    func loadUser(id: String) {
        isLoading = true
        Task {
            do {
                let user = try await userService.getUser(id: id)
                await MainActor.run {
                    self.user = user
                    self.isLoading = false
                }
            } catch {
                await MainActor.run {
                    self.error = error
                    self.isLoading = false
                }
            }
        }
    }
}
```

## Memory Management (MANDATORY)

ALWAYS avoid retain cycles:

```swift
// WRONG: Retain cycle
class ViewController: UIViewController {
    var closure: (() -> Void)?
    
    func setup() {
        closure = {
            self.doSomething()  // Retain cycle!
        }
    }
}

// CORRECT: Weak self
class ViewController: UIViewController {
    var closure: (() -> Void)?
    
    func setup() {
        closure = { [weak self] in
            guard let self = self else { return }
            self.doSomething()
        }
    }
}

// CORRECT: Unowned self (when self always exists)
closure = { [unowned self] in
    self.doSomething()
}
```

## Optionals (CRITICAL)

ALWAYS handle optionals safely:

```swift
// WRONG: Force unwrapping
let value = optionalValue!  // Crashes if nil!

// CORRECT: Optional binding
if let value = optionalValue {
    print(value)
}

// CORRECT: Guard statement
guard let value = optionalValue else {
    return
}
print(value)

// CORRECT: Nil coalescing
let value = optionalValue ?? defaultValue
```

## Async/Await (MANDATORY)

ALWAYS use async/await for async operations:

```swift
// WRONG: Completion handlers
func fetchData(completion: @escaping (Result<Data, Error>) -> Void) {
    URLSession.shared.dataTask(with: url) { data, response, error in
        // Callback hell
        completion(.success(data!))
    }.resume()
}

// CORRECT: Async/await
func fetchData() async throws -> Data {
    let (data, _) = try await URLSession.shared.data(from: url)
    return data
}

// CORRECT: Task usage
Task {
    do {
        let data = try await fetchData()
        await updateUI(with: data)
    } catch {
        await showError(error)
    }
}
```

## Protocol-Oriented Programming (CRITICAL)

PREFER protocols over classes:

```swift
// WRONG: Class inheritance
class Animal {
    func makeSound() {}
}

class Dog: Animal {
    override func makeSound() {
        print("Woof")
    }
}

// CORRECT: Protocol-oriented
protocol Animal {
    func makeSound()
}

struct Dog: Animal {
    func makeSound() {
        print("Woof")
    }
}

// CORRECT: Protocol extensions
extension Animal {
    func sleep() {
        print("Sleeping")
    }
}
```

## SwiftUI (MANDATORY)

ALWAYS use SwiftUI best practices:

```swift
// CORRECT: SwiftUI view
struct UserView: View {
    @StateObject private var viewModel = UserViewModel()
    
    var body: some View {
        VStack {
            if viewModel.isLoading {
                ProgressView()
            } else if let user = viewModel.user {
                Text(user.name)
            } else if let error = viewModel.error {
                Text("Error: \(error.localizedDescription)")
            }
        }
        .task {
            await viewModel.loadUser(id: "123")
        }
    }
}
```

## Code Quality Checklist

Before marking work complete:
- [ ] MVVM or Clean Architecture used
- [ ] No retain cycles (weak/unowned self)
- [ ] Optionals handled safely (no force unwrap)
- [ ] Async/await used (no completion handlers)
- [ ] Protocol-oriented programming preferred
- [ ] SwiftUI views properly structured
- [ ] Error handling with do-catch
- [ ] No force casts (as!)
- [ ] Access control modifiers used
- [ ] SwiftLint warnings resolved
