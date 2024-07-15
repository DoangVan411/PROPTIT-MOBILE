# TÍNH CHẤT CỦA OOP TRONG KOTLIN
## Encapsulation
Đóng gói liên quan đến việc đóng gói dữ liệu và các phương thức hoạt động trên dữ liệu đó trong một class.

Trong Kotlin, đóng gói được thực hiện thông qua các bộ visibility modifiers như private, protected, internal, và public.

| Visibility Modifier | Phạm vi truy cập                                  |
|---------------------|---------------------------------------------------|
| private             | Bên trong file/class chứa khai báo                |
| protected           | Bên trong file/class chứa khai báo và các lớp con |
| internal            | Mọi nơi trong cùng một module                     |
| public              | Mọi nơi                                           |

## Inheritance
Nếu khai báo mặc định, các class trong Kotlin là final và không thể được kế thừa. Nếu muốn một class có thể được kế thửa bởi class khác, ta phải khai báo class cùng với từ khóa `open`
```java
open class Person(name: String)
```
Để khai báo lớp cha, đặt tên lớp cha đằng sau dấu `:` trong tiêu đề lớp:
```java
open class Person(name: String)

class Student(name: String): Person(name)
```
- Nếu lớp dẫn xuất có primary constructor, lớp cơ sở có thể và phải được khởi tạo trong primary constructor đó theo các tham số của nó.
- Nếu lớp dẫn xuất không có hàm tạo chính, thì mỗi hàm tạo phụ phải khởi tạo kiểu cơ sở bằng cách sử dụng từ khóa super hoặc nó phải ủy quyền cho một hàm tạo khác thực hiện điều đó. Lưu ý rằng trong trường hợp này, các hàm tạo phụ khác nhau có thể gọi các hàm tạo khác nhau của kiểu cơ sở.
```java
open class Person(name: String)

class Student : Person{
    constructor(name: String): super(name)
}
```
### Overriding methods
Kotlin yêu cầu modifiers rõ ràng cho các thành viên có thể ghi đè và ghi đè:
```java
open class Shape{
    open fun draw(){}
    fun fill(){}
}

class Circle(): Shape(){
    override fun draw() {}
}
```
Modifier `override` là bắt buộc cho `Circle.draw()`. Nếu thiếu nó, trình biên dịch sẽ báo lỗi.

Nếu một phương thức không có modifier `open` như phương thức `Shape.fill()`, ta không thể khai báo một phương thức với cùng signature trong lớp Circle, dù có hay không có `override`.

Nếu một thành viên được đánh dấu là `override` thì cũng là open, nên có thể được override trong các lớp con. Nếu muốn cấm ghi đè, thêm từ khóa `final`:
```java
open class Rectangle() : Shape() {
    final override fun draw() { /*...*/ }
}
```

### Overriding properties
Cơ chế ghi đè hoạt động trên các thuộc tính theo cách tương tự như trên các phương thức.

Bạn cũng có thể ghi đè một thuộc tính `val` bằng một thuộc tính `var`, nhưng không được phép ngược lại.

Có thể sử dụng từ khóa override như một phần của khai báo thuộc tính trong primary constructor.

### Calling the superclass implementation
Trong một lớp dẫn xuất có thể gọi đến hàm và truy cập thuộc tính của lớp cha bằng cách sử dụng từ khóa `super`
```java
open class Animal{
    open fun eat(){
        println("yum yum")
    }
    val tailColor: String = "black"
}

class Dog: Animal(){
    override fun eat() {
        super.eat()
        println("woof woof")
    }
    val color: String = super.tailColor
}
```


## Polymorphism
Tính đa hình được chia thành 2 loại:
- Đa hình trong compile-time
- Đa hình trong runtime

### Đa hình trong compile-time
Các hàm có thể có tên giống nhau nhưng kiểu trả về phải khác nhau.

Trong compile time, compiler xác định hàm nào sẽ được gọi dựa trên số lượng, kiểu dữ liệu và thứ tự của các tham số truyền vào.

```java
fun main(args: Array<String>) {
    println(add(4, 5))
    println(add(4, 5, 6))
}

fun add(a: Int, b: Int): Int{
    return a + b
}

fun add(a: Int, b: Int, c: Int): Int{
    return a + b + c
}
```
Kết quả:
```java
9
15
```
### Runtime polymorphism
Trong đa hình trong runtime, compiler gọi đến các phương thức đã được ghi đè tại runtime. Điều này được thực hiện bằng cách ghi đè phương thứcnơi mà một lớp con cung cấp một thực thi cụ thể của một phương thức đã được định nghĩa trong lớp cha của nó. 
```java
open class Animal {
    open fun sound() {
        println("The animal makes a sound")
    }
}

class Dog : Animal() {
    override fun sound() {
        println("The dog barks")
    }
}

class Cat : Animal() {
    override fun sound() {
        println("The cat meows")
    }
}

fun main() {
    val myAnimal: Animal = Dog()
    myAnimal.sound() // Output: The dog barks

    val anotherAnimal: Animal = Cat()
    anotherAnimal.sound() // Output: The cat meows
}
```
Kết quả:
```java
The dog barks
The cat meows
```
## Abstract
Tính trừu tượng cho phép bạn định nghĩa các lớp hoặc phương thức mà không cung cấp chi tiết thực hiện cụ thể. 

Trong Kotlin, bạn có thể sử dụng lớp trừu tượng (abstract class) và giao diện (interface) để đạt được tính trừu tượng.

### Abstract class
Từ khóa `abstract` được sử dụng để khai báo abstract class trong Kotlin.

Không thể tạo ra một đối tượng của abstract class. Tuy nhiên, nó có thể được kế thừa bởi các class con.

Các hàm và thuộc tính của lớp trừu tượng là non-abstract trừ khi dùng từ khóa `abstract` trước chúng.

Ví dụ:
```java
abstract class Eniemies{
    val damage: Int = 40

    fun attack(skill: String){
        println("$skill!!!!!!!")
    }

    abstract fun defense()
}
```
Nếu muốn override một thành viên trong abstract class, chúng phải được đánh dấu là `open`.

Một phương thức được đánh dấu là `abstract` phải được override trong lớp con của nó.

> Chú ý: Abstract class luôn là `open`. Không cần khai báo `open` để kế thừa chúng.

### Interface
Một interface có thể chứa các phương thức trừu tượng cũng như các phương thức có nội dung

Một interface được định nghĩa bằng từ khóa `interface`
```java
interface MyInterface {
    fun bar()
    fun foo() {
      // optional body
    }
}
```
Bạn có thể khai báo các thuộc tính trong interface, nhưng thực chất, bản chất khi khai báo một thuộc tính trong interface là tạo ra 2 hàm getter/setter cho thuộc tính đó.
 Nếu là `val` thì chỉ có getter, còn `var` thì có cả getter và setter
### Resolving overriding conflicts
Nếu một lớp implements nhiều interface, mà những interface đó có hàm với tên giống nhau, thì khi gọi hàm super trong khi override, phải chỉ định hàm đó thuộc interface nào.

Cú pháp: `super<tên_interface>.tên_hàm()`

```java
interface A {
    fun callMe(){
        println("From A")
    }
}

interface B{
    fun callMe(){
        println("From B")
    }
}

class C : A, B {
    override fun callMe() {
        super<A>.callMe()
    }
}

fun main(args: Array<String>) {
    val c = C()
    c.callMe()
}
```

## Backing field
### Getter/setter

Khai báo thuộc tính một cách đầy đủ như sau:
```java
var <tên_thuộc_tính>[: <kiểu_dữ_liệu>] [= <giá_trị_khởi_tạo>]
    [<getter>]
    [<setter>]
```
Có thể custom accessors cho thuộc tính:
```java
class Rectangle(val width: Int, val height: Int) {
    val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
```
```java
var stringRepresentation: String
    get() = this.toString()
    set(value) { //Có thể thay value bằng tên khác nếu muốn
        setDataFromString(value) // parses the string and assigns values to other properties
    }
```
### Backing field
Trong Kotlin, một trường (field) chỉ được sử dụng như một phần của thuộc tính để giữ giá trị của nó trong bộ nhớ. Các trường không thể được khai báo trực tiếp. Tuy nhiên, khi một thuộc tính cần một trường lưu trữ, Kotlin sẽ tự động cung cấp nó. Trường lưu trữ này có thể được tham chiếu trong các accessor sử dụng định danh `field`.
```java
class Person{
    var counter = 0 // the initializer assigns the backing field directly
        set(value) {
            field = value
            // counter = value // ERROR StackOverflow: Using actual name 'counter' would make setter recursive
        }
}

fun main(args: Array<String>) {
    val p1 = Person()
    p1.counter = 5
}
```
Nếu thay `field` bằng `counter`, khi chạy chương trình sẽ báo StackOverFlowError do khi ta gọi `counter` thực chất là đang gọi đến accessor mặc định.
Vậy nên trong Kotlin `person.counter = value` giống với `person.setCounter(value)`.

Vậy nên khi `set(value){ counter = value }` được gọi, xảy ra recursive call vì ta đang gọi setter trong setter,  gây ra StackOverFlow.
