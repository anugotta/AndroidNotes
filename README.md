# Android Developer Interview Guide

## Table of Contents
- [Core Android Concepts](#core-android-concepts)
- [Kotlin Fundamentals](#kotlin-fundamentals)
- [Modern Android Development](#modern-android-development)
- [Architecture Components](#architecture-components)
- [Background Processing](#background-processing)
- [Data Management](#data-management)
- [Performance & Optimization](#performance--optimization)
- [Testing](#testing)
- [Build & Deployment](#build--deployment)
- [Data Structures & Algorithms](#data-structures--algorithms)

## Core Android Concepts

### Activities & Fragments

#### Activity Lifecycle
- `onCreate()`: Activity is first created
  - Initialize UI
  - Set content view
  - Initialize variables
  
- `onStart()`: Activity becomes visible
  - Prepare UI elements
  - Register broadcast receivers
  
- `onResume()`: Activity starts interacting with user
  - Start animations/video playback
  - Initialize foreground services
  
- `onPause()`: Activity partially visible but not focused
  - Pause ongoing operations
  - Save draft data
  
- `onStop()`: Activity no longer visible
  - Save persistent data
  - Release resources
  
- `onDestroy()`: Activity being destroyed
  - Cleanup resources
  - Unregister receivers

Common Flows:
```plaintext
Normal Flow:
onCreate → onStart → onResume → onPause → onStop → onDestroy

Background Flow:
onStop → onRestart → onStart

Configuration Change:
onPause → onSaveInstanceState → onStop → onDestroy → 
onCreate → onStart → onRestoreInstanceState → onResume
```

#### Fragment Lifecycle
```plaintext
Complete Flow:
onAttach → onCreate → onCreateView → onViewCreated → 
onActivityCreated → onStart → onResume → onPause → 
onStop → onDestroyView → onDestroy → onDetach
```

Key Methods:
- `onAttach()`: Fragment attached to activity
- `onCreateView()`: Create and return view hierarchy
- `onViewCreated()`: View setup after creation
- `onDestroyView()`: View hierarchy being destroyed

### Services

#### Types of Services

1. **Foreground Service**
```kotlin
class MusicService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val notification = createNotification()
        startForeground(NOTIFICATION_ID, notification)
        return START_STICKY
    }
}
```

2. **Background Service**
```kotlin
class DataSyncService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // Perform background operation
        return START_NOT_STICKY
    }
}
```

3. **Bound Service**
```kotlin
class LocalService : Service() {
    private val binder = LocalBinder()
    
    inner class LocalBinder : Binder() {
        fun getService(): LocalService = this@LocalService
    }
    
    override fun onBind(intent: Intent): IBinder = binder
}
```

4. **IntentService** (Deprecated, use WorkManager instead)
```kotlin
class DataProcessingService : IntentService("DataProcessingService") {
    override fun onHandleIntent(intent: Intent?) {
        // Process data in background
    }
}
```

### Intents & Communication

#### Types of Intents

1. **Explicit Intents**
```kotlin
// Start specific activity
val intent = Intent(context, TargetActivity::class.java)
intent.putExtra("key", "value")
startActivity(intent)
```

2. **Implicit Intents**
```kotlin
// Open URL
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)

// Share text
val shareIntent = Intent().apply {
    action = Intent.ACTION_SEND
    type = "text/plain"
    putExtra(Intent.EXTRA_TEXT, "Share this")
}
startActivity(Intent.createChooser(shareIntent, "Share via"))
```

#### Broadcast Receivers

1. **Static Receiver** (Manifest-declared)
```xml
<receiver
    android:name=".MyReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
    </intent-filter>
</receiver>
```

2. **Dynamic Receiver** (Runtime registration)
```kotlin
private val receiver = object : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // Handle broadcast
    }
}

override fun onResume() {
    super.onResume()
    registerReceiver(receiver, IntentFilter("ACTION_NAME"))
}

override fun onPause() {
    super.onPause()
    unregisterReceiver(receiver)
}
```

## Kotlin Fundamentals

### Key Features

#### Null Safety
```kotlin
// Nullable types
var nullableString: String? = null

// Safe call operator
nullableString?.length

// Elvis operator
val length = nullableString?.length ?: 0

// Not-null assertion
val definiteLength = nullableString!!.length // Throws if null
```

#### Properties
```kotlin
// Lazy initialization
val expensive: String by lazy {
    // Computed only on first access
    computeExpensiveString()
}

// Late initialization
lateinit var lateinitVar: String
// Initialize later
lateinitVar = "Now initialized"

// Custom getters/setters
var counter = 0
    get() = field
    set(value) {
        if (value >= 0) field = value
    }
```

## Modern Android Development

### Jetpack Compose

#### Basic Composables
```kotlin
@Composable
fun Greeting(name: String) {
    Column {
        Text(text = "Hello $name!")
        Button(onClick = { /* action */ }) {
            Text("Click me")
        }
    }
}
```

#### State Management
```kotlin
// Local state
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }
    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}

// Hoisted state
@Composable
fun StatefulCounter(
    count: Int,
    onCountChange: (Int) -> Unit
) {
    Button(onClick = { onCountChange(count + 1) }) {
        Text("Count: $count")
    }
}
```

#### Side Effects
```kotlin
@Composable
fun MyScreen() {
    // Run on first composition
    LaunchedEffect(Unit) {
        // Launch coroutine
    }

    // Run on every successful composition
    SideEffect {
        // Update non-compose code
    }

    // Cleanup when leaving composition
    DisposableEffect(Unit) {
        onDispose {
            // Cleanup
        }
    }
}
```

### Navigation Component

#### Navigation Graph
```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/nav_graph"
    app:startDestination="@id/homeFragment">

    <fragment
        android:id="@+id/homeFragment"
        android:name=".HomeFragment">
        <action
            android:id="@+id/to_detail"
            app:destination="@id/detailFragment">
            <argument
                android:name="itemId"
                app:argType="string" />
        </action>
    </fragment>

    <fragment
        android:id="@+id/detailFragment"
        android:name=".DetailFragment" />
</navigation>
```

#### Navigation in Code
```kotlin
// Using Safe Args
class HomeFragment : Fragment() {
    private val navArgs: HomeFragmentArgs by navArgs()
    
    fun navigate() {
        val action = HomeFragmentDirections.toDetail(itemId = "123")
        findNavController().navigate(action)
    }
}
```

## Architecture Components

### MVVM Pattern
```kotlin
// Model
data class User(val id: String, val name: String)

// ViewModel
class UserViewModel : ViewModel() {
    private val _user = MutableLiveData<User>()
    val user: LiveData<User> = _user

    fun loadUser(id: String) {
        viewModelScope.launch {
            _user.value = repository.getUser(id)
        }
    }
}

// View
class UserFragment : Fragment() {
    private val viewModel: UserViewModel by viewModels()
    
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        viewModel.user.observe(viewLifecycleOwner) { user ->
            // Update UI
        }
    }
}
```

### State Management

#### StateFlow
```kotlin
class UserViewModel : ViewModel() {
    private val _uiState = MutableStateFlow<UiState>(UiState.Initial)
    val uiState: StateFlow<UiState> = _uiState.asStateFlow()

    fun loadData() {
        viewModelScope.launch {
            _uiState.value = UiState.Loading
            try {
                val result = repository.getData()
                _uiState.value = UiState.Success(result)
            } catch (e: Exception) {
                _uiState.value = UiState.Error(e.message)
            }
        }
    }
}

// States
sealed class UiState {
    object Initial : UiState()
    object Loading : UiState()
    data class Success(val data: Data) : UiState()
    data class Error(val message: String?) : UiState()
}
```

### Room Database
```kotlin
// Entity
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val email: String
)

// DAO
@Dao
interface UserDao {
    @Query("SELECT * FROM users")
    fun getAll(): Flow<List<UserEntity>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)

    @Delete
    suspend fun delete(user: UserEntity)
}

// Database
@Database(entities = [UserEntity::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

### Dependency Injection with Hilt
```kotlin
// Application class
@HiltAndroidApp
class MyApplication : Application()

// Module
@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    @Provides
    @Singleton
    fun provideDatabase(@ApplicationContext context: Context): AppDatabase {
        return Room.databaseBuilder(
            context,
            AppDatabase::class.java,
            "app_database"
        ).build()
    }
}

// ViewModel injection
@HiltViewModel
class MainViewModel @Inject constructor(
    private val repository: Repository
) : ViewModel()

// Activity injection
@AndroidEntryPoint
class MainActivity : AppCompatActivity()
```

## Background Processing

### Coroutines
```kotlin
class MainViewModel : ViewModel() {
    // Different Coroutine Scopes
    init {
        // ViewModel Scope
        viewModelScope.launch {
            // Runs on Main dispatcher by default
        }

        // Background work with IO dispatcher
        viewModelScope.launch(Dispatchers.IO) {
            // Network or database operations
        }

        // CPU-intensive work
        viewModelScope.launch(Dispatchers.Default) {
            // Complex calculations
        }
    }

    // Parallel Execution
    suspend fun loadDataInParallel() {
        coroutineScope {
            val result1 = async { api.getData1() }
            val result2 = async { api.getData2() }
            val combinedResult = result1.await() + result2.await()
        }
    }
}
```

### WorkManager
```kotlin
// Define Work
class DataSyncWorker(
    context: Context,
    params: WorkerParameters
) : CoroutineWorker(context, params) {
    
    override suspend fun doWork(): Result {
        return try {
            // Do background work
            Result.success()
        } catch (e: Exception) {
            Result.retry()
        }
    }
}

// Schedule Work
class Repository {
    fun scheduleSync() {
        val constraints = Constraints.Builder()
            .setRequiredNetworkType(NetworkType.CONNECTED)
            .setRequiresBatteryNotLow(true)
            .build()

        val syncWork = OneTimeWorkRequestBuilder<DataSyncWorker>()
            .setConstraints(constraints)
            .setBackoffCriteria(BackoffPolicy.LINEAR, 10, TimeUnit.MINUTES)
            .build()

        WorkManager.getInstance(context)
            .enqueueUniqueWork(
                "sync_work",
                ExistingWorkPolicy.REPLACE,
                syncWork
            )
    }
}
```

### RxJava
```kotlin
// Basic Observable
Observable.just(1, 2, 3, 4, 5)
    .map { it * 2 }
    .filter { it > 5 }
    .subscribe { println(it) }

// Network Call with RxJava
interface ApiService {
    @GET("users")
    fun getUsers(): Single<List<User>>
}

class Repository {
    fun getUsers(): Single<List<User>> {
        return apiService.getUsers()
            .subscribeOn(Schedulers.io())
            .observeOn(AndroidSchedulers.mainThread())
    }
}
```

## Data Management

### Secure Data Storage
```kotlin
// Encrypted Shared Preferences
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()

val sharedPreferences = EncryptedSharedPreferences.create(
    context,
    "secret_prefs",
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)

// Encrypted Files
val encryptedFile = EncryptedFile.Builder(
    context,
    File(context.filesDir, "secret.txt"),
    masterKey,
    EncryptedFile.FileEncryptionScheme.AES256_GCM_HKDF_4KB
).build()
```

### Network Security
```kotlin
// Certificate Pinning with OkHttp
val certificatePinner = CertificatePinner.Builder()
    .add("example.com", "sha256/XXXX=")
    .build()

val okHttpClient = OkHttpClient.Builder()
    .certificatePinner(certificatePinner)
    .build()

// Retrofit with Security
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(GsonConverterFactory.create())
    .build()
```

## Testing

### Unit Testing
```kotlin
@Test
fun `test user validation`() {
    val validator = UserValidator()
    
    assertTrue(validator.isValidEmail("test@example.com"))
    assertFalse(validator.isValidEmail("invalid-email"))
}

// ViewModel Testing
@Test
fun `test loading state`() = runTest {
    val repository = mockk<Repository>()
    coEvery { repository.getData() } returns Result.success(data)
    
    val viewModel = MainViewModel(repository)
    viewModel.loadData()
    
    assertEquals(UiState.Success(data), viewModel.uiState.value)
}
```

### UI Testing with Espresso
```kotlin
@Test
fun testLoginFlow() {
    // Launch activity
    ActivityScenario.launch(LoginActivity::class.java)

    // Type text
    onView(withId(R.id.emailInput))
        .perform(typeText("test@example.com"))

    // Click button
    onView(withId(R.id.loginButton))
        .perform(click())

    // Verify text
    onView(withId(R.id.statusText))
        .check(matches(withText("Success")))
}
```

### Compose UI Testing
```kotlin
@Test
fun testCounter() {
    composeTestRule.setContent {
        Counter()
    }

    composeTestRule
        .onNodeWithText("Count: 0")
        .assertExists()
        .performClick()

    composeTestRule
        .onNodeWithText("Count: 1")
        .assertExists()
}
```

## Performance Optimization

### Memory Management
```kotlin
class MainActivity : AppCompatActivity() {
    // Avoid memory leaks with weak references
    private val weakReference = WeakReference(this)
    
    // Proper bitmap handling
    private fun loadBitmap() {
        val options = BitmapFactory.Options().apply {
            inJustDecodeBounds = true
        }
        BitmapFactory.decodeResource(resources, R.drawable.image, options)
        
        options.apply {
            inSampleSize = calculateInSampleSize(options, reqWidth, reqHeight)
            inJustDecodeBounds = false
        }
    }
}

// Memory Cache
class ImageCache {
    private val memoryCache = object : LruCache<String, Bitmap>(
        (Runtime.getRuntime().maxMemory() / 1024).toInt() / 8
    ) {
        override fun sizeOf(key: String, bitmap: Bitmap): Int {
            return bitmap.byteCount / 1024
        }
    }
}
```

## Build & Deployment

### Gradle Configuration
```groovy
// App level build.gradle.kts
plugins {
    id("com.android.application")
    id("kotlin-android")
    id("kotlin-kapt")
    id("dagger.hilt.android.plugin")
}

android {
    defaultConfig {
        applicationId = "com.example.app"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0.0"
    }

    buildTypes {
        release {
            isMinifyEnabled = true
            proguardFiles(getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro")
        }
        
        debug {
            applicationIdSuffix = ".debug"
            isDebuggable = true
        }
    }

    buildFeatures {
        compose = true
        viewBinding = true
    }
}
```

### APK vs App Bundle
```kotlin
// App Bundle benefits:
// 1. Smaller download size
// 2. Dynamic feature delivery
// 3. Optimized for different devices

// Configure dynamic feature
android {
    dynamicFeatures = mutableSetOf(":feature_module")
}

// Install dynamic feature
class MainActivity : AppCompatActivity() {
    private fun installFeature() {
        val request = SplitInstallRequest.newBuilder()
            .addModule("feature_module")
            .build()

        splitInstallManager.startInstall(request)
            .addOnSuccessListener { /* Handle success */ }
            .addOnFailureListener { /* Handle failure */ }
    }
}
```

### CI/CD Pipeline
```yaml
# Example GitHub Actions workflow
name: Android CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up JDK
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        
    - name: Run Tests
      run: ./gradlew test
      
    - name: Build Debug APK
      run: ./gradlew assembleDebug
      
    - name: Upload APK
      uses: actions/upload-artifact@v2
      with:
        name: app-debug
        path: app/build/outputs/apk/debug/app-debug.apk
```

## Data Structures & Algorithms

### Common Interview Problems

#### 1. String Manipulation
```kotlin
// Reverse String
fun reverseString(str: String): String {
    return str.reversed()
    // Or manual implementation:
    return str.toCharArray()
        .apply { reverse() }
        .joinToString("")
}

// Check Palindrome
fun isPalindrome(str: String): Boolean {
    val cleanStr = str.lowercase().filter { it.isLetterOrDigit() }
    return cleanStr == cleanStr.reversed()
}
```

#### 2. Array Operations
```kotlin
// Find Missing Number
fun findMissingNumber(arr: IntArray): Int {
    val n = arr.size + 1
    val expectedSum = (n * (n + 1)) / 2
    val actualSum = arr.sum()
    return expectedSum - actualSum
}

// Maximum Subarray Sum
fun maxSubArraySum(arr: IntArray): Int {
    var maxSoFar = arr[0]
    var maxEndingHere = arr[0]
    
    for (i in 1 until arr.size) {
        maxEndingHere = maxOf(arr[i], maxEndingHere + arr[i])
        maxSoFar = maxOf(maxSoFar, maxEndingHere)
    }
    return maxSoFar
}
```

#### 3. Linked List Operations
```kotlin
data class ListNode(
    var value: Int,
    var next: ListNode? = null
)

// Reverse Linked List
fun reverseList(head: ListNode?): ListNode? {
    var prev: ListNode? = null
    var current = head
    
    while (current != null) {
        val next = current.next
        current.next = prev
        prev = current
        current = next
    }
    return prev
}

// Detect Cycle
fun hasCycle(head: ListNode?): Boolean {
    var slow = head
    var fast = head
    
    while (fast?.next != null) {
        slow = slow?.next
        fast = fast.next?.next
        if (slow == fast) return true
    }
    return false
}
```

### Android-Specific Optimizations

#### 1. View Hierarchy Optimization
```kotlin
// Flatten view hierarchy
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- Child views -->
</merge>

// Use ConstraintLayout for complex layouts
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <!-- Constraints reduce nesting -->
</androidx.constraintlayout.widget.ConstraintLayout>
```

#### 2. RecyclerView Optimization
```kotlin
class OptimizedAdapter : RecyclerView.Adapter<ViewHolder>() {
    // Implement DiffUtil
    private val diffCallback = object : DiffUtil.ItemCallback<Item>() {
        override fun areItemsTheSame(oldItem: Item, newItem: Item): Boolean {
            return oldItem.id == newItem.id
        }
        
        override fun areContentsTheSame(oldItem: Item, newItem: Item): Boolean {
            return oldItem == newItem
        }
    }
    
    // Use ListAdapter for automatic diff
    class EfficientAdapter : ListAdapter<Item, ViewHolder>(diffCallback)
    
    // View pool for shared ViewHolders
    init {
        setHasStableIds(true)
        recyclerView.setRecycledViewPool(viewPool)
    }
}
```

## Best Practices & Tips

### Code Quality
```kotlin
// Use meaningful names
fun processUserData() instead of fun process()

// Single Responsibility Principle
class UserRepository {
    fun getUser() // Only user-related operations
}

// Dependency Injection over direct instantiation
class MyViewModel @Inject constructor(
    private val repository: Repository
)

// Error Handling
sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Error(val exception: Exception) : Result<Nothing>()
}
```

### Security Best Practices
```kotlin
// Secure data storage
private fun storeSecurely(data: String) {
    val encryptedData = encrypt(data)
    securePreferences.edit().putString("key", encryptedData).apply()
}

// Network security
private fun configureNetworkSecurity() {
    // Force HTTPS
    android:usesCleartextTraffic="false"
    
    // Certificate pinning
    val certificatePinner = CertificatePinner.Builder()
        .add("example.com", "sha256/XXXX=")
        .build()
}
```

### Performance Tips
```kotlin
// Lazy loading
private val expensiveObject by lazy {
    // Created only when first accessed
    ExpensiveObject()
}

// Efficient image loading
private fun loadImageEfficiently() {
    Glide.with(context)
        .load(imageUrl)
        .transition(DrawableTransitionOptions.withCrossFade())
        .diskCacheStrategy(DiskCacheStrategy.ALL)
        .into(imageView)
}
```

## Additional Resources
- Keep up with [Android Developers Blog](https://android-developers.googleblog.com/)
- Follow [Android Dev Summit](https://developer.android.com/dev-summit)
- Practice with [Android Code Labs](https://codelabs.developers.google.com/?cat=Android)
- Read official [Material Design Guidelines](https://material.io/design)
- Join Android development communities on Reddit and Stack Overflow
