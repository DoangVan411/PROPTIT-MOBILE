# KOTLIN CLASS
## Visibility Modifiers
Có 4 visibility modifiers trong kotlin: private, protected, internal và public

Visibility modifiers mặc định là public

| Visibility Modifier | Phạm vi truy cập                                    |
|---------------------|-----------------------------------------------------|
| private             | Bên trong file/class chứa khai báo                  |
| protected           | Bên trong file/class chứa khai báo và các class con |
| internal            | Mọi nơi trong cùng một module                       |
| public              | Mọi nơi                                             |

## Constructor
Một lớp trong Kotlin có một constructor chính (primary constructor) và có thể có 1 hoặc nhiều constructor phụ (secondary constructors). Constructor chính là một hần của tiêu đề lớp: nó đứng sau tên lớp

Ví dụ:
```java
class Person constructor (firstName: String){
    
}
```
```java
class Person(val firstName: String, val lastName: String, var age: Int){
    
}
```
Nếu constructor chính không có bất kỳ annotations hoặc định nghĩa quyền truy cập (public, private, protected, internal), từ khóa constructor có thể được bỏ qua.

Nếu constructor có các annotations hoặc visibility modifiers, cần phải sử dụng từ khóa `constructor` đứng sau modifiers

Ví dụ:
```java
class Customer public @Inject constructor(name: String) { /*...*/ }
```
### Các hàm khởi tạo phụ (Secondary constructors)
Một hàm khởi tạo phụ được khai báo bằng từ khóa `constructor`

Trong Kotlin, khi một lớp có một hàm tạo chính (primary constructor), mỗi hàm tạo phụ (secondary constructor) phải ủy quyền cho hàm tạo chính. Điều này đảm bảo rằng hàm tạo chính luôn được gọi, duy trì tính toàn vẹn và nhất quán của việc khởi tạo đối tượng. Việc ủy quyền cho một hàm tạo khác của cùng lớp được thực hiện bằng cách sử dụng từ khóa `this`.

Ví dụ:
```java
class Person(val name: String, val age: Int) {

    // Hàm tạo phụ ủy quyền cho hàm tạo chính
    constructor(name: String) : this(name, 0) {
        println("Hàm tạo phụ với tham số name được gọi")
    }

    // Một hàm tạo phụ khác ủy quyền cho hàm tạo chính
    constructor() : this("Không rõ", 0) {
        println("Hàm tạo phụ không có tham số được gọi")
    }

    init {
        println("Hàm tạo chính được gọi với name: $name và age: $age")
    }
}

fun main() {
    val person1 = Person("Alice", 25)
    val person2 = Person("Bob")
    val person3 = Person()
}
```
Kết quả:
```java
Hàm tạo chính được gọi với name: Alice và age: 25
Hàm tạo chính được gọi với name: Bob và age: 0
Hàm tạo phụ với tham số name được gọi
Hàm tạo chính được gọi với name: Không rõ và age: 0
Hàm tạo phụ không có tham số được gọi
```
## Từ khóa `init`
Các constructor chính không thể chứa bất kỳ đoạn mã nào. Nếu muốn chạy code trong khi tạo đối tượng, cần sử dụng các khối khởi tạo bên trong nội dung lớp.

Các khối khởi tạo được khai báo bằng từ khóa `init` và theo sau là `{}`.

Ví dụ:
```java
class Person (name: String) {

    init{
        println("Hello $name!")
    }

}

fun main(args: Array<String>) {
    Person("Vân")
}
```
Kết quả:
```java
Hello Vân!
```
### Khởi tạo đối tượng của class
Kotlin không có từ khóa `new` khi khởi tạo một đối tượng

Ví dụ: 
```java
val person1 = Person("Alice", 25)
val person2 = Person("Bob")
val person3 = Person()
```
## Class Members
Class trong Kotlin có thể bao gồm
- Hàm khởi tạo và khối khởi tạo
- Các hàm
- Properties
- Nested and Inner Classes
- Object Declarations

### Hàm
Cú pháp khai báo hàm:
```java
fun <tên hàm>(<tên tham số>: <kiểu dữ liệu>, <tên tham số>: <kiểu dữ liệu>,...){
    //code    
}
```
Theo mặc định, nếu bạn không chỉ định loại dữ liệu trả về, thì loại dữ liệu trả về mặc định là `Unit`. `Unit` có nghĩa là hàm không trả về một giá trị (tương đương với void trong Java).
```java
class Person(val name: String, val age: Int) {
    fun hello(): Unit{
        println("hello $name")
    }
}
```
Nếu muốn hàm trả về một giá trị, ta cần thêm kiểu dữ liệu vào dòng khai báo hàm.
```java
class Person(val name: String, val age: Int) {
    fun hello(): String{
        val chao = "Hello $name"
        return chao
    }
}

fun main() {
    val person = Person("Van", 10)
    println(person.hello())
}
```
Kết quả:
```java
Hello Van
```

#### NAMED ARGUMENTS

Có thể chỉ định tên cho một hoặc nhiều tham số khi gọi hàm. Những tham số này được gọi là tham số được đặt tên (named arguments).

Khi bạn sử dụng các đối số có tên trong lời gọi hàm, bạn có thể tự do thay đổi thứ tự mà chúng được liệt kê. Nếu bạn muốn sử dụng giá trị mặc định của chúng, bạn chỉ cần bỏ qua các đối số này.

Ví dụ:
```java
class Person() {
    fun hello(name: String, age: Int = 10): String{ //tham số age được gọi là tham số mặc định (default argument) - được gán giá trị mặc định
        val chao = "Hello $name $age tuoi"
        return chao
    }
}

    fun main() {
        val person = Person()
        println(person.hello(age = 8, name = "Van"))
        println(person.hello("Van"))
    }
```
Kết quả:
```java
Hello Van 8 tuoi
Hello Van 10 tuoi
```

### Single-expression functions
Khi thân hàm chỉ bao gồm một biểu thức duy nhất, dấu ngoặc nhọn có thể được bỏ qua và thân hàm được xác định sau dấu `=`.

Ví dụ:
```java
    fun double(x: Int): Int = x * 2
hoặc:
    fun double(x: Int) = x * 2
```

### Variable number of arguments (varargs) - Truyền các tham số với số lượng biến đổi
Trong Kotlin, bạn có thể truyền một số lượng biến đổi các đối số vào một hàm bằng cách khai báo hàm với một tham số vararg. Một tham số vararg kiểu T được biểu diễn nội bộ như một mảng kiểu T (Array<T>) bên trong thân hàm.

Ví dụ:
```java
fun main (args: Array<String>) {
    someMethod ( "as", "you", "know", "this", "works")
}

fun someMethod (vararg a: String) {
    for (str in a) {
        println(str)
    }
}
```
Nếu bạn đã có sẵn một mảng các giá trị, bạn có thể truyền nó vào hàm sử dụng toán tử `*`.
Ví dụ:
```java
fun main(args: Array<String>) {
val list = arrayOf ("as", "you", "know", "this", "works")
    someMethod (*list)
}

fun someMethod (vararg a: String) {
    for (str in a) {
        println(str)
    }
}
```
Nếu trong hàm có nhiều tham số khác ngoài vararg, tham số vararg thường được đặt ở cuối.

Nếu không, nếu bạn muốn truyền giá trị cho các tham số đứng sau vararg, bạn phải đặt tên cho tham số (named arguments)

Ví dụ:
```java
fun main (args: Array<String>) {
    someMethod ("3", "as", "you", "know", "this", "works", c = "what")
}

fun someMethod (b: String, vararg a: String, c: String) {
    println ("b: " + b)
    for (a_ in a){
        println(a_)
    }
    println ("c: " + c)
}
```
Kết quả:
```java
b: 3
as
you
know
this
works
c: what
```
### Infix function
Các hàm được đánh dấu bằng từ khóa `infix` cũng có thể được gọi bằng cách sử dụng infix notation (bỏ qua dấu chấm và dấu ngoặc đơn khi gọi). Các hàm infix phải đáp ứng các yêu cầu sau:

- Chúng phải là các hàm thành viên hoặc hàm mở rộng.
- Chúng phải có một tham số duy nhất.
- Tham số không được chấp nhận số lượng đối số biến đổi và không có giá trị mặc định.

Ví dụ:
```java
class Person(val name: String) {
    infix fun learn(str: String){
        println("$name learn $str")
    }
}

fun main() {
    val person = Person("Van")
    person.learn("English")
    person learn "English"
}
```
Kết quả:
```java
Van learn English
Van learn English
```

### Hàm cục bộ (Local function)
Local function là hàm được khai báo bên trong một hàm khác. Hàm cục bộ có thể truy cập các biến cục bộ của các hàm ngoài (closure).

Ví dụ:
```java
fun printTime(time: Int? = null) {
    //    printThree()
    fun printThree() { // 1
        println(3)
        if (time != null) println(time)
        println("===")
    }
    printThree() // 2
}
fun main(args: Array<String>) {
    printTime()
    printTime(4)
}
```
Kết quả:
```java
3
===
3
4
===
```

## Object

### Khai báo Object
Singleton là một mẫu hướng đối tượng trong đó một class chỉ có thể có một instance (đối tượng).

Kotlin cung cấp một cách dễ dàng để tạo các singleton bằng cách sử dụng tính năng khai báo đối tượng. Để làm được điều đó, chúng ta sử dụng từ khóa object.

```java
object SingletonExample {
    ... .. ...
    // body of class
    ... .. ...
}
```
Ví dụ:
```java
object MyCat{
    val age: Int = 5
    val name: String = ""
    fun meow(){
        println("meow meow")
    }
}
fun main(args: Array<String>) {
    MyCat.meow()
    println(MyCat.age)
}
```
Kết quả:
```java
meow meow
5
```
Một khai báo Object có thể chứa các thuộc tính, hàm,... Tuy nhiên, chúng không được chứa hàm tạo.

### Companion objects
Có thể khai báo một Object bên trong một class với từ khóa `companion`. Các thành phần của companion object có thể được gọi đơn giản bằng cách sử dụng tên lớp:
```java
class Subject{
    companion object Language{
        fun chaotay(){
            println("hello")
        }
        fun chaota(){
            println("xin chao")
        }
    }
}
fun main(args: Array<String>) {
    Subject.chaotay()
    Subject.chaota()
}
```
Kết quả:
```java
hello
xin chao
```
## Special class
### Data class
Data classes trong Kotlin được sử dụng để lưu dữ liệu.

Với mỗi data class, compiler tự động tạo những phương thức thành viên bổ sung cho phép bạn in ra một đối tượng có thể đọc được, so sánh, sao chép các đối tượng,...

Data classes được khai báo bằng từ khóa `data`:
```java
data class Person(val firstName: String, val lastName: String, val age: Int)
```
Trình biên dịch tự động tạo ra các thành viên sau từ tất cả các thuộc tính được khai báo trong constructor chính:

- .equals() và .hashCode(): So sánh và tạo mã băm cho đối tượng.
- .toString(): Trả về đối tượng dưới dạng chuỗi có thể đọc được
- .componentN(): Các hàm tương ứng với thứ tự khai báo các thuộc tính.
- .copy(): Tạo ra một bản sao của đối tượng

#### Các quy tắc để tạo data classes
Có một số hạn chế cơ bản để duy trì tính nhất quán / hành vi của mã được tạo ra:

- Constructor chính phải được tạo ra với ít nhất 1 tham số
- Tham số của constructor nên được đánh dấu là val hoặc var
- Data class không thể là abstract, open, sealed hoặc inner
- Data class không nên kế thừa từ lớp khác (nhưng vẫn có thể implement interfaces)

#### Tạo ra một đối tượng của lớp Person:
```java
val person1 = Person("Van", "Doan", 10)
```
#### In ra nội dung của đối tượng
```java
println(person1.toString())
```
Kết quả:
```java
Person(firstName=Van, lastName=Doan, age=10)
```
#### Sao chép đối tượng
Tạo một đối tượng person2 sao chép từ person1, đổi age = 8
```java
val person2 = person1.copy(age = 8)
```

#### Visibility Modifiers
CÓ thể điều khiển visibility modifiers của getters/setters tạo ra bằng cách cung cấp chúng trong constructor:
```java
data class Person(private val firstName: String, val lastName: String, private val age: Int)
```

#### Destructuring Declarations
Hàm componentN() sẽ ánh xạ đến thuộc tính thứ N của đối tượng.
```java
println(person1.component1())
println(person1.component2())
println(person1.component3())
```
Kết quả:
```java
Van
Doan
10
```
Một khai báo hủy cấu trúc có dạng:
```java
val (ten, tuoi, ho) = person1
```
#### Một số data classes được xây dựng sẵn
Kotlin cũng cung cấp các data classes Pair và Triple cho các xử lý chung.
```java
val pair: Pair<Int, String> = Pair(10, "Ten")
val triple: Triple<Int, String, Boolean> = Triple(1, "One", true)
```

### Enum class
Enum class là một kiểu dữ liệu (data type) bao gồm một tập các giá trị được đặt tên.

Mỗi hằng số enum là một object, được phân cách nhau bởi dấu phẩy.

Mọi hằng số enum đều có các thuộc tính để lấy tên và vị trí của nó trong khai báo lớp enum, đó là:
```java
val name: String
val ordinal: Int
```
Ví dụ:
```java	
enum class Direction {
   NORTH, SOUTH, WEST, EAST
}
```
Kiểu enum cũng có thể có constuctor(hàm khởi tạo) của nó và có thể có dữ liệu tùy chỉnh được liên kết với mỗi hằng số enum:
```java
Ex: enum class Color(val r: Int,val g: Int, val b:Int){
   RED(255, 0, 0),
   ORANGE(255, 165, 0),
   BLUE(0, 0, 255),
}
fun main(){
   val color = Color.BLUE
   println("value of color BLUE: r = ${color.r}, g = ${color.g}, b = ${color.b}")
}
```
Kết quả:
```java
value of color BLUE: r = 0, g = 0, b = 255
```

### Sealed class
Một lớp niêm phong định nghĩa một tập hợp các lớp con bên trong nó. Lớp niêm phong được sử dụng khi biết trước rằng một kiểu sẽ tuân theo một trong các kiểu lớp con.

Để khai báo sealed class sử dụng từ khóa `sealed`. Một sealed class được ngầm định là `abstract` do đó không thể được khởi tạo.

```java
sealed class Demo {
    class A : Demo() {
        fun display() {
            println("Subclass A of Sealed class Demo")
        }
    }
    class B : Demo() {
        fun display() {
            println("Subclass B of Sealed class Demo")
        }
    }
}

fun main(args: Array<String>) {
    val obj = Demo.B()
    obj.display()
    val obj1 = Demo.A()
    obj1.display()
}
```
Kết quả:
```java
Subclass B of Sealed class Demo
Subclass A of Sealed class Demo
```
> Lưu ý: Tất cả các lớp con của lớp niêm phong phải được định nghĩa trong cùng một tệp Kotlin. Tuy nhiên, không cần thiết phải định nghĩa chúng bên trong lớp niêm phong, chúng có thể được định nghĩa trong bất kỳ phạm vi nào mà lớp niêm phong có thể nhìn thấy.

Sử dụng sealed class với biểu thức `when`:
```java

sealed class Shape{
    class Circle(var radius: Float): Shape()
    class Square(var length: Int): Shape()
    object Rectangle: Shape()
    {
        var length: Int = 0
        var breadth : Int = 0
    }
}

fun eval(e: Shape) =
        when (e) {
            is Shape.Circle -> println("Circle area is ${3.14*e.radius*e.radius}")
            is Shape.Square -> println("Square area is ${e.length*e.length}")
            Shape.Rectangle -> println("Rectangle area is ${Shape.Rectangle.length*Shape.Rectangle.breadth}")
        }
```
Trong ví dụ trên, không cần câu lệnh else vì trình biên dịch biết tất cả các trường hợp có thể của lớp Shape.