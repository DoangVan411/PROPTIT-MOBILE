# KOTLIN SYNTAX 

## Biến và kiểu dữ liệu 
### Nhập xuất trong Kotlin
- Nhập dữ liệu từ bàn phím dùng hàm `readln()` hoặc `readLine()`

Ví dụ:
```java
var x = readln()
var x = readLine()
```
- Xuất dữ liệu ra màn hình dùng hàm `print()` hoặc `println()`

Ví dụ:
```java
println(x)
println("x = " + x)
println("x = $x") (dùng $<tên biến> để in ra biến trong dấu ngoặc kép)
```
### Các kiểu dữ liệu
![](imgs\Kieudulieu.webp)
### Khai báo biến
Trong kotlin có 2 từ khóa để khai báo biến là var và val

- Từ khóa var sử dụng cho biến, trong khi đó val sử dụng khi là hằng số

Cú pháp:
```
Khai báo đầy đủ:
    var (val) <tên biến>: <kiểu dữ liệu> = <giá trị>
```
Ví dụ: 
```
    var number: Int = 5
```
hoặc:
```
Khai báo không có kiểu dữ liệu:
    var (val) <tên biến> = <giá trị>
    // Kotlin sẽ lấy kiểu dữ liệu dựa vào giá trị truyền vào
```
Ví dụ:
```
    var str = "hehe"
```
>Chú ý: 
> 
> Dấu gạch dưới có thể được thêm vào các số để dễ đọc hơn. Ví dụ: 1_000_000
> 
> Khai báo kiểu float cần thêm `f` hoặc `F`. Ví dụ: 5.0f
> 
> Khai báo kiểu long cần thêm `L`. Ví dụ: 5L
### Ép kiểu
>
> toByte() -> Byte
>
> toShort() -> Short
>
> toInt() -> Int
>
> toLong() -> Long
>
> toFloat() -> Float
>
> toDouble() -> Double
>
> toChar() -> Char
>
- Ép kiểu rộng: Đưa từ kiểu có vùng lưu trữ nhỏ lên kiểu có vùng lưu trữ lớn==>không sợ mất mát dữ liệu.
  - Ví dụ:
      ```java
      var num: Int = 5
      println(num.toDouble())
      ```
  - Kết quả:
    ```java
    5.0
    ```
- Ép kiểu hẹp: Đưa từ kiểu có vùng lưu trữ lớn về kiểu có vùng lưu trữ nhỏ==>có thể bị mất mát dữ liệu
  - Ví dụ:
    ```java
    var num: Double = 5.5
    println(num.toInt())
    ```
  - Kết quả:
    ```java
    5
    ```
### Mảng
Cú pháp:
```java
    var(val) tên_mảng: <kiểu dữ liệu mảng> = typeArrayOf(giá trị 1, giá trị 2,…, giá trị n)
hoặc:
    var(val) tên_mảng = arrayOf<kiểu dữ liệu>(giá trị 1, giá trị 2, ...)
```
Ví dụ:
```
    var arr: IntArray = intArrayOf(1, 2, 3, 4, 5)
    var arr: CharArray= charArrayOf('a','b','c')
    var arr = arrayOf<Int>(1, 2, 3, 4, 5)
```
## Các toán tử 
Trong Kotlin, ta có thể sử dụng các Operator thuần túy và cũng có thể dùng bằng các phương thức.

**Các toán tử toán học**

| Toán tử | Phương thức |
|---------|-------------|
| +       | a.plus(b)   |
| -       | a.minus(b)  |
| *       | a.times(b)  |
| /       | a.div(b)    |
| %       | a.rem(b)    |

**Các toán tử so sánh**

| Toán tử | Phương thức         |
|---------|---------------------|
| ==      | a.equals(b)         |
| !=      | !a.equals(b)        |
| <       | a.compareTo(b) < 0  |
| <=      | a.compareTo(b) <= 0 | 
| \>      | a.compareTo(b) > 0  |
| \>=     | a.compareTo(b) >= 0 |


## Câu lệnh rẽ nhánh
### Câu lệnh If - Else
Tương tự trong Java:
```java
val number = 5

    // if expression
    if (number%2 == 0)
        println("even")
    else println("odd")
```
Kết quả:
```java
odd
```

### Câu lệnh When
Tương tự câu lệnh switch - case ở các ngôn ngữ lập trình khác

Cú pháp:
```java
when(expression){ 
    <giá trị 1> -> <câu lệnh 1>
    <giá trị 2> -> <câu lệnh 2>
        ...
    else -> <câu lệnh>
}
```

Ví dụ:
```java
var x: Int = 5

    when(x){

        2 -> println("Monday")
        3 -> println("Tuesday")
        4 -> println("Wednesday")
        5 -> println("Thursday")
        6 -> println("Friday")
        7 -> println("Saturday")
        8 -> println("Sunday")
        else -> println("$x isn't a day of a week")
    }
```
Kết quả:
```java
Thursday
```

## Vòng lặp
### Vòng lặp for
- `for (i in a..b)`: duyệt từ a đến b (tăng dần)
- `for (i in a until b)`: duyệt từ a đến b-1 (tăng dần)
- `for (i in a .. b step x)`: duyệt từ a đến b với bước nhảy x (tăng dần)
- `for (i in b downTo a) `: duyệt từ a đến b (giảm dần)
- `for (i in b downTo a step x)`: duyệt từ a đến b với bước nhảy x (giảm dần)
- `for (item in collection)`: duyệt từng đối tượng
### Vòng lặp while
```java
while(expression)
{
  statement
}
```
### Repeat
```java
// greets three times
repeat(3) {
    println("Hello")
}

// greets with an index
repeat(3) { index ->
    println("Hello with index $index")
}

repeat(0) {
    error("We should not get here!")
}
```
Kết quả
```java
Hello
Hello
Hello with index 0
Hello with index 1
Hello with index 2
```
## Các collections trong Kotlin

### List
- Có 2 loại List đó là List (chỉ đọc) và MutableList (có thể chỉnh sửa) triển khai List
- Để tạo một `List` ta sử dụng hàm `listOf()`. Ví dụ: `val list = listOf<String>("Hello", "hello", "hehe")`
- Để tạo một `MutableList` ta sử dụng hàm `mutableListOf()`. Ví dụ: `val mutableList = mutableListOf<String>("Hello", "hello", "hehe")`
- Một số thuộc tính và phương thức thường được sử dụng:

| Thuộc tính/ Phương thức | Mô tả                                                                                                                  |
|-------------------------|------------------------------------------------------------------------------------------------------------------------|
| size                    | Số phần tử trong danh sách                                                                                             |
| get(i)                  | Trả về phần tử tại chỉ mục i của danh sách                                                                             |
| indexOf(item)           | Trả về chỉ mục xuất hiện lần đầu tiên của phần tử item. Nếu không có phần tử trong danh sách, chỉ số được trả về là -1 |

- Một số phương thức thường sử dụng của MutableList:

| Phương thức | Mô tả      | Ví dụ                                        |
|------------|------------|----------------------------------------------|
| add()      | Thêm một phần tử vào trong MutableList | strArr.add("hehe")<br/>strArr.add(2, "hehe") |
| removeAt(i) | Xóa phần tử có chỉ số i | arr.removeAt(1)                              |
| remove(item) | Xóa phần tử item | arr.remove("hehe")                           |            |                                              |

### Set
- Cũng giống như List, Set cũng có Set và Mutable Set
- Để tạo Set và MutableSet tương tự List, sử dụng 2 hàm `setOf()` và `mutableSetOf()`
- Để lưu các phần tử, Set sử dụng Mã băm (hashcode)
- Mỗi phần tử trong Set là duy nhất

### Map
- Map lưu dữ liệu dưới dạng key - value
- Mỗi key là duy nhất, value có thể trùng nhau
- Khai báo Map:
```java
val <name> = mapOf<K, V>(key to value, key to value,...)
val <name> = mutableMapOf<K, V>(key to value, key to value,...)
```
Ví dụ:
```java
val map = mapOf<String, Int>("al" to 3, "na" to 1)
val mutableMap = mutableMapOf<String, Int>("al" to 3, "na" to 1)
```
## Null Safety
Trong Kotlin giá trị null sẽ được xử lý riêng biệt và bạn sẽ không được phép khai báo một giá trị null cho một đối tượng.

Ví dụ:
```java
fun main(args: Array<String>) {
    var name: String
    name = null
    println(name)
}
```
Ta nhận được thông báo:
```java
Null cannot be a value of a non null-type String
```
Bất cứ khi nào bạn nghĩ rằng biến có thể có giá trị null thì bạn sẽ phải thêm một dấu hỏi chấm (?) ngay phía sau kiểu dữ liệu

Ví dụ:
```java
fun main(args: Array<String>) {
    var name: String? // Đã thêm dấu hỏi chấm
    name = null
    println(name)
}
```
Kết quả:
```java
null
```
- Toán tử khẳng định not-null `!!` chuyển đổi bất kỳ giá trị nào thành kiểu not-null và ném ra ngoại lệ nếu giá trị là null 

Ví dụ:
```
val name: String? = null// Đã thêm dấu hỏi chấm
print(name!!)
```
Kết quả:
```java
Exception in thread "main" java.lang.NullPointerException
        at MainKt.main(Main.kt:3)
```
### Safe calls
Có thể gọi đến một thuộc tính của một đối tượng có thể null bằng safe call sử dụng toán tử `?`

Ví dụ:
```java
var name: String?
name = null
println(name?.length)
```
Nếu name = null, kết quả được in ra sẽ là null, ngược lại sẽ in ra độ dài của xâu name

Để thực hiện một thao tác nhất định chỉ cho các giá trị not-null, bạn có thể sử dụng toán tử gọi an toàn cùng với hàm `let`:
```java
val listWithNulls: List<String?> = listOf("Kotlin", null)
    for (item in listWithNulls) {
        item?.let { println(it) }
}
```
Kết quả:
```java
Kotlin //Giá trị null bị bỏ qua
```

