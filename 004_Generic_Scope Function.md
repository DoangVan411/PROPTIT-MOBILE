# GENERIC & SCOPE FUNCTION

<!-- TOC -->
* [GENERIC & SCOPE FUNCTION](#generic--scope-function)
  * [Generic](#generic)
    * [Variance (Biến đổi kiểu)](#variance--bin-i-kiu-)
      * [Từ khóa `out`](#t-kha-out)
      * [Từ khóa `in`](#t-kha-in)
      * [Type projections](#type-projections)
      * [Star projections](#star-projections)
    * [Generic Functions](#generic-functions)
    * [Generic constraints](#generic-constraints)
  * [Extension Functions](#extension-functions)
    * [Extensions được giải quyết tĩnh trong Kotlin](#extensions-c-gii-quyt-tnh-trong-kotlin)
  * [Scope Function](#scope-function)
    * [Lựa chọn hàm](#la-chn-hm)
    * [Sử dụng `this` và `it`](#s-dng-this-v-it)
      * [`this`](#this)
      * [`it`](#it)
    * [Giá trị trả về](#gi-tr-tr-v)
      * [Đối tượng ngữ cảnh](#i-tng-ng-cnh)
      * [Lambda return](#lambda-return)
    * [Usecases](#usecases)
      * [1. `let`](#1-let)
      * [2. `apply`](#2-apply)
      * [3. `with`](#3-with)
      * [4. `run`](#4-run)
      * [5. `also`](#5-also)
<!-- TOC -->

## Generic
Generics là một tính năng mà cho phép chúng ta có thể định nghĩa và truy cập các classes, methods, properties bằng cách sử dụng các kiểu dữ liệu khác nhau mà vẫn sẽ hoạt động giống nhau.

Kiểu generic (hay kiểu tổng quát) là một khái niệm trong lập trình, cho phép bạn định nghĩa các lớp, phương thức, hoặc thuộc tính có thể làm việc với bất kỳ kiểu dữ liệu nào mà vẫn đảm bảo tính an toàn kiểu (type safety) trong quá trình biên dịch.

Một trong những ví dụ về generics là các collection, ví dụ MutableList. Có thể khai báo các kiểu dữ liệu khác nhau cho MutableList nhưng cách hoạt động của MutableList vẫn không thay đổi.
```java
val list1: MutableList<Int> = mutableListOf(1, 2, 3, 4)
val list2: MutableList<String> = mutableListOf("Mot", "Hai", "Ba")
```

Để khai báo một lớp generic sử dụng `<>`:
```java
class MyClass<T>(text: T) {
    val name = text
}
```
Khi tạo một đối tượng của lớp generic, ta cần cung cấp kiểu dữ liệu của các tham số.
```java
val myClass = MyClass<String>("MyClass")
```
Nếu các tham số có thể được suy ra từ khác đối số của hàm khởi tạo, ta có thể bỏ qua các tham số kiểu
```java
val myClass = MyClass("MyClass")
```
### Variance (Biến đổi kiểu)
>Bất biến (Invariance)
Bất biến là tính chất mà qua đó một hàm hoặc lớp generic đã được định nghĩa cho một kiểu dữ liệu cụ thể không thể chấp nhận hoặc trả về một kiểu dữ liệu khác. Ví dụ:
>```
>class Box<T>(val value: T)
>```
>Trong trường hợp này, Box<String'> và Box<Any'> không thể thay thế lẫn nhau.

Kotlin tạo ra các mảng bất biến theo mặc định. Mở rộng ra, kiểu generic là bất biến trong Kotlin. 
Tính bất biến là thuộc tính mà theo đó một hàm/lớp chung tiêu chuẩn đã được xác định cho một kiểu dữ liệu cụ thể, không thể chấp nhận hoặc trả về một kiểu dữ liệu khác.

Điều này được quản lý bằng các từ khóa `in` và `out` trong Kotlin.

Variance có 2 loại:
1. Variance trong khai báo(sử dụng `in` và `out`)
2. Variance trong sử dụng: Type projection

#### Từ khóa `out`
Trong Kotlin, chúng ta có thể sử dụng từ khóa `out` trên kiểu generic, nghĩa là có thể gán tham chiếu này cho bất kỳ supertype nào của nó.
```java
```java
class Producer<out T> (val value: T){
    fun produce(): T{
        return value
    }
}

fun main(args: Array<String>) {
    val out = Producer("Hello Vietnam!")
    val ref: Producer<Any> = out
}
```
Tạo một class tên Producer có thể tạo ra giá trị kiểu T. Ta có thể gán một đối tượng của Producer cho một supertype của nó.

**Chú ý:** Giá trị đầu ra chỉ có thể tạo bởi lớp được cho, không thể được tiêu thụ (sử dụng, truy cập, thay đổi). 
```java
class Producer<out T> (val value: T){
    fun produce(): T{
        return value
    }
    fun consume(par: T){
        println(par)
    }
}
```
Compiler sẽ báo lỗi vì Producer chỉ có thể tạo ra các hàm trả về kiểu T, không thể tạo ra các hàm nhận tham số có kiểu dữ liệu T.

Từ khóa `out` được sử dụng để thực hiện **Convariance (Hiệp phương sai)**

`Convariance` cho phép thay thế các kiểu con, nhưng không cho phép thay thế các supertype, nghĩa là hàm/lớp generic có thể chấp nhận các kiểu con của kiểu dữ liệu mà nó đã được định nghĩa. 

Ví dụ, một lớp generic được định nghĩa là `Number` có thể chấp nhận `Int`, nhưng một lớp generic được định nghĩa là `Int` không thể chấp nhận `Number`.
```java
    fun main(args: Array<String>) {
        val x: MyClass<Any> = MyClass<Int>()	 // Error: Type mismatch
        val y: MyClass<out Any> = MyClass<String>() // Works since String is a subtype of Any
        val z: MyClass<out String> = MyClass<Any>() // Error since Any is a supertype of String
    }
    class MyClass<T>
    
hoặc
        
    fun main(args: Array<String>) {
        val y: MyClass<Any> = MyClass<String>() // Compiles without error
    }
    class MyClass<out T>
```

#### Từ khóa `in`
Nếu ta muốn gán một đối tượng cho tham chiếu kiểu con của nó thì có thể sử dụng từ khóa `in`. Từ khóa `in` chỉ có thể được sử dụng trên loại tham số được tiêu thụ, không được tạo ra (các hàm trong lớp sử dụng từ khóa `in` chỉ có thể nhận tham số kiểu T, không thể có hàm trả về kiểu T).
```java
class InClass<in T> {
    fun toString(value: T): String {
        return value.toString()
    }
}
```
Ta có thể gán một tham chiếu kiểu `Number` cho một tham chiếu kiểu con của nó - `Int`:
```java
val inClassObject: InClass<Number> = InClass()
val ref: InClass<Int> = inClassObject
```
Sử dụng từ khóa `in` để thực hiện **Contracovariance (Chống hiệp phương sai)**

Contravariance được sử dụng để thay thế giá trị của supertype trong các kiểu con, nghĩa là hàm/lớp generic có thể chấp nhận các supertype của kiểu dữ liệu mà nó đã được định nghĩa.

Ví dụ, một lớp generic được định nghĩa cho `Number` không thể chấp nhận `Int`, nhưng một lớp generic được định nghĩa cho `Int` có thể chấp nhận `Number`.

```java
fun main(args: Array<String>) {
    var a: Container<Dog> = Container<Animal>() // Biên dịch không lỗi
    var b: Container<Animal> = Container<Dog>() // Gây ra lỗi biên dịch
}

open class Animal
class Dog : Animal()

class Container<in T>
```
Trong ví dụ trên, `Container<Dog>` có thể được gán cho `Container<Animal>`, nhưng ngược lại thì không thể. Điều này bởi vì `in` cho phép tiêu thụ các giá trị của kiểu `Dog` thông qua `Container<Animal>`.

#### Type projections
Có thể sao chép tất cả các phần tử của một mảng thuộc kiểu nào đó vào một mảng thuộc kiểu `Any`, nhưng để cho phép compiler compile code ta cần ghi chú tham số đầu vào với từ khóa `out`. Điều này làm cho compiler suy ra được tham số đầu vào có thể là bất cứ kiểu nào thuộc kiểu con của `Any`
```java
fun copy(from: Array<out Any>, to: Array<Any>) {
	assert(from.size == to.size)
	// copying (from) array to (to) array
	for (i in from.indices)
		to[i] = from[i]
	// printing elements of array in which copied
	for (i in to.indices) {
	    println(to[i])
	}
}
fun main(args :Array<String>) {
	val ints: Array<Int> = arrayOf(1, 2, 3)
	val any :Array<Any> = Array<Any>(3) { "" }
	copy(ints, any)
}
```
#### Star projections
Khi ta không biết kiểu dữ liệu cụ thể và ta chỉ muốn in tất cả các phần tử của một mảng thì ta sử dụng phép chiếu sao(*).
```java
// star projection in array
fun printArray(array: Array<*>) {
	array.forEach { print(it) }
}
fun main(args :Array<String>) {
	val name = arrayOf("Mot","Hai","Ba")
	printArray(name)
}
```
### Generic Functions
Các `function` (hàm) cũng có thể có `type parameters` `<T>` → vị trí trước tên của hàm.

Để gọi `generic function` , ta sử dụng `type argument` sau tên hàm. Các `type argument` cũng có thể được bỏ qua nếu được suy ra

```java
fun main() {
    val fiTest = toString(567)
    println(fiTest) // 567

    val seTest = toString<Double>(6.51)
    println(seTest) // 6.51
}

fun <T> toString(c: T): String {
    return c.toString()
}
```
### Generic constraints
`Generic constraints` :  Cho phép xác định các hạn chế về các `types` có thể sử dụng thể thay thế cho kiểu dữ liệu `generic` → Giúp ta đảm bảo rằng các phương thức và thuộc tính trong kiểu generic sẽ chỉ hoạt động với các kiểu thỏa mãn ràng buộc đã chỉ định.

Loại ràng buộc phổ biến nhất là: `upper bound`

`Type` được chỉ định sau dấu `:` được gọi là giới hạn trên `upper bound`
```java
fun <T : Comparable<T>> sort(list: List<T>) {  ... }

sort(listOf(1, 2, 3)) // OK. Int is a subtype of Comparable<Int>
sort(listOf(HashMap<Int, String>())) // Error: HashMap<Int, String> is not a subtype of Comparable<HashMap<Int, String>>
```
Giới hạn trên `upper bound` mặc định nếu không được chỉ định là `Any?`

Trong `<>` chỉ có thể chỉ định 1 `upper bound`  → Nếu cần nhiều hơn, sử dụng `where`
```java
fun <T> copyWhenGreater(list: List<T>, threshold: T): List<String>
    where T : CharSequence,
          T : Comparable<T> {
    return list.filter { it > threshold }.map { it.toString() }
}
```
`Type` được truyền phải đáp ứng đồng thời tất cả các điều kiện của mệnh đề `where`.
## Extension Functions
Kotlin cung cấp khả năng mở rộng một class hoặc interface với chức năng mới mà không cần phải kế thừa từ lớp hoặc sử dụng các design pattern như Decorator. Điều này được thực hiện thông qua các khai báo đặc biệt gọi là `extensions`.

Extension functions cho phép bạn thêm các hàm mới vào một lớp hiện có mà không cần sửa đổi mã nguồn của lớp đó.

Để khai báo một extension function, thêm một tiền tố là `receiver type`, đề cập đến kiểu mà bạn đang muốn mở rộng.

Ví dụ: Thêm hàm swap vào MutableList
```java
fun MutableList<Int>.swap(index1: Int, index2: Int){
    val tmp = this[index1]
    this[index1] = this[index2]
    this[index2] = tmp
}
```
Từ khóa `this` bên trong một extension function tương ứng với đối tượng nhận (đối tượng được truyền trước dấu chấm). 

Bây giờ, hàm swap có thể sử dụng cho bất cứ `MutableList<Int>` nào.

Hàm này có ý nghĩa cho bất kỳ `MutableList<T>` nào, và bạn có thể làm cho nó thành hàm generic:
```java
fun <T> MutableList<T>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' tương ứng với danh sách
    this[index1] = this[index2]
    this[index2] = tmp
}
```
Bạn cần khai báo tham số kiểu generic trước tên hàm để nó có sẵn trong biểu thức kiểu nhận.
### Extensions được giải quyết tĩnh trong Kotlin
Extensions trong Kotlin không thực sự sửa đổi các lớp mà chúng mở rộng. Bằng cách định nghĩa một extension, bạn không chèn các thành viên mới vào một lớp, mà chỉ làm cho các hàm mới có thể gọi được bằng cú pháp dấu chấm trên các biến của kiểu này.

Extension functions được phân giải tĩnh. Điều này có nghĩa là hàm extension nào được gọi đã được biết tại thời điểm biên dịch dựa trên kiểu của đối tượng nhận. Ví dụ:
```java
open class Shape
class Rectangle: Shape()

fun Shape.getName() = "Shape"
fun Rectangle.getName() = "Rectangle"

fun printClassName(s: Shape) {
    println(s.getName())
}

fun main(args: Array<String>) {
    printClassName(Rectangle())
}

//Shape
```

Ví dụ trên in ra "Shape", vì hàm extension được gọi phụ thuộc vào kiểu khai báo của tham số s, là lớp Shape.

Nếu một lớp có một hàm thành viên và một hàm extension được định nghĩa có cùng kiểu nhận, cùng tên và áp dụng cho các đối số được cho, hàm thành viên luôn thắng. Ví dụ:
```
class Student{
    fun getScore(){
        println("I got an 8 on my Maths test")
    }
}

fun Student.getScore(){
    println("I got an 9.8 on my English test")
}

fun main(args: Array<String>) {
    val student = Student()
    student.getScore()
}

//I got an 8 on my Maths test
```
## Scope Function
Thư viện chuẩn của Kotlin chứa nhiều hàm có mục đích duy nhất là thực thi một khối mã trong ngữ cảnh của một đối tượng. Khi bạn gọi các hàm này trên một đối tượng với một biểu thức lambda, nó tạo ra một phạm vi tạm thời. Trong phạm vi này, bạn có thể truy cập đối tượng mà không cần sử dụng tên của nó. Các hàm này được gọi là scope functions. Có năm hàm như vậy: `let`, `run`, `with`, `apply`, và `also`.

Về cơ bản, các hàm này đều thực hiện cùng một hành động: thực thi một khối mã trên một đối tượng. Điểm khác biệt là **cách đề cập đến một context object** (tức là sử dụng từ khóa `this` hoặc `it`)
**giá trị trả về** (tức là trả về `context object` hoặc `lambda result`)

Ví dụ:
```java
class Student() {
    lateinit var name: String
    lateinit var id: String
}

fun main() {
    // using scope function
    val student = Student().apply {
        // don't need to use object
        // name to refer members
        name = "Van"
        id = "B22DCCN890"
    }
    println(student.name)
}
    
//Van
```
### Lựa chọn hàm
Bảng dưới tóm tắt sự khác biệt giữa các hàm:

| Function | Object reference | Return value   | Is extension function                        |
|----------|------------------|----------------|----------------------------------------------|
| let      | it               | Lambda result  | Yes                                          |
| run      | this             | Lambda result  | Yes                                          |
| run      | -                | Lamda result   | No: called without the context object        |
| with     | this             | Lambda result  | No: takes the context object as an argument. |
| apply    | this             | Context object | Yes                                          |
| also     | it               | Context object | Yes                                          |

### Sử dụng `this` và `it`
Trong các hàm scope function của Kotlin, context object có thể được tham chiếu bởi hai cách: `this` hoặc `it`. Mỗi cách cung cấp những khả năng tương tự, nhưng có các ưu và nhược điểm khác nhau tùy theo trường hợp sử dụng.

#### `this`
Các hàm `run`, `with`, và `apply` tham chiếu context object như là một `lambda receiver` thông qua từ khóa `this`. Trong các lambda của chúng, đối tượng có thể truy cập như cách nó có trong các hàm của lớp thông thường.
```java
val adam = Person("Adam").apply { 
    age = 20                       // tương đương với this.age = 20
    city = "London"
}
println(adam)
```

#### `it`
Ngược lại, các hàm `let` và `also` tham chiếu đến context object như một `lambda argument`. Nếu tên tham số không được chỉ định, đối tượng được truy cập bằng tên mặc định là `it`. `it` ngắn hơn `this` và các biểu thức với `it` thường dễ đọc hơn.

Tuy nhiên, khi gọi các hàm hoặc thuộc tính của đối tượng, bạn không có đối tượng có sẵn ngầm định như `this`. Do đó, việc truy cập context object thông qua `it` tốt hơn khi đối tượng chủ yếu được sử dụng như một tham số trong các lời gọi hàm. `it` cũng tốt hơn nếu bạn sử dụng nhiều biến trong khối mã.

```java
fun getRandomInt(): Int {
    return Random.nextInt(100).also {
        println("getRandomInt() generated value $it")
    }
}

fun main(args: Array<String>) {
    val i = getRandomInt()
    println(i)
}

//getRandomInt() generated value 73
//73
```
### Giá trị trả về
Các hàm scope function khác nhau bởi kết quả mà chúng trả về:

`apply` và `also` trả về context object.

`let`, `run`, và `with` trả về kết quả lambda.

Bạn nên xem xét cẩn thận giá trị trả về bạn muốn dựa trên những gì bạn muốn làm tiếp theo trong mã của mình. Điều này giúp bạn chọn hàm scope function tốt nhất để sử dụng.

#### Đối tượng ngữ cảnh
Giá trị trả về của `apply` và `also` là chính context object. Do đó, chúng có thể được bao gồm vào các chuỗi gọi hàm như các bước phụ: bạn có thể tiếp tục chuỗi gọi hàm trên cùng một đối tượng, từng cái một.
```java
val numberList = mutableListOf<Double>()
numberList.also { println("Populating the list") }
    .apply {
        add(2.71)
        add(3.14)
        add(1.0)
    }
    .also { println("Sorting the list") }
    .sort()
```
Chúng cũng có thể được sử dụng trong các câu lệnh trả về của các hàm trả về context object.

#### Lambda return
`let`, `run`, và `with` trả về kết quả của lambda. Vì vậy, bạn có thể sử dụng chúng khi gán kết quả cho một biến, chuỗi các thao tác trên kết quả, v.v.
```java
val numbers = mutableListOf("one", "two", "three")
val countEndsWithE = numbers.run {
    add("four")
    add("five")
    count { it.endsWith("e") }
}
fun main(args: Array<String>) {
    println("There are $countEndsWithE elements that end with e.")
}

// There are 3 elements that end with e.
```
Ngoài ra, bạn có thể bỏ qua giá trị trả về và sử dụng một hàm scope function để tạo ra một phạm vi tạm thời cho các biến cục bộ.
```java
fun main(args: Array<String>) {
    val numbers = mutableListOf("one", "two", "three")
    with(numbers) {
        val firstItem = first()
        val lastItem = last()
        println("First item: $firstItem, last item: $lastItem")
    }

}

//First item: one, last item: three
```
### Usecases
#### 1. `let`
> Context object  :   it
> 
> Return value    :   lambda result

Hàm `let` thường được sử dụng để cung cấp các lệnh gọi null safety. Sử dụng toán tử null safety (?) với let cho null safety. Nó chỉ triển khai khối code với giá trị không null.
```java
fun main() { 
	// nullable variable a with value as null 
	var a: Int? = null
	// using let function 
	a?.let { 
		// statement(s) will not execute as a is null 
		print(it) 
	} 
	// re-initializing value of a to 2 
	a = 2
	a?.let { 
		// statement(s) will execute as a is not null 
		print(a) 
	} 
}

//a
```
#### 2. `apply`
> Context object   :    this
> 
> Return value     :    context object

Giống như tên ám chỉ - "Apply this to the object". Hàm `apply` có thể được sử dụng hầu hết để khởi tạo các thành phần của receiver object.
```java
class Student() {
    lateinit var name: String
    lateinit var id: String
}

fun main() {
    // using scope function
    val student = Student().apply {
        // don't need to use object
        // name to refer members
        name = "Van"
        id = "B22DCCN890"
    }
    println(student.name)
}
    
//Van
```
#### 3. `with`
> Context object  :   this
> 
> Return value    :   lambda result

Hàm `with` được sử dụng để gọi hàm trên context object khi không cần sử dụng kết quả trả về. Trong code, `with` có thể được hiểu là **"with this object, do the following"**.
```java
class Student() {
    lateinit var name: String
    lateinit var id: String
}

fun main() {
    // using scope function
    val student = Student().apply {
        // don't need to use object name to refer members
        name = "Van"
        id = "B22DCCN890"
    }
    with(student){
        // similar to println( "MSV: ${this.name}" )
        println("MSV: $id")
    }
}

//MSV: B22DCCN890
```
#### 4. `run`
> Context object   :    this
> 
> Return value     :    lambda result
> 
> Hàm `run` có thể được coi như sự kết hợp giữa hàm `let` và `with`.

Hàm `run` được sử dụng khi đối tượng lambda chứa cả khởi tạo và tính toán giá trị trả về. Sử dụng `run` ta có thể thực hiện các lệnh gọi null safety cũng như các phép tính khác.
```java
data class Person(var name: String, var age: Int)

fun main() {
    // Sử dụng run để khởi tạo và tính toán giá trị trả về
    val result = run {
        val person = Person("Charlie", 40)
        person.age += 5 // Tăng tuổi thêm 5
        "Updated person: ${person.name}, age ${person.age}"
    }

    println(result) 
}

// Updated person: Charlie, age 45
```
#### 5. `also`
> Context object   :    it
> 
> Return value     :    context object

Hàm `also` được sử dụng để biểu diễn các thao tác bổ sung khi ta đã khởi tạo các thành phần của đối tượng.
```java
fun main() { 
    // initialized 
    val list = mutableListOf<Int>(1, 2, 3) 
    
    // later if we want to perform 
    // multiple operations on this list 
    list.also { 
        it.add(4) 
        it.remove(2) 
        // more operations if needed 
    } 
    println(list) 
}

// [1, 3, 4]
```