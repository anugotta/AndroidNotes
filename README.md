**Android Notes**




# Kotlin Data Class:

One of the many handy features that the Kotlin language offers is the data keyword. When we declare a class with the data keyword the compiler implements the equals(Object o), hashCode(), and toString() methods, thus saving us from the trouble to do it manually.

# Kotlin Open Class:

In Kotlin, all the classes are final by default i.e. they can’t be inherited by default. In Java, you have to make your class final explicitly. So, to make a class inheritable to the other classes, you must mark it with the open keyword otherwise you will get an error saying “type is final so can’t be inherited”.

An Open class is an ordinary class that is open for extension. Meaning: by default, when you write a class in Kotlin, it can not be extended. Yes, inheritance is prevented by default. By declaring a class to be open, you tell the compiler: "I intend to extend this class". In other words: if you want to have Child extends Base, then your Base class must be declared open in Kotlin.

In short: data classes are meant as containers for values, that can be used as that. Open is a different concept, it simply boils down to: can be inherited (respectively overridden when talking about methods) by some child class.

# Kotlin Scopes

In Kotlin, there are several "scope functions" that can be used to control the visibility and accessibility of variables and other elements within a program. These functions include:

let: The let function allows you to execute a block of code on an object, and it makes the object available within the block using the it keyword. This can be useful for reducing the amount of boilerplate code that is needed when working with an object.

run: The run function is similar to the let function, but it also returns the result of the block of code that is executed. This can be useful for performing multiple operations on an object and then returning the final result.

apply: The apply function allows you to execute a block of code on an object and modify the object within the block. The object is then returned after the block has been executed. This can be useful for initializing an object and setting its properties.

with: The with function is similar to the apply function, but it does not return the object after the block has been executed. This can be useful for calling multiple methods on an object without having to repeat the object's name.

Overall, these scope functions can be useful for simplifying and clarifying the code when working with objects in Kotlin. They can help to make the code more readable and maintainable.

Scope functions in Kotlin are a set of functions that allow you to modify and access the elements within a certain scope. The main goal of these functions is to simplify the code and make it more readable. Some of the most commonly used scope functions in Kotlin are let, run, apply, also, and with.

let function: The let function allows you to execute a block of code on a specific object and return the result. It takes a lambda expression as an argument, which specifies the operations that need to be performed on the object. For example:

    val string = "Hello, World!" string.let { println(it.length) }

In this example, the let function is used to print the length of the string. The argument it refers to the object on which the operations are being performed.

run function: The run function is similar to the let function but with the difference that it can modify the object on which the operations are being performed. It also takes a lambda expression as an argument and returns the result. For example:

    val string = "Hello, World!" val length = string.run { length } println(length)

In this example, the run function is used to get the length of the string and assign it to the length variable.

apply function: The apply function is used to perform operations on an object and return the same object. It is commonly used for setting multiple properties of an object in a concise manner. For example:

    val stringBuilder = StringBuilder().apply { append("Hello") append(" ") append("World!") } println(stringBuilder)

In this example, the apply function is used to build a string and return the same StringBuilder object.

also function: The also function is used to perform operations on an object and return the same object, similar to the apply function. The main difference between the two is that the also function returns the object after the operations are performed, while the apply function returns the object before the operations are performed. For example:

    val string = "Hello, World!" val result = string.also { println(it.length) } println(result)

In this example, the also function is used to print the length of the string and return the same string.

with function: The with function is used to perform operations on an object within a specific scope. It takes the object as the receiver and returns the result of the operations. For example:

    val string = "Hello, World!" val length = with(string) { length } println(length)

In this example, the with function is used to get the length of the string and assign it to the length variable.

In conclusion, scope functions in Kotlin provide a convenient and efficient way to perform operations on objects within a specific scope. They help to simplify the code and make it more readable, which makes them an essential tool for any Kotlin developer.


# Lazy, LateInit, Livedata

Lazy initialization: Lazy initialization helps us prevent unnecessary initialization of objects. In lazy initialization, the object is initialized only once and if requested again, it is taken from cache. Only val

    val myLazyObject: String by lazy { "Hello, World!" }

LateInit : As the name says, lateinit allows us to declare the object first and initialize it at a later stage. If the object is called without initialization, it throws an error. Only var.

Mutable: That can be changed Immutable: That cannot be changed

MutableLiveData: its value could be changed. It is used in viewmodel to observe the data from the model. It exposes set and postvalue methods publicly.

LiveData: its immutable, that means, its value cannot be changed. It is mainly used to update the view from viewmodel. Both livedata and mutable livedata are used together. Livedata does not provide set and post value methods publicly.

//LIve data for updating UI 
`private val _seconds = MutableLiveData<Int>() private val _finished = MutableLiveData<Boolean>()`

```
// getter method for seconds var
fun seconds():LiveData<Int>{
    return _seconds
}

// getter method for finished var
fun finished():LiveData<Boolean>{
    return _finished
}


```

setValue: Runs on main thread

postValue: Runs on background thread

# Static vs final:
Static means the value remains the same throughout the scope.
Final means the value cant be changed. A constant is declared by using both final static. It is done in kotlin using a companion object.

In Kotlin, there are several ways to declare a constant:

Using the const keyword: You can use the const keyword to declare a constant variable. This keyword can only be used on top-level variables or properties, and on variables or properties that are declared inside the companion object of a class. A const variable is a variable that can be evaluated at compile-time, and its value is known at compile time. Constants are defined at the top level or as a property of an object or companion object, and must be either a primitive type or a string.
For example:
const val PI = 3.14

Using the val keyword: If you want to declare a constant variable inside a class or function, you can use the val keyword. The variable cannot be reassigned, but the object it refers to can still be modified if it is mutable. For example:
val name = "John"

### val changes in this case

    val d
    get() = System.currentTimeMillis()
    
    println(d)
    //Wait a millisecond
    println(d) //Surprise!, the value of d will be different both times

### 

Using object: You can also create a singleton object that holds your constants. This way, you can group related constants together and also make sure that they are accessible from everywhere in your codebase. For example:

    object Constants {
    const val PI = 3.14
    const val SPEED_OF_LIGHT = 299792458
    }

Using companion object: You can also use a companion object to define constants. A companion object is an object that is defined inside a class and can be accessed without creating an instance of the class. For example:

    class MyClass {
    companion object {
    const val PI = 3.14
    const val SPEED_OF_LIGHT = 299792458
    }

In Kotlin, a static variable or method is defined using the "companion object" keyword, and is associated with a class rather than an instance of the class. A singleton, on the other hand, is a design pattern that ensures only one instance of a class exists at any given time. The singleton pattern can be implemented in Kotlin using the "object" keyword to create a singleton class.

It's important to note that while static variables and methods are associated with a class, they are not the same as a singleton. A static variable or method can be called without creating an instance of the class, while a singleton requires an instance to be created.

In Kotlin, const and final are both used to define variables that cannot be reassigned. However, they have some differences in their usage and behavior:

const is used to define a compile-time constant. The value of a const variable must be a compile-time constant, such as a string or a primitive type. It must be defined at the top level or as a property of an object or companion object. Once the value of a const variable is set, it cannot be changed.

final is used to define a variable that cannot be reassigned. The value of a final variable does not have to be a compile-time constant, it can be evaluated at runtime. It can be defined at the top level, as a property of an object or companion object, or as a local variable. Once the value of a final variable is set, it cannot be changed.

Here is an example of using both const and final:

    const val PI = 3.14
    final val radius = 10.0

In summary, const is used for variables that have a compile-time constant value, while final is used for variables that cannot be reassigned, regardless of whether the value is known at compile time or at runtime.

In Kotlin, the const keyword is used to define a compile-time constant, while the static keyword is used to define a class-level variable or method.

A const variable is a variable that can be evaluated at compile-time, and its value is known at compile time. Constants are defined at the top level or as a property of an object or companion object, and must be either a primitive type or a string.

    const val PI = 3.14

On the other hand, static variables or methods are defined using the "companion object" keyword, and are associated with a class rather than an instance of the class. They can be accessed directly from the class rather than an instance of the class.

Here is an example of a static method in Kotlin:

    class MyClass {
    companion object {
    fun myStaticMethod() {
    // code here
    }
    }
    }

It's important to note that while const variables are similar to static variables in that they can be accessed directly from the class, they are not the same. const variables are evaluated at compile-time and cannot be changed, while static variables can be changed at runtime.

Also, const can only be used for primitive types and strings, while static can be used for any types.

In programming, constants are values that do not change during program execution. In general, constants can be divided into two categories: compile-time constants and runtime constants.

Compile-time constants are values that are known to the compiler at the time of compilation and are hardcoded directly into the program's executable code. These constants are replaced with their actual values during the compilation process and are not stored in memory at runtime. As a result, they are usually more efficient than runtime constants in terms of memory and performance.

For example, consider the following code:

    const val PI = 3.14
    fun calculateArea(radius: Double): Double {
    return PI * radius * radius
    }

In this code, PI is a compile-time constant because its value is known at compile-time and is hardcoded into the program's executable code. The calculateArea function uses PI to calculate the area of a circle given its radius.

On the other hand, runtime constants are values that are determined during program execution and may vary depending on input or other factors. These constants are usually defined as variables that are initialized with constant values and marked as val instead of var to prevent them from being modified later in the program. Runtime constants are stored in memory at runtime and may have a performance impact compared to compile-time constants.

For example, consider the following code:

    fun calculateTax(price: Double, taxRate: Double): Double {
    val tax = price * taxRate
    return price + tax
    }

In this code, taxRate is a runtime constant because its value is not known at compile-time and is determined during program execution based on the input. The calculateTax function calculates the total price of an item given its price and the tax rate, which is a runtime constant.

In summary, compile-time constants are hardcoded values that are known at compile-time and are replaced with their actual values during the compilation process, while runtime constants are values that are determined during program execution and are stored in memory at runtime. Compile-time constants are generally more efficient in terms of memory and performance, while runtime constants are more flexible and can vary depending on input or other factors.

# Access modifiers

Kotlin modifiers work in a similar way to Java's, but with some nuances: In Java, the default modifier is package private, in Kotlin is public

Access Modifiers
In Java, there are four access modifiers that restrict the accessibility of the method or variable to which the modifier is applied. They are only used within classes, not within methods. public and private are the most relevant modifiers to our work, but we will briefly discuss all of them.

private: The most restrictive modifier. It limits access to methods and variables to the class in which they are declared. private is chosen when there is no need to use certain methods or variables outside the class.

default: Allows access only from within the current package. If there is no specified access modifier, the method or variable will take on this one. Learn more about the default modifier.

protected: Allows access to a method or variable only from within the current package, unless it is accessed through a child class outside of the package. Learn more about the protected modifier.

public: The least restrictive modifier. It allows access to a class, method or variable not only from within the class in which it is declared, but outside as well. This is the modifier we will most commonly use, but to understand the scenarios in which to use the others,

//Kotlin modifiers , here there is no package private modifier instead it has internal.

. In a simple case, a module is a set of files compiled at the same time in a given project. A set of files being compiled with a single Kotlin compiler invocation.


# Kotlin Vs Java
Kotlin is a statically-typed programming language that is concise, expressive, and designed to be safe. It is important because it is an officially supported language for Android development, and can be used to write Android apps in a more concise and expressive way compared to Java. Kotlin is more concise and expressive than Java, and has improved syntax for things like collections, lambdas, and type inference. Kotlin also has better support for functional programming constructs like higher-order functions, inline functions, and lambdas, which can make code more readable and maintainable. Additionally, Kotlin has improved type inference and null safety, which can help prevent common programming errors. Yes, Kotlin can be used with Android. It is fully compatible with Android and can be used to write Android apps using the Android Studio development environment. Kotlin can be more concise and expressive than Java, and can also help prevent common programming errors due to its improved type inference and null safety. The Kotlin Standard Library is a set of core functions and classes that are included with every Kotlin installation. It includes functions and classes for things like collections, strings, numbers, and more. Some features of Kotlin that I find particularly useful are its concise and expressive syntax, improved type inference and null safety, and support for functional programming constructs. I also find the ability to mix Kotlin and Java code in the same project to be very useful, as it allows for a gradual migration from Java to Kotlin. In Kotlin, variables and properties can be marked as nullable or non-nullable, and the type system helps prevent null references from being used in a non-nullable context. This helps prevent common null-related errors, such as null pointer exceptions. A primary constructor in Kotlin is a constructor that is declared at the same level as the class header, and is used to initialize the class. A secondary constructor is an additional constructor that is defined inside the class body and can be used to provide additional ways to initialize an object of the class. A data class in Kotlin is a class that is specifically designed to hold data. Data classes automatically generate certain methods, such as equals, hashCode, and toString, based on the properties defined in the class. Data classes are useful when you have a simple class that primarily holds data and does not have any complex behavior. A sealed class in Kotlin is a class that is marked as sealed, and can only be subclassed within the same file as the sealed class. Sealed classes are useful when you want to define a restricted set of subclasses for a class, and want to ensure that the set of subclasses is known at compile-time. Kotlin's type system is similar to Java's, with a few key differences. Kotlin has improved type inference, which allows the compiler to infer the type of a variable based on the context in which it is used. Kotlin also has improved support for nullable types and functional types, which can make the type system more powerful and expressive.

# Volatile and Synchronized

In Kotlin, volatile is a modifier that is used to indicate that a variable is subject to changes from multiple threads and needs to be read and written to in a thread-safe manner. The volatile keyword ensures that the value of a variable is always retrieved from main memory, rather than from a CPU cache, and written back to main memory, rather than to a CPU cache. This prevents other threads from seeing stale or inconsistent values of the variable, which can occur when multiple threads access the same variable without proper synchronization.

Here's an example of using the volatile keyword in Kotlin:

    import java.util.concurrent.atomic.AtomicBoolean
    
    var stopRequested: Boolean = false
    
    fun backgroundThread() {
    while (!stopRequested) {
    // do work
    }
    }
    
    fun main() {
    backgroundThread()
    stopRequested = true
    }

In the example above, the stopRequested variable is accessed by both the backgroundThread and the main functions. Without the volatile keyword, the backgroundThread function might never see the updated value of stopRequested, leading to an infinite loop. By marking stopRequested as volatile, you ensure that the value of the variable is updated and visible to all threads in a consistent manner.

In Kotlin, synchronized is a keyword that is used to enforce mutual exclusion on access to a shared resource, ensuring that only one thread can access the resource at a time. The synchronized keyword is used to define a critical section of code, also known as a synchronization block, that can only be executed by one thread at a time.

Here's an example of using the synchronized keyword in Kotlin:

    import java.util.concurrent.atomic.AtomicInteger
    
    val counter = AtomicInteger(0)
    
    fun incrementCounter() {
    counter.incrementAndGet()
    }
    
    fun main() {
    val threads = List(1000) {
    Thread {
    for (i in 0 until 1000) {
    synchronized(counter) {
    incrementCounter()
    }
    }
    }
    }
    threads.forEach { it.start() }
    threads.forEach { it.join() }
    println("Counter value: ${counter.get()}")
    }

In the example above, the incrementCounter function is called by multiple threads, but access to the shared resource counter is synchronized using the synchronized keyword. This ensures that the value of the counter variable is updated atomically, and that there are no race conditions or inconsistencies between the threads.

It's important to note that synchronization blocks can be used in a variety of contexts, including on object instances and class-level objects, to enforce mutual exclusion on access to shared resources.

---

volatile and synchronized are two keywords in Kotlin that are used to enforce thread safety and ensure consistent access to shared resources. However, they have different use cases and serve different purposes:

**volatile:**

volatile is used to indicate that a variable is subject to changes from multiple threads and needs to be read and written to in a thread-safe manner.
The volatile keyword ensures that the value of a variable is always retrieved from main memory, rather than from a CPU cache, and written back to main memory, rather than to a CPU cache.
This prevents other threads from seeing stale or inconsistent values of the variable, which can occur when multiple threads access the same variable without proper synchronization.
volatile is a good choice for simple variables that are read and written to by multiple threads.

**synchronized:**

synchronized is used to enforce mutual exclusion on access to a shared resource, ensuring that only one thread can access the resource at a time.
The synchronized keyword is used to define a critical section of code, also known as a synchronization block, that can only be executed by one thread at a time.
This ensures that the code in the synchronization block is executed atomically, and that there are no race conditions or inconsistencies between the threads.
synchronized is a good choice for more complex use cases, where multiple threads need to access and modify a shared resource in a coordinated and thread-safe manner.
In summary, volatile is used to ensure that a variable is always up-to-date and visible to all threads, while synchronized is used to enforce mutual exclusion on access to a shared resource. Both volatile and synchronized are important tools for ensuring thread safety in concurrent programming, and the choice between them depends on the specific requirements of your use case.

# Sealed Classes

In Kotlin, a sealed class is a special type of class that can have a limited set of subclasses. The purpose of a sealed class is to restrict the types that can inherit from it, ensuring that all possible types are known at compile time.

A sealed class is declared using the sealed modifier before the class declaration. Here's an example of a sealed class with two subclasses:

    sealed class Result<out T> {
    class Success<out T>(val value: T) : Result<T>()
    class Error(val message: String) : Result<Nothing>()
    }

In this example, the Result class is declared as a sealed class. It has two subclasses: Success and Error. The Success subclass has a generic type parameter T and a value property of type T, while the Error subclass has a message property of type String.

The out modifier on the type parameter T indicates that T is a covariant type parameter, which means that it can be used in a read-only context (e.g. as a return type) but not in a write context (e.g. as a parameter).

Since Result is a sealed class, any other subclasses of Result must be defined within the same file. This ensures that all possible subclasses of Result are known at compile time.

Here's an example of how to use the Result sealed class:

    fun divide(a: Int, b: Int): Result<Int> {
    return if (b == 0) {
    Result.Error("Cannot divide by zero")
    } else {
    Result.Success(a / b)
    }
    }
    
    fun main() {
    val result1 = divide(6, 3)
    val result2 = divide(6, 0)
    
    ```
    when (result1) {
        is Result.Success -> println("Result1: ${result1.value}")
        is Result.Error -> println("Result1: ${result1.message}")
    }
    
    when (result2) {
        is Result.Success -> println("Result2: ${result2.value}")
        is Result.Error -> println("Result2: ${result2.message}")
    }
    
    ```
    
    }

In this example, the divide function returns a Result object that either contains the result of the division (if b is not zero) or an error message (if b is zero).

In the main function, we call the divide function twice with different arguments. We use the when expression to check the type of the Result objects and print the appropriate message to the console.

By using a sealed class, we ensure that all possible outcomes of the divide function are known at compile time, and we can handle them in a type-safe manner using the Result subclasses. This helps prevent runtime errors and makes the code more robust and maintainable.

# Atomic 
In Kotlin, `Atomic` is a class that provides atomic operations for variables. It is part of the `kotlinx.atomicfu` library, which is an atomic multiplatform library for Kotlin. The library offers atomic operations that ensure thread safety and prevent data races when working with mutable shared variables in concurrent programming.

The `Atomic` class provides atomic operations on variables of different types, such as `AtomicInt`, `AtomicBoolean`, `AtomicReference`, etc. These classes provide atomic operations like compare-and-set, get-and-set, increment, decrement, and more. Atomic operations guarantee that the operations are performed atomically, meaning that they are indivisible and not affected by concurrent modifications.

Here's an example of using `AtomicInt` in Kotlin:

```
import kotlinx.atomicfu.atomic

fun main() {
    val counter = atomic(0) // Create an AtomicInt with initial value 0

    counter.incrementAndGet() // Atomically increments the value by 1
    println(counter.get()) // Prints: 1

    counter.compareAndSet(1, 5) // Atomically sets the value to 5 if it's currently 1
    println(counter.get()) // Prints: 5

    counter.getAndDecrement() // Atomically gets the current value and decrements it by 1
    println(counter.get()) // Prints: 4
}

```

In the example above, `AtomicInt` is used to ensure atomicity when working with the `counter` variable. The `incrementAndGet()` and `getAndDecrement()` methods provide atomic increment and decrement operations, respectively. The `compareAndSet()` method allows for an atomic compare-and-set operation, where the value is set to a new value only if the current value matches the expected value.

Using atomic variables from the `kotlinx.atomicfu` library can help prevent data races and ensure thread safety when multiple threads are accessing and modifying shared mutable variables.

