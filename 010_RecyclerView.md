# Recycler View
RecyclerView là một ViewGroup được dùng để chuẩn bị và hiện thị các View tương tự nhau, nó là sự kế thừa và nâng cao của ListView và GridView được hỗ trợ trong support-v7 version.

Một trong những ưu điểm nổi trội của Recycler:
- Buộc sử dụng ViewHolder để tái sử dụng xml item cho mỗi phần tử, tăng tốc hiệu năng cho danh sách.
- Có thể sử dụng nhiều item type, và đương nhiên là với ViewHolder thì ta cũng có thể tái sử dụng nó.
- Không chỉ hỗ trợ dạng danh sách dọc, RecyclerView còn hỗ trợ dạng Grid, dọc, ngang đủ cả.

## Các thành phần của RecyclerView
![](imgs/recyclerview.webp)

- Một RecyclerView cần phải có một **layout manager** và **adapter** được cài đặt. Vị trí và cách sắp xếp Item trong RecyclerView được quyết định bởi Layout Manager. **RecyclerView** cung cấp cho người lập trình 3 LayoutManager sau:

  - `LinearLayoutManager` hiển thị Item theo chiều ngang hay chiều dọc như Scroll list.
  - `GridLayoutManager` hiển thị Item theo kiểu Grid
  - `StaggeredGridLayoutManager` hiển thị Item theo tùy kích thước của Item trong Grid.
  
  Để khởi tạo Layout Manager bạn cần kế thừa lớp RecyclerView.LayoutManager.
- `RecyclerView.Adapter` **RecyclerView** có một adapter mới. Nó cũng gần giống như các Adapter cũ, nhưng nó một số đặc tính riêng như là bắt buộc sử dụng `ViewHolder`. Bạn sẽ cần ghi đè 2 phương thức chính, 1 là để định nghĩa VIew và ViewHolder 2 là để chuyển dữ liệu tới View.
- ItemAnimator `RecyclerView.ItemAnimator` là một animation của ViewGroup, mọi hành động thêm/sửa/xóa đều được thông báo cho Adapter. **DefaultItemAnimator** được sử dụng như là 1 Animation mặc định và hoạt động khá tốt.

> Adapter là class để ánh xạ item view của bạn và item trong danh sách đầu vào.
>
> ViewHolder là một lớp (class) chứa các tham chiếu đến các view trong một item của RecyclerView. Khi sử dụng ViewHolder, bạn lưu trữ các view (chẳng hạn như TextView, ImageView) trong một đối tượng và sử dụng đối tượng đó để cập nhật nội dung.

## Sử dụng RecyclerView
Để sử dụng RecyclerView sẽ có 7 bước chính sau đây:

- Thêm RecyclerView support library vào `gradle build file`
- Định nghĩa ra `model class` để sử dụng `data source`
- Thêm RecyclerView vào trong `activity` mà bạn muốn hiển thị
- Tạo một tệp XML để xác định một item được biểu diễn như nào
- Tạo ra `RecyclerView.Adapter` và `ViewHolder` để gán dữ liệu cho các item
- Kết nối `adapter` với `data source` để đưa vào RecyclerView
### Cài đặt thư viện
Đảm bảo bạn đã thêm RecyclerView suport library vào dependency trong app/build.gradle:
```
dependencies {
    ...
    implementation 'com.android.support:design:28.0.0'
}
```
### Định nghĩa ra Model
Mỗi RecyclerView cần có một nguồn dữ liệu.

Ví dụ: Tạo ra một lớp Hero đại diện cho data model sẽ được hiển thị bởi RecyclerView:
```java
data class Hero(
      val name: String,
      val image: Int
)
```
### Thêm RecyclerView vào trong layout
Bên trong file layout của activity mà bạn muốn hiển thị `res/layout/activity_hero.xml`, hãy thêm RecyclerView từ support library vào:
```java
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerHero"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>
```

![](imgs/hiện recycler view.webp)

Giờ đây RecyclerView đã được nhúng vào activity layout file. 
### Tạo ra Custom row layout
Trước khi tạo ra Adapter , hãy định nghĩa ra XML layout file sử dụng cho mỗi row của danh sách. item layout file dưới đây chỉ chứa một ImageView hiển thị ảnh của Hero và một TextView hiển thị tên Hero.
```java
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <ImageView
        android:id="@+id/image_hero"
        android:layout_width="0dp"
        android:layout_height="@dimen/dp_180"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:scaleType="center"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/text_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:text="TextView"
        android:textColor="#FFF"
        app:layout_constraintBottom_toBottomOf="@+id/image_hero"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.112"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/image_hero"
        app:layout_constraintVertical_bias="1.0" />
</android.support.constraint.ConstraintLayout>
```
### Tạo ra RecyclerView.Adapter
Tại đây chúng ta sẽ tạo ra Adapter là nơi xử lý dữ liệu và gán data cho RecyclerVIew. Vai trò của Adapter sẽ chuyển đổi một object tại một vị trí trở thành 1 hàng của danh sách sẽ được gắn vào RecyclerView.

Tuy nhiên đối với RecyclerView Adapter sẽ yêu cầu **ViewHolder** object trong đó mô tả và cung cấp quyền truy cập vào tất cả các View trong mỗi item row.
```java
class HeroAdapter(
        private val mContext: Context,
        private val mHeros: List<Hero>
) : RecyclerView.Adapter<HeroAdapter.ViewHolder>() {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
      val inflater = LayoutInflater.from(mContext)
      val heroView = inflater.inflate(R.layout.item_hero, parent, false)
      return ViewHolder(heroView)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
      val hero = mHeros[position]
      Glide.with(mContext)
          .load(hero.image ?: R.drawable.default_image)
          .into(holder.mImageHero)
      holder.mTextName.text = hero.name ?: ""
    }

    override fun getItemCount(): Int = mHeros.size

    inner class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
      val mImageHero: ImageView = itemView.findViewById(R.id.image_hero)
      val mTextName: TextView = itemView.findViewById(R.id.text_name)
    }
}

```
### Kết nối Adapter với RecyclerView
```java
class HeroActivity : AppCompatActivity() {

    private lateinit var mHeros: MutableList<Hero>
    private lateinit var mRecyclerHero: RecyclerView
    private lateinit var mHeroAdapter: HeroAdapter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_hero)

        mRecyclerHero = findViewById(R.id.recyclerHero)
        mHeros = mutableListOf()
        createHeroList()
        mHeroAdapter = HeroAdapter(this, mHeros)
        mRecyclerHero.adapter = mHeroAdapter
        mRecyclerHero.layoutManager = LinearLayoutManager(this)
    }

    private fun createHeroList() {
        mHeros.apply {
            add(Hero("Thor", R.drawable.image_thor))
            add(Hero("IronMan", R.drawable.image_ironman))
            add(Hero("Hulk", R.drawable.image_hulk))
            add(Hero("SpiderMan", R.drawable.image_spiderman))
            add(Hero("Thor", R.drawable.image_thor))
            add(Hero("IronMan", R.drawable.image_ironman))
            add(Hero("Hulk", R.drawable.image_hulk))
            add(Hero("SpiderMan", R.drawable.image_spiderman))
            add(Hero("Thor", R.drawable.image_thor))
            add(Hero("IronMan", R.drawable.image_ironman))
        }
    }
}
```
Kết quả:

![](imgs/heros.webp)

## Tính tái sử dụng của RecyclerView

Một khi RecyclerView được kết nối với Adapter , Adapter sẽ tạo ra đối tượng của các hàng (ViewHolder object) cho đến khi lấp đầy kích thước của RecyclerView và lưu trong HeapMemory . Sau đó sẽ không tạo thêm bất kỳ hàng nào để lưu trong bộ nhớ nữa Theo cách này nếu ngườ dùng trượt danh sách, các item đã trượt khỏi màn hình sẽ được giữ ở trong bộ nhớ để tái sử dụng lại sau và mỗi khi một hàng mới được chèn vào màn hình thì đối tượng ViewHolder được lưu trong bộ nhớ sẽ được mang ra để tái sử dụng và gán dữ liệu . Nếu không gán lại dữ liệu cho ViewHolder object thì sẽ hiện thị dữ liệu được gán trước đó. Theo cách này thì kể cả danh sách có 1000 item, thì chỉ có khoảng 7 đối tượng ViewHolder được tạo ra.

## List Adapter
ListAdapter là một class mới trong support library 27.1.0 và đơn giản hóa code cần thiết để làm việc với RecyclerView. Và kết quả màn bạn nhật được là bạn cần viết ít code hơn và recyclerview animation hoạt động tốt hơn.

ListAdapter tự động lưu trữ list item trước và sử dụng DiffUtil để chỉ update những items trong recycler mà có sự thay đổi.

Adapter mặc định chỉ xử lý việc thêm một item mới và không xử lý việc xóa hoặc sửa một item. Có thể sử dụng `notifyDataSetChanged()` nhưng nó sẽ vẽ lại tất cả các view, kể cả những phần không bị thay đổi. Để xử lý chúng sao cho hiệu năng tốt và animation đẹp, chúng ta cần làm nhiều việc hơn một chút đó là sử dụng `DiffUtil`.

### DiffUtil
ListAdapter xây dựng dựa trên DiffUtil nghĩa là nó giúp bạn xử lý một vài diffing logic. Nếu bạn chưa từng biết về DiffUtils thì nó được tạo ra để khuyến khích developer cho phép RecyclerView chỉ update nội dung thay đổi, dẫn đến hiệu suất tốt hơn khi những item được inserted, updated hoặc deleted.

Để xác định dữ liệu mới, DiffUtil yêu cầu bạn ghi đè các phương thức `areItemsTheSame()` và `areContentsTheSame()`. `areItemsTheSame()` kiểm tra xem hai mục có thực sự là cùng một mục không. `areContentsTheSame()` kiểm tra xem hai mục có dữ liệu giống nhau không.

Trong lớp Adapter, thêm một đối tượng DiffUtil ghi đè các phương thức `areItemsTheSame()` và `areContentsTheSame()`.

```java
object FlowerDiffCallback : DiffUtil.ItemCallback<Flower>() {
    override fun areItemsTheSame(oldItem: Flower, newItem: Flower): Boolean {
        return oldItem.id == newItem.id
    }
    
    override fun areContentsTheSame(oldItem: Flower, newItem: Flower): Boolean {
        return oldItem == newItem
    }
}
```
Adapter thay vì kế thừa từ class `RecyclerView.Adapter` thì sẽ kế thừa `ListAdapter`, truyền vào `DiffCallBack`.
```java
class FlowerAdapter : ListAdapter<Flower, FlowerAdapter.FlowerViewHolder>(FlowerDiffCallback)
```
## RecyclerView Multiple View Type
Trong Android, RecyclerView hỗ trợ nhiều kiểu hiển thị (view types) khác nhau trong cùng một danh sách. Điều này rất hữu ích khi bạn muốn hiển thị các loại item khác nhau trong danh sách, chẳng hạn như một số item là hình ảnh, một số là văn bản, hoặc một số có các kiểu bố trí khác nhau.

Việc chúng ta xây dựng RecylerView có nhiều kiểu item khác nhau cũng giống như RecyclerView chỉ có 1 kiểu item nhưng có một chút khác biệt ở ở Custom Adapter như sau:
- Phải tạo ViewHolder cho từng kiểu view
- Tạo các file XML layout khác nhau cho từng kiểu view mà bạn muốn hiển thị.
- Override lại phương thức `getItemViewType(position: Int)` để get type tương ứng với từng position
- Trong fun `onCreateViewHolder()` dựa vào view type để tạo ViewHolder tương ứng.
  - Trong fun `onBindViewHolder()` dựa và view type để bind data tương ứng vào ViewHolder.

Ví dụ: 

Lớp adapter với 2 view type:
```java
class MainAdapter(
    private val typeOneList: MutableList<String>,
    private val typeTwoList: MutableList<String>
) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {
    override fun onCreateViewHolder(p0: ViewGroup, p1: Int): RecyclerView.ViewHolder {
        val inflater = LayoutInflater.from(p0?.context)
        return when (p1) {
            ViewType.TYPE_ONE.type -> {
                val view = inflater.inflate(R.layout.type_one, p0, false)
                TypeOneViewHodel(view)
            }
            else -> {
                val view = inflater.inflate(R.layout.type_two, p0, false)
                TypeTwoViewHodel(view)
            }
        }

    }

    override fun getItemCount(): Int {
        return typeOneList.count() + typeTwoList.count()
    }

    override fun onBindViewHolder(p0: RecyclerView.ViewHolder, p1: Int) {
        p0.apply {
            when (p0) {
                is TypeOneViewHodel -> p0.bind(typeOneList[p1])
                is TypeTwoViewHodel -> p0.bind(typeTwoList[p1 - typeOneList.count() ])
            }
        }
    }

    override fun getItemViewType(position: Int): Int {
        return when (position) {
            in 0 until typeOneList.count() -> ViewType.TYPE_ONE.type
            else -> ViewType.TYPE_TWO.type
        }
    }

    inner class TypeOneViewHodel(itemView: View) : RecyclerView.ViewHolder(itemView) {
        fun bind(item: String) {
            with(itemView) {
                tvContact.text = item
            }

        }

    }

    inner class TypeTwoViewHodel(itemView: View) : RecyclerView.ViewHolder(itemView) {
        fun bind(item: String) {
            with(itemView) {
                tvAccount.text = item

            }
        }

    }

}

enum class ViewType(val type: Int) {
    TYPE_ONE(0),
    TYPE_TWO(1)
}
```

Trong class Activity:
```java
 fun initView() {
        addTypeList1()
        addTypeList2()
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = MainAdapter(typeOneList, typeTwoList)
    }

fun addTypeList1() {
    typeOneList.add("type 1 1")
    typeOneList.add("type 1 2")
    typeOneList.add("type 1 3")
    typeOneList.add("type 1 4")
    typeOneList.add("type 1 5")

}

fun addTypeList2() {
    typeTwoList.add("type 2 1")
    typeTwoList.add("type 2 2")
    typeTwoList.add("type 2 3")
    typeTwoList.add("type 2 4")
    typeTwoList.add("type 2 5")
}

```