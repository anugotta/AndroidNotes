# Android Notes

## Table of contents

- [Chapter 1 - Clean Code](#chapter1)
- [Chapter 2 - Meaningful Names](#chapter2)
- [Chapter 3 - Functions](#chapter3)
- [Chapter 4 - Comments](#chapter4)
- [Chapter 5 - Formatting](#chapter5)
- [Chapter 6 - Objects and Data Structures](#chapter6)
- [Chapter 7 - Error Handling](#chapter7)
- [Chapter 8 - Boundaries](#chapter8)
- [Chapter 9 - Unit Tests](#chapter9)
- [Chapter 10 - Classes](#chapter10)
- [Chapter 11 - Systems](#chapter11)
- [Chapter 12 - Emergence](#chapter12)
- [Chapter 13 - Concurrency](#chapter13)
- [Chapter 14 - Successive Refinement](#chapter14)
- [Chapter 15 - JUnit Internals](#chapter15)
- [Chapter 16 - Refactoring SerialDate](#chapter15)
- [Chapter 17 - Smells and Heuristics](#chapter17)

<a name="chapter1">
    

# Interface vs Abstract Class:
    
 In Kotlin, starting from version 1.0, interfaces can define functions with default implementations. This feature allows interfaces to provide method implementations, making them more versatile. Here are some examples:

```kotlin
interface Vehicle {
    fun start()
    fun stop()

    fun honk() { // Default implementation
        println("Beep beep!")
    }
}

class Car : Vehicle {
    override fun start() {
        println("Car started")
    }

    override fun stop() {
        println("Car stopped")
    }
}

class Bike : Vehicle {
    override fun start() {
        println("Bike started")
    }

    override fun stop() {
        println("Bike stopped")
    }

    override fun honk() {
        println("Ring ring!")
    }
}

fun main() {
    val car = Car()
    car.start()
    car.stop()
    car.honk()

    val bike = Bike()
    bike.start()
    bike.stop()
    bike.honk()
}
```

In the example above, we define an interface called `Vehicle` that declares three functions: `start()`, `stop()`, and `honk()`. The `start()` and `stop()` functions are abstract methods, meaning they don't provide an implementation in the interface itself. On the other hand, the `honk()` function has a default implementation that prints "Beep beep!".

The `Car` class implements the `Vehicle` interface and provides specific implementations for `start()` and `stop()`. It inherits the default implementation of `honk()`.

The `Bike` class also implements the `Vehicle` interface but overrides the `honk()` function with its own implementation that prints "Ring ring!".

In the `main()` function, we create instances of `Car` and `Bike` and invoke the `start()`, `stop()`, and `honk()` functions on them. The output would be:

```
Car started
Car stopped
Beep beep!
Bike started
Bike stopped
Ring ring!
```

As shown in this example, interfaces in Kotlin can have both abstract and default methods, allowing for flexible implementation and providing default behavior for common functions.
    
    
    
Interfaces with default method implementations in Kotlin share some similarities with abstract classes, but there are important differences between them:

1. Multiple Inheritance:
   - In Kotlin, a class can implement multiple interfaces, but it can only inherit from a single abstract class.
   - This means that interfaces with default implementations provide a way to achieve multiple inheritance of behavior, while abstract classes do not.

2. Constructor Involvement:
   - Abstract classes can have constructors, including primary and secondary constructors.
   - Interfaces cannot have constructors because they are not allowed to maintain state.
   - However, interfaces can have property declarations, which abstract classes can also have.

3. Open/Closed Principle:
   - Abstract classes are designed to be subclassed and extended. They provide a base implementation that subclasses can inherit and override.
   - Interfaces, on the other hand, are designed to define contracts that classes can implement. They do not provide an implementation themselves.
   - By allowing default method implementations, interfaces in Kotlin can introduce additional behavior to implementers without breaking the open/closed principle. Subclasses of abstract classes, however, are required to provide their own implementations for abstract methods.

In summary, interfaces with default method implementations in Kotlin offer a way to define contracts with default behavior that can be implemented by multiple classes. They enable multiple inheritance of behavior and allow for greater flexibility in designing class hierarchies. Abstract classes, on the other hand, are more suitable when you want to create a base class that provides a common implementation and can be extended by subclasses.
    
    
    

# Kotlin Runtime vs Compile time:
    
In Kotlin, "compile time" and "run time" are two distinct phases of a program's lifecycle. Let's explore each of these phases and provide examples to illustrate their meaning:

1. Compile Time:
   - The compile time refers to the phase when your Kotlin source code is translated into bytecode by the Kotlin compiler (such as the `kotlinc` command or the Kotlin compiler integrated in your IDE).
   - During this phase, the compiler performs syntax and type checking, generates bytecode instructions, and ensures that your code adheres to the language rules.
   - Compilation errors, if any, are detected and reported at this stage.
   - Examples of compile-time operations include:
     - Syntax validation and verification.
     - Type checking.
     - Generating bytecode.

   Example:
   ```kotlin
   fun main() {
       val name: String = "John"
       val age: Int = "25" // Compilation error: Type mismatch

       println("Name: $name, Age: $age")
   }
   ```
   In this example, during the compile-time phase, the Kotlin compiler detects a type mismatch error because we are assigning a string value to an integer variable. The error is reported, and the code fails to compile.

2. Run Time:
   - The run time refers to the phase when your compiled Kotlin bytecode is executed by the Java Virtual Machine (JVM) or any other Kotlin runtime environment.
   - During this phase, the compiled code is loaded, interpreted, and executed by the runtime environment.
   - Any runtime errors or exceptions that occur during program execution are handled at this stage.
   - Examples of run-time operations include:
     - Object instantiation.
     - Method invocation.
     - Exception handling.

   Example:
   ```kotlin
   fun main() {
       val numbers = listOf(1, 2, 3)
       val result = numbers[5] // IndexOutOfBoundsException at runtime

       println("Result: $result")
   }
   ```
   In this example, at runtime, the program attempts to access the element at index 5 in the `numbers` list. However, since the list contains only three elements, an `IndexOutOfBoundsException` occurs at runtime. The program execution halts, and the exception can be caught and handled if desired.

Understanding the distinction between compile time and run time is essential because it helps developers identify and resolve issues before the program is executed (compile-time errors) or handle unexpected situations that may occur during program execution (run-time errors).

    

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
    
    
In Kotlin, access modifiers are keywords that control the visibility and accessibility of classes, interfaces, functions, properties, and other elements within your code. There are four access modifiers available in Kotlin:

1. Public (default):
   - The public access modifier is the default modifier in Kotlin.
   - It means that the element is visible and accessible from anywhere within the same module or project.
   - If no access modifier is specified, the element is automatically considered public.

2. Private:
   - The private access modifier restricts the visibility of the element to the same declaration scope or enclosing class.
   - It means that the element is only accessible within the file or the class where it is declared.
   - Private elements cannot be accessed from outside of their declared scope, including subtypes or extensions.

3. Protected:
   - The protected access modifier allows access to the element from the same class, its subclasses, and the same file.
   - It is similar to private, but with the addition of being accessible within subclasses.
   - Protected elements are not visible outside of the file where they are declared, even if they are inherited by a subclass.

4. Internal:
   - The internal access modifier allows access to the element from any code within the same module.
   - A module in Kotlin is a set of Kotlin files compiled together, such as a Gradle module or an IntelliJ IDEA module.
   - Internal elements are not visible outside of the module boundary, meaning that they are not accessible from other modules.

Here's an example to illustrate the use of access modifiers:

```kotlin
class Example {
    private val privateProperty = 42
    protected val protectedProperty = "Hello"
    internal val internalProperty = listOf(1, 2, 3)
    
    private fun privateFunction() {
        // Can only be accessed within the same class
    }
    
    protected fun protectedFunction() {
        // Can be accessed within the same class and subclasses
    }
    
    internal fun internalFunction() {
        // Can be accessed within the same module
    }
    
    fun publicFunction() {
        // Default access modifier is public
        // Can be accessed from anywhere
    }
}
```

In the example above, `privateProperty` and `privateFunction()` are accessible only within the `Example` class. `protectedProperty` and `protectedFunction()` can also be accessed from subclasses of `Example`. `internalProperty` and `internalFunction()` are visible within the same module. Finally, `publicFunction()` has public access and can be accessed from anywhere.


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

# Kotlin Flows

Kotlin Flow is a reactive stream library for asynchronous programming in Kotlin. It allows you to write asynchronous, non-blocking code in a more readable and concise manner.

Kotlin Flow provides a sequence of values that can be emitted asynchronously over time. The values can be emitted from a producer, which can be a coroutine, a callback, or any other asynchronous source. Once emitted, the values can be processed by one or more consumers, which can be coroutines or other asynchronous components.

Kotlin Flow provides a set of operators that can be used to transform, filter, merge, and combine flows. It also supports backpressure, which allows consumers to control the rate at which they receive data from producers.

Kotlin Flow is built on top of Kotlin coroutines, which makes it easy to integrate with other coroutine-based libraries and frameworks. It is designed to be composable, type-safe, and efficient, making it a powerful tool for building reactive applications in Kotlin.

Kotlin Flows is a reactive programming library that is built on top of Kotlin coroutines. Kotlin Flows allows developers to work with asynchronous data streams in a more efficient and intuitive way by providing a set of operators and tools for transforming and manipulating data streams.

Kotlin Flows provides several benefits for Android developers, including:

Asynchronous and Non-Blocking: Kotlin Flows are asynchronous and non-blocking, which means that they can be used to handle long-running operations without blocking the main thread of the application. This can help to improve the performance and responsiveness of the application.

Type-Safe: Kotlin Flows are type-safe, which means that the compiler can help to catch errors and ensure that the data is handled correctly. This can help to reduce the number of bugs and errors in the code.

Integration with Coroutines: Kotlin Flows are built on top of Kotlin Coroutines, which means that they can be integrated easily with existing coroutine-based code. This makes it easy for developers to add reactive programming features to their existing applications.

Backpressure Handling: Kotlin Flows also provide support for backpressure handling, which means that they can handle situations where the producer is generating data faster than the consumer can consume it. This can help to prevent data loss and ensure that the application behaves correctly under load.

Overall, Kotlin Flows is a powerful reactive programming library that can help developers to build more efficient and responsive Android applications by allowing them to work with asynchronous data streams in a more intuitive way.

[https://www.youtube.com/embed/ZX8VsqNO_Ss](https://www.youtube.com/embed/ZX8VsqNO_Ss)

In Kotlin Flow, there are several operators that you can use to transform, combine, or filter the data emitted by a flow. Here are some of the most commonly used operators in Kotlin Flow, along with examples of how to use them:

1. `map` operator: This operator transforms the data emitted by a flow into a new form.

```
val numbers = flowOf(1, 2, 3)
val squaredNumbers = numbers.map { it * it }
squaredNumbers.collect { println(it) } // prints: 1, 4, 9

```

1. `filter` operator: This operator filters the data emitted by a flow based on a given predicate.

```
val numbers = flowOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
evenNumbers.collect { println(it) } // prints: 2

```

1. `flatMapConcat` operator: This operator transforms the data emitted by a flow into a new flow and then concatenates the results into a single flow.

```
val numbers = flowOf(1, 2, 3)
val oddAndEvenNumbers = numbers.flatMapConcat {
    if (it % 2 == 0) {
        flowOf("even")
    } else {
        flowOf("odd", "more odd")
    }
}
oddAndEvenNumbers.collect { println(it) } // prints: odd, more odd, even

```

1. `zip` operator: This operator combines the data emitted by two flows into pairs.

```
val numbers = flowOf(1, 2, 3)
val letters = flowOf("a", "b", "c")
val pairs = numbers.zip(letters) { number, letter ->
    "$number$letter"
}
pairs.collect { println(it) } // prints: 1a, 2b, 3c

```

1. `take` operator: This operator limits the number of elements emitted by a flow.

```
val numbers = flowOf(1, 2, 3)
val limitedNumbers = numbers.take(2)
limitedNumbers.collect { println(it) } // prints: 1, 2

```

These are just a few of the operators available in Kotlin Flow. You can find more operators in the `kotlinx.coroutines.flow` package, and you can also create custom operators by combining existing ones.

Sure, here are some more operators available in Kotlin Flow:

1. `flatMapMerge` operator: This operator transforms the data emitted by a flow into a new flow and then merges the results into a single flow.

```
val numbers = flowOf(1, 2, 3)
val words = flowOf("apple", "banana", "cherry")
val merged = numbers.flatMapMerge { number ->
    words.map { word -> "$number $word" }
}
merged.collect { println(it) } // prints: 1 apple, 1 banana, 1 cherry, 2 apple, 2 banana, 2 cherry, 3 apple, 3 banana, 3 cherry

```

1. `distinctUntilChanged` operator: This operator filters out consecutive duplicate elements emitted by a flow.

```
val numbers = flowOf(1, 2, 2, 3, 3, 3)
val distinctNumbers = numbers.distinctUntilChanged()
distinctNumbers.collect { println(it) } // prints: 1, 2, 3

```

1. `scan` operator: This operator applies an accumulator function to the data emitted by a flow and emits the intermediate results.

```
val numbers = flowOf(1, 2, 3)
val runningTotal = numbers.scan(0) { accumulator, value -> accumulator + value }
runningTotal.collect { println(it) } // prints: 0, 1, 3, 6

```

1. `onCompletion` operator: This operator allows you to perform an action when the flow completes.

```
val numbers = flowOf(1, 2, 3)
numbers
    .onCompletion { println("Done!") }
    .collect { println(it) } // prints: 1, 2, 3, Done!

```

1. `catch` operator: This operator allows you to handle exceptions that occur during the execution of a flow.

```
val numbers = flowOf(1, 2, 3).map { if (it == 2) throw Exception("Error!") else it }
numbers
    .catch { e -> println("Caught exception: $e") }
    .collect { println(it) } // prints: 1, Caught exception: java.lang.Exception: Error!, 3

```

These operators, along with the ones mentioned earlier, allow you to perform a wide range of transformations and actions on Kotlin Flow data.

In Kotlin Flow, there are two methods you can use to consume the data emitted by a flow: `collect` and `onEach`. Although both methods allow you to process the data emitted by a flow, they have some differences in their behavior.

`collect` is a suspending function that blocks the calling coroutine until the flow is completed. It emits each element of the flow to the collector, which can be used to process the data. Here's an example:

```
val numbers = flowOf(1, 2, 3)
numbers.collect { println(it) }

```

This code prints:

```
1
2
3

```

`onEach` is a non-suspending function that applies a given action to each element of the flow, and then returns a new flow that emits the same elements. Here's an example:

```
val numbers = flowOf(1, 2, 3)
numbers.onEach { println(it) }.collect()

```

This code also prints:

```
1
2
3

```

However, note that `onEach` does not block the calling coroutine and does not emit any elements to the collector. It only applies the given action to each element of the flow.

So, the main difference between `collect` and `onEach` is that `collect` is used to consume the data emitted by a flow, while `onEach` is used to apply an action to each element of the flow. If you just want to process the data emitted by a flow, use `collect`. If you want to apply an action to each element of the flow before processing it, use `onEach`.

# Coroutines

In Kotlin coroutines, a `Dispatcher` is an object that determines which thread or threads the coroutine will run on. A `Dispatcher` provides a context for the coroutine that specifies where the coroutine should execute. There are several built-in `Dispatchers` available in Kotlin coroutines, each with a different purpose. Here are some of the most commonly used dispatchers:

1. `Dispatchers.Default`: This is the default dispatcher and is designed for CPU-intensive operations. It uses a thread pool with a fixed number of threads equal to the number of CPU cores.

2. `Dispatchers.IO`: This dispatcher is designed for I/O-bound operations like reading from a file, making network requests, or working with a database. It uses a thread pool that is optimized for I/O operations and can create new threads as needed.

3. `Dispatchers.Main`: This dispatcher is designed for running coroutines on the main UI thread in Android applications. It can only be used in Android applications and is not available in other contexts.

4. `Dispatchers.Unconfined`: This dispatcher runs the coroutine on the same thread that called the suspend function. It is often used for testing or for scenarios where you need fine-grained control over which thread the coroutine is running on.

5. Custom Dispatchers: You can also create custom dispatchers to specify your own execution context for coroutines.

When you launch a coroutine using `launch`, `async`, or any other coroutine builder, you can specify the dispatcher that the coroutine should run on by providing a `CoroutineContext`. For example:

```kotlin

// Run the coroutine on the Default dispatcher

GlobalScope.launch(Dispatchers.Default) {

// Coroutine code here

}

// Run the coroutine on the IO dispatcher

GlobalScope.async(Dispatchers.IO) {

// Coroutine code here

}

```

In summary, `Dispatchers` are used to determine which thread or threads a coroutine should run on in Kotlin coroutines, and there are several built-in dispatchers available for different scenarios.

In Kotlin coroutines, a `scope` is an object that manages the lifecycle of a group of related coroutines. A `scope` is responsible for launching coroutines and ensuring that they are properly cancelled when the scope is cancelled. This helps to prevent memory leaks and ensure that coroutines are properly cleaned up when they are no longer needed.

There are several types of scopes available in Kotlin coroutines, including `GlobalScope`, `CoroutineScope`, and `SupervisorScope`.

Here are some examples of how scopes are used in coroutines:

1. `GlobalScope`: The `GlobalScope` is a singleton object that represents the global scope of coroutines in an application. When you launch a coroutine using `GlobalScope.launch`, the coroutine is launched in the global scope and will continue to run until the application process is terminated. Here's an example:

```kotlin

GlobalScope.launch {

// Coroutine code here

}

```

2. `CoroutineScope`: The `CoroutineScope` is a generic scope that can be used to launch coroutines with a specific context. When the scope is cancelled, all of the coroutines launched in the scope will be cancelled. Here's an example:

```kotlin

val myScope = CoroutineScope(Dispatchers.Default)

myScope.launch {

// Coroutine code here

}

// Cancel the scope when you're done

myScope.cancel()

```

3. `SupervisorScope`: The `SupervisorScope` is a type of `CoroutineScope` that is designed for situations where you want to launch child coroutines that can fail independently of each other. When a child coroutine fails, it does not cancel the other child coroutines. Here's an example:

```kotlin

val myScope = CoroutineScope(SupervisorJob() + Dispatchers.Default)

myScope.launch {

// Coroutine code here

}

// Cancel the scope when you're done

myScope.cancel()

```

In summary, scopes are used to manage the lifecycle of groups of related coroutines in Kotlin coroutines. They help to ensure that coroutines are properly cancelled and cleaned up when they are no longer needed, and there are several types of scopes available for different use cases.

You are correct, I apologize for not including information on `ViewModelScope` and `LifecycleScope` in my previous answer.

Here's some information on these two additional scopes:

1. `ViewModelScope`: The `ViewModelScope` is a `CoroutineScope` that is tied to the lifecycle of a `ViewModel`. When the `ViewModel` is cleared or destroyed, any coroutines launched in the `ViewModelScope` will automatically be cancelled. This helps to prevent memory leaks and ensures that coroutines are properly cleaned up when they are no longer needed. Here's an example:

```kotlin

class MyViewModel : ViewModel() {

val myScope = ViewModelScope

fun loadData() {

myScope.launch {

// Coroutine code here

}

}

}

```

2. `LifecycleScope`: The `LifecycleScope` is a `CoroutineScope` that is tied to the lifecycle of an Android `LifecycleOwner`, such as an `Activity` or `Fragment`. When the `LifecycleOwner` is destroyed, any coroutines launched in the `LifecycleScope` will automatically be cancelled. This helps to prevent memory leaks and ensures that coroutines are properly cleaned up when they are no longer needed. Here's an example:

```kotlin

class MyFragment : Fragment() {

val myScope = lifecycleScope

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {

super.onViewCreated(view, savedInstanceState)

myScope.launch {

// Coroutine code here

}

}

}

```

In summary, `ViewModelScope` and `LifecycleScope` are additional `CoroutineScope` implementations that are tied to the lifecycle of a `ViewModel` or an Android `LifecycleOwner`. They help to ensure that coroutines are properly cancelled and cleaned up when they are no longer needed, and they are useful in Android app development to prevent memory leaks and ensure that coroutines are cancelled when the associated component is destroyed.

`launch` and `async` are two functions used for launching coroutines in Kotlin.

The `launch` function is used to launch a coroutine that does not return a result. It takes a lambda as an argument and immediately returns a `Job` instance, which can be used to cancel the coroutine if needed. Here's an example:

```kotlin

val job = CoroutineScope(Dispatchers.Default).launch {

// Coroutine code here

}

```

The `async` function is used to launch a coroutine that returns a result in the form of a `Deferred` object. A `Deferred` is a non-blocking way to obtain the result of a computation that is being performed in another thread or coroutine. Here's an example:

```kotlin

val deferredResult = CoroutineScope(Dispatchers.Default).async {

// Coroutine code here

return@async 42

}

val result = deferredResult.await()

```

The `async` function returns a `Deferred` object immediately, which can be used to obtain the result of the coroutine at a later time using the `await()` function. The `await()` function blocks the current thread until the result is available.

In summary, `launch` is used to launch a coroutine that does not return a result, while `async` is used to launch a coroutine that returns a result in the form of a `Deferred` object. Both functions are used for launching coroutines in Kotlin, but they serve different purposes depending on whether or not you need to obtain a result from the coroutine.

**To make multiple coroutines give a single result in Kotlin, you can use the `async` and `await` functions from the `kotlinx.coroutines` library.**

First, you can define each coroutine using the `async` function, which returns a `Deferred` object that represents the result of the coroutine:

```
val deferred1 = async { /* coroutine 1 */ }
val deferred2 = async { /* coroutine 2 */ }
val deferred3 = async { /* coroutine 3 */ }

```

Next, you can use the `await` function to wait for all the coroutines to complete and return their results. You can use the `awaitAll` function to wait for multiple `Deferred` objects to complete:

```
val results = awaitAll(deferred1, deferred2, deferred3)

```

The `awaitAll` function will suspend the current coroutine until all the `Deferred` objects are complete, and then it will return a list of the results.

Here is an example of how you can use these functions together to run multiple coroutines and return a single result:

```
import kotlinx.coroutines.async
import kotlinx.coroutines.awaitAll
import kotlinx.coroutines.runBlocking

fun main() = runBlocking {
    val deferred1 = async { getResult1() }
    val deferred2 = async { getResult2() }
    val deferred3 = async { getResult3() }

    val results = awaitAll(deferred1, deferred2, deferred3)
    val finalResult = combineResults(results)
    println(finalResult)
}

suspend fun getResult1(): Int {
    // simulate a long-running operation
    delay(1000)
    return 1
}

suspend fun getResult2(): Int {
    // simulate a long-running operation
    delay(2000)
    return 2
}

suspend fun getResult3(): Int {
    // simulate a long-running operation
    delay(3000)
    return 3
}

fun combineResults(results: List<Int>): Int {
    return results.sum()
}

```

In this example, we define three coroutines using the `async` function, each of which simulates a long-running operation and returns a result. We then use the `awaitAll` function to wait for all three coroutines to complete, and we combine the results into a single value using the `combineResults` function. Finally, we print the final result to the console.

Kotlin Coroutine is a lightweight concurrency design pattern used to manage asynchronous and concurrent programming in Kotlin. It is used to simplify the code that involves long-running and time-consuming tasks such as network calls or file I/O.

Kotlin Coroutines are based on suspending functions, which are functions that can be paused and resumed later without blocking the main thread. This allows you to write asynchronous code that looks and behaves like synchronous code.

Here's an example of how to use Kotlin Coroutines:
fun main() {
// Start a new coroutine using the launch function
GlobalScope.launch {
// Print "Hello" after a delay of 1 second
delay(1000)
println("Hello")

```
    // Print "World" after a delay of 2 seconds
    delay(2000)
    println("World")
}

// Print "Coroutine example" immediately
println("Coroutine example")

// Wait for the coroutine to finish before exiting
Thread.sleep(3000)

```

}
In this example, we're using the launch function from the GlobalScope to start a new coroutine. We're using the delay function to pause the coroutine for a specified amount of time. After the delay, we're printing "Hello" and "World" to the console.

We're also printing "Coroutine example" immediately after starting the coroutine. However, this code will be executed on the main thread, while the coroutine runs on a separate thread. This means that "Coroutine example" will be printed first, followed by "Hello" and "World".

We're using Thread.sleep(3000) to wait for the coroutine to finish before exiting the application.

Here's another example that demonstrates how to use coroutines to retrieve data from a remote server using the Retrofit library:

suspend fun getUser(userId: String): User {
return withContext([Dispatchers.IO](http://dispatchers.io/)) {
// Create a Retrofit instance
val retrofit = Retrofit.Builder()
.baseUrl("[https://api.example.com](https://api.example.com/)")
.build()

```
    // Create an instance of the ApiService interface
    val apiService = retrofit.create(ApiService::class.java)

    // Call the getUserById method using the ApiService interface
    apiService.getUserById(userId)
}

```

}

In this example, we're using the withContext function to perform a network call in a background thread. We're passing [Dispatchers.IO](http://dispatchers.io/) as a parameter to indicate that we want the work to be performed on the I/O thread pool.

Inside the withContext block, we're creating a new instance of the Retrofit library and an instance of the ApiService interface. We're then using the getUserById method from the ApiService interface to retrieve a User object from the server.

This function is marked as suspend, which means that it can be paused and resumed later without blocking the main thread. This allows you to use it in a coroutine without worrying about blocking the UI.

In summary, Kotlin Coroutines are a powerful and easy-to-use concurrency tool that simplifies the management of asynchronous and concurrent programming in Kotlin. They allow you to write code that looks and behaves like synchronous code, but with the added benefits of concurrency and parallelism.

To make multiple coroutines give a single result in Kotlin, you can use the `async` and `await` functions from the `kotlinx.coroutines` library.

First, you can define each coroutine using the `async` function, which returns a `Deferred` object that represents the result of the coroutine:

```
val deferred1 = async { /* coroutine 1 */ }
val deferred2 = async { /* coroutine 2 */ }
val deferred3 = async { /* coroutine 3 */ }

```

Next, you can use the `await` function to wait for all the coroutines to complete and return their results. You can use the `awaitAll` function to wait for multiple `Deferred` objects to complete:

```
val results = awaitAll(deferred1, deferred2, deferred3)

```

The `awaitAll` function will suspend the current coroutine until all the `Deferred` objects are complete, and then it will return a list of the results.

Here is an example of how you can use these functions together to run multiple coroutines and return a single result:

```
import kotlinx.coroutines.async
import kotlinx.coroutines.awaitAll
import kotlinx.coroutines.runBlocking

fun main() = runBlocking {
    val deferred1 = async { getResult1() }
    val deferred2 = async { getResult2() }
    val deferred3 = async { getResult3() }

    val results = awaitAll(deferred1, deferred2, deferred3)
    val finalResult = combineResults(results)
    println(finalResult)
}

suspend fun getResult1(): Int {
    // simulate a long-running operation
    delay(1000)
    return 1
}

suspend fun getResult2(): Int {
    // simulate a long-running operation
    delay(2000)
    return 2
}

suspend fun getResult3(): Int {
    // simulate a long-running operation
    delay(3000)
    return 3
}

fun combineResults(results: List<Int>): Int {
    return results.sum()
}

```

In this example, we define three coroutines using the `async` function, each of which simulates a long-running operation and returns a result. We then use the `awaitAll` function to wait for all three coroutines to complete, and we combine the results into a single value using the `combineResults` function. Finally, we print the final result to the console.


# MVVM

MVVM (Model-View-ViewModel) is an architectural pattern that is widely used in developing Android applications. It provides a separation of concerns that makes it easy to develop and maintain complex applications. In this pattern, the user interface (View) is separated from the application logic (ViewModel), which in turn is separated from the data (Model).

The MVVM pattern consists of three main components:

Model: This component represents the data and the business logic of the application. It could be a local database or an API that retrieves data from a remote server. The Model is responsible for storing and managing the data that the application needs to function.

View: This component represents the user interface of the application. It includes all the UI elements like buttons, text fields, and other widgets that the user interacts with. The View is responsible for displaying the data to the user and receiving input from the user.

ViewModel: This component acts as a mediator between the View and the Model. It exposes the data to the View and provides methods to handle user input. The ViewModel also contains the logic that manipulates the data based on user input and updates the View accordingly.

The MVVM architecture has several benefits, including:

Separation of concerns: The separation of the three components in the MVVM pattern makes it easier to develop and maintain the application. Each component can be developed and tested independently, which makes it easier to identify and fix bugs.

Testability: The MVVM pattern makes it easier to test the application as each component can be tested separately.

Scalability: The MVVM pattern can be used to develop applications of any size, from small to large.

In an Android application, the View is implemented using XML layout files, while the ViewModel is implemented using Kotlin or Java classes. The Model can be implemented using a variety of technologies like SQLite, Room, or Retrofit.

In summary, the MVVM architecture provides a structured and organized approach to developing Android applications. It allows developers to create maintainable, testable, and scalable applications that are easy to maintain and extend over time.


# Jetpack Compose

Jetpack compose: create helloworld

    onCreate(){
    super.onCreate…
    
    setContent(){
    
    messageCard(“Android”)
    
    }
    }
    
    @Composable
    Fun messageCard(val name : String){
    Text(text = “Name is $name”)
    }
    
    @Preview
    @Composable
    fun showPreview(){
    messageCard(“Android”)
    }

# Git

## Git merge vs rebase:

- Git merge and rebase are ways to merge two branches for example we need to merge a feature branch to a master branch.
- In case of git merge, whatever commits are in the feature branch, that is merged as a single commit in the master branch and we can easily track the merge history.
- In case of rebase, the commits in feature branch is as it is replicated in the master branch. So the commit history is linear and it's difficult to track the history of commits.

[https://www.edureka.co/blog/git-rebase-vs-merge/](https://www.edureka.co/blog/git-rebase-vs-merge/)

# Android
`ComponentActivity` and `AppCompatActivity` are both base classes provided by the Android framework for building activities in Android applications. While they serve similar purposes, there are some differences between the two.

1. Inheritance:
    - `ComponentActivity`: Introduced in AndroidX library, `ComponentActivity` is the base class for activities in modern Android development. It is part of the Android Jetpack components.
    - `AppCompatActivity`: `AppCompatActivity` is a class provided by the Support Library (now deprecated in favor of AndroidX) that allows backward compatibility with older versions of Android. It extends `FragmentActivity` and provides additional features like support for the action bar.
2. AndroidX vs Support Library:
    - `ComponentActivity`: It is part of the AndroidX library, which is the successor to the Support Library. AndroidX provides improved support for modern Android development practices, including modularization and improved dependency management.
    - `AppCompatActivity`: It is part of the Support Library, which was the original library for backward compatibility in Android. The Support Library is now deprecated and AndroidX is recommended for new projects.
3. Namespace:
    - `ComponentActivity`: The `ComponentActivity` class is in the `androidx.activity` namespace.
    - `AppCompatActivity`: The `AppCompatActivity` class is in the `android.support.v7.app` namespace.
4. Theming:
    - `ComponentActivity`: `ComponentActivity` is not tied to a specific theme by default. You can set the theme for a `ComponentActivity` using the `android:theme` attribute in the manifest or programmatically.
    - `AppCompatActivity`: `AppCompatActivity` is designed to work with the Material Design theme and provides support for the app bar (action bar) out of the box. It automatically applies the theme specified in the `android:theme` attribute of the activity in the manifest.

In general, if you are starting a new project or using AndroidX, it is recommended to use `ComponentActivity`. It provides the latest features and benefits from ongoing updates and improvements. However, if you are working on a legacy project or need backward compatibility, you can use `AppCompatActivity` from the Support Library.

In Android, the Java Collections Framework is extensively used for working with data structures and collections of objects. The Java Collections Framework provides a set of classes and interfaces that allow you to store, manipulate, and retrieve collections of objects efficiently. Here are some commonly used collections in Android:

1. ArrayList: `ArrayList` is a dynamic array implementation that allows fast random access to elements. It can dynamically resize itself as elements are added or removed.
2. LinkedList: `LinkedList` is a doubly linked list implementation that provides efficient insertion and removal of elements at both ends. It is useful when frequent insertions and deletions are required.
3. HashMap: `HashMap` is a hash table-based implementation of the `Map` interface. It stores key-value pairs and provides constant-time performance for basic operations like get and put.
4. HashSet: `HashSet` is an implementation of the `Set` interface that uses a hash table internally. It stores unique elements and does not allow duplicates. The order of elements in a HashSet is not guaranteed.
5. TreeSet: `TreeSet` is an implementation of the `SortedSet` interface that uses a tree structure internally. It stores elements in a sorted order, allowing efficient retrieval and iteration in a sorted manner.
6. LinkedHashMap: `LinkedHashMap` is an implementation of the `Map` interface that maintains the insertion order of keys. It combines a hash table with a linked list to preserve the order of elements.

These collections can be used to store and manipulate data in Android applications. Depending on your requirements, you can choose the appropriate collection class that best suits your needs in terms of performance, memory usage, and order of elements.

In Android, a Handler is a class that allows you to communicate with the main thread's message queue and schedule tasks to be executed on the main thread. It provides a way to post messages or Runnable objects to the main thread from a background thread. The main use of a Handler is to perform operations on the UI thread, such as updating UI elements or triggering UI-related actions.

Here's a detailed explanation of a Handler with Kotlin examples:

1. Creating a Handler:
To create a Handler, you can instantiate it with the main thread's Looper using the `Handler(Looper.getMainLooper())` constructor. This associates the Handler with the main thread's message queue.

```
val handler = Handler(Looper.getMainLooper())

```

1. Posting a Runnable to the main thread:
You can use the `post()` method of the Handler to post a Runnable object to the main thread's message queue. The Runnable object's `run()` method will be executed on the main thread.

```
handler.post {
    // Update UI or perform UI-related actions here
    textView.text = "Hello from Handler!"
}

```

In this example, the code inside the lambda expression will be executed on the main thread, allowing you to update the UI.

1. Posting a delayed Runnable:
The `postDelayed()` method of the Handler can be used to schedule a Runnable object to be executed after a specified delay.

```
handler.postDelayed({
    // Perform delayed UI update or action here
    progressBar.visibility = View.GONE
}, 2000) // 2 seconds delay

```

In this example, the code inside the lambda expression will be executed on the main thread after a delay of 2 seconds.

1. Removing pending Runnables:
If you have posted a Runnable to the main thread but later want to cancel it before it runs, you can use the `removeCallbacks()` method of the Handler.

```
handler.removeCallbacks(runnable)

```

Here, `runnable` refers to the Runnable object that you want to remove.

Handlers can also be used for other purposes, such as sending messages between threads or implementing periodic tasks. However, for UI-related tasks, it's recommended to use `post()` or `postDelayed()` methods for simplicity.

Handlers are especially useful when you're working with background threads (e.g., using threads, services, or async operations) and need to update the UI from those threads. They allow you to safely communicate with the main thread and make UI updates without causing synchronization issues.

Remember to be mindful of potential memory leaks when using Handlers. Make sure to cancel any pending callbacks or remove them when they are no longer needed, such as in `onDestroy()` of an activity or when stopping a service.

Overall, Handlers are a powerful tool for interacting with the main thread's message queue and updating the UI from background threads in Android applications.

# Activities / Fragments

onPause: Activity is visible but not interactive. Eg if some system dialog comes up.

onStop: Activity is not visible and not interactive.

Activity lifecycle:
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy

When activity goes in background:

onStop -> onRestart -> onStart

On orientation change:

onPause -> onSaveInstanceState ->
onStop -> onDestroy -> onCreate -> onStart -> onRestoreInstanceState
-> onResume

In Android app development, `onCreate()`, `onStart()`, and `onResume()` are three different methods that are part of the Activity Lifecycle. Each of these methods is called at different stages of an Activity's lifecycle and serves a different purpose.

1. `onCreate()`: This method is called when the Activity is first created, and it is where the initialization of UI elements and other variables takes place. It is only called once during the lifecycle of an Activity.
2. `onStart()`: This method is called when the Activity becomes visible to the user, but before it actually becomes the foreground Activity. This means that the Activity is not yet interacting with the user, but it is visible on the screen.
3. `onResume()`: This method is called when the Activity is brought to the foreground and becomes the user's focus. It is the last method that is called before the Activity starts interacting with the user.

In summary, the main differences between `onCreate()`, `onStart()`, and `onResume()` are:

- `onCreate()` is called only once when the Activity is first created, and it is used for initializing UI elements and other variables.
- `onStart()` is called when the Activity becomes visible, but it is not yet interacting with the user.
- `onResume()` is called when the Activity is brought to the foreground and is ready to interact with the user.

Fragment lifecycle:
onAttact -> onCreate -> onCreateView -> onViewCreated -> onActivityCreated -> onStart -> onResume -> onPause -> onStop -> onViewDestroyed -> onDestroy -> onDetach

The lifecycle of a Fragment in Android follows a specific sequence of events. Here is an explanation of the different methods in the order they are called:

1. `onAttach()`: This method is called when the Fragment is associated with its parent Activity. This method allows the Fragment to access the Activity and other Fragments that are currently running.
2. `onCreate()`: This method is called when the Fragment is created. This method is typically used to initialize variables and set up the Fragment's state.
3. `onCreateView()`: This method is called when the Fragment's UI is being created. This is where you inflate the Fragment's layout and set up any UI elements that the Fragment needs.
4. `onViewCreated()`: This method is called when the Fragment's view hierarchy has been created. This is where you can access and manipulate the Fragment's UI elements.
5. `onActivityCreated()`: This method is called when the Fragment's parent Activity has finished its own `onCreate()` method. This is where you can interact with the Activity and set up any necessary interactions between the Fragment and the Activity.
6. `onStart()`: This method is called when the Fragment becomes visible. This is where you can start any animations or background processes that the Fragment needs.
7. `onResume()`: This method is called when the Fragment is ready to interact with the user. This is where you can start any interactions with the user and set up any listeners or event handlers.
8. `onPause()`: This method is called when the Fragment is no longer in the foreground and is being covered by another Fragment or Activity. This is where you should save any state information that the Fragment needs to retain.
9. `onStop()`: This method is called when the Fragment is no longer visible. This is where you should stop any animations or background processes that the Fragment was running.
10. `onViewDestroyed()`: This method is called when the Fragment's view hierarchy is being destroyed. This is where you can release any resources that the Fragment was using.
11. `onDestroy()`: This method is called when the Fragment is being destroyed. This is where you can release any resources that the Fragment was using and perform any final cleanup.
12. `onDetach()`: This method is called when the Fragment is no longer associated with its parent Activity. This is where you can release any references to the Activity or other Fragments that the Fragment was using.

# Recyclerview vs ListView:

Recyclerview was introduced in Android Lollipop (5) as a successor for listview which was there from API 1.

RecyclerView enforces use of Viewholder pattern thus offers recycling of views. Recycler view has layout managers which enable us to add grid or horizontal / vertical lists easily. We can also use our custom layout managers. Recycler view supports animations better as compared to listviews. Can be used with diffutils

Listview on the other hand was easier to implement. Did Not enforce user of viewholders. Thus bad for performance. Animations were hard to implement.has click listeners.

# Service

The service lifecycle in Android defines the different states that a service can go through during its lifetime. The four main states that a service can be in are:

Created: When the service is first created, it is in the created state. This is the initial state of the service and no clients are bound to it.

Started: When a client binds to the service, the service transitions to the started state. In this state, the service is running and can perform tasks in the background.

Bound: When a client binds to the service, the service transitions to the bound state. In this state, the service is running and can perform tasks in the background, but it is also able to communicate with the client.

Destroyed: When the service is no longer needed, it is destroyed and transitions to the destroyed state. In this state, the service is no longer running and all clients are unbound.

[https://data-flair.training/blogs/android-service-tutorial/](https://data-flair.training/blogs/android-service-tutorial/)

There are two types of services in Android:

Foreground Service: A foreground service is a service that the user is actively aware of and runs in the foreground with a notification. These services have a higher priority and are less likely to be terminated by the system. Examples of foreground services include music playback or navigation apps.

Background Service: A background service is a service that runs in the background without a visible user interface. These services have a lower priority and may be terminated by the system if resources are low. Examples of background services include data backups or scheduling tasks.

Additionally, there is also a special type of service called IntentService which is designed for running tasks in the background and it stops itself once the task is completed.

Another way to classify Android services is based on their lifecycle and how they are started:

Started Service: A started service is one that is started by an application component (such as an activity) using the startService() method. This type of service runs in the background and continues to run, even if the component that started it is destroyed.

Bound Service: A bound service is one that allows application components to bind to it by calling the bindService() method. This type of service runs as long as at least one component is bound to it. Once all components are unbound, the service is destroyed.

Hybrid Service: A hybrid service is a combination of a started service and a bound service. It is started using startService() and also allows binding using bindService(). It runs as long as at least one component is bound to it or started.

In summary, the main difference between these services is how they are started and how they are managed by the system. A started service runs independently of the component that starts it, while a bound service runs in conjunction with the component that binds to it.

Another way to classify services in Android is based on their functionality:

IntentService: An IntentService is a subclass of Service that handles asynchronous requests (expressed as "intents") on demand. It is designed for running tasks in background and it stops itself once the task is completed. It also provides a simple mechanism for handling the result of the task.

JobIntentService: A JobIntentService is an extension of IntentService that is compatible with Android JobScheduler. It can be used to schedule the execution of tasks in the background. It is an efficient way to run background tasks, as it uses the JobScheduler API that is optimized for battery usage.

Foreground Service: A Foreground Service is a service that runs in the foreground of the Android system, with a notification displayed to the user. It is used for tasks that are noticeable to the user, such as playing music or navigation.

Background Service: A Background Service is a service that runs in the background, without a visible user interface. It is used for tasks that are not noticeable to the user, such as data backup or scheduling tasks.

So, the IntentService and JobIntentService are classified based on their functionality, while the Foreground and Background Services are classified based on their visibility and priority.

IntentService()
There’s an additional service class, that extends Service class, IntentService Class. It is a base class for services to handle asynchronous requests. It enables running an operation on a single background. It executes long-running programs without affecting any user’s interface interaction. Intent services run and execute in the background and terminate themself as soon as they are executed completely.

Certain important features of Intent are :

It queues up the upcoming request and executes them one by one.
Once the queue is empty it stops itself, without the user’s intervention in its lifecycle.
It does proper thread management by handling the requests on a separate thread.

# Intent

In Android, an Intent is an abstract description of an operation to be performed. It is a messaging object that can be used to request an action from another app component, such as an activity, a service, or a broadcast receiver.

An Intent can contain information such as the type of action to be performed (such as ACTION_VIEW or ACTION_EDIT), the data to be acted on (such as a URL or a file), and any additional information needed to perform the action.

Intents are used in Android to allow components to communicate with each other. For example, you can use an Intent to launch an activity, start a service, or broadcast a message. When an Intent is sent, the Android system looks for the component that can handle the request and starts it.

Intents are also used to send data between activities, services, and broadcast receivers. They allow you to pass information between components and make it easy to share data between apps.

In summary, Intents are a fundamental part of the Android framework and are used to perform operations, communicate between components, and share data between apps.

There are two main types of Intents in Android:

Explicit Intents: These are used to specifically target a component, such as an activity, a service, or a broadcast receiver. You use an explicit Intent to start a specific component by specifying its class name in the Intent object explicitly. For example, you can use an explicit Intent to start a specific activity in your app.

Implicit Intents: These are used to perform a general action, such as opening a URL or sending an email. You don't specify the exact component to start, but instead describe the action to be performed. The Android system then selects the best component to handle the Intent based on the information in the Intent object. For example, you can use an implicit Intent to start a browser to open a URL.

There are also special Intents that perform specific actions, such as launching the camera app or sending a broadcast message. Additionally, there are Intents for returning results from an activity and for communicating between services.

In general, explicit Intents are used to start components within your own app, while implicit Intents are used to interact with components from other apps.

An Intent is a message object that can be used to communicate between Android components, such as activities, services, and broadcast receivers. Intents are used to start activities, start services, deliver broadcasts, and more.

There are two types of intents in Android:

Implicit intents: These intents do not specify the component to be called, but instead specify an action to be performed and the data to act on. For example, an intent to open a web page or take a photo. The Android system will then determine the best component to handle the intent based on the data provided.

Explicit intents: These intents specify the exact component to be called, such as a specific activity or service. For example, if you want to start a specific activity in your app, you would use an explicit intent.

An Intent object is created using the Intent() constructor, and it can contain several pieces of information, such as:

The action to be performed (e.g., Intent.ACTION_VIEW)
The data to act on (e.g., a specific URI or URL)
The type of data (e.g., "image/jpeg" or "text/plain")
Additional extras, such as flags or options.
Once an Intent object is created, it can be used to start an activity, a service, or deliver a broadcast.

For starting activities, the method startActivity() or startActivityForResult() is used, depending on whether you want to receive a result from the activity.

For starting a service, the method startService() is used.

For delivering a broadcast, the method sendBroadcast(), sendOrderedBroadcast() or sendStickyBroadcast() is used.

Intents are a powerful mechanism for communication and coordination in Android, and they are used extensively throughout the platform. They allow components to be loosely coupled and enable a high degree of flexibility and reusability in app development.

Explicit intents are used to specify the exact component to be called, such as a specific activity or service. They allow you to start a specific component within your own app or another app installed on the device.

When creating an explicit intent, you need to specify the component name of the activity or service you want to start. This is done using the setComponent() or setClass() method of the Intent object.

Here's an example of how to use an explicit intent to start a specific activity:

    Intent intent = new Intent();
    intent.setComponent(new ComponentName("com.example.app", "com.example.app.MyActivity"));
    startActivity(intent);

or you can use the setClass() method

    Intent intent = new Intent(this, MyActivity.class);
    startActivity(intent);

You can also include additional data or parameters in the intent, by using the putExtra() method. This can be used to pass data to the activity or service that is being started. For example:



    Intent intent = new Intent(this, MyActivity.class);
    intent.putExtra("parameter_name", "parameter_value");
    startActivity(intent);

You can also use explicit intents to start services. For example:


    Intent intent = new Intent(this, MyService.class);
    startService(intent);

When using explicit intents, the component to be called must be specified explicitly, and it must be present on the device. It's important to note that if the component doesn't exist or is not accessible, an exception will be thrown.

Explicit intents are useful when you want to start a specific component within your own app or another app installed on the device. They give you more control over the components being called and allow you to pass data to the component.

Implicit intents do not specify the component to be called, but instead specify an action to be performed and the data to act on. They allow you to perform a certain action, such as opening a web page or taking a photo, and the Android system will determine the best component to handle the intent based on the data provided.

When creating an implicit intent, you need to specify the action to be performed and the data to act on. This is done using the setAction() and setData() methods of the Intent object.

Here's an example of how to use an implicit intent to open a web page:


    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("[http://www.example.com](http://www.example.com/)"));
    startActivity(intent);

You can also include additional data or parameters in the intent, by using the putExtra() method. This can be used to pass data to the activity or service that is being started. For example:


    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("[http://www.example.com](http://www.example.com/)"));
    intent.putExtra("parameter_name", "parameter_value");
    startActivity(intent);

The Android system will then determine the best component to handle the intent based on the data provided. If there is more than one component that can handle the intent, the user will be prompted to choose one.

It's important to note that implicit intents rely on the system to find the appropriate component, and as such, they are less predictable and more error-prone than explicit intents. Additionally, if the system is unable to find a component to handle the intent, an ActivityNotFoundException will be thrown.

Implicit intents are useful when you want to perform a certain action, such as opening a web page or taking a photo, and don't care which app handles the request. They allow for more flexibility and reusability in app development by allowing multiple apps to handle the same intent.

# Notifications

In Android, you can group notifications programmatically using the NotificationCompat.Builder class and the setGroup() method
It's important to note that the group notifications feature is only available on Android 7.0 (API level 24) or higher.
the setGroup() method is used to set the group key for each of the notifications. The group key is a string that identifies the group of notifications. The setGroupSummary() method is used to set the summary notification, which is a separate notification that represents the group as a whole.


# APK / App bundle

Here are some of the key differences between AAB and APK:

Size: An AAB file is generally smaller in size than an APK file because it only contains the parts of the app that are necessary for a specific device configuration. This allows the app store (Google Play Store) to generate and serve optimized APKs for each device configuration, rather than the developer having to build and distribute multiple APKs.

Dynamic Delivery: With Android App Bundle, developers can use Dynamic delivery to deliver only the code and resources that are needed by the device at install time. This allows the app to be smaller, faster, and more efficient.

Signing: AAB and APK files must be signed before they can be distributed to users. However, AAB files use the new app signing by Google Play feature, which means that Google Play signs the app and manages the key for the developer.

Distribution: AAB files can be distributed through the Google Play Store, while APK files can be distributed through various app stores and also directly to users.

Compatibility: AAB files are compatible with Android 5.0 (API level 21) and higher, while APK files are compatible with all versions of Android.

# Broadcast

Broadcast receivers are a component in Android that allow apps to respond to system-wide broadcast announcements. They are used to receive and respond to broadcast announcements, such as the arrival of a text message or a change in network connectivity. Broadcast receivers can be registered dynamically at runtime, or declared statically in the app's AndroidManifest.xml file. Once a broadcast receiver is registered, it will receive and respond to any broadcasts that match the intent filters specified in its registration. These responses can include performing a specific task, or launching an Activity or Service in the app.

There are two types of broadcast receivers in Android:

Static Broadcast Receiver: These receivers are declared in the AndroidManifest.xml file and are always active, even when the app is not running. They are useful for listening to system-wide events such as the device booting up or the battery level dropping below a certain threshold.

Dynamic Broadcast Receiver: These receivers are registered and unregistered at runtime, typically within the lifecycle of an activity or service. They are useful for listening to local events within the app, such as a button press or the completion of a background task.

Both types of broadcast receivers can respond to broadcast intents, which are messages sent by the system or by other apps, indicating that a certain event has occurred. Broadcast receivers can also be used to send custom broadcasts within the app, allowing different components of the app to communicate with each other.

Broadcast receivers are a fundamental component of the Android operating system that allow applications to receive and respond to system-wide or application-specific events. A broadcast receiver is an Android component that listens for broadcast intents, which are messages sent by the system or by other applications.

When a broadcast intent is sent, the Android operating system identifies any broadcast receivers that have been registered to receive that intent and sends the intent to those receivers. The receivers can then perform a variety of actions in response to the intent, such as updating the user interface, playing a sound, or launching an activity.

Here are some key features and characteristics of broadcast receivers in Android:

Registration: A broadcast receiver must be registered in the AndroidManifest.xml file of an application or dynamically registered using the registerReceiver() method. When registering a broadcast receiver, developers can specify the intent filters for which the receiver should receive intents.

Intent Filters: An intent filter is a set of criteria that specify the type of intent that a broadcast receiver can receive. These criteria may include the action, data, and category of the intent.

Permissions: Some types of broadcasts require special permissions in order to be received by an application. For example, an application may need the RECEIVE_SMS permission in order to receive SMS messages.

Context: A broadcast receiver is a context-based component, which means that it must have a valid context in order to function. This context can be obtained through the onReceive() method, which is called when an intent is received.

Priority: When multiple broadcast receivers are registered to receive the same intent, the order in which they receive the intent is determined by their priority. Broadcast receivers with a higher priority receive the intent first.

Background Processing: Broadcast receivers are typically intended for short-term operations, such as updating the UI or playing a sound. If a broadcast receiver needs to perform long-running operations, it should be started in a separate thread or in a service to avoid blocking the UI thread.

In summary, broadcast receivers are a key component of the Android operating system that allow applications to receive and respond to system-wide or application-specific events. By registering a broadcast receiver and specifying the appropriate intent filters, developers can create applications that can respond to a wide variety of events and provide a better user experience.

There are two types of broadcast receivers in Android: static and dynamic.

Static Broadcast Receivers: Static broadcast receivers are declared in the AndroidManifest.xml file and are registered automatically when the application is installed. They can listen to system-wide broadcast intents, but cannot receive intents that are not declared in the manifest. Static broadcast receivers can be useful for listening to system events, such as battery low or screen on/off, but are less flexible than dynamic broadcast receivers.

Dynamic Broadcast Receivers: Dynamic broadcast receivers are registered at runtime using the registerReceiver() method and can listen to both system-wide and application-specific broadcast intents. They are more flexible than static broadcast receivers because they can be registered and unregistered at runtime, allowing the application to dynamically respond to changing conditions. Dynamic broadcast receivers can also be used to communicate between different components within an application.

Both static and dynamic broadcast receivers are important for building robust and responsive Android applications. Depending on the use case, developers may choose to use one or both types of broadcast receivers in their applications. It's important to keep in mind that broadcast receivers can have a significant impact on battery life, so it's important to use them judiciously and make sure that they are unregistering when they are no longer needed.

# CI / CD

CI/CD:
We have implemented CI/CD using Jenkins. There is a jenkins config file where we need to mention what happens during each stage of the pipeline. We also need to set up a dockerfile to configure the docker container. We are using sonarcube and test coverage reports as stages in the pipeline.

CI/CD stands for Continuous Integration and Continuous Delivery/Deployment. In the context of Android app development, CI/CD refers to a set of practices and tools that automate the process of building, testing, and deploying Android apps.

Continuous Integration (CI) is the practice of automatically building and testing an application whenever changes are made to the codebase. The goal of CI is to catch any issues early in the development cycle, before they have a chance to cause larger problems down the line.

Continuous Delivery (CD) is the practice of automatically deploying an application to a staging environment after it has been built and tested. The goal of CD is to ensure that the application is ready for release as soon as it has passed all the necessary tests.

Continuous Deployment (CD) takes things a step further by automatically deploying the application to production after it has been built and tested.

Implementing CI/CD in an Android app development workflow involves using a combination of tools and processes to automate various tasks, such as building the app, running tests, and deploying to various environments.

Here are some steps to implement CI/CD for an Android app:

Use a version control system, such as Git, to manage your codebase.

Use a build automation tool, such as Gradle, to automate the process of building your app.

Use a testing framework, such as JUnit, to automate the process of testing your app.

Use a continuous integration tool, such as Jenkins or CircleCI, to automatically build and test your app whenever changes are made to the codebase.

Use a continuous delivery/deployment tool, such as Fastlane or Google Play Console, to automatically deploy your app to various environments, such as a staging environment or production environment.

Monitor your app using tools such as Crashlytics or Firebase Analytics, to catch and fix issues as they arise.

By implementing CI/CD in your Android app development workflow, you can reduce the amount of time and effort required to build, test, and deploy your app, while also improving the overall quality of your app.

[https://medium.com/velos/setting-up-firebase-app-distribution-with-automatic-builds-from-jenkins-2587028e2a3a](https://medium.com/velos/setting-up-firebase-app-distribution-with-automatic-builds-from-jenkins-2587028e2a3a)

# Dependency Injection

Benefits of dependency injection:
Decouple modules (provide dependency from outside)
Helps with unit testing by mocking dependencies.
Improves code reusability
Reduce boilerplate code in application class since all the work to initialize dependencies is handled by injector component

Hilt vs Dagger:
Hilt can be considered as an extension of dagger. It helps us reduce the boilerplate code that's associated with dagger by generating the setup code and components by itself thus helping us ease the integration. Hilt is designed specifically for android unlike dagger which is a Java DI framework

Example injection a viewmodel using hilt is as easy as adding a few annotations.

@HiltAndroidApp
Application class

@HiltViewmodel
Viewmodel class

@HiltEntryPoint
Activity Class

In dependency injection (DI), an injector is a mechanism or framework that creates and manages the dependencies of an object and injects them into the object at runtime. The injector is responsible for resolving dependencies, creating the necessary objects, and providing them to the requesting object.

The injector typically works by first analyzing the dependencies of the object being requested, and then recursively analyzing the dependencies of those dependencies until all dependencies have been resolved. The injector then creates the necessary objects and injects them into the requesting object, often through a constructor or a setter method.

DI injectors can be implemented in different ways, depending on the programming language, framework, or library being used. Some DI frameworks use reflection or metadata to analyze and resolve dependencies, while others use configuration files or annotations.

An important feature of an injector is the ability to manage the lifetime of the objects it creates. Depending on the requirements of the application, the injector may create a new object for each request, reuse an existing object if available, or maintain a pool of objects to be reused as needed.

Overall, an injector is a key component of dependency injection, responsible for creating and managing the dependencies of an object and injecting them at runtime. It provides a powerful way to decouple objects from their dependencies and make applications more modular, maintainable, and testable.

# Testing

Local Unit test: JUnit, Mockito

[]()

Instrumentation tests(a kind of functional tests): Espresso, Robolectric, Robotium

unit test vs instrumentation test in android
Unit tests in Android are tests that are written to test individual units of code, such as a single method or class. These tests are typically run on a local development machine and do not require an Android device or emulator to run.

Instrumentation tests, on the other hand, are tests that are run on an Android device or emulator. These tests have access to the Android framework and can test the application's user interface and interactions with the system. They are typically slower to run and more complex to write than unit tests.

import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class ExampleUnitTest {
@Test
public void addition_isCorrect() {
assertEquals(4, 2 + 2);
}
}

This test case uses the assertEquals method to verify that the result of the expression 2 + 2 is equal to 4. The @Test annotation indicates that this is a test method that should be executed by the test runner.

To run this test, you'll need to create a test class in your Android project and add this code to the class. You can then run the tests using a test runner, such as the JUnit test runner provided by Android Studio. If the test passes, you'll see a green checkmark next to the test method. If the test fails, you'll see a red X and an error message indicating what went wrong.

import org.junit.Test;

import static org.junit.Assert.assertFalse;
import static org.junit.Assert.assertTrue;

public class EmailValidatorTest {

```
@Test
public void emailValidator_CorrectEmailSimple_ReturnsTrue() {
    assertTrue(EmailValidator.isValidEmail("name@email.com"));
}

@Test
public void emailValidator_CorrectEmailSubDomain_ReturnsTrue() {
    assertTrue(EmailValidator.isValidEmail("name@email.co.uk"));
}

@Test
public void emailValidator_InvalidEmailNoTld_ReturnsFalse() {
    assertFalse(EmailValidator.isValidEmail("name@email"));
}

@Test
public void emailValidator_InvalidEmailDoubleDot_ReturnsFalse() {
    assertFalse(EmailValidator.isValidEmail("name@email..com"));
}

@Test
public void emailValidator_InvalidEmailNoUsername_ReturnsFalse() {
    assertFalse(EmailValidator.isValidEmail("@email.com"));
}

```

}

In JUnit, the assert keywords are used to perform assertions or verifications in your test cases. Assertions allow you to verify that the code under test produces the expected results. If an assertion fails, the test case will fail and an error message will be reported by the test runner.

Here are some of the most commonly used assert keywords in JUnit:

assertEquals(expected, actual): Verifies that the expected and actual values are equal.
assertTrue(condition): Verifies that the condition is true.
assertFalse(condition): Verifies that the condition is false.
assertNull(object): Verifies that the object is null.
assertNotNull(object): Verifies that the object is not null.
assertArrayEquals(expectedArray, actualArray): Verifies that two arrays are equal.
You can use these assertions in your test cases to verify the behavior of your code and ensure that it works correctly. By using assertions, you can catch bugs and defects early in the development process and improve the overall quality of your code.

# Mock test

Mock testing is a technique used in software development to isolate and test individual components of a system by replacing their dependencies with "mock" objects that mimic the behavior of the real dependencies. This is useful in Android development because it allows you to test code that relies on complex or external dependencies in isolation, without requiring access to those dependencies.

One approach to implementing mock testing in Android is through the use of dependency injection (DI), a design pattern that provides a way to manage the creation and lifetime of objects in a system. There are several ways to implement DI in Android, including using a framework like Dagger or Koin. Here are some steps to follow to do mock testing in Android using dependency injection:

Identify the dependencies of the component you want to test: Before you can begin mock testing a component, you need to identify which dependencies it relies on. These might include APIs, databases, external services, or other components in your app.

Create an interface for the dependency: To make the dependency more testable, create an interface that defines the methods or properties that the component uses to interact with the dependency.

Create a mock implementation of the dependency: Next, create a mock implementation of the dependency that implements the interface you created in step 2. This mock implementation should mimic the behavior of the real dependency as closely as possible, but without relying on any external resources.

Inject the dependency into the component: Modify the component to use the interface you created in step 2 instead of directly using the real dependency. Then, use a DI framework to inject the real or mock dependency into the component.

Write tests for the component: With the mock dependency in place, you can now write tests for the component that exercise its behavior in isolation. Use the mock implementation of the dependency to simulate different scenarios, and verify that the component behaves as expected.

By using dependency injection and mock objects, you can test your Android components more thoroughly and with greater confidence that they will work as expected in production. This can help you catch bugs earlier in the development process and reduce the risk of introducing regressions when making changes to your code.

here is an example of how you can use mock testing in Android with code:

Let's say you have an Android app that retrieves data from a remote server using the Retrofit library. You want to test a component of your app that uses this library, but you don't want to rely on a real server during testing. To do this, you can use mock testing and dependency injection to substitute a mock version of the Retrofit library during testing.

First, you need to create an interface that defines the methods you want to use from the Retrofit library. For example:
interface ApiService {
@GET("/users/{userId}")
suspend fun getUserById(@Path("userId") userId: String): User
}

This interface defines a single method getUserById that retrieves a User object from the server based on an ID.

Next, you can create a mock implementation of this interface using a mocking library such as Mockito. For example:

class MockApiService : ApiService {
override suspend fun getUserById(userId: String): User {
// Return a mock User object for testing purposes
return User(userId, "Mock User")
}
}

This mock implementation simply returns a hard-coded User object instead of making a network request to the server.

Now you can modify the component you want to test to use this interface instead of the real Retrofit library. For example:

class MyComponent(private val apiService: ApiService) {
suspend fun fetchUser(userId: String): User {
// Use the ApiService interface to retrieve the user
return apiService.getUserById(userId)
}
}

This component takes an ApiService object as a constructor parameter and uses it to retrieve a user based on an ID.

Finally, in your test class, you can create an instance of the MyComponent class using the MockApiService mock implementation. For example:

class MyComponentTest {
private lateinit var myComponent: MyComponent

```
@Before
fun setUp() {
    // Create an instance of MyComponent with a mock ApiService
    val mockApiService = MockApiService()
    myComponent = MyComponent(mockApiService)
}

@Test
fun testFetchUser() {
    // Test the fetchUser method using the mock ApiService
    val user = runBlocking { myComponent.fetchUser("123") }
    assertEquals(user.id, "123")
    assertEquals(user.name, "Mock User")
}

```

}

This test class creates an instance of MyComponent with a MockApiService object, and then tests the fetchUser method using the mock object. Because the MockApiService returns a hard-coded User object, you can test the behavior of MyComponent without relying on a real server.

By using mock testing and dependency injection, you can test your Android components in isolation and with greater control over their dependencies. This can help you catch bugs and ensure that your app behaves as expected in a variety of scenarios.

# Alarm manager , Job scheduler

The AlarmManager is a class in the Android framework that allows you to schedule tasks, or "alarms," to be executed at a specific time or after a specific interval. It can be used to trigger a broadcast receiver, start a service, or execute a PendingIntent at a specific time in the future. These alarms can be scheduled to occur even if the device is in sleep mode.

The AlarmManager class provides several methods for scheduling alarms, including:

set() which schedules a repeating or non-repeating alarm that can be used to start a service or broadcast an Intent.
setExact() and setExactAndAllowWhileIdle() which schedule an alarm that will be executed at an exact time, even if the device is in power-saving mode.
setInexactRepeating() which schedules a repeating alarm that will be executed at a general time and allows the system to adjust the actual execution time to conserve power.

There are several ways to set an alarm in Android using the AlarmManager class:

setRepeating(): This method schedules a repeating alarm that will be executed at a specific interval. It takes in four parameters: type, triggerAtMillis, intervalMillis, and PendingIntent.

setAndAllowWhileIdle(): This method schedules an alarm that will be executed even if the device is in idle mode. It takes in three parameters: type, triggerAtMillis, and PendingIntent.

AlarmManager also provides a cancel() method which can be used to cancel a previously scheduled alarm.

It's important to note that AlarmManager is not a service by itself, it's just a class that allows you to schedule operations to be executed in the future. The actual operations are executed by the system by starting a Service, BroadcastReceiver or an Intent.

An alarm service is not a built-in service in Android, but it is a feature that can be implemented using the AlarmManager class which is a part of the Android framework. The AlarmManager allows you to schedule tasks, or "alarms," to be executed at a specific time or after a specific interval. It can be used to trigger a broadcast receiver, start a service, or execute a PendingIntent at a specific time in the future.

It is important to note that AlarmManager is not a service by itself, it is an class that allows you to schedule operations to be executed in future. You can use the set() method to schedule an alarm, and the cancel() method to cancel an alarm. AlarmManager can be used to schedule alarms that will be executed even if the device is in sleep mode.

In summary, AlarmManager is a feature of Android framework that allows you to schedule tasks to be executed in the future, it is not a service, but it can be used in combination with services to schedule background tasks.

Besides using AlarmManager, another way to set alarms in Android is by using the JobScheduler API. The JobScheduler is a system service that allows you to schedule background tasks to be executed at specific intervals or under certain conditions, such as when the device is connected to a certain type of network or when the battery level is high.

JobScheduler is an efficient way to schedule background tasks because it uses the system resources to optimize the task execution and conserve battery. To schedule a job, you need to create a JobInfo object that describes the job and then call the schedule() method on the JobScheduler object passing the JobInfo object.

Additionally, Firebase JobDispatcher is a third party library that can be used to schedule background tasks on Android. It is an open-source library that provides a simple API for scheduling jobs and works on Android 4.0 (API level 14) and higher. It uses the JobScheduler API on Android Lollipop and above, and the AlarmManager on pre-Lollipop devices.

In summary, AlarmManager, JobScheduler and Firebase JobDispatcher are all ways to schedule background tasks on Android, each one with its own advantages and disadvantages, it's important to choose the one that fits best to your use case.

# Jetpack components
[https://developer.android.com/jetpack/androidx/explorer?case=all](https://developer.android.com/jetpack/androidx/explorer?case=all)

Jetpack components used:
Navigation
Workmanager
Room
Hilt
Emojis
Viewmodel
Livedata
Appcompat
Permissions

Android Architecture Components is a collection of libraries that provide guidance on building robust and maintainable Android apps. These libraries are part of the larger Jetpack components that are designed to make it easier to develop Android applications by providing a set of well-tested, backward-compatible libraries.

There are several Android Architecture Components, each designed to address different aspects of app development:

Room: a SQLite object mapping library that provides an abstraction layer over SQLite to make it easier to work with databases.

LiveData: a data holder class that provides an observable stream of data to update UI components.

ViewModel: a class that provides a lifecycle-aware holder for UI-related data.

WorkManager: a library that manages background jobs that need to be executed at specific times or intervals.

Navigation: a library that provides a framework for navigating between different screens in an app.

Paging: a library that helps with handling large sets of data that need to be loaded incrementally.

Data Binding: a library that allows you to bind UI components in your layouts to data sources in your app.

Lifecycle: a library that provides a set of classes and interfaces for managing the lifecycle of your app's components.

By using these components, developers can create more scalable, maintainable, and testable apps with less code. The components are designed to work well together and provide a consistent development experience across different Android versions and device types.

# Work manager

what is workmanager in android?

WorkManager is a part of the Android Jetpack library, it is a library for managing background tasks in Android, specifically for tasks that need to be executed even if the app is closed or the device is rebooted.

WorkManager allows you to schedule and execute deferrable, asynchronous tasks that are guaranteed to run, even if the app is closed or the device is rebooted. It also allows you to chain multiple tasks together, called a "WorkSequence", to create more complex workflows.

WorkManager also allows you to set constraints on when the task should be executed, such as requiring a network connection or waiting until the device is charging. It also provides a way to monitor the status of the task and get the result of the execution.

It is built on top of the JobScheduler, AlarmManager and Firebase JobDispatcher which means that it can schedule tasks on any version of Android, and it will use the best underlying system API depending on the device capabilities, this feature makes it a great alternative to the previous libraries, since it abstracts the underlying implementation and provides a consistent API.

In summary, WorkManager is an Android library that allows you to schedule and execute background tasks in a robust and flexible way. It's a great alternative to schedule background tasks in an app, especially when those tasks need to be executed even if the app is closed or the device is rebooted.

There are several alternatives to WorkManager for scheduling background tasks in Android:

AlarmManager: The AlarmManager is a built-in service in Android that allows you to schedule tasks to be executed at a specific time or after a specific interval. However, AlarmManager does not provide an easy way to chain multiple tasks together and does not handle constraints or guarantee that the task will be executed if the app is closed or the device is rebooted.

JobScheduler: The JobScheduler is a built-in service in Android that allows you to schedule background tasks to be executed under certain conditions, such as when the device is connected to a certain type of network or when the battery level is high. However, JobScheduler does not provide an easy way to chain multiple tasks together and also it's not guaranteed to run on all versions of android.

Firebase JobDispatcher: Firebase JobDispatcher is a third-party library that can be used to schedule background tasks on Android. It is an open-source library that provides a simple API for scheduling jobs and works on Android 4.0 (API level 14) and higher. However, it does not provide an easy way to chain multiple tasks together and also it's not guaranteed to run on all versions of android.

Coroutines: Coroutines are a new way of managing concurrency in Android, introduced in Kotlin. They can be used to schedule background tasks and handle the results of those tasks. However, Coroutines are not built for handling tasks that need to be executed even if the app is closed or the device is rebooted.

RxJava: RxJava is a library that allows you to handle asynchronous tasks and chain multiple tasks together, similar to WorkManager. However, it does not provide a way to handle constraints or guarantee that the task will be executed if the app is closed or the device is rebooted.

In summary, while WorkManager provides a robust and flexible way to schedule background tasks, there are other libraries and techniques available that can be used depending on the specific needs of the project.

WorkManager and AlarmManager are both used for scheduling background tasks in Android, but they have some key differences:

Guaranteed execution: WorkManager guarantees that the background task will be executed, even if the app is closed or the device is rebooted, while AlarmManager does not guarantee that the task will be executed if the device is rebooted or the app is closed.

Constraints: WorkManager allows you to set constraints on when the task should be executed, such as requiring a network connection or waiting until the device is charging, while AlarmManager does not have this functionality.

Chaining Tasks: WorkManager allows you to chain multiple tasks together, called a "WorkSequence", to create more complex workflows, while AlarmManager does not provide an easy way to chain multiple tasks together.

Abstraction: WorkManager abstracts the underlying implementation and provides a consistent API, it uses the JobScheduler, AlarmManager and Firebase JobDispatcher depending on the device capabilities, while AlarmManager is a built-in service in Android.

Results and Data: WorkManager allows you to pass input and receive output data from the task, it also provide a way to monitor the status of the task and get the result of the execution, AlarmManager does not provide this functionality.

In summary, WorkManager provides a more robust and flexible way to schedule background tasks, it's a great alternative to schedule background tasks in an app, especially when those tasks need to be executed even if the app is closed or the device is rebooted. However, it's important to note that AlarmManager is more lightweight and suitable for simple alarm use cases.

# Singleton

In Kotlin, a thread-safe singleton can be implemented using the object keyword, which creates a class with a single instance that is lazily initialized and can be accessed from anywhere in the code.

To make the singleton thread-safe, you can use the double-checked locking pattern. Here's an example implementation:

object MySingleton {
@Volatile private var instance: MySingleton? = null

```
fun getInstance(): MySingleton {
    val currentInstance = instance
    if (currentInstance != null) {
        return currentInstance
    }

    synchronized(this) {
        var newInstance = instance
        if (newInstance == null) {
            newInstance = MySingleton()
            instance = newInstance
        }
        return newInstance
    }
}

```

}

In this implementation, the instance variable is marked as @Volatile, which ensures that any changes to it are immediately visible to all threads. The getInstance() method first checks if an instance has already been created and returns it if it exists. If not, it acquires a lock on the MySingleton object and checks again to ensure that no other threads have created the instance while waiting for the lock. If the instance still doesn't exist, it creates a new one and assigns it to the instance variable.

Note that while this implementation is thread-safe, it can still suffer from performance issues due to the overhead of acquiring and releasing the lock. An alternative approach is to use a lazy-initialized property with a thread-safe initializer function:

object MySingleton {
val instance by lazy(mode = LazyThreadSafetyMode.SYNCHRONIZED) { MySingleton() }
}

In this implementation, the instance property is lazily initialized and uses the SYNCHRONIZED mode to ensure that the initialization is thread-safe. This approach can be more efficient than the double-checked locking pattern, as the lock is only acquired once during the initialization process.

In this example, the Singleton class has a private constructor, which prevents other classes from instantiating it directly. Instead, other classes must use the getInstance() method to access the singleton instance. This method uses lazy initialization to create the instance object only if it hasn't been created already. This ensures that only one instance of the Singleton class exists at any given time.

public class Singleton {
private static Singleton instance;

```
private Singleton() {
    // private constructor to prevent instantiation
}

public static Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}

```

}

There are several ways to create a singleton class in Kotlin. Here are a few examples:

Using a companion object: In this approach, the singleton class has a companion object with a getInstance() method that returns the singleton instance. The instance object is created using lazy initialization to ensure that it is only created when it is needed. Here is an example of this approach:

//Kotlin
class MySingleton private constructor() {
companion object {
private val instance = MySingleton()

```
        fun getInstance(): MySingleton {
            return instance
        }
    }

    fun doSomething() {
        // implementation
    }
}

```

Using the object keyword: In this approach, the singleton class is declared using the object keyword. This creates a singleton object that is initialized when the class is loaded. Here is an example of this approach:

object Singleton {
// Singleton class implementation goes here
}

Using the by lazy delegate: In this approach, the instance object is created using the by lazy delegate, which ensures that the object is only created when it is first accessed. Here is an example of this approach:

class Singleton private constructor() {
companion object {
private val instance: Singleton by lazy {
Singleton()
}

```
    fun getInstance(): Singleton {
        return instance
    }
}

```

}

A static variable or method can be called without creating an instance of the class, while a singleton requires an instance to be created.
# Design Pattern
Factory pattern: Based on some conditions, it will decide which object to create. Eg. polygon factory where we pass the no. of sides.

Builder pattern: it helps us build an object where multiple parameters are involved. Eg name - age class

# SOLID Principles

The SOLID principles are a set of guidelines that were developed to help improve the design of object-oriented software. These principles are intended to make it easier to maintain and modify software over time.
Here's a brief overview of each of the SOLID principles, along with an example to illustrate each one:
Single Responsibility Principle (SRP): This principle states that a class should have only one reason to change. For example, consider a class that is responsible for connecting to a database and retrieving data. This class should not also be responsible for formatting the data or displaying it to the user. Instead, it should only be responsible for the database connection and retrieval of data.
Open/Closed Principle (OCP): This principle states that software entities (such as classes or modules) should be open for extension but closed for modification. For example, imagine that you have a class that performs some calculation, and you want to add a new type of calculation. According to the OCP, you should be able to add this new calculation by extending the class without modifying the existing code.
Liskov Substitution Principle (LSP): This principle states that objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program. For example, consider a class hierarchy where the superclass is a shape, and the subclasses are specific shapes such as circles and squares. According to the LSP, you should be able to use a circle or a square wherever a shape is expected, without affecting the correctness of the program.
Interface Segregation Principle (ISP): This principle states that clients should not be forced to depend on interfaces they do not use. For example, consider a class that provides methods for retrieving and updating data from a database. If you have a client that only needs to retrieve data, it should not have to depend on the methods for updating data, as this would violate the ISP.
Dependency Inversion Principle (DIP): This principle states that high-level modules should not depend on low-level modules, but rather both should depend on abstractions. For example, imagine that you have a class that performs some calculation and another class that depends on the first class to perform a specific calculation. According to the DIP, the second class should depend on an abstraction (such as an interface) rather than the concrete implementation of the first class. This allows you to change the implementation of the first class without affecting the second class.
The principle is used to decouple the high-level and low-level modules of an application, which allows for more flexibility and maintainability.

The Dependency Inversion Principle (DIP) is a design principle in object-oriented programming that encourages code to depend on abstractions rather than on concrete implementations. The principle states that high-level modules should not depend on low-level modules, but both should depend on abstractions.

In simpler terms, this means that instead of coding to a specific implementation, we should code to an interface or an abstract class, so that our code can be more flexible and easily interchangeable with other implementations. This allows us to make changes to the implementation of a particular feature without affecting the code that uses it.

Here's an example to illustrate this principle:

Suppose we have a class called UserService that interacts with a database to retrieve and store user data. In a tightly coupled implementation, the UserService class would depend directly on the concrete implementation of the database, like so:
class UserService {
private val database = MySQLDatabase()

```
fun getUser(id: Int): User {
    return database.getUser(id)
}

fun saveUser(user: User) {
    database.saveUser(user)
}

```

}

This tightly couples the UserService class to the MySQL database implementation, making it difficult to swap out the database for another implementation in the future. In addition, this approach violates the Dependency Inversion Principle because the UserService class depends on a low-level module, the MySQLDatabase class.

To implement DIP, we can introduce an abstraction between the UserService class and the database implementation. We can define an interface called Database that the MySQLDatabase class implements, like so:
interface Database {
fun getUser(id: Int): User
fun saveUser(user: User)
}

class MySQLDatabase : Database {
override fun getUser(id: Int): User {
// Implementation for getting user from MySQL database
}

```
override fun saveUser(user: User) {
    // Implementation for saving user to MySQL database
}

```

}

We can then modify the UserService class to depend on the Database interface, rather than the MySQLDatabase class directly:
class UserService(private val database: Database) {
fun getUser(id: Int): User {
return database.getUser(id)
}

```
fun saveUser(user: User) {
    database.saveUser(user)
}

```

}

Now, the UserService class depends on the Database interface instead of a concrete implementation, making it much more flexible and easily interchangeable with other database implementations that also implement the Database interface. This approach follows the Dependency Inversion Principle by depending on abstractions rather than concrete implementations.

# RxJava

RxJava is a Java-based library that provides a set of reactive programming types and operators that you can use to build responsive, scalable, and concurrent applications. It allows you to compose asynchronous and event-based programs by using observable sequences, and it can be used to build fast and responsive applications that can handle a high volume of concurrency.
LiveData is a lifecycle-aware data holder that allows you to observe changes to data across multiple components of your app, including activities, fragments, and services. It is designed to hold data that is used in UI and that can be observed by UI components. LiveData is lifecycle-aware, meaning it respects the lifecycle of other app components, such as activities and fragments. This allows it to automatically stop and start observing data changes when the lifecycle state of the observing component changes.
In summary, RxJava is a reactive programming library that allows you to build asynchronous and event-based programs, while LiveData is a data holder that is lifecycle-aware and can be observed by UI components. They can be used together, but they serve different purposes and are not directly related to each other.

# Serializable and Parcelable

In Android, Serializable and Parcelable are two mechanisms for transferring data between activities, services, and other Android components.

Serializable is a standard Java interface that is used to serialize an object into a stream of bytes and deserialize a stream of bytes back into an object. This is useful for storing data on disk or transferring data over a network connection. Here is an example of how to implement Serializable in an Android class:

public class MyClass implements Serializable {
private String name;
private int age;

```
public MyClass(String name, int age) {
    this.name = name;
    this.age = age;
}

// Getters and setters

```

}

Parcelable is an Android-specific interface that is used to serialize data in a more efficient way than Serializable. It is generally faster and requires less memory than Serializable, but it is more complex to implement. Here is an example of how to
implement Parcelable in an Android class:

public class MyClass implements Parcelable {
private String name;
private int age;

```
public MyClass(String name, int age) {
    this.name = name;
    this.age = age;
}

protected MyClass(Parcel in) {
    name = in.readString();
    age = in.readInt();
}

public static final Creator<MyClass> CREATOR = new Creator<MyClass>() {
    @Override
    public MyClass createFromParcel(Parcel in) {
        return new MyClass(in);
    }

    @Override
    public MyClass[] newArray(int size) {
        return new MyClass[size];
    }
};

    @Override
    public int describeContents() {
        return 0;
    }
    
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(name);
        dest.writeInt(age);
    }
    
        // Getters and setters
        
        ```
        
        }
        
```
# IPC / AIDL

There are several ways to communicate between two different apps in Android:

Intent: An Intent is a message object that can be used to communicate between Android components, such as activities, services, and broadcast receivers.

Content Provider: A Content Provider is a component that allows apps to share data with other apps.

AIDL: Android Interface Definition Language (AIDL) is a way to define a service interface that can be used to communicate between apps.

Broadcast Receiver: A Broadcast Receiver is a component that allows apps to receive and respond to system-wide broadcast announcements.

Services: Services are another way to establish communication between apps, as they allow an app to initiate a long-running operation in the background.

Binder : Binder is Android's inter-process communication (IPC) mechanism that allows different processes to communicate with one another.

Android Interface Definition Language (AIDL) is a way to define a service interface that can be used to communicate between apps. It allows apps to call methods on a service, which runs in a separate process, in a way that is both safe and efficient.

AIDL is used to define the interface of a service, which is a set of methods that can be called by other apps. The service implementation can then be provided by any app that wants to offer the service. Once the interface is defined, apps can bind to the service and call its methods as if it were running in the same process.

AIDL uses IPC (Inter-Process Communication) mechanism to communicate between apps, it is used when the service is running in a different process than the client. AIDL automatically generates the necessary code to handle the communication between the client and service using a process called marshalling.

AIDL is particularly useful for implementing services that need to handle complex data types, such as custom objects. It also allows for multi-threading, allowing multiple clients to bind to the service simultaneously without blocking each other.

Overall AIDL is useful when different apps need to share complex data types, and when a service needs to handle multiple requests at the same time.

An alternative to AIDL for inter-process communication (IPC) in Android is using a Messenger. A Messenger is a lightweight wrapper around a Handler that allows for message passing between processes.

Messenger is simpler to use than AIDL, since it only requires the developer to define a Handler and pass it to the Messenger. The developer does not need to define an interface or deal with marshalling.

Another alternative is using Binder, it is Android's built-in IPC mechanism that allows different processes to communicate with one another. Binder is used behind the scenes in many Android system services, such as the activity manager and the window manager. Binder uses a transaction-based model, which means that you can send requests and receive responses, this is similar to AIDL.

Overall, the choice of IPC mechanism will depend on the specific requirements of the app. AIDL is useful when the service needs to handle complex data types or handle multiple requests simultaneously, while Messenger is simpler to use and is suitable for simple communication needs. Binder is also a good option when the app needs a powerful and flexible IPC mechanism.


# OOPs

method overloading vs overriding
Method overloading and method overriding are two concepts in object-oriented programming that are related to the way methods are defined and used in classes.

Method overloading refers to the ability of a class to have two or more methods with the same name but with different parameters. The compiler determines which method to call based on the number and types of arguments passed to the method.

Method overriding, on the other hand, refers to the ability of a subclass to provide a specific implementation of a method that is already defined in its superclass. The subclass' implementation of the method "overrides" the superclass' implementation. The subclass' method is called instead of the superclass' method when the method is called on an instance of the subclass.

It's important to note that when a method is overridden, the new method should have the same name, number of parameters, and parameter types as the method being overridden. Also, the method should be marked with the override keyword.

In summary, method overloading allows for multiple methods with the same name but different parameters to be defined in the same class, while method overriding allows for a subclass to provide a new implementation for a method already defined in its superclass.

In Android, OOPs concepts are implemented in various ways, some examples include:

Inheritance - This concept is used in Android when we extend the functionality of existing classes. For example, extending the AppCompatActivity class to create a custom activity class.

Abstraction - This concept is used in Android through the use of abstract classes, which define the common behavior of a group of related classes. For example, View class is an abstract class that provides basic drawing and event handling functionality for visual components.

Encapsulation - This concept is used in Android through the use of private member variables and methods, which are hidden from other parts of the code and can only be accessed through public methods. For example, the implementation details of a database adapter class can be hidden from other parts of the code using encapsulation.

Polymorphism - This concept is used in Android through the use of interfaces, which allow multiple classes to implement a common behavior in different ways. For example, the OnClickListener interface is used to handle button clicks in different ways for different classes that implement it.

These are just a few examples of how OOPs concepts are used in Android development.

Java does not support multiple inheritance, which means a class cannot inherit from multiple parent classes simultaneously. There are a few reasons why Java was designed this way:

1. Simplicity and clarity: Java aims to be a simple and easy-to-understand language. Multiple inheritance can introduce complexities and ambiguities, especially when conflicts arise between methods or member variables inherited from different parent classes. By disallowing multiple inheritance, Java avoids these issues and promotes clearer code.
2. Diamond problem: The diamond problem occurs when a class inherits from two or more classes that have a common parent. If those parent classes have methods with the same name and signature, it becomes unclear which implementation should be used. This ambiguity is referred to as the diamond problem, and languages that support multiple inheritance need to address it explicitly. Java avoids this problem by not allowing multiple inheritance.
3. Interface-based design: In Java, interfaces provide a mechanism for achieving multiple inheritance of types. By implementing multiple interfaces, a class can inherit multiple sets of behavior. This approach promotes a more flexible and modular design, as interfaces allow classes to be loosely coupled and provide contract-based programming.
4. Code reuse through composition: Instead of relying on multiple inheritance, Java encourages code reuse through composition, where classes are constructed by combining multiple objects of other classes. Composition offers a more flexible and maintainable approach to code reuse, as it allows for greater modularity and avoids the complexities associated with multiple inheritance.

While multiple inheritance can be useful in certain scenarios, Java's designers made a deliberate decision to prioritize simplicity, clarity, and maintainability by omitting this feature from the language.

REST, which stands for Representational State Transfer, is an architectural style for building web services that are scalable, flexible, and easy to maintain. RESTful APIs allow applications to communicate with each other over the internet using HTTP protocols.

At a high level, a REST API consists of resources, which are typically represented by URLs. These resources can be anything that can be identified by a unique identifier, such as users, products, or blog posts.

The REST API uses HTTP methods such as GET, POST, PUT, and DELETE to perform CRUD (Create, Read, Update, Delete) operations on these resources. For example, a GET request to the URL `/users` might return a list of all the users in a system, while a POST request to the URL `/users` might create a new user.

REST APIs typically return data in a format that is easily consumable by client applications, such as JSON or XML. This data can be used by the client application to display information to users or perform further processing.

Overall, REST APIs are a popular choice for building web services due to their simplicity, scalability, and flexibility.

# App Cache

To increase the cache size of an Android application, you can follow these steps:

Modify the cache size in the onCreate() method of your Application class: You can set the cache size by using the getCacheDir() method of the Context class. For example:

val cacheSize = 5 * 1024 * 1024(max cache can be as big as free memory)
val myCache = Cache(context.cacheDir, cacheSize.toLong())

Use a third-party caching library: There are several third-party caching libraries available for Android that you can use to manage the cache size. For example, Glide, Picasso, or Fresco, all provide an easy way to increase the cache size.

Modify the cache size in the onLowMemory() method: If you need to increase the cache size dynamically based on the available memory, you can use the onLowMemory() method to monitor the memory and increase the cache size as needed.

It's important to note that increasing the cache size will consume more memory, so it's important to strike a balance between performance and memory usage. Additionally, it's important to regularly clean the cache to prevent it from becoming too large and causing performance issues.

# ViewModel in detail

The ViewModel component is a lifecycle-aware component that allows data to survive configuration changes such as orientation changes, without the need to reload data from scratch. When an orientation change occurs, the activity or fragment that contains the ViewModel is destroyed and recreated. However, the ViewModel instance is not destroyed along with it and is instead retained in memory by the ViewModelStore.

The ViewModelStore is a key-value store that is responsible for holding onto ViewModel instances. When a new activity or fragment is created after an orientation change, it can retrieve the existing ViewModel instance from the ViewModelStore instead of creating a new one. This ensures that any data that was previously loaded and stored in the ViewModel is retained and can be accessed by the newly created activity or fragment.

The ViewModelStore is managed by the activity or fragment's ViewModelProvider. The ViewModelProvider is responsible for creating and managing the lifecycle of ViewModel instances. When an activity or fragment is first created, it creates a ViewModelProvider instance and initializes the ViewModelStore associated with it. This ViewModelProvider is then used to create and retrieve ViewModel instances associated with the activity or fragment.

The ViewModelProvider ensures that the ViewModel instances are destroyed when they are no longer needed, such as when the associated activity or fragment is destroyed. This ensures that the ViewModel instances do not continue to occupy memory unnecessarily.

In summary, the ViewModel survives orientation changes because it is retained in memory by the ViewModelStore. The ViewModelStore is managed by the activity or fragment's ViewModelProvider, which ensures that the ViewModel instances are created, retrieved, and destroyed appropriately based on the lifecycle of the associated activity or fragment. This ensures that data is not lost and can be accessed by the newly created activity or fragment.

The ViewModelStore is a simple key-value store that is used to hold and manage ViewModel instances in an Android app. It is a part of the ViewModel architecture component introduced by Google in the Android Jetpack library. The ViewModelStore is responsible for retaining ViewModel instances across configuration changes such as screen rotation or device configuration changes.

Each activity or fragment has its own instance of the ViewModelStore, and it manages the ViewModel instances associated with that activity or fragment. The ViewModelStore is a key-value map that stores the ViewModel instances using a unique key generated by the system. The key is used to identify and retrieve the ViewModel instance from the store.

The ViewModelStore is created and initialized by the ViewModelProvider, which is a factory class that creates and manages ViewModel instances. When an activity or fragment is created, it initializes its own instance of the ViewModelProvider, which in turn creates and initializes the ViewModelStore associated with that activity or fragment. The ViewModelProvider also creates and retrieves ViewModel instances associated with that activity or fragment using the ViewModelStore.

When an activity or fragment is destroyed, the ViewModel instances associated with it are removed from the ViewModelStore. This ensures that ViewModel instances are only retained as long as they are needed and that they do not continue to occupy memory unnecessarily.

In summary, the ViewModelStore is a simple key-value store that is used to hold and manage ViewModel instances in an Android app. It is responsible for retaining ViewModel instances across configuration changes and ensures that ViewModel instances are only retained as long as they are needed. The ViewModelStore is created and initialized by the ViewModelProvider, which is a factory class that creates and manages ViewModel instances.

The ViewModelStore is not destroyed by the ViewModelProvider on configuration changes because its purpose is to retain ViewModel instances across configuration changes.

When an activity or fragment is destroyed and recreated due to a configuration change such as screen rotation, the ViewModelProvider creates a new instance of the activity or fragment and initializes its own instance of the ViewModelStore. The ViewModelProvider then retrieves the ViewModel instances associated with the activity or fragment from the existing ViewModelStore and associates them with the new instance of the activity or fragment. This ensures that any data that was previously loaded and stored in the ViewModel is retained and can be accessed by the newly created activity or fragment.

If the ViewModelStore were destroyed on configuration changes, then the ViewModel instances stored within it would be lost, and the app would need to reload the data from scratch. This would result in a poor user experience as the user would have to wait for the data to be reloaded, which can take a considerable amount of time.

In summary, the ViewModelStore is not destroyed by the ViewModelProvider on configuration changes because its purpose is to retain ViewModel instances across configuration changes. This ensures that any data that was previously loaded and stored in the ViewModel is retained and can be accessed by the newly created activity or fragment, resulting in a better user experience.

### 

ViewModelProviders is a class in the Android Architecture Components library that provides an easy way to instantiate a ViewModel object in an Android application.

The ViewModel class is designed to store and manage UI-related data that survives configuration changes such as screen rotations. By using a ViewModel object, you can ensure that your UI data is preserved across device rotations and other configuration changes, and is available to your UI components such as Activities and Fragments.

ViewModelProviders provides a factory class that can be used to create instances of ViewModel objects. By using this factory, you can ensure that each instance of a ViewModel object is associated with a specific scope, such as a particular Activity or Fragment. This allows the ViewModel object to be properly initialized and maintained throughout the lifecycle of the associated UI component.

In summary, ViewModelProviders is a utility class that makes it easy to create and manage ViewModel objects in an Android application, and is an essential part of the Android Architecture Components library.


# Memory leaks / Profiler

Memory leaks are a common issue in Android development, and they occur when objects are allocated and never released, causing the memory usage of an app to grow over time. These leaks can cause performance issues, crashes, and other unexpected behaviors. In this answer, we'll discuss various ways to check and handle memory leaks in Android.

Use Profiler tools: Android Studio provides a number of profiler tools to analyze the memory usage of an app, including the Memory Profiler, Allocation Tracker, and Heap Dump Analyzer. These tools can help identify memory leaks by providing detailed information about memory usage and allocations.

Use LeakCanary: LeakCanary is an open-source library that can detect and report memory leaks in Android apps. It works by monitoring the memory allocation of an app and reporting any leaks it detects. LeakCanary can be added to an app as a dependency, and it will automatically detect and report memory leaks.

Be aware of Context usage: Context objects are a common source of memory leaks in Android apps, as they can hold onto resources that are no longer needed. It's important to be aware of how Context objects are used in an app and ensure that they are released when they are no longer needed.

Avoid static references: Static references can also cause memory leaks in Android apps, as they prevent objects from being garbage collected. It's important to avoid using static references whenever possible and instead use weak references or other techniques to manage object lifecycles.

Use the lifecycle-aware components: The Android Jetpack library provides a number of lifecycle-aware components that can help manage object lifecycles and prevent memory leaks. These components, including ViewModel and LiveData, are designed to be aware of the lifecycle of an activity or fragment and can automatically release resources when they are no longer needed.

Test thoroughly: It's important to thoroughly test an app for memory leaks before releasing it. This includes testing the app under different conditions, such as low memory situations, to ensure that it can handle memory management properly.

In summary, memory leaks are a common issue in Android development, but there are various ways to check and handle them. These include using profiler tools, using LeakCanary, being aware of Context usage, avoiding static references, using lifecycle-aware components, and testing thoroughly. By following these best practices, developers can minimize the risk of memory leaks and ensure that their apps perform well over time.

Android Studio provides a number of profiler tools that can help analyze the performance of an Android app, including the CPU Profiler, Memory Profiler, Network Profiler, and Battery Profiler. In this answer, we'll focus on how to use the Memory Profiler to analyze memory usage in an Android app.

Here's an example of how to use the Memory Profiler:

Open the app in Android Studio: To use the Memory Profiler, you'll need to open your app in Android Studio.

Start the Memory Profiler: Once your app is open, select "Profile" from the top menu and then select "Memory" from the dropdown. This will open the Memory Profiler.

Run your app: With the Memory Profiler open, run your app in the emulator or on a physical device. You should see the memory usage of your app displayed in the Memory Profiler.

Analyze the memory usage: The Memory Profiler provides a number of tools to analyze memory usage, including a heap dump, which shows the memory usage of your app at a specific point in time. To take a heap dump, click on the "Dump Java Heap" button in the Memory Profiler.

Identify memory leaks: Once you have a heap dump, you can use the Memory Profiler to identify memory leaks in your app. Look for objects that are holding onto memory but are no longer being used, or objects that are being created multiple times when they should only be created once.

Fix the memory leaks: Once you've identified memory leaks in your app, you'll need to fix them. This may involve releasing objects when they are no longer needed, using weak references instead of strong references, or reusing objects instead of creating new ones.

In addition to the Memory Profiler, Android Studio provides a number of other profiler tools that can help analyze the performance of an app. By using these tools, developers can identify and fix performance issues in their apps, resulting in a better user experience.

Using the Android Profiler tool is one of the most effective ways to identify memory leaks in an Android app. Here are the steps to use the Profiler to identify memory leaks:

Open Android Studio and open your project.

Click on "View" in the top menu and select "Tool Windows" and then "Profiler".

Connect a device or emulator and click on the "Attach Debugger to Android Process" button.

Select the "Memory" tab in the Profiler window.

Click on the "Record" button to start profiling your app.

Use your app normally and perform various actions that may trigger memory usage.

After you have used your app for a while, click on the "Stop" button in the Profiler to stop recording.

Inspect the memory usage graph for any sudden spikes or increases in memory usage.

Click on the "Allocations" dropdown and select "Allocation Tracking" to view detailed information about the memory allocations in your app.

Look for any objects that are being repeatedly allocated but are not being deallocated, as this may indicate a memory leak.

Use the "Callstacks" tab to identify where in your code the objects are being allocated and not deallocated.

Fix the memory leak by identifying the cause and making the necessary code changes.

By following these steps, you can effectively use the Android Profiler to identify memory leaks in your app and take the necessary steps to fix them. It's important to note that memory leaks can have a significant impact on the performance and stability of your app, so it's important to regularly use the Profiler to monitor and identify any potential issues.

# Operators

`map` and `filter` are both methods used in functional programming to transform collections of data. Here's a brief explanation of each:

`map`:

- Transforms each element of a collection based on a given function
- Returns a new collection with the transformed elements
- The number of elements in the new collection is the same as the original collection

Example:

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

List<Integer> doubledNumbers = numbers.stream()
                                      .map(number -> number * 2)
                                      .collect(Collectors.toList());

```

In this example, we start with a list of integers and use the `map` method to double each number. The resulting `doubledNumbers` list contains the same number of elements as the original `numbers` list, but each element has been transformed.

`filter`:

- Selects only the elements of a collection that satisfy a given condition
- Returns a new collection with the selected elements
- The number of elements in the new collection can be less than or equal to the number of elements in the original collection

Example:

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

List<Integer> evenNumbers = numbers.stream()
                                    .filter(number -> number % 2 == 0)
                                    .collect(Collectors.toList());

```

In this example, we start with a list of integers and use the `filter` method to keep only the even numbers. The resulting `evenNumbers` list contains only the even elements from the original `numbers` list.

In summary, `map` is used to transform each element of a collection, while `filter` is used to select only certain elements from a collection based on a given condition. Both `map` and `filter` are powerful tools in functional programming that can help you write more expressive, efficient, and maintainable code.

Yes, there are several other functions used for transforming data in functional programming. Here are a few examples:

1. `reduce`:
- Combines the elements of a collection into a single value
- The result of the function is passed as an accumulator to the next element in the collection

Example:

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int sum = numbers.stream()
                 .reduce(0, (accumulator, number) -> accumulator + number);

```

In this example, we use the `reduce` method to sum the elements of the `numbers` list. We start with an initial value of `0` and then add each element to the accumulator to get the final sum.

1. `flatMap`:
- Transforms each element of a collection into a stream of elements
- Flattens the resulting stream of streams into a single stream

Example:

```
List<List<Integer>> listOfLists = Arrays.asList(
    Arrays.asList(1, 2),
    Arrays.asList(3, 4),
    Arrays.asList(5, 6)
);

List<Integer> flatList = listOfLists.stream()
                                     .flatMap(list -> list.stream())
                                     .collect(Collectors.toList());

```

In this example, we start with a list of lists and use the `flatMap` method to flatten it into a single list. Each sub-list is transformed into a stream of integers, and then the resulting streams are flattened into a single stream of integers.

1. `distinct`:
- Removes duplicate elements from a collection

Example:

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 2, 4);

List<Integer> uniqueNumbers = numbers.stream()
                                      .distinct()
                                      .collect(Collectors.toList());

```

In this example, we start with a list of integers that contains duplicates. We use the `distinct` method to remove the duplicates and get a list of unique numbers.

These are just a few examples of the many transform functions available in functional programming. By combining these functions in different ways, you can build powerful and expressive programs that are easy to reason about and maintain.

You're right, I apologize for omitting `switchMap` in my previous response. `switchMap` is a transform function commonly used in reactive programming, particularly with RxJava and Kotlin Flow. Here's a brief explanation of what it does:

`switchMap`:

- Maps each element of a stream to a new stream, and then switches to emitting the items from the most recently mapped stream
- Can be used to cancel previous emissions and start a new stream when a new event occurs

Example:

```
val searchQueryFlow: Flow<String> = searchQueryEditText.textChanges()
    .debounce(500)
    .map { it.toString() }
    .distinctUntilChanged()

val searchResultsFlow: Flow<List<SearchResult>> = searchQueryFlow
    .flatMapLatest { query ->
        repository.search(query)
    }

```

In this example, we have a search box (`searchQueryEditText`) that emits a stream of text changes. We use `debounce` to only emit events when the user stops typing for 500 milliseconds, and then map each text change to a string. We also use `distinctUntilChanged` to only emit a new query when the text has actually changed.

Next, we use `flatMapLatest` to map each search query to a new stream of search results from the repository. The `flatMapLatest` operator will cancel any previous search requests that are still in progress and only emit the results of the most recent search query.

So, `switchMap` is a powerful transform function that can be used to dynamically switch between streams and cancel previous emissions when new events occur. It's especially useful in scenarios where you need to perform expensive operations, such as network requests or database queries, in response to user input.

There are a few other transform functions that are commonly used in reactive programming. Here's a brief explanation of each:

1. `concatMap`:
- Maps each element of a stream to a new stream, and then concatenates the emissions from each mapped stream in the order they were emitted
- Useful when you need to maintain the order of emissions, but some emissions may take longer to complete than others

Example:

```
val clicksFlow: Flow<Int> = buttonClicks.asFlow()

val resultsFlow: Flow<String> = clicksFlow
    .concatMap { clickCount ->
        repository.fetchResults(clickCount)
    }

```

In this example, we have a button that emits a stream of click events. We use `concatMap` to map each click event to a new stream of results from the repository, and then concatenate the emissions from each stream in the order they were emitted. This ensures that the results for each click event are emitted in the correct order, even if some requests take longer than others.

1. `buffer`:
- Collects a certain number of emissions into a buffer, and then emits the buffer as a single element
- Useful when you need to batch emissions to reduce overhead or optimize performance

Example:

```
val clicksFlow: Flow<Int> = buttonClicks.asFlow()

val bufferFlow: Flow<List<Int>> = clicksFlow
    .buffer(10)

```

In this example, we have a button that emits a stream of click events. We use `buffer` to collect 10 click events into a buffer, and then emit the buffer as a single list of integers. This reduces the number of emissions and can help optimize performance in some scenarios.

1. `scan`:
- Applies a function to each element of a stream, and emits the intermediate results as they're generated
- Useful for calculating running totals, accumulating values, or generating a sequence of intermediate results

Example:

```
val numbersFlow: Flow<Int> = numbers.asFlow()

val sumFlow: Flow<Int> = numbersFlow
    .scan(0) { accumulator, number ->
        accumulator + number
    }

```

In this example, we have a stream of numbers. We use `scan` to calculate the running sum of the numbers, by adding each number to the accumulator and emitting the intermediate result as it's generated.

These are just a few more examples of the many transform functions available in reactive programming. By combining these functions in different ways, you can build powerful and expressive programs that are highly responsive and resilient to errors.

# DSA 
[https://leetcode.com/problems/house-robber/](https://leetcode.com/problems/house-robber/)  ✅
[https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)  
[https://leetcode.com/problems/valid-palindrome/](https://leetcode.com/problems/valid-palindrome/) ✅
[https://leetcode.com/problemset/all/?difficulty=EASY&page=1](https://leetcode.com/problemset/all/?difficulty=EASY&page=1) [https://leetcode.com/problemset/all/?difficulty=MEDIUM&page=1](https://leetcode.com/problemset/all/?difficulty=MEDIUM&page=1) 
[https://leetcode.com/problems/number-of-islands](https://leetcode.com/problems/number-of-islands/)

Reverse part of linked list
max sum subarray ✅
Swap 2 numbers without using a third variable ✅
Prime number✅, reverse string✅, reverse number✅
count digits✅, fibonacci, maxprofit

write a program to find the nth largest number in an array of integers
Arrange 1 's and 0's in a given array

1. Arrays and Strings:
    - Find the missing number in an array of integers.
    - Determine if two strings are anagrams of each other.
    - Rotate an array by a given number of positions.
2. Linked Lists:
    - Reverse a linked list.
    - Detect and remove a loop in a linked list.
    - Merge two sorted linked lists.
3. Trees and Graphs:
    - Find the lowest common ancestor of two nodes in a binary tree.
    - Perform a depth-first search (DFS) or breadth-first search (BFS) on a graph.
    - Determine if a binary tree is a binary search tree (BST).
4. Sorting and Searching:
    - Implement various sorting algorithms (e.g., bubble sort, merge sort, quicksort).
    - Find the kth largest element in an unsorted array.
    - Perform a binary search on a sorted array.
5. Dynamic Programming:
    - Find the maximum sum of a subarray within an array.
    - Compute the nth Fibonacci number using dynamic programming.
    - Determine the longest common subsequence between two strings.

Design pattern in android
 Singleton (Creational pattern)
 Factory (Creational pattern)
 Builder (Creational pattern)
 Dependency Injection (Creational pattern)
 Adapter (structural pattern)

benefits of clean architecture

- Reduces code duplicacy in viewmodel
- Declutter viewmodels
- Screaming architecture. We can get to know the functionalities just by looking at the packages

multiple inheritance : not allowed in java , diamond problem, ambiguity
interfaces with same functions : it will have a single interface definition for both
methods with same name and different return types : error. not allowed. name and params are important

When you click on a button in Activity A and navigate to Activity B in Android, the following is the sequence of lifecycle events that occur:

1. Activity A's `onPause()` method is called. This method is used to pause any ongoing operations and prepare for the transition to the next activity.
2. Activity B is created by calling its constructor and then the following methods are invoked in order:
    - Activity B's `onCreate()` method is called. This is where you perform initialization tasks, such as setting up the UI and initializing variables.
    - Activity B's `onStart()` method is called. This indicates that the activity is about to become visible to the user.
    - Activity B's `onResume()` method is called. This method completes the activity's transition to the foreground and the activity becomes interactive.
3. Activity A's `onStop()` method is called. This indicates that the activity is no longer visible to the user and is stopped. It allows the system to reclaim resources used by Activity A.

Note: If Activity A is no longer needed, its `onDestroy()` method will be called after `onStop()`.

1. When you navigate back to Activity A from Activity B, the following lifecycle events occur:
    - Activity B's `onPause()` method is called. This indicates that Activity B is being paused as it is no longer in the foreground.
    - Activity A's `onRestart()` method is called. This indicates that Activity A is being brought back to the foreground after being stopped.
    - Activity A's `onStart()` method is called. This indicates that Activity A is about to become visible to the user again.
    - Activity A's `onResume()` method is called. This completes the transition, and Activity A becomes interactive again.

It's important to handle the lifecycle events properly in each activity to ensure that resources are managed efficiently and the user experience is smooth during the transitions between activities.

An example of the Adapter pattern in Android can be seen in the usage of RecyclerView and its associated adapters. RecyclerView is a powerful and flexible UI component used for displaying large sets of data efficiently. It requires an adapter to bind the data to the individual views within the RecyclerView.

Let's say you have an existing list of objects that you want to display in a RecyclerView. However, the objects are in a format that is incompatible with the RecyclerView's adapter requirements. In this case, you can create an adapter that acts as a bridge between your existing objects and the RecyclerView. This adapter would be an example of the Adapter pattern.

Here's an example implementation:

```
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
    private List<MyObject> data;

    public MyAdapter(List<MyObject> data) {
        this.data = data;
    }

    // Create ViewHolder and bind data to views
    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        // Inflate layout for each item
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_layout, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        MyObject object = data.get(position);
        // Bind data to views within the ViewHolder
        holder.textView.setText(object.getName());
        // ...
    }

    @Override
    public int getItemCount() {
        return data.size();
    }

    // ViewHolder class
    public static class ViewHolder extends RecyclerView.ViewHolder {
        TextView textView;

        public ViewHolder(View itemView) {
            super(itemView);
            // Initialize views within the ViewHolder
            textView = itemView.findViewById(R.id.text_view);
            // ...
        }
    }
}

```

In this example, `MyAdapter` acts as the adapter for the RecyclerView. It takes a list of `MyObject` as input and adapts it to work with the RecyclerView's requirements. The adapter inflates the layout for each item, binds the data to the views within the `ViewHolder`, and handles the total item count.

By implementing this adapter, you can seamlessly connect your existing list of `MyObject` with the RecyclerView and display the data correctly. The Adapter pattern allows you to adapt your existing object structure to work with the RecyclerView component, providing a bridge between the incompatible interfaces.

Memory leaks in Android can occur when objects are no longer needed by an application but are still held in memory, preventing the garbage collector from reclaiming the memory they occupy. Here are some common examples of memory leaks in Android:

1. Activity/Fragment Leaks:
- Holding a reference to an `Activity` or `Fragment` beyond its lifecycle: If a long-lived object holds a reference to an `Activity` or `Fragment`, it can prevent them from being garbage collected even after they are destroyed. This commonly happens when using inner classes or static variables.
1. Context Leaks:
- Holding onto a `Context` object unnecessarily: Holding a reference to a `Context` object (such as an `Activity` or `Service`) for a longer duration than needed can cause memory leaks. This can happen when passing a `Context` to a static or long-lived object.
1. Listener Leaks:
- Not unregistering listeners or callbacks: If listeners or callbacks are registered with an object but not unregistered when no longer needed, the object holding the reference to the listener can stay in memory even after the associated component is destroyed.
1. Thread Leaks:
- Mishandling of background threads: Starting a background thread (e.g., using `Thread` or `AsyncTask`) and not properly managing its lifecycle or stopping it can lead to thread leaks, as the thread may keep running and holding resources unnecessarily.
1. Bitmap and Drawable Leaks:
- Not releasing references to large Bitmaps or Drawables: Large bitmaps or drawables can consume a significant amount of memory. If not properly released when no longer needed (e.g., in `onDestroy()`), they can cause memory leaks.
1. Singleton Leaks:
- Improper implementation of singletons: If a singleton instance holds onto references that should be released or keeps growing over time, it can cause memory leaks. It's important to ensure proper cleanup and lifecycle management within singleton implementations.

To avoid memory leaks, it's crucial to follow best practices such as releasing references to objects when they are no longer needed, unregistering listeners and callbacks, using weak references where appropriate, and being mindful of the lifecycle of objects and components.

Additionally, tools like the Android Profiler, LeakCanary, and lint checks can help identify and detect memory leaks during development and testing stages, enabling you to address them promptly.

