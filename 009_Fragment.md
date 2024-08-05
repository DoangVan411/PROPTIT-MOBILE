# Fragment
Fragment đóng vai trò quản lý một giao diện của màn hình y như Activity. Nhưng Fragment lại không phải là một thành phần quản lý giao diện độc lập như Activity, Fragment thuộc về quản lý của Activity.

Có thể hình dung Fragment chịu trách nhiệm quản lý một không gian màn hình, nhiều khi không gian màn hình có thể là toàn màn hình. Không gian của Fragment phải nằm trong một Activity nào đó.

Một Activity có thể có nhiều Fragment, một Fragment có thể được khai báo và sử dụng bởi nhiều Activity khác nhau.

## Fragment Manager
Fragment Manager bao gồm một bộ các phương thức để quản lý về vấn đề BackStack của Fragment. 

```java
val fragmentManager: FragmentManager = supportFragmentManager
```

Các phương thức thường sủ dụng của FragmentManager:

| Phương thức                                     | Mô tả                                                                                                                                                                               |                                                                                           
|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `findFragmentById(id) / findFragmentByTag(tag)` | Trả về một fragment tương ứng với tham số truyền vào.                                                                                                                               | 
| `popBackStack() / popBackStackImmediate()`      | Loại bỏ fragment trên đỉnh Stack. popBackStack() là phương thức void đưa request vào queue và xử lý từ từ, popBackStackImmediate() là phương thức trả về bool và xử lý ngay lập tức |                                                                                                   
|                            `getBackStackEntryCount()   `                  |Trả về số lượng item trong Stack hiện tại.                                                                                                                                                                                     |                                                                                                   
|`addOnBackStackChangeListener()`                                                 |Trả về callback khi Stack có sự thay đổi                                                                                                                                                                                     |                                                                                                   
|`getFragments()`                                                 |Trả về danh sách Fragment trong Stack hiện tại dưới dạng List.                                                                                                                                                                                     |
|        `beginTransaction()`                                                         |Tạo một đối tượng FragmentTransaction để thực hiện các thay đổi đối với các fragment, như thêm, xóa hoặc thay thế.                                                                                                                                                                                                                                                 |

## Fragment Transaction
FragmentTransaction là một Class cho phép thêm, xóa, sửa các fragment trong Stack.FragmentTransaction được xây dựng trên Builder Pattern nên các hàm có thể sử dụng nối với nhau. FragmentTransaction được gọi từ một FragmentManager.

```java
val fragmentTransaction: FragmentTransaction = fragmentManager.beginTransaction()
```

Các phương thức thường được sử dụng:

| Phương thức                  | Mô tả                                                                                                                                       |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| `add(layoutId, fragment, tag)` | Thêm 1 fragment vào stack, đè chồng lên fragment hiện có. Fragment bị đè lên vẫn ở trạng thái active                                        |
|  `replace(layoutId, fragment, tag)`                            | Thêm instance của fragment vào stack, thay fragment đang hiển thị bằng fragment mới, fragment bị thay thếsẽ vào trạng thái pause.           |
|   `setCustomAnimations(enter, exit, popEnter, popExit)`                           | Tạo hiệu ứng khi chuyển fragment                                                                                                            |
|   `addToBackStack(name)`                           | Thêm một instance của fragment vào backstack và lưu lại trạng thái với tên trạng thái = tham số truyền vào                                  |
|`remove(fragment)`                              | Xóa fragment hiện tại, hoặc fragment chỉ định. Danh sách Fragment có thể lấy được từ `fragmentManager.getFragments()`                       |
|`commit()`                              | Lệnh commit() được gọi cuối cùng khi đã setup xong biến transaction. Các cấu hình của fragment sẽ chỉ được thực hiện khi commit() được goi. |

## Fragment Lifecycle
### Sơ đồ

![](imgs/fragment_lifecycle.webp)

### Mô tả sơ đồ
Sơ đồ bắt đầu khi Fragment được gắn vào Activity. Khi đó các callback `onAttach(), onCreate(), onCreateView(), onActivityCreated(), onStart() và onResume()` lần lượt được gọi.

Sau khi các callback trên được gọi, thì Fragment lúc bấy giờ mới chính thức được xem là đang chạy.

Sau đó, nếu người dùng nhấn nút Back, hay có bất kỳ thao tác gỡ/thay thế (remove/replace) Fragment ra khỏi Activity nào, thì các callback `onPause(), onStop(), onDestroyView(), onDestroy() và onDetach()` sẽ được gọi.

Nhưng có một điều là, nếu Fragment được đưa vào Back Stack kèm với lệnh gỡ/thay thế, thì `onDestroy() và onDetach()` sẽ chưa được gọi ngay. Để khi rơi vào trường hợp sau đó khi Fragment này được hiển thị lại trong Back Stack, thì `onCreateView()` sẽ được gọi lại.

## Giao tiếp Fragment với Fragment, Activity với Fragment
Fragment có cung cấp cho lập trình viên 2 tùy chọn để thiết lập giao tiếp là Shared ViewModel và Fragment Result API. Để chia sẻ dữ liệu với các Fragment khác mà không chung FragmentManager bạn nên sử dụng `Shared ViewModel`, còn các Fragment có chung FragmentManager mà các bạn muốn gửi và nhận sữ liệu 1 lần thông qua "Bundle" thì các bạn nên sử dụng `Fragment Result API`.
### Share data using ViewModel
Sử dụng Share ViewModel là 1 lựa chọn lý tưởng để giúp bạn chuyển tiếp dữ liệu giữa nhiều Fragment hoặc các Fragment có chung Host Acticity. 

#### Share data with the host activity
Trong 1 số trường hợp mà bạn muốn chia sẻ dữ liệu giữa nhiều Fragment có chung `host activity` nhưng lại khác `FragmentManager` thì các bạn có thể dụng `ShareViewModel` mà vòng đời của ViewModel phụ thuộc vào vòng đời của Activity.

- Định nghĩa file ItemViewModel:
```java
// Kotlin Code
class ItemViewModel : ViewModel() {
    private val mutableSelectedItem = MutableLiveData<Item>()
    val selectedItem: LiveData<Item> get() = mutableSelectedItem

    fun selectItem(item: Item) {
        mutableSelectedItem.value = item
    }
}
```
Trong đoạn code trên, dữ liệu sẽ được chứa trong `MutableLiveData`. LiveData là 1 kiểu dữ liệu mà chỉ có thể đọc chứ không thế thay đổi được giá trị của nó, dữ liệu kiểu MutableLiveData thì giúp chung ta thay đổi được giá trị mà nó chứa. 

Cả Fragment và Activity có thể truy cập tới `shared instance of a ViewModel` với vòng đời phụ thuộc vào vòng đời của Activity thông qua `ViewModelProvider`. `ViewModelProvider` sẽ quản lý việc khỏi tạo ViewModel hoặc truy xuất nó nếu nó đã tồn tại.

```java
// Kotlin Code
class MainActivity : AppCompatActivity() {
    // Using the viewModels() Kotlin property delegate from the activity-ktx
    // artifact to retrieve the ViewModel in the activity scope
    private val viewModel: ItemViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        viewModel.selectedItem.observe(this, Observer { item ->
            // Perform an action with the latest item data
        })
    }
}

class ListFragment : Fragment() {
    // Using the activityViewModels() Kotlin property delegate from the
    // fragment-ktx artifact to retrieve the ViewModel in the activity scope
    private val viewModel: ItemViewModel by activityViewModels()

    // Called when the item is clicked
    fun onItemClicked(item: Item) {
        // Set a new item
        viewModel.selectItem(item)
    }
}
```

#### Share data between fragments
```java
// Kotlin code
class ListViewModel : ViewModel() {
    val filters = MutableLiveData<Set<Filter>>()

    private val originalList: LiveData<List<Item>>() = ...
    val filteredList: LiveData<List<Item>> = ...

    fun addFilter(filter: Filter) { ... }

    fun removeFilter(filter: Filter) { ... }
}

class ListFragment : Fragment() {
    // Using the activityViewModels() Kotlin property delegate from the
    // fragment-ktx artifact to retrieve the ViewModel in the activity scope
    private val viewModel: ListViewModel by activityViewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filteredList.observe(viewLifecycleOwner, Observer { list ->
            // Update the list UI
        }
    }
}

class FilterFragment : Fragment() {
    private val viewModel: ListViewModel by activityViewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filters.observe(viewLifecycleOwner, Observer { set ->
            // Update the selected filters UI
        }
    }

    fun onFilterSelected(filter: Filter) = viewModel.addFilter(filter)

    fun onFilterDeselected(filter: Filter) = viewModel.removeFilter(filter)
}
```
Ở ví dự dưới đây chúng ta sẽ có 2 Framgent. 1 Fragment cho phép hiển thị 1 danh sách, 1 Fragment cho phép người dùng áp dụng các bộ lọc lên các danh sách đó. Nếu 2 Fragment này được thêm vào 2 Page của ViewPager trong đó 1 Fragment cho phép dụng bộ lọc đang được hiển thị còn Fragment hiển thị danh sách đang được ẩn đi thì việc viết 1 liên kết trực tiếp giữa 2 Fragment này khá là khó khăn và việc viết liên kết như thế sẽ không đảm bảo tính độc lập của các Fragment.

Trong trường hợp này thì các bạn có thể khởi tạo 1 ViewModel với Scope là Activtiy chứa 2 Fragment đó thực hiện việc chia sẻ dữ liệu giữa các Fragment thông qua ViewModel đó.

#### Share data between a parent and child fragment
```java
// Kotlin code
class ListFragment: Fragment() {
    // Using the viewModels() Kotlin property delegate from the fragment-ktx
    // artifact to retrieve the ViewModel
    private val viewModel: ListViewModel by viewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filteredList.observe(viewLifecycleOwner, Observer { list ->
            // Update the list UI
        }
    }
}

class ChildFragment: Fragment() {
    // Using the viewModels() Kotlin property delegate from the fragment-ktx
    // artifact to retrieve the ViewModel using the parent fragment's scope
    private val viewModel: ListViewModel by viewModels({requireParentFragment()})
    ...
}
```

### Get results using the Fragment Result API
Trong 1 vài trường hợp bạn chỉ muốn chuyển dữ liệu 1 lần giữa các Framgent và dữ liệu đó sẽ không cần lưu lại để tái sử dụng. Bắt đầu từ `Fragment 1.3.0-alpha04` các `FragmentManager` đều được implements `FragmentResultOwner`. Việc này khiến cho `FragmentManager` có thể hoạt động như 1 trung tâm chứa và chia sẻ dữ liệu giữa các Fragment mà nó quản lý. Thay đổi này cho phép các Fragment mà được quản lý bởi chung 1 `FragmentManager` có thể thiết lập các giao tiếp với nhau thông qua hàm Callback được định nghĩ trong `FragmentManager` mà không cần tạo các liên kết trực tiếp với nhau.

#### Pass results between fragments
Để chuyển tiếp dữ liệu giữa FragmentB tới FragmentA trước tiên hay viết 1 lắng nghe sự kiện thay đổi trong FragmentA.
```java
// Kotlin code
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // Use the Kotlin extension in the fragment-ktx artifact
    setFragmentResultListener("requestKey") { requestKey, bundle ->
        // We use a String here, but any type that can be put in a Bundle is supported
        val result = bundle.getString("bundleKey")
        // Do something with the result
    }
}
```
Trên FragmentB hãy gửi dữ liệu thông qua hàm `setFragmentResult()`. Lưu ý là cả 2 Fragment A và B đều phải được quản lý bởi 1 FragmentManager.
```java
button.setOnClickListener {
    val result = "result"
    // Use the Kotlin extension in the fragment-ktx artifact
    setFragmentResult("requestKey", bundleOf("bundleKey" to result))
}
```
Fragment A nhận kết quả từ Fragment B thông qua `listener callback` 1 lần khi hàm listener được khai báo trong `onCreate()`.

![](imgs/fd7d3ee0-8433-46f1-9f36-d55afd8885ad.webp)

#### Pass results between parent and child fragments
```java
// Parent Fragment
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // We set the listener on the child fragmentManager
    childFragmentManager.setFragmentResultListener("requestKey") { key, bundle ->
        val result = bundle.getString("bundleKey")
        // Do something with the result
    }
}

...

// Child Fragment
button.setOnClickListener {
    val result = "result"
    // Use the Kotlin extension in the fragment-ktx artifact
    setFragmentResult("requestKey", bundleOf("bundleKey" to result))
}
```
![](imgs/chia sẻ giữa fragment cha và con.webp)