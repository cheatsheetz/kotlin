# Kotlin Cheat Sheet

Comprehensive reference for Kotlin programming language covering Android development, null safety, coroutines, and essential features for modern Kotlin development.

---

## Table of Contents
- [Basic Syntax](#basic-syntax)
- [Variables and Data Types](#variables-and-data-types)
- [Null Safety](#null-safety)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Classes and Objects](#classes-and-objects)
- [Inheritance and Interfaces](#inheritance-and-interfaces)
- [Collections](#collections)
- [Higher-Order Functions](#higher-order-functions)
- [Coroutines](#coroutines)
- [Android Development Basics](#android-development-basics)
- [Interoperability with Java](#interoperability-with-java)

---

## Basic Syntax

| Feature | Syntax | Example |
|---------|--------|---------|
| Package Declaration | `package com.example` | `package com.example.myapp` |
| Import | `import kotlin.math.*` | `import android.util.Log` |
| Main Function | `fun main()` | `fun main() { println("Hello") }` |
| Print | `println()` or `print()` | `println("Hello, World!")` |
| Comments | `//` or `/* */` | `// Single line comment` |
| String Templates | `$variable` or `${expression}` | `"Hello, $name!"` |

### Basic Program Structure
```kotlin
package com.example.myapp

import kotlin.math.PI

fun main() {
    println("Hello, Kotlin!")
    
    val name = "Kotlin"
    println("Welcome to $name programming!")
    
    val calculation = "Result: ${2 + 3}"
    println(calculation)
}
```

## Variables and Data Types

### Variable Declaration
```kotlin
// Immutable variables (read-only)
val name: String = "Kotlin"
val age = 25  // Type inference
val pi = 3.14159

// Mutable variables
var counter: Int = 0
var message = "Hello"  // Type inferred as String

// Late initialization
lateinit var adapter: RecyclerView.Adapter<*>

// Lazy initialization
val expensiveValue: String by lazy {
    println("Computing...")
    "Expensive result"
}

// Constants (compile-time constants)
const val API_KEY = "your-api-key"
const val MAX_RETRY = 3
```

### Basic Data Types
```kotlin
// Numbers
val byte: Byte = 127
val short: Short = 32767
val int: Int = 2147483647
val long: Long = 9223372036854775807L
val float: Float = 3.14f
val double: Double = 3.14159265359

// Characters and Booleans
val char: Char = 'K'
val boolean: Boolean = true

// Strings
val string: String = "Hello, Kotlin!"
val multilineString = """
    This is a
    multiline string
    with proper indentation
""".trimIndent()

// String templates
val name = "World"
val greeting = "Hello, $name!"
val math = "2 + 3 = ${2 + 3}"

// Arrays
val numbers: Array<Int> = arrayOf(1, 2, 3, 4, 5)
val strings = arrayOf("apple", "banana", "cherry")
val ints = intArrayOf(1, 2, 3)  // Primitive array
val nullableStrings: Array<String?> = arrayOfNulls(5)

// Type checking and casting
val obj: Any = "Hello"
if (obj is String) {
    println(obj.length)  // Smart cast to String
}

val str: String? = obj as? String  // Safe cast
val definiteStr: String = obj as String  // Unsafe cast
```

## Null Safety

### Nullable Types
```kotlin
// Non-null by default
var name: String = "Kotlin"
// name = null  // Compilation error

// Nullable types
var nullableName: String? = "Kotlin"
nullableName = null  // OK

// Safe call operator
val length: Int? = nullableName?.length
val upperCase: String? = nullableName?.uppercase()

// Chaining safe calls
data class Person(val name: String, val address: Address?)
data class Address(val street: String, val city: String)

val person: Person? = getPerson()
val city: String? = person?.address?.city

// Elvis operator (null-coalescing)
val name = nullableName ?: "Default Name"
val length = nullableName?.length ?: 0

// Not-null assertion (!!)
val definiteLength: Int = nullableName!!.length  // Throws NPE if null
```

### Safe Calls and Let Function
```kotlin
// let function with safe call
nullableName?.let { name ->
    println("Name is $name")
    println("Length is ${name.length}")
}

// also, apply, run functions
val person = Person("John", null).also { person ->
    println("Created person: ${person.name}")
}

val result = nullableName?.run {
    println("Processing $this")
    uppercase()
}

// Checking for null
if (nullableName != null) {
    println(nullableName.length)  // Smart cast to non-null
}

// Platform types (from Java)
// String! - platform type (nullable unknown)
val javaString = getStringFromJava()  // Could be null
val safeLength = javaString?.length
```

## Control Flow

### Conditional Expressions
```kotlin
// if as expression
val max = if (a > b) a else b

val result = if (x > 0) {
    println("Positive")
    "positive"
} else if (x < 0) {
    println("Negative")
    "negative"
} else {
    println("Zero")
    "zero"
}

// when expression (replaces switch)
val day = 3
val dayName = when (day) {
    1 -> "Monday"
    2 -> "Tuesday"
    3 -> "Wednesday"
    4, 5 -> "Thursday or Friday"
    in 6..7 -> "Weekend"
    else -> "Invalid day"
}

// when with conditions
val score = 85
val grade = when {
    score >= 90 -> "A"
    score >= 80 -> "B"
    score >= 70 -> "C"
    score >= 60 -> "D"
    else -> "F"
}

// when with type checking
fun describe(obj: Any): String = when (obj) {
    1 -> "One"
    "Hello" -> "Greeting"
    is Long -> "Long number"
    !is String -> "Not a string"
    else -> "Unknown"
}
```

### Loops
```kotlin
// for loops
val items = listOf("apple", "banana", "cherry")
for (item in items) {
    println(item)
}

// for with index
for ((index, item) in items.withIndex()) {
    println("$index: $item")
}

// for with range
for (i in 1..5) {
    println(i)  // 1, 2, 3, 4, 5
}

for (i in 1 until 5) {
    println(i)  // 1, 2, 3, 4
}

for (i in 5 downTo 1) {
    println(i)  // 5, 4, 3, 2, 1
}

for (i in 1..10 step 2) {
    println(i)  // 1, 3, 5, 7, 9
}

// while loops
var x = 10
while (x > 0) {
    println(x)
    x--
}

do {
    println("This runs at least once")
    x++
} while (x < 5)

// break and continue with labels
loop@ for (i in 1..3) {
    for (j in 1..3) {
        if (i == 2 && j == 2) break@loop
        println("$i, $j")
    }
}
```

### Ranges
```kotlin
// Range types
val range1 = 1..10        // 1 to 10 inclusive
val range2 = 1 until 10   // 1 to 9
val range3 = 10 downTo 1  // 10 to 1
val range4 = 1..10 step 2 // 1, 3, 5, 7, 9

// Character ranges
val letters = 'a'..'z'
val isLowerCase = 'c' in letters

// Checking ranges
if (score in 80..89) {
    println("Grade B")
}

if (x !in 1..10) {
    println("x is not in range")
}
```

## Functions

### Function Declaration
```kotlin
// Basic function
fun greet() {
    println("Hello!")
}

// Function with parameters
fun greet(name: String) {
    println("Hello, $name!")
}

// Function with return type
fun add(a: Int, b: Int): Int {
    return a + b
}

// Single expression function
fun multiply(a: Int, b: Int): Int = a * b
fun isEven(number: Int) = number % 2 == 0

// Function with default parameters
fun greet(name: String = "World", greeting: String = "Hello") {
    println("$greeting, $name!")
}

greet()                           // Hello, World!
greet("Alice")                    // Hello, Alice!
greet("Bob", "Hi")               // Hi, Bob!
greet(greeting = "Hey")          // Hey, World!

// Named parameters
fun createUser(name: String, email: String, age: Int = 18) {
    // Implementation
}

createUser(name = "John", email = "john@example.com", age = 25)
createUser(email = "jane@example.com", name = "Jane")

// Variable number of arguments (vararg)
fun sum(vararg numbers: Int): Int {
    var result = 0
    for (number in numbers) {
        result += number
    }
    return result
}

val total = sum(1, 2, 3, 4, 5)
val array = intArrayOf(1, 2, 3)
val totalFromArray = sum(*array)  // Spread operator
```

### Extension Functions
```kotlin
// Extension function for String
fun String.removeWhitespace(): String {
    return this.replace("\\s".toRegex(), "")
}

val text = "Hello World"
val noSpaces = text.removeWhitespace()  // "HelloWorld"

// Extension function with receiver
fun Int.isEven(): Boolean = this % 2 == 0
fun Int.isOdd(): Boolean = this % 2 != 0

val number = 42
println(number.isEven())  // true

// Extension properties
val String.lastIndex: Int
    get() = this.length - 1

println("Hello".lastIndex)  // 4

// Nullable receiver
fun String?.isNullOrEmpty(): Boolean {
    return this == null || this.isEmpty()
}

val nullString: String? = null
println(nullString.isNullOrEmpty())  // true
```

### Infix Functions
```kotlin
// Infix function
infix fun Int.times(str: String) = str.repeat(this)

val result = 3 times "Hello "  // "Hello Hello Hello "

// Custom infix for collections
infix fun <T> T.addTo(list: MutableList<T>) {
    list.add(this)
}

val numbers = mutableListOf<Int>()
5 addTo numbers
10 addTo numbers
println(numbers)  // [5, 10]

// Built-in infix functions
val map = mapOf("a" to 1, "b" to 2)  // to is infix
val range = 1 until 10               // until is infix
val isInRange = 5 in 1..10          // in is infix
```

### Scope Functions
```kotlin
data class Person(var name: String, var age: Int)

val person = Person("Alice", 30)

// let - returns lambda result, it refers to object
val nameLength = person.let { p ->
    println("Name: ${p.name}")
    p.name.length
}

// run - returns lambda result, this refers to object
val description = person.run {
    println("Name: $name, Age: $age")
    "Person description"
}

// with - returns lambda result, this refers to object
val info = with(person) {
    println("Processing person: $name")
    "Info: $name is $age years old"
}

// apply - returns object, this refers to object
val modifiedPerson = person.apply {
    name = "Bob"
    age = 35
}

// also - returns object, it refers to object
val personWithLog = person.also { p ->
    println("Person created: ${p.name}")
}

// takeIf - returns object if condition is true, null otherwise
val adult = person.takeIf { it.age >= 18 }

// takeUnless - returns object if condition is false, null otherwise
val notSenior = person.takeUnless { it.age >= 65 }
```

## Classes and Objects

### Class Declaration
```kotlin
// Basic class
class Person {
    var name: String = ""
    var age: Int = 0
}

// Primary constructor
class Person(firstName: String, age: Int) {
    val name: String = firstName
    val age: Int = age
    
    init {
        println("Person created: $name, $age")
    }
}

// Concise primary constructor
class Person(val name: String, val age: Int)

// Constructor with default values
class Person(val name: String = "Unknown", val age: Int = 0)

// Secondary constructors
class Person(val name: String) {
    var age: Int = 0
    var email: String = ""
    
    constructor(name: String, age: Int) : this(name) {
        this.age = age
    }
    
    constructor(name: String, age: Int, email: String) : this(name, age) {
        this.email = email
    }
}

val person1 = Person("Alice")
val person2 = Person("Bob", 25)
val person3 = Person("Charlie", 30, "charlie@example.com")
```

### Properties and Fields
```kotlin
class Person(firstName: String) {
    // Custom getter and setter
    var name: String = firstName
        get() = field.uppercase()
        set(value) {
            field = value.trim()
        }
    
    // Read-only property
    val nameLength: Int
        get() = name.length
    
    // Property with backing field
    var age: Int = 0
        set(value) {
            if (value >= 0) field = value
        }
    
    // Late-initialized property
    lateinit var address: String
    
    // Delegated property
    var password: String by lazy {
        generateSecurePassword()
    }
}

// Property delegation
import kotlin.properties.Delegates

class User {
    var name: String by Delegates.observable("Initial") { prop, old, new ->
        println("$old -> $new")
    }
    
    var positiveNumber: Int by Delegates.vetoable(0) { prop, old, new ->
        new >= 0
    }
}
```

### Data Classes
```kotlin
// Data class automatically provides equals, hashCode, toString, copy
data class User(val name: String, val age: Int, val email: String)

val user1 = User("Alice", 30, "alice@example.com")
val user2 = User("Alice", 30, "alice@example.com")

println(user1 == user2)  // true (structural equality)
println(user1)           // User(name=Alice, age=30, email=alice@example.com)

// Copy with modifications
val user3 = user1.copy(age = 31)
println(user3)  // User(name=Alice, age=31, email=alice@example.com)

// Destructuring declarations
val (name, age, email) = user1
println("Name: $name, Age: $age")

// Data class with methods
data class Point(val x: Int, val y: Int) {
    fun distanceFromOrigin(): Double {
        return kotlin.math.sqrt((x * x + y * y).toDouble())
    }
}
```

### Sealed Classes and Enums
```kotlin
// Sealed class (restricted class hierarchy)
sealed class Result<out T>
data class Success<T>(val data: T) : Result<T>()
data class Error(val exception: Throwable) : Result<Nothing>()
object Loading : Result<Nothing>()

fun handleResult(result: Result<String>) {
    when (result) {
        is Success -> println("Data: ${result.data}")
        is Error -> println("Error: ${result.exception.message}")
        Loading -> println("Loading...")
    }
    // No else clause needed - compiler knows all cases are covered
}

// Enum classes
enum class Color(val rgb: Int) {
    RED(0xFF0000),
    GREEN(0x00FF00),
    BLUE(0x0000FF);
    
    fun containsRed(): Boolean = (this.rgb and 0xFF0000) != 0
}

enum class Direction {
    NORTH, SOUTH, EAST, WEST;
    
    fun opposite(): Direction = when (this) {
        NORTH -> SOUTH
        SOUTH -> NORTH
        EAST -> WEST
        WEST -> EAST
    }
}

// Enum with interface
interface Printable {
    fun prettyPrint()
}

enum class ProtocolState : Printable {
    WAITING {
        override fun prettyPrint() = println("Waiting...")
        override fun signal() = TALKING
    },
    
    TALKING {
        override fun prettyPrint() = println("Talking...")
        override fun signal() = WAITING
    };
    
    abstract fun signal(): ProtocolState
}
```

### Object Declarations and Expressions
```kotlin
// Object declaration (singleton)
object DatabaseManager {
    fun connect() {
        println("Connected to database")
    }
    
    fun disconnect() {
        println("Disconnected from database")
    }
}

DatabaseManager.connect()

// Companion object (similar to static in Java)
class MyClass {
    companion object {
        const val CONSTANT = "constant value"
        
        fun create(): MyClass {
            return MyClass()
        }
    }
}

val instance = MyClass.create()
println(MyClass.CONSTANT)

// Object expression (anonymous object)
val clickListener = object : View.OnClickListener {
    override fun onClick(v: View?) {
        println("Clicked!")
    }
}

// Object expression with multiple interfaces
interface A {
    fun funcA()
}

interface B {
    fun funcB()
}

val ab = object : A, B {
    override fun funcA() = println("Function A")
    override fun funcB() = println("Function B")
}
```

## Inheritance and Interfaces

### Class Inheritance
```kotlin
// Open class (can be inherited)
open class Animal(val name: String) {
    open fun makeSound() {
        println("$name makes a sound")
    }
    
    open val sound: String = "generic sound"
}

// Inheritance
class Dog(name: String, val breed: String) : Animal(name) {
    override fun makeSound() {
        println("$name barks")
    }
    
    override val sound: String = "bark"
    
    fun wagTail() {
        println("$name wags tail")
    }
}

class Cat(name: String) : Animal(name) {
    override fun makeSound() {
        super.makeSound()  // Call parent method
        println("$name meows")
    }
}

val dog = Dog("Buddy", "Golden Retriever")
val cat = Cat("Whiskers")

dog.makeSound()  // Buddy barks
cat.makeSound()  // Whiskers makes a sound \n Whiskers meows

// Abstract classes
abstract class Shape {
    abstract fun calculateArea(): Double
    abstract val perimeter: Double
    
    open fun display() {
        println("This is a shape")
    }
}

class Rectangle(val width: Double, val height: Double) : Shape() {
    override fun calculateArea(): Double = width * height
    override val perimeter: Double
        get() = 2 * (width + height)
    
    override fun display() {
        super.display()
        println("Rectangle: ${width}x${height}")
    }
}
```

### Interfaces
```kotlin
// Interface definition
interface Drawable {
    fun draw()
    fun getArea(): Double {  // Default implementation
        return 0.0
    }
}

interface Clickable {
    fun click()
    fun showTooltip() = println("Tooltip")  // Default implementation
}

// Implementing interfaces
class Button : Drawable, Clickable {
    override fun draw() {
        println("Drawing button")
    }
    
    override fun click() {
        println("Button clicked")
    }
    
    override fun getArea(): Double = 100.0
}

// Interface inheritance
interface TouchableDrawable : Drawable, Clickable {
    fun onTouch()
}

class ImageButton : TouchableDrawable {
    override fun draw() = println("Drawing image button")
    override fun click() = println("Image button clicked")
    override fun onTouch() = println("Image button touched")
}

// Resolving conflicts
interface Named {
    val name: String
    fun getName() = name
}

interface Person : Named {
    val firstName: String
    val lastName: String
    
    override val name: String get() = "$firstName $lastName"
}

class Employee(
    override val firstName: String,
    override val lastName: String,
    val id: Int
) : Person {
    override fun getName(): String {
        return super.getName().uppercase()
    }
}
```

### Delegation
```kotlin
// Interface delegation
interface Base {
    fun printMessage()
    fun printLine()
}

class BaseImpl(val x: Int) : Base {
    override fun printMessage() = println("BaseImpl: $x")
    override fun printLine() = println("Line from BaseImpl")
}

class Derived(b: Base) : Base by b {
    override fun printLine() = println("Line from Derived")
}

val base = BaseImpl(10)
val derived = Derived(base)
derived.printMessage()  // BaseImpl: 10 (delegated)
derived.printLine()     // Line from Derived (overridden)

// Property delegation
class DelegatedProperties {
    var token: String by lazy {
        getTokenFromServer()
    }
    
    var data: String by Delegates.observable("initial") { prop, old, new ->
        println("$old -> $new")
    }
}

// Custom delegate
import kotlin.reflect.KProperty

class StringDelegate(private var value: String = "") {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return value
    }
    
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("Setting ${property.name} to $value")
        this.value = value
    }
}

class MyClass {
    var customProperty: String by StringDelegate()
}
```

## Collections

### Lists
```kotlin
// Immutable list
val readOnlyList = listOf("apple", "banana", "cherry")
val emptyList = emptyList<String>()

// Mutable list
val mutableList = mutableListOf("apple", "banana")
mutableList.add("cherry")
mutableList += "date"  // Operator overloading
mutableList.removeAt(0)

// ArrayList
val arrayList = ArrayList<String>()
arrayList.addAll(listOf("item1", "item2"))

// List operations
val numbers = listOf(1, 2, 3, 4, 5)
println(numbers.size)
println(numbers.isEmpty())
println(numbers.contains(3))
println(numbers[0])  // First element
println(numbers.first())
println(numbers.last())
println(numbers.indexOf(3))

// Filtering and transforming
val evenNumbers = numbers.filter { it % 2 == 0 }
val doubled = numbers.map { it * 2 }
val sum = numbers.sum()
val max = numbers.maxOrNull()
val sorted = numbers.sorted()
val reversed = numbers.reversed()

// Sublist operations
val subList = numbers.subList(1, 4)  // [2, 3, 4]
val dropped = numbers.drop(2)        // [3, 4, 5]
val taken = numbers.take(3)          // [1, 2, 3]
```

### Sets
```kotlin
// Immutable set
val readOnlySet = setOf("apple", "banana", "cherry", "apple")  // Duplicates removed
println(readOnlySet.size)  // 3

// Mutable set
val mutableSet = mutableSetOf("apple", "banana")
mutableSet.add("cherry")
mutableSet += "date"
mutableSet.remove("apple")

// Set operations
val set1 = setOf(1, 2, 3, 4)
val set2 = setOf(3, 4, 5, 6)

val union = set1 union set2           // [1, 2, 3, 4, 5, 6]
val intersection = set1 intersect set2 // [3, 4]
val difference = set1 subtract set2   // [1, 2]

// HashSet and LinkedHashSet
val hashSet = hashSetOf(1, 2, 3)
val linkedHashSet = linkedSetOf(1, 2, 3)  // Maintains insertion order
val sortedSet = sortedSetOf(3, 1, 2)      // [1, 2, 3]
```

### Maps
```kotlin
// Immutable map
val readOnlyMap = mapOf(
    "apple" to 5,
    "banana" to 3,
    "cherry" to 8
)

val emptyMap = emptyMap<String, Int>()

// Mutable map
val mutableMap = mutableMapOf("apple" to 5, "banana" to 3)
mutableMap["cherry"] = 8
mutableMap.put("date", 2)
mutableMap += "elderberry" to 6

// Map operations
println(mutableMap.size)
println(mutableMap.isEmpty())
println(mutableMap.containsKey("apple"))
println(mutableMap.containsValue(5))
println(mutableMap["apple"])  // Returns nullable value
println(mutableMap.getValue("apple"))  // Throws exception if not found

// Safe access
val appleCount = mutableMap["apple"] ?: 0
val orangeCount = mutableMap.getOrDefault("orange", 0)
val kiwiCount = mutableMap.getOrElse("kiwi") { 0 }

// Iteration
for ((key, value) in mutableMap) {
    println("$key: $value")
}

for (key in mutableMap.keys) {
    println("Key: $key")
}

for (value in mutableMap.values) {
    println("Value: $value")
}

// Map transformations
val doubledValues = mutableMap.mapValues { (_, value) -> value * 2 }
val filteredMap = mutableMap.filter { (_, value) -> value > 3 }
```

### Collection Operations
```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

// Filtering
val evenNumbers = numbers.filter { it % 2 == 0 }
val greaterThanFive = numbers.filter { it > 5 }
val firstThreeEvens = numbers.filter { it % 2 == 0 }.take(3)

// Mapping
val squared = numbers.map { it * it }
val strings = numbers.map { "Number: $it" }

// FlatMap
val words = listOf("hello", "world")
val characters = words.flatMap { it.toList() }  // All characters

// Grouping
val fruits = listOf("apple", "apricot", "banana", "cherry", "avocado")
val groupedByFirstLetter = fruits.groupBy { it.first() }

// Partitioning
val (evens, odds) = numbers.partition { it % 2 == 0 }

// Aggregation
val sum = numbers.sum()
val average = numbers.average()
val count = numbers.count { it % 2 == 0 }
val maxNumber = numbers.maxOrNull()
val minNumber = numbers.minOrNull()

// Folding and reducing
val product = numbers.fold(1) { acc, element -> acc * element }
val concatenated = fruits.reduce { acc, element -> "$acc, $element" }

// Any, all, none
val hasEven = numbers.any { it % 2 == 0 }
val allPositive = numbers.all { it > 0 }
val noNegative = numbers.none { it < 0 }

// Finding elements
val firstEven = numbers.first { it % 2 == 0 }
val firstEvenOrNull = numbers.firstOrNull { it % 2 == 0 }
val lastOdd = numbers.last { it % 2 != 0 }

// Sequences (lazy evaluation)
val sequence = numbers.asSequence()
    .filter { it % 2 == 0 }
    .map { it * it }
    .take(3)
    .toList()
```

## Higher-Order Functions

### Function Types
```kotlin
// Function type variables
val sum: (Int, Int) -> Int = { a, b -> a + b }
val multiply = { a: Int, b: Int -> a * b }
val greet: (String) -> Unit = { name -> println("Hello, $name!") }

// Functions as parameters
fun calculate(x: Int, y: Int, operation: (Int, Int) -> Int): Int {
    return operation(x, y)
}

val result1 = calculate(5, 3, sum)
val result2 = calculate(5, 3) { a, b -> a - b }  // Trailing lambda

// Functions as return types
fun getOperation(operator: String): (Int, Int) -> Int {
    return when (operator) {
        "+" -> { a, b -> a + b }
        "-" -> { a, b -> a - b }
        "*" -> { a, b -> a * b }
        "/" -> { a, b -> if (b != 0) a / b else 0 }
        else -> { _, _ -> 0 }
    }
}

val addOperation = getOperation("+")
val addResult = addOperation(10, 5)
```

### Lambda Expressions and Anonymous Functions
```kotlin
// Lambda syntax
val numbers = listOf(1, 2, 3, 4, 5)

// Full syntax
val doubled1 = numbers.map({ number -> number * 2 })

// If lambda is the last parameter, it can be outside parentheses
val doubled2 = numbers.map() { number -> number * 2 }

// If lambda is the only parameter, parentheses can be omitted
val doubled3 = numbers.map { number -> number * 2 }

// Single parameter lambda can use 'it'
val doubled4 = numbers.map { it * 2 }

// Multi-line lambda
val processed = numbers.map { number ->
    println("Processing $number")
    number * 2
}

// Lambda with multiple parameters
val pairs = listOf(1 to 2, 3 to 4, 5 to 6)
val sums = pairs.map { (first, second) -> first + second }

// Anonymous functions
val square = fun(x: Int): Int { return x * x }
val isEven = fun(x: Int) = x % 2 == 0

// Anonymous function as expression
val filtered = numbers.filter(fun(number): Boolean {
    return number % 2 == 0
})
```

### Inline Functions
```kotlin
// Inline function to avoid function call overhead
inline fun measureTime(block: () -> Unit): Long {
    val start = System.currentTimeMillis()
    block()
    val end = System.currentTimeMillis()
    return end - start
}

val time = measureTime {
    // Some expensive operation
    Thread.sleep(1000)
}

// noinline parameter (prevents inlining of specific lambda)
inline fun processData(
    data: List<Int>,
    inline processor: (Int) -> Int,
    noinline logger: (String) -> Unit
) {
    data.forEach { item ->
        val processed = processor(item)
        logger("Processed: $processed")
    }
}

// crossinline (lambda parameter cannot use non-local returns)
inline fun runInContext(crossinline block: () -> Unit) {
    Thread {
        block()  // Cannot return from enclosing function
    }.start()
}
```

### Standard Higher-Order Functions
```kotlin
val person = Person("Alice", 30)

// let - execute block and return result
val nameLength = person.let { p ->
    println("Person: ${p.name}")
    p.name.length
}

// run - execute block in context and return result
val greeting = person.run {
    "Hello, I'm $name and I'm $age years old"
}

// with - execute block with receiver and return result
val description = with(person) {
    "Person named $name, age $age"
}

// apply - execute block and return receiver object
val modifiedPerson = person.apply {
    age = 31
}

// also - execute block and return receiver object
val personWithSideEffect = person.also { p ->
    println("Created person: ${p.name}")
}

// takeIf - return object if predicate is true, null otherwise
val adult = person.takeIf { it.age >= 18 }

// takeUnless - return object if predicate is false, null otherwise
val notSenior = person.takeUnless { it.age >= 65 }

// repeat - execute block n times
repeat(5) { index ->
    println("Iteration $index")
}
```

## Coroutines

### Basic Coroutines
```kotlin
import kotlinx.coroutines.*

// Basic coroutine
fun main() = runBlocking { // Coroutine scope
    launch { // Fire-and-forget coroutine
        delay(1000)
        println("World!")
    }
    println("Hello,")
}

// Suspending functions
suspend fun doSomething(): String {
    delay(1000) // Simulate async work
    return "Result"
}

suspend fun doSomethingElse(): String {
    delay(1000)
    return "Another result"
}

// Sequential execution
fun main() = runBlocking {
    val time = measureTimeMillis {
        val result1 = doSomething()
        val result2 = doSomethingElse()
        println("$result1, $result2")
    }
    println("Completed in $time ms")  // ~2000ms
}

// Concurrent execution with async
fun main() = runBlocking {
    val time = measureTimeMillis {
        val deferred1 = async { doSomething() }
        val deferred2 = async { doSomethingElse() }
        println("${deferred1.await()}, ${deferred2.await()}")
    }
    println("Completed in $time ms")  // ~1000ms
}
```

### Coroutine Scopes and Context
```kotlin
import kotlinx.coroutines.*

// Different dispatchers
fun main() = runBlocking {
    // Main/UI thread (Android)
    launch(Dispatchers.Main) {
        // Update UI
    }
    
    // IO dispatcher for file/network operations
    launch(Dispatchers.IO) {
        // File operations, network calls
        val data = downloadData()
        withContext(Dispatchers.Main) {
            // Switch back to main thread to update UI
            updateUI(data)
        }
    }
    
    // Default dispatcher for CPU-intensive work
    launch(Dispatchers.Default) {
        // Heavy computation
        val result = performCalculation()
        println(result)
    }
    
    // Unconfined dispatcher (not recommended for most use cases)
    launch(Dispatchers.Unconfined) {
        // Runs in current thread until first suspension
    }
}

// Structured concurrency
class UserRepository {
    private val scope = CoroutineScope(Dispatchers.IO + SupervisorJob())
    
    suspend fun getUser(id: String): User = withContext(Dispatchers.IO) {
        // Network call
        apiService.getUser(id)
    }
    
    fun refreshUserData() {
        scope.launch {
            try {
                val users = getAllUsers()
                // Process users
            } catch (e: Exception) {
                // Handle error
            }
        }
    }
    
    fun cleanup() {
        scope.cancel() // Cancel all coroutines
    }
}
```

### Error Handling and Cancellation
```kotlin
// Exception handling
fun main() = runBlocking {
    val job = launch {
        try {
            repeat(1000) { i ->
                println("Job: I'm sleeping $i")
                delay(500)
            }
        } catch (e: CancellationException) {
            println("Job cancelled")
            throw e // Re-throw CancellationException
        } finally {
            println("Job: cleanup")
        }
    }
    
    delay(1300)
    println("Cancelling job")
    job.cancel() // Cancel the job
    job.join()   // Wait for completion
    println("Main: done")
}

// Exception handling with async
fun main() = runBlocking {
    val deferred = async {
        throw Exception("Something went wrong")
    }
    
    try {
        deferred.await()
    } catch (e: Exception) {
        println("Caught: ${e.message}")
    }
}

// SupervisorJob - doesn't cancel siblings when one fails
fun main() = runBlocking {
    val supervisor = SupervisorJob()
    val scope = CoroutineScope(Dispatchers.Default + supervisor)
    
    val child1 = scope.launch {
        delay(1000)
        throw Exception("Child 1 failed")
    }
    
    val child2 = scope.launch {
        delay(2000)
        println("Child 2 completed")
    }
    
    joinAll(child1, child2)
}

// Timeout
fun main() = runBlocking {
    try {
        withTimeout(1000) {
            repeat(1000) { i ->
                println("I'm sleeping $i")
                delay(500)
            }
        }
    } catch (e: TimeoutCancellationException) {
        println("Timed out!")
    }
}
```

### Flows
```kotlin
import kotlinx.coroutines.flow.*

// Creating flows
fun simple(): Flow<Int> = flow {
    for (i in 1..3) {
        delay(100)
        emit(i) // Emit values
    }
}

// Flow from collections
val numbersFlow = (1..5).asFlow()

// Flow operators
fun main() = runBlocking {
    simple()
        .map { it * it }        // Transform
        .filter { it > 4 }      // Filter
        .collect { value ->     // Terminal operator
            println(value)
        }
}

// Flow context
fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")

fun simple(): Flow<Int> = flow {
    log("Started simple flow")
    for (i in 1..3) {
        emit(i)
    }
}.flowOn(Dispatchers.Default) // Change context for upstream

fun main() = runBlocking {
    simple()
        .collect { value -> 
            log("Collected $value") 
        }
}

// Exception handling in flows
fun main() = runBlocking {
    flow {
        for (i in 1..3) {
            emit(i)
            if (i == 2) throw RuntimeException("Error at $i")
        }
    }
    .catch { e -> 
        println("Caught exception: $e")
        emit(0) // Emit default value
    }
    .collect { value ->
        println(value)
    }
}

// Flow completion
fun main() = runBlocking {
    simple()
        .onCompletion { cause ->
            if (cause != null) {
                println("Flow completed exceptionally: $cause")
            } else {
                println("Flow completed successfully")
            }
        }
        .collect { value -> println(value) }
}
```

## Android Development Basics

### Activity and Fragment
```kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        // Coroutines in Android
        lifecycleScope.launch {
            val data = fetchDataFromServer()
            updateUI(data)
        }
    }
    
    private suspend fun fetchDataFromServer(): String {
        // Simulate network call
        delay(1000)
        return "Data from server"
    }
    
    private fun updateUI(data: String) {
        // Update UI elements
        findViewById<TextView>(R.id.textView).text = data
    }
}

// Fragment
class MyFragment : Fragment() {
    private var _binding: FragmentMyBinding? = null
    private val binding get() = _binding!!
    
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentMyBinding.inflate(inflater, container, false)
        return binding.root
    }
    
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        
        binding.button.setOnClickListener {
            // Handle click
        }
    }
    
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

### ViewModel and LiveData
```kotlin
import androidx.lifecycle.*
import kotlinx.coroutines.launch

class UserViewModel : ViewModel() {
    private val _users = MutableLiveData<List<User>>()
    val users: LiveData<List<User>> = _users
    
    private val _isLoading = MutableLiveData<Boolean>()
    val isLoading: LiveData<Boolean> = _isLoading
    
    private val repository = UserRepository()
    
    fun loadUsers() {
        viewModelScope.launch {
            _isLoading.value = true
            try {
                val userList = repository.getAllUsers()
                _users.value = userList
            } catch (e: Exception) {
                // Handle error
            } finally {
                _isLoading.value = false
            }
        }
    }
    
    // Transformation
    val userNames: LiveData<List<String>> = users.map { userList ->
        userList.map { it.name }
    }
    
    // Combining LiveData
    val combinedData: LiveData<String> = users.switchMap { userList ->
        liveData {
            emit("Users: ${userList.size}")
        }
    }
}

// In Activity/Fragment
class MainActivity : AppCompatActivity() {
    private lateinit var viewModel: UserViewModel
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        viewModel = ViewModelProvider(this)[UserViewModel::class.java]
        
        // Observe LiveData
        viewModel.users.observe(this) { users ->
            updateUserList(users)
        }
        
        viewModel.isLoading.observe(this) { isLoading ->
            progressBar.visibility = if (isLoading) View.VISIBLE else View.GONE
        }
        
        viewModel.loadUsers()
    }
}
```

### Room Database
```kotlin
import androidx.room.*

// Entity
@Entity(tableName = "users")
data class User(
    @PrimaryKey val id: String,
    @ColumnInfo(name = "user_name") val name: String,
    val email: String,
    val age: Int
)

// DAO
@Dao
interface UserDao {
    @Query("SELECT * FROM users")
    suspend fun getAllUsers(): List<User>
    
    @Query("SELECT * FROM users WHERE id = :userId")
    suspend fun getUserById(userId: String): User?
    
    @Query("SELECT * FROM users WHERE user_name LIKE :name")
    fun getUsersByName(name: String): Flow<List<User>>
    
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertUser(user: User)
    
    @Update
    suspend fun updateUser(user: User)
    
    @Delete
    suspend fun deleteUser(user: User)
    
    @Query("DELETE FROM users WHERE id = :userId")
    suspend fun deleteUserById(userId: String)
}

// Database
@Database(
    entities = [User::class],
    version = 1,
    exportSchema = false
)
@TypeConverters(Converters::class)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
    
    companion object {
        @Volatile
        private var INSTANCE: AppDatabase? = null
        
        fun getDatabase(context: Context): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    AppDatabase::class.java,
                    "app_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}

// Type converters for complex types
class Converters {
    @TypeConverter
    fun fromStringList(value: List<String>): String {
        return Gson().toJson(value)
    }
    
    @TypeConverter
    fun toStringList(value: String): List<String> {
        return Gson().fromJson(value, object : TypeToken<List<String>>() {}.type)
    }
}
```

### Retrofit for Networking
```kotlin
import retrofit2.*
import retrofit2.http.*

// Data classes
data class ApiResponse<T>(
    val success: Boolean,
    val data: T?,
    val message: String?
)

data class User(
    val id: String,
    val name: String,
    val email: String
)

// API interface
interface ApiService {
    @GET("users")
    suspend fun getUsers(): Response<ApiResponse<List<User>>>
    
    @GET("users/{id}")
    suspend fun getUser(@Path("id") userId: String): Response<ApiResponse<User>>
    
    @POST("users")
    suspend fun createUser(@Body user: User): Response<ApiResponse<User>>
    
    @PUT("users/{id}")
    suspend fun updateUser(
        @Path("id") userId: String,
        @Body user: User
    ): Response<ApiResponse<User>>
    
    @DELETE("users/{id}")
    suspend fun deleteUser(@Path("id") userId: String): Response<ApiResponse<Unit>>
    
    @POST("upload")
    @Multipart
    suspend fun uploadFile(
        @Part file: MultipartBody.Part,
        @Part("description") description: RequestBody
    ): Response<ApiResponse<String>>
}

// Repository
class UserRepository(private val apiService: ApiService) {
    
    suspend fun getUsers(): Result<List<User>> {
        return try {
            val response = apiService.getUsers()
            if (response.isSuccessful) {
                val apiResponse = response.body()
                if (apiResponse?.success == true && apiResponse.data != null) {
                    Result.success(apiResponse.data)
                } else {
                    Result.failure(Exception(apiResponse?.message ?: "Unknown error"))
                }
            } else {
                Result.failure(Exception("HTTP ${response.code()}: ${response.message()}"))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
    
    suspend fun createUser(user: User): Result<User> {
        return try {
            val response = apiService.createUser(user)
            if (response.isSuccessful) {
                val apiResponse = response.body()
                if (apiResponse?.success == true && apiResponse.data != null) {
                    Result.success(apiResponse.data)
                } else {
                    Result.failure(Exception(apiResponse?.message ?: "Failed to create user"))
                }
            } else {
                Result.failure(Exception("HTTP ${response.code()}"))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}

// Network module (using Dagger Hilt or manual)
object NetworkModule {
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://api.example.com/")
        .addConverterFactory(GsonConverterFactory.create())
        .build()
    
    val apiService: ApiService = retrofit.create(ApiService::class.java)
}
```

## Interoperability with Java

### Calling Kotlin from Java
```kotlin
// Kotlin class
class KotlinClass {
    fun kotlinFunction(name: String): String {
        return "Hello, $name from Kotlin!"
    }
    
    // Static-like function
    companion object {
        @JvmStatic
        fun staticFunction(): String = "Static function"
    }
}

// Top-level function
fun topLevelFunction(): String = "Top level function"

// Extension function
fun String.addPrefix(): String = "Prefix: $this"
```

```java
// Java code calling Kotlin
public class JavaClass {
    public void callKotlin() {
        // Calling Kotlin class
        KotlinClass kotlinObj = new KotlinClass();
        String result = kotlinObj.kotlinFunction("Java");
        
        // Calling companion object function
        String staticResult = KotlinClass.staticFunction();
        
        // Calling top-level function (generated as static method in class)
        String topLevelResult = MyFileKt.topLevelFunction();
        
        // Extension functions become static methods
        String withPrefix = MyFileKt.addPrefix("test");
    }
}
```

### Calling Java from Kotlin
```java
// Java class
public class JavaClass {
    private String name;
    
    public JavaClass(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public static void staticMethod() {
        System.out.println("Java static method");
    }
}
```

```kotlin
// Kotlin code calling Java
fun callJava() {
    // Creating Java object
    val javaObj = JavaClass("Test")
    
    // Calling getters/setters as properties
    println(javaObj.name)  // Calls getName()
    javaObj.name = "New Name"  // Calls setName()
    
    // Calling static method
    JavaClass.staticMethod()
    
    // Platform types - nullable unknown
    val javaString: String = getStringFromJava()  // Could be null
    val safeString: String? = getStringFromJava()  // Better approach
}
```

### Annotations for Java Interop
```kotlin
// @JvmName - change generated method name
@JvmName("filterValid")
fun List<String>.filter(predicate: (String) -> Boolean): List<String> {
    // Implementation
}

// @JvmOverloads - generate overloaded methods for default parameters
@JvmOverloads
fun greet(name: String, greeting: String = "Hello"): String {
    return "$greeting, $name!"
}
// Java can call: greet("John") or greet("John", "Hi")

// @JvmStatic - generate static method
class MyClass {
    companion object {
        @JvmStatic
        fun staticMethod() = "Static"
    }
}

// @JvmField - expose property as field
class DataClass {
    @JvmField
    val data: String = "exposed as field"
}

// @Throws - declare checked exceptions
@Throws(IOException::class)
fun riskyOperation() {
    throw IOException("Something went wrong")
}
```

---

## Resources
- [Kotlin Official Documentation](https://kotlinlang.org/docs/)
- [Kotlin Koans](https://play.kotlinlang.org/koans)
- [Android Kotlin Fundamentals](https://developer.android.com/courses/kotlin-bootcamp/overview)
- [Kotlin Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)
- [Kotlin for Android Developers](https://developer.android.com/kotlin)

---
*Originally compiled from various sources. Contributions welcome!*