# OBJECT & CALLBACK

<!-- TOC -->
* [OBJECT & CALLBACK](#object--callback)
  * [Object](#object)
    * [Object Expressions](#object-expressions)
      * [Tạo ra các object ẩn danh từ đầu](#to-ra-cc-object-n-danh-t-u)
      * [Kế thừa object ẩn danh từ supertypes](#k-tha-object-n-danh-t-supertypes)
      * [Sử dụng object ẩn danh như kiểu trả về và kiểu giá trị](#s-dng-object-n-danh-nh-kiu-tr-v-v-kiu-gi-tr)
      * [Truy cập các biến từ các object ẩn danh](#truy-cp-cc-bin-t-cc-object-n-danh)
    * [Object Declarations](#object-declarations)
      * [Data Object](#data-object)
  * [Callback](#callback)
    * [Higher-Order Functions](#higher-order-functions)
      * [Truyền vào biểu thức lambda làm tham số cho hàm bậc cao](#truyn-vo-biu-thc-lambda-lm-tham-s-cho-hm-bc-cao)
      * [Truyền vào hàm làm tham số cho hàm bậc cao](#truyn-vo-hm-lm-tham-s-cho-hm-bc-cao)
    * [Lambdas Expressions and Anonymous Functions](#lambdas-expressions-and-anonymous-functions)
      * [Lambda Expression](#lambda-expression)
      * [Cú pháp của biểu thức lambda:](#c-php-ca-biu-thc-lambda-)
      * [Trailing Lambda](#trailing-lambda)
      * [`it`: tên ngầm định của một tham số duy nhất](#it--tn-ngm-nh-ca-mt-tham-s-duy-nht)
      * [Trả về một giá trị trong một biểu thức lambda](#tr-v-mt-gi-tr-trong-mt-biu-thc-lambda)
      * [Anonymous functions](#anonymous-functions)
      * [Functions literals with receiver](#functions-literals-with-receiver)
    * [Callback Function](#callback-function)
      * [Ưu, nhược điểm của callback](#u-nhc-im-cu-callback)
<!-- TOC -->

## Object
Đôi khi bạn cần tạo một đối tượng là một sự thay đổi nhỏ của một lớp nào đó, mà không cần phải khai báo một lớp con mới cho nó. Kotlin có thể xử lý điều này bằng cách sử dụng các biểu thức đối tượng và khai báo đối tượng.
### Object Expressions
Object expressions tạo ra các đối tượng của lớp ẩn danh (những lớp không cần khai báo rõ ràng bằng khai báo `class`). Những lớp như vậy rất hữu dụng trong trường hợp chỉ dùng 1 lần. Bạn có thể định nghĩa chúng từ đầu, kế thừa từ các lớp hiện có hoặc implement các interface.

Các đối tượng của lớp ẩn danh cũng được coi là object ẩn danh vì chúng cũng được định nghĩa bằng biểu thức, không phải bằng tên.

#### Tạo ra các object ẩn danh từ đầu
Object expression bắt đầu với từ khóa `object`.

Nếu bạn chỉ cần một object không có supertype không tầm thường, viết các thành phần của nó trong dấu ngoặc nhọn sau `object`:
```java
fun main(args: Array<String>) {
    val helloWorld = object {
        val hello = "Hello"
        val world = "World"
        // Object expression kế thừa Any, nên từ khóa `override` được yêu cầu cho hàm to 
        override fun toString() = "$hello $world"
    }

    println(helloWorld)
}

//Hello World
```
#### Kế thừa object ẩn danh từ supertypes
Để tạo ra một object của một lớp ẩn danh mà kế thừa từ một kiểu (hoặc các kiểu), chỉ định kiểu này sau từ khóa `object` và `:`. Sau đó, triển khai hoặc ghi đè thành phần của lớp này như thể bạn đang kế thừa nó.
```
open class Person(){
    open fun eat(){
        println("Măm măm.")
    }
}

fun main() {
    val person = object: Person() {
        override fun eat(){
            println("Yum yum.")
        }
    }
}

// Yum yum.
```
Nếu supertype có constructor, cần phải truyền tham số phù hợp cho nó. Nếu object kế thừa nhiều supertype, sử dụng dấu `,` để ngăn cách các supertype.
```java
open class A(x: Int) {
    public open val y: Int = x
}

interface B { /*...*/ }

val ab: A = object : A(1), B {
    override val y = 15
}
```

#### Sử dụng object ẩn danh như kiểu trả về và kiểu giá trị
Khi một đối tượng ẩn danh được sử dụng như một khai báo (hàm hoặc thuộc tính) local hoặc private (không phải inline), tất cả các thành phần của nó có thể được truy cập thông qua hàm hoặc thuộc tính này.
```java
class C {
    private fun getObject() = object {
        val x: String = "x"
    }

    fun printX() {
        println(getObject().x)
    }
}
```
#### Truy cập các biến từ các object ẩn danh
Code trong các object expression có thể truy cập các biến từ phạm vi bao quanh nó.
```java
open class Person(){
    open fun eat(){
        println("Măm măm.")
    }
}

fun main() {
    var weight = 50
    val person = object: Person(){
        fun eatFastFood(){
            eat()
            weight += 5
        }
    }
    person.eatFastFood()
    println(weight)
}

// Măm măm.
// 55
```
### Object Declarations
Singleton là một mẫu hướng đối tượng trong đó một class chỉ có thể có một instance (đối tượng).

Kotlin cung cấp một cách dễ dàng để tạo các singleton bằng cách sử dụng tính năng khai báo đối tượng. Để làm được điều đó, chúng ta sử dụng từ khóa `object`.
```java
object SingletonExample {
    ... .. ...
    // body of class
    ... .. ...
}
```
Nó được gọi là **object declaration**. Giống như một khai báo biến, một object declaration không phải là một biểu thức, và nó không thể đứng ở vế phải của một toán tử gán.

Để gọi đến một object, sử dụng trực tiếp tên của nó. Ví dụ: `SingletonExample.hello()`

Các object cũng có thể có các supertype:
```java
object DefaultListener : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { ... }

    override fun mouseEntered(e: MouseEvent) { ... }
}
```
> Object declaration không thể là local, nhưng có thể được lồng trong các object declaration hoặc non-inner class khác.

#### Data Object
Giống như data class, data object cũng tạo sẵn một số hàm cho object: 
- `toString()` trả về tên của data object
- `equals()/hashCode()`

Hàm equals() cho một đối tượng dữ liệu đảm bảo rằng tất cả các đối tượng có kiểu của đối tượng dữ liệu của bạn được coi là bằng nhau.

Hàm hashCode() được tạo ra có hành vi nhất quán với hàm equals(), để tất cả các thể hiện runtime của một đối tượng dữ liệu có cùng một mã băm.

## Callback
### Higher-Order Functions
Một hàm bậc cao là một hàm nhận các hàm như là tham số, hoặc trả về một hàm. Thay vì truyền tham số kiểu Int, String hoặc Array, ta sẽ truyền vào hàm ẩn danh hoặc lamda.
Thông thường, lamda được truyền dưới dạng tham số trong hàm Kotlin để thuận tiện hơn.

#### Truyền vào biểu thức lambda làm tham số cho hàm bậc cao
Có 2 loại biểu thức lambda có thể được truyền vào:
- Biểu thức lambda trả về Unit

```java
Ví dụ 1:
        
// lambda expression
var lambda = {println("Lambda expression returning Unit") }
// higher-order function
fun higherfunc( lmbd: () -> Unit ) { // accepting lambda as parameter
    lmbd()	//invokes lambda expression
}
fun main(args: Array<String>) {
    //invoke higher-order function
    higherfunc(lambda)	// passing lambda as parameter
}
// Lambda expression returning Unit
```
- Biểu thức lambda trả về các giá trị khác

```java
Ví dụ 2:
        
    // lambda expression
var lambda = {a: Int , b: Int -> a + b }
    // higher order function
fun higherfunc( lmbd: (Int, Int) -> Int) { // accepting lambda as parameter
         
    var result = lmbd(2,4)    // invokes the lambda expression by passing parameters                    
    println("The sum of two numbers is: $result")
}
 
fun main(args: Array<String>) {
    higherfunc(lambda)  //passing lambda as parameter
}
// The sum of two numbers is: 6
```
#### Truyền vào hàm làm tham số cho hàm bậc cao
Có 2 loại hàm có thể truyền vào:
- Hàm trả về Unit
> Muốn gọi một hàm nào đó trong lời gọi hàm bậc cao, cần có dấu `::` trước tên hàm
```java
Ví dụ 3:

// regular function definition
fun printMe(s:String): Unit{
    println(s)
}
// higher-order function definition
fun higherfunc( str : String, myfunc: (String) -> Unit){
    // invoke regular function using local name
    myfunc(str)
}
fun main(args: Array<String>) {
    // invoke higher-order function
    higherfunc("Hello World!",::printMe)
}
// Hello World!
```
- Hàm trả về các giá trị khác
```java
Ví dụ 4:

// regular function definition
fun add(a: Int, b: Int): Int{
    var sum = a + b
    return sum
}
// higher-order function definition
fun higherfunc(addfunc:(Int,Int)-> Int){
    // invoke regular function using local name
    var result = addfunc(3,6)
    print("The sum of two numbers is: $result")
}
fun main(args: Array<String>) {
    // invoke higher-order function
    higherfunc(::add)
}
// The sum of two numbers is: 9
```
### Lambdas Expressions and Anonymous Functions
Biểu thức lambda và hàm ẩn danh là những function literal. Function literal là những hàm không được khai báo mà được truyền ngay lập tức như một biểu thức.
#### Lambda Expression
#### Cú pháp của biểu thức lambda:
```java
val lambda_name : Data_type = { argument_List -> code_body }
```
Ví dụ:
```java
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }
```

> - Biểu thức lambda luôn được bao quanh bởi dấu ngoặc nhọn.
>
> - Các khai báo tham số trong dạng cú pháp đầy đủ nằm bên trong dấu ngoặc nhọn và có chú thích kiểu dữ liệu tùy chọn.
>
> - Thân của lambda nằm sau dấu `->`.
>
> - Nếu kiểu trả về suy luận của lambda không phải là Unit, biểu thức cuối cùng (hoặc có thể là duy nhất) bên trong thân lambda được coi là giá trị trả về.

Nếu bỏ qua các chú thích tùy chọn, biểu thức lambda sẽ như dưới đây:
```java
val sum = {a: Int , b: Int -> a + b}
```
#### Trailing Lambda
Theo quy ước của Kotlin, nếu tham số cuối cùng của một hàm là một hàm khác, thì biểu thức lambda được truyền như một đối số tương ứng có thể được đặt bên ngoài dấu ngoặc đơn.
```java
fun calculate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

fun main() {
    // Gọi hàm với lambda truyền thống
    val result1 = calculate(3, 4, { x, y -> x + y })
    println(result1)
    
    // Gọi hàm với trailing lambda
    val result2 = calculate(3, 4) { x, y -> x + y }
    println(result2)
}

//7
//7
```
Cú pháp này được gọi là `trailing lambda`.

Nếu lambda là đối số duy nhất trong lời gọi hàm, dấu ngoặc đơn có thể được bỏ qua hoàn toàn.
```java
fun doSthg (action: () -> Unit){
    action()
}

fun main(args: Array<String>) {
    doSthg { println("Running...") }
}

//Running...
```
#### `it`: tên ngầm định của một tham số duy nhất
Rất phổ biến cho biểu thức lambda có chỉ 1 tham số.

Nếu trình biên dịch có thể phân tích signature mà không có bất kỳ tham số nào thì tham số không cần phải được khai báo và `->` có thể được bỏ qua. Tham số sẽ được khai báo ngầm định dưới tên `it`.
```java
val ints = listOf(-1, 2, -3, 4)

ints.filter { it > 0 } // biểu thức lambda này có kiểu '(it: Int) -> Boolean'
```
#### Trả về một giá trị trong một biểu thức lambda
Bạn có thể trả về một giá trị từ biểu thức lambda bằng cách sử dụng cú pháp return đủ điều kiện. Nếu không, giá trị của biểu thức cuối cùng sẽ được trả về ngầm định.

Hai đoạn mã sau đây là tương đương:
```java
val ints = listOf(-1, 2, -3, 4)

ints.filter {
    val shouldFilter = it > 0
    shouldFilter
}

ints.filter {
    val shouldFilter = it > 0
    return@filter shouldFilter
}
```
#### Anonymous functions
Hàm ẩn danh rất giống với hàm bình thường ngoại trừ tên hàm được loại bỏ khỏi khai báo. Phần thân hàm có thể là biểu thức hoặc một khối code.

Ví dụ:
```
    fun(a: Int, b: Int) : Int = a * b
hoặc
    fun(a: Int, b: Int): Int {
        val mul = a * b
        return mul
    }
```
> 1. Kiểu trả về và các tham số cũng được chỉ rõ giống như trong hàm bình thường nhưng ta có thể lược bỏ kiểu tham số nếu chúng có thể được suy ra từ ngữ cảnh.
> 2. Kiểu trả về của hàm ẩn danh có thể được suy luận tự động từ hàm nếu hàm là single-expression, nhưng phải chỉ rõ(hoặc được cho là Unit) với hàm ẩn danh với block body.

#### Functions literals with receiver
Kotlin cho phép bạn tạo các kiểu hàm với receiver, ví dụ như A.(B) -> C, và có thể khởi tạo chúng bằng một dạng đặc biệt của function literals – function literals với receiver.

Ví dụ:
Trong ví dụ dưới đây, sum là một function literal với receiver, nơi plus được gọi trên đối tượng receiver:
```java
val sum: Int.(Int) -> Int = { other -> plus(other) }
```
Trong ví dụ này, hàm `sum` có kiểu `Int.(Int) -> Int`, nghĩa là nó nhận một `Int` làm receiver và một `Int` khác làm tham số, trả về một kiểu `Int`.

Có thể gọi hàm như sau:
```java
fun main() {
    val result = 3.sum(4)
    println(result) // 7
}
```
Biểu thức lambda có thể được sử dụng như function literal với receiver khi kiểu của receiver có thể được suy ra từ ngữ cảnh.
```java
fun String.transform(transformer: String.() -> String): String {
    return this.transformer()
}

fun main() {
    val result = "Kotlin".transform { this.uppercase() }
    println(result) 
}

// KOTLIN
```
### Callback Function
Trong lập trình Kotlin, một hàm callback là một hàm được truyền dưới dạng đối số cho hàm khác. 

Các hàm callback là một thành phần thiết yếu của lập trình bất đồng bộ trong Kotlin. Các hàm callback cũng thường được sử dụng để xử lý sự kiện.

Trong Kotlin, các hàm callback được định nghĩa như các biểu thức lambda.
```java
{ argumentList -> codeBody }
```
Các hàm callback thường được sử dụng trong lập trình Kotlin để xử lý các tác vụ và sự kiện bất đồng bộ.

Ví dụ:
```java
fun loadDataFromServer(callback: (List<String>) -> Unit) {
    // Giả lập việc tải dữ liệu từ một máy chủ từ xa
    Thread.sleep(5000)
    
    // Trả về một số dữ liệu giả
    val data = listOf("John", "Jane", "Bob", "Alice")
    
    // Gọi hàm callback với dữ liệu đã tải
    callback(data)
}
fun main() {
    println("Loading data...")
    loadDataFromServer { data ->
    println("Loaded data: $data")
}
//Loading data...
//Loaded data: [John, Jane, Bob, Alice]
```
Khi chúng ta chạy chương trình này, chúng ta thấy rằng thông báo "Loading data..." được in ra trước, sau đó là thông báo "Loaded data: [John, Jane, Bob, Alice]" sau năm giây.

#### Ưu, nhược điểm của callback
- Ưu điểm:
  - Đơn giản và dễ hiểu: Callbacks thường rất đơn giản và dễ hiểu, đặc biệt đối với các lập trình viên mới.
  - Trực quan cho các thao tác bất đồng bộ: Chúng giúp bạn xử lý các thao tác bất đồng bộ một cách rõ ràng và dễ theo dõi.
- Nhược điểm:
  - Callback Hell (Địa ngục callback): Khi có quá nhiều callback lồng nhau, mã nguồn của bạn có thể trở nên rất phức tạp và khó đọc. Đây thường được gọi là "callback hell".
  