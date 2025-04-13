# Dagger-Hilt

---
## Dagger
Dagger là một thư viện Dependency Injection (DI) cho Java và Android do Google phát triển. Nó giúp bạn tự động cung cấp các đối tượng phụ thuộc cho một class thay vì phải tạo thủ công bằng `new`.

Dagger tự động sinh mã (code) giống như những gì bạn sẽ phải viết bằng tay nếu không sử dụng nó. Vì mã này được sinh ra tại thời điểm biên dịch (compile time) nên:
- Dễ dàng theo dõi và debug.
- Hiệu năng cao hơn so với các thư viện dựa trên reflection như Guice.

### Một ví dụ đơn giản với Dagger: Tạo một factory
Để minh họa cách bạn có thể sử dụng Dagger, hãy cùng tạo một factory đơn giản cho lớp UserRepository như sơ đồ sau đây (không có sơ đồ nhưng bạn có thể hình dung là UserRepository phụ thuộc vào hai lớp UserLocalDataSource và UserRemoteDataSource).

- Định nghĩa UserRepository như sau:
```declarative
class UserRepository(
    private val localDataSource: UserLocalDataSource,
    private val remoteDataSource: UserRemoteDataSource
) { ... }
```

- Thêm annotation `@Inject `vào constructor của UserRepository để Dagger biết cách tạo đối tượng này:
```declarative
class UserRepository @Inject constructor(
    private val localDataSource: UserLocalDataSource,
    private val remoteDataSource: UserRemoteDataSource
) { ... }
```
>Trong đoạn mã trên, bạn đang nói cho Dagger biết:
>- Cách để tạo ra một instance của UserRepository (thông qua constructor có `@Inject`).
>- Những phụ thuộc (dependencies) mà UserRepository cần: UserLocalDataSource và UserRemoteDataSource.

- Nhưng Dagger mới chỉ biết cách tạo UserRepository, còn các phụ thuộc của nó thì chưa.
  
  - Nếu bạn cũng thêm `@Inject` vào constructor của `UserLocalDataSource` và `UserRemoteDataSource`, thì Dagger sẽ biết cách tạo luôn cả hai dependency đó.
    ```declarative
    // @Inject cho phép Dagger tạo các dependency cần thiết
    class UserLocalDataSource @Inject constructor() { ... }
    
    class UserRemoteDataSource @Inject constructor() { ... }
    ```
### Các Component trong Dagger
Dagger có thể tạo biểu đồ phụ thuộc (graph of dependencies) trong dự án của bạn để biết cần lấy những phụ thuộc này ở đâu khi cần. Để làm được điều này, bạn cần tạo một interface và đánh dấu nó bằng annotation `@Component`. Dagger sẽ tạo ra một container (thùng chứa các phụ thuộc), tương tự như cách bạn làm khi tiêm dependency thủ công.

```declarative
// @Component makes Dagger create a graph of dependencies
@Component
interface ApplicationGraph {
    // The return type  of functions inside the component interface is
    // what can be provided from the container
    fun repository(): UserRepository
}
```

Khi bạn build project, Dagger sẽ tự động sinh ra implement của interface `ApplicationGraph`:
```declarative
// Tạo instance của Dagger component
val applicationGraph: ApplicationGraph = DaggerApplicationGraph.create()

// Lấy instance của UserRepository từ component
val userRepository: UserRepository = applicationGraph.repository()

```
### Scoping with Dagger

Trong Dagger, bạn có thể sử dụng annotation @Singleton để cho Dagger biết rằng bạn muốn chỉ tạo duy nhất một instance cho một lớp nào đó — giống như Singleton Pattern.

VD: UserRepository chỉ nên có một instance trong toàn bộ ứng dụng.
- Đánh dấu lớp UserRepository và các dependency của nó bằng `@Singleton`.
- Đánh dấu component (`@Component`) cũng bằng `@Singleton`.
```declarative
// Scope annotations on a @Component interface informs Dagger that classes annotated
// with this annotation (i.e. @Singleton) are bound to the life of the graph and so
// the same instance of that type is provided every time the type is requested.
@Singleton
@Component
interface ApplicationGraph {
    fun repository(): UserRepository
}

// Scope this class to a component using @Singleton scope (i.e. ApplicationGraph)
@Singleton
class UserRepository @Inject constructor(
    private val localDataSource: UserLocalDataSource,
    private val remoteDataSource: UserRemoteDataSource
) { ... }
```
## Hilt

Hilt là một thư viện dependency injection (DI) cho Android giúp giảm bớt mã lặp lại khi thực hiện DI thủ công trong dự án của bạn. Việc DI thủ công yêu cầu bạn phải tự tay xây dựng từng lớp và các phụ thuộc của nó, cũng như sử dụng các container để tái sử dụng và quản lý các phụ thuộc.

Hilt cung cấp một cách tiêu chuẩn để sử dụng DI trong ứng dụng của bạn bằng cách cung cấp các container cho mỗi lớp Android trong dự án và tự động quản lý vòng đời của chúng. Hilt được xây dựng trên nền tảng thư viện DI phổ biến Dagger để tận dụng sự chính xác trong thời gian biên dịch, hiệu suất trong thời gian chạy, khả năng mở rộng và hỗ trợ từ Android Studio mà Dagger mang lại.

Để thêm các phụ thuộc vào dự án, đầu tiên, bạn cần thêm plugin `hilt-android-gradle-plugin` vào tệp `build.gradle` ở cấp độ gốc của dự án:
```declarative
plugins {
  ...
  id("com.google.dagger.hilt.android") version "2.51.1" apply false
}
```
Sau đó, áp dụng plugin Gradle và thêm các phụ thuộc này vào tệp `app/build.gradle`:

```declarative
plugins {
  id("kotlin-kapt")
  id("com.google.dagger.hilt.android")
}

android {
  ...
}

dependencies {
  implementation("com.google.dagger:hilt-android:2.51.1")
  kapt("com.google.dagger:hilt-android-compiler:2.51.1")
}

// Cho phép tham chiếu đến mã được tạo ra
kapt {
  correctErrorTypes = true
}
```
Hilt sử dụng các tính năng của Java 8. Để kích hoạt Java 8 trong dự án của bạn, hãy thêm phần sau vào tệp `app/build.gradle`:
```declarative
android {
  ...
  compileOptions {
    sourceCompatibility = JavaVersion.VERSION_1_8
    targetCompatibility = JavaVersion.VERSION_1_8
  }
}
```

### Lớp Application với Hilt
Tất cả các ứng dụng sử dụng Hilt phải có một lớp Application được chú thích với `@HiltAndroidApp`.

`@HiltAndroidApp` kích hoạt quá trình sinh mã của Hilt, bao gồm một lớp cơ sở cho ứng dụng của bạn, đóng vai trò là container phụ thuộc cấp ứng dụng.
```declarative
@HiltAndroidApp
class ExampleApplication : Application() { ... }
```

Lớp Hilt được sinh ra này được gắn liền với vòng đời của đối tượng Application và cung cấp các phụ thuộc cho nó. Ngoài ra, đây cũng là component cha của ứng dụng, có nghĩa là các component khác có thể truy cập vào các phụ thuộc mà nó cung cấp.

### Tiêm phụ thuộc vào các lớp Android
Khi Hilt đã được cấu hình trong lớp Application và có sẵn một component cấp ứng dụng, Hilt có thể cung cấp phụ thuộc cho các lớp Android khác có chú thích `@AndroidEntryPoint`:
```declarative
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() { ... }
```
Hiện tại, Hilt hỗ trợ các lớp Android sau:
- Application (sử dụng @HiltAndroidApp)
- ViewModel (sử dụng @HiltViewModel)
- Activity
- Fragment
- View
- Service
- BroadcastReceiver

Nếu bạn chú thích một lớp Android với @AndroidEntryPoint, bạn cũng phải chú thích các lớp Android phụ thuộc vào nó. Ví dụ, nếu bạn chú thích một fragment, bạn cũng phải chú thích bất kỳ activity nào mà bạn sử dụng fragment đó.

### Định nghĩa các bindings của Hilt
Để thực hiện tiêm phụ thuộc vào trường, Hilt cần biết cách cung cấp các instance của các phụ thuộc cần thiết từ component tương ứng. Một binding chứa thông tin cần thiết để cung cấp các instance của một kiểu dữ liệu như một phụ thuộc.

Một cách để cung cấp thông tin binding cho Hilt là thông qua constructor injection. Sử dụng chú thích `@Inject` trên constructor của lớp để cho Hilt biết cách cung cấp các instance của lớp đó:

```declarative
class AnalyticsAdapter @Inject constructor(
  private val service: AnalyticsService
) { ... }
```
> Lưu ý: Trong quá trình xây dựng, Hilt sinh ra các Dagger components cho các lớp Android. Sau đó, Dagger sẽ quét qua mã nguồn của bạn và thực hiện các bước sau:
>
> - Xây dựng và xác nhận các đồ thị phụ thuộc, đảm bảo không có phụ thuộc nào chưa được thỏa mãn và không có vòng phụ thuộc.
>- Sinh ra các lớp mà Dagger sử dụng tại thời gian chạy để tạo ra các đối tượng thực tế và các phụ thuộc của chúng.

### Các Hilt modules
Đôi khi, một kiểu dữ liệu không thể được constructor-injected. Điều này có thể xảy ra vì nhiều lý do. Ví dụ, bạn không thể constructor-inject một interface. Bạn cũng không thể constructor-inject một kiểu mà bạn không sở hữu, chẳng hạn như một lớp từ thư viện bên ngoài. Trong những trường hợp này, bạn có thể cung cấp thông tin binding cho Hilt bằng cách sử dụng Hilt modules.

Một Hilt module là một lớp được chú thích với `@Module`. Giống như một Dagger module, nó thông báo cho Hilt cách cung cấp các instance của các kiểu dữ liệu nhất định. Không giống như Dagger modules, bạn phải chú thích Hilt modules với `@InstallIn` để cho Hilt biết lớp Android nào sẽ sử dụng hoặc cài đặt module đó.

```declarative
@Module
@InstallIn(SingletonComponent::class)
object RepositoryModule {
    @Provides
    @Singleton
    fun provideChatRepository(firebaseChatDataSource: FirebaseChatDataSource): ChatRepository {
        return ChatRepositoryImpl(firebaseChatDataSource)
    }

    @Provides
    @Singleton
    fun provideAccountRepository(firebaseUserDataSource: FirebaseUserDataSource): AccountRepository {
        return AccountRepositoryImpl(firebaseUserDataSource)
    }

    @Provides
    @Singleton
    fun provideAuthRepositoryImpl(firebaseAuth: FirebaseAuth, firestore: FirebaseFirestore): AuthRepository{
        return AuthRepositoryImpl(firebaseAuth, firestore)
    }
}
```

### Inject các instance của interface với `@Binds`
Xem xét ví dụ với `AnalyticsService`. Nếu `AnalyticsService` là một interface, bạn không thể constructor-inject nó. Thay vào đó, bạn sẽ cung cấp thông tin binding cho Hilt bằng cách tạo một phương thức trừu tượng được chú thích với `@Binds` trong một Hilt module.

Chú thích `@Binds` chỉ cho Hilt biết phải sử dụng triển khai nào khi cần cung cấp một instance của một interface.

Phương thức được chú thích cung cấp thông tin sau cho Hilt:
- Kiểu trả về của phương thức chỉ cho Hilt biết interface mà phương thức này cung cấp các instance.
- Tham số của phương thức chỉ cho Hilt biết triển khai nào sẽ được cung cấp.
```declarative
interface AnalyticsService {
  fun analyticsMethods()
}

// Constructor-injected, vì Hilt cũng cần biết cách cung cấp các instance của AnalyticsServiceImpl.
class AnalyticsServiceImpl @Inject constructor(
  ...
) : AnalyticsService { ... }

@Module
@InstallIn(ActivityComponent::class)
abstract class AnalyticsModule {

  @Binds
  abstract fun bindAnalyticsService(
    analyticsServiceImpl: AnalyticsServiceImpl
  ): AnalyticsService
}
```
- AnalyticsModule là một Hilt module được chú thích với `@InstallIn(ActivityComponent::class)`, có nghĩa là bạn muốn Hilt tiêm phụ thuộc này vào tất cả các activity trong ứng dụng của bạn.
- Phương thức `bindAnalyticsService` trong module chỉ cho Hilt biết rằng khi cần cung cấp một `AnalyticsService`, nó sẽ sử dụng `AnalyticsServiceImpl`.
### Inject instances với @Provides
Không chỉ interfaces, mà constructor-injection cũng không thể thực hiện nếu bạn không sở hữu class đó, ví dụ như các lớp đến từ thư viện bên ngoài (chẳng hạn như Retrofit, OkHttpClient, hoặc Room databases), hoặc khi các instance cần được tạo ra bằng builder pattern.

Xem xét ví dụ trước. Nếu bạn không trực tiếp sở hữu class AnalyticsService, bạn có thể chỉ cho Hilt cách cung cấp các instance của kiểu này bằng cách tạo một phương thức trong một Hilt module và chú thích phương thức đó với `@Provides`.

Phương thức được chú thích cung cấp thông tin sau cho Hilt:
- Kiểu trả về của phương thức chỉ cho Hilt biết loại mà phương thức này sẽ cung cấp các instance.
- Các tham số của phương thức chỉ cho Hilt biết các phụ thuộc của loại tương ứng.
- Thân phương thức cho Hilt biết cách cung cấp instance của loại tương ứng. Hilt sẽ thực thi thân phương thức mỗi khi cần cung cấp một instance của kiểu này.
```declarative
@Module
@InstallIn(ActivityComponent::class)
object AnalyticsModule {

  @Provides
  fun provideAnalyticsService(
    // Các phụ thuộc tiềm năng của kiểu này
  ): AnalyticsService {
      return Retrofit.Builder()
               .baseUrl("https://example.com")
               .build()
               .create(AnalyticsService::class.java)
  }
}
```