# Android App Development Rules

## Architecture (CRITICAL)

ALWAYS use MVVM + Clean Architecture:
- ViewModel NEVER holds Activity/Fragment references
- Use LiveData/StateFlow for UI state
- Repository pattern for data layer
- UseCase for business logic

```kotlin
// WRONG: ViewModel holding Activity reference
class UserViewModel(private val activity: Activity) {
    fun loadUser() {
        // MUTATION of Activity!
    }
}

// CORRECT: ViewModel with Repository
class UserViewModel(
    private val userRepository: UserRepository
) : ViewModel() {
    private val _userState = MutableStateFlow<UserState>(UserState.Loading)
    val userState: StateFlow<UserState> = _userState.asStateFlow()
    
    fun loadUser() {
        viewModelScope.launch {
            _userState.value = UserState.Loading
            try {
                val user = userRepository.getUser()
                _userState.value = UserState.Success(user)
            } catch (e: Exception) {
                _userState.value = UserState.Error(e.message)
            }
        }
    }
}
```

## Memory Leak Prevention (MANDATORY)

NEVER create memory leaks:

```kotlin
// WRONG: Anonymous inner class holding Activity reference
button.setOnClickListener(object : View.OnClickListener {
    override fun onClick(v: View?) {
        processData(data)  // Holds Activity reference!
    }
})

// CORRECT: Lambda (Kotlin compiler optimizes)
button.setOnClickListener { view ->
    processData(data)
}

// CORRECT: Static inner class with WeakReference
class MainActivity : AppCompatActivity() {
    private val clickListener = ClickListener(this)
    
    private class ClickListener(activity: MainActivity) : View.OnClickListener {
        private val activityRef = WeakReference(activity)
        
        override fun onClick(v: View?) {
            activityRef.get()?.processData()
        }
    }
}
```

## Lifecycle Awareness (CRITICAL)

ALWAYS use lifecycle-aware components:

```kotlin
// WRONG: Manual lifecycle management
override fun onResume() {
    super.onResume()
    startLocationUpdates()
}

override fun onPause() {
    super.onPause()
    stopLocationUpdates()  // Easy to forget!
}

// CORRECT: Lifecycle-aware observer
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        locationFlow.collect { location ->
            updateUI(location)
        }
    }
}
```

## Dependency Injection (MANDATORY)

ALWAYS use Hilt for DI:

```kotlin
// WRONG: Manual dependency creation
class UserRepository {
    private val api = RetrofitClient.create()
    private val db = AppDatabase.getInstance()
}

// CORRECT: Hilt injection
@HiltViewModel
class UserViewModel @Inject constructor(
    private val userRepository: UserRepository
) : ViewModel()

@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    @Provides
    @Singleton
    fun provideUserRepository(
        api: UserApi,
        db: AppDatabase
    ): UserRepository = UserRepositoryImpl(api, db)
}
```

## Async Operations (CRITICAL)

ALWAYS use Coroutines, NEVER use Callbacks:

```kotlin
// WRONG: Callback hell
fun loadUser(userId: String, callback: (User?) -> Unit) {
    api.getUser(userId) { user ->
        db.saveUser(user) { saved ->
            callback(saved)
        }
    }
}

// CORRECT: Coroutines
suspend fun loadUser(userId: String): User {
    val user = api.getUser(userId)
    return db.saveUser(user)
}
```

## Resource Management (MANDATORY)

ALWAYS clean up resources:

```kotlin
// CORRECT: Resource cleanup
class MainActivity : AppCompatActivity() {
    private val handler = MyHandler(this)
    
    override fun onDestroy() {
        super.onDestroy()
        handler.removeCallbacksAndMessages(null)
    }
}
```

## Code Quality Checklist

Before marking work complete:
- [ ] No memory leaks (no Activity/Fragment references in ViewModel)
- [ ] All async operations use Coroutines
- [ ] Lifecycle-aware components used
- [ ] Hilt dependency injection implemented
- [ ] Resources cleaned up in onDestroy/onCleared
- [ ] No hardcoded strings (use string resources)
- [ ] Proper error handling with sealed classes
- [ ] ViewModel tested with unit tests
