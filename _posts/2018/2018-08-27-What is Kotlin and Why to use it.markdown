---
layout: post
title:  "What is Kotlin and Why should you use it?"
date:   2018-08-26 10:00:00+0530
categories: main
tags: kotlin
comments: true
---

**Kotlin** is a **modern programming** lanngauge which targets java platform and run on JVM. Although it's not a very new language but it has become quite popular after **Google** started to support Kotlin as its official language. Kotlin v1.0 developed by [JetBrains](https://www.jetbrains.com/) was released way back in [Feb, 2016](https://blog.jetbrains.com/kotlin/2016/02/kotlin-1-0-released-pragmatic-language-for-jvm-and-android/). Kotlin is **concise, more expressive than Java, safe, functional, pragmatice and interoperable with Java**.

# Kotlin Primary Features
Kotlin has most of the key features a modern programming language should have. Kotlin is **based on JVM** and aims to provide a safer alternative to Java. Java is an immensely popular language and used in a vast variety of platforms <u>(eg. From embedded systems to Data centers)</u>. Kotlin can do most of the work which is done by Java like **building server applications, android applications and running microservices**. It can also be compiled to **Javascript** which means you can run **Kotlin in Browser**. But here are some of the key traits which are not present in Java

## 1. Expressiveness
Kotlin can help you in <u>reducing the amount of boilerplate code</u> that you write, results in saving your time to write **business logic code**.For example, if you are building a mobile application, you will have to write (or atleast generate) a **POJO** class like below - 
{% highlight java %}
public class Actor {
    private long id;
    private String name;
    private int age;
    private String url;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    @Override
    public String toString() {
        return "Actor{" +
                "id=" + id +
                ", name=" + name +
                ", age=" + age +
                ", url=" + url +
                "}";
    }
}
{% endhighlight %}
Imagine if you have 20s or 50s of such **POJO**. It would be difficult to write all those classes and overriding `toString()`, `equals()` and `hashCode()` etc. In **Kotlin**, you just need to use **data class**:
{% highlight java %}
data class Actor(
        var id: Long,
        var name: String,
        var age: Int,
        var url: String
)
{% endhighlight %}
And Kotlin will write all the boilerplate code (**setters**, **getters** and even overrides the **equals()** and **hashCode()**) which makes code <u>less verbose and more expressive</u>.

## 2. Statically Typed
Kotlin is a **statically typed language** same as **Java**. Statically typed means that the <u>type of every expression is known at compile time</u> and thus compiler can validate the methods and fields. But unlike java, Kotlin doesn't require you to specify the type of every variable explicitly. Most of the time, it can automatically be <u>determined by the context</u>. This ability of compiler (determining type by context) is known as **Type Interfernce**.
{% highlight java %}
val x = 10;
{% endhighlight %}
Here, a **variable** is initialized with an integer value, so **Kotlin Compiler** automatically determines the type of variable **x** which is **Int**.

## 3. Null Safety
While writing java code, you may find a situation when your app throws a **NullPointerException (The Billion Dollar Mistake)** and then you realized that you forgot to add a null check. In Java, you <u>constantly need to check if your instance reference is null or not before using it</u>, thus making the code **defensive**. **Kotlin's type system** is aimed to eliminate NPE's from code, thus making Kotlin a **null safe language** like many other modern language. You have to explicitly specify if something can be null or not by using **? operator**. Only possible situations when a NPE can be thrown:
- Calling `throw NullPointerException()` explicitly.
- Usage of `!! operator` (will be described in some other post).
- Data inconsistency with initialization. 
- Java Interoperation (accessing a member of null reference of a platform type, Generic type with incorrect nullability, etc).

## 4. Object Oriented and Functional
While Kotlin is a **Object Oriented** programming language, it's <u>functions and properties are first class</u>. First class functions means you can **work with them as values**, **store in variables**, **pass them as arguments** or even **return them from other Higher Order Functions** (will be explained in other post). In short, you can use them as you use non-function values which also provides more abstraction and less duplication of code. Using [functional programming (Wikipedia has a very good article written about it)](https://en.wikipedia.org/wiki/Functional_programming) paradigm, objects become immutable, which guarantees that their state can't be changed after creation. Functional style programming is **more concise**, **elegant** and **succinct compared to imperative programming**. Kotlin has many features to support functional style programming, some of them are listed below:

- **Functional Types** - allows functions to receive other function as parameters or return other functions.
{% highlight java %}
fun <T, R> Collection<T>.fold(
    initial: R, 
    combine: (acc: R, nextElement: T) -> R
): R {
    var accumulator: R = initial
    for (element: T in this) {
        accumulator = combine(accumulator, element)
    }
    return accumulator
}
{% endhighlight %}

- **Lambda Expressions** - A lambda expression is a function literal (way of defining functions) that are not declared, but passed as an expression which prevents you from having to write the specification in an abstract class or interface and then implementation in a class.
{% highlight java %}
val sum = { x: Int, y: Int -> x + y }
{% endhighlight %}

- **Data Classes** - Already explained earlier.

## 5. Use-site Variance
**Generic types** in java are invariant. There are **wildcards** in java but Kotlin doesn't have any. In java, `Collection<String>` is a subtype of `Collection<? extends Object>` but **not** a subtype of `Collection<Object>` which is obvious to ensure runtime-safety. 
{% highlight java %}
interface Collection<E> ... {
  void addAll(Collection<? extends E> items);
}
{% endhighlight %}
The wildcard type arugment `? extends E` means we can <u>safely read E's from the items</u> but can not <u>write to it because we don't know what objects comply to that unknown subtype of E</u>. This **extends-bound** (upper bound) is of covariant type. Similarly, there is contravariant type **super-bound** (lower bound) which suggest that you can <u>only write E to the collections of `? super E`</u> but can not <u>read E's from it</u>.

Kotlin doesn't have any such wildcards but have variance annotations. Kotlin Provides two type of variance annotation, **out** (<u>makes a type parameter covariant</u>) and **in** (<u>makes a type variant contravariant</u>). Consider the following example: 
{% highlight java %}
interface Event<out T> {
    fun nextT(): T
}

fun demo(strs: Event<String>) {
    val objects: Event<Any> = strs // This is OK, since T is an out-parameter
}
{% endhighlight %}

**out** variance is kind of **Producer** (`? extends`) and complementary **in** variance is kind of **Consumer** (`? super`) (Similar to PECS). The above kind of variance declaration is known as **Declaration-site Variance** which is quite similar to Java wildcards but less verbose and complex.
> PECS: Producer Extends, Consumer Super.

In addition to Declaration-site Variance, Kotlin supports **Use-site Variance** (Type projections). Sometimes, there are situations where a type parameter **can not** be either <u>co- or contravariant</u>. Have a look to the following class:
{% highlight java %}
class Event<T> {
    fun pull(): T { ... }
    fun push(event: T) { ... }
}
{% endhighlight %}

Here, `Event` can not be restricted to only either produce or consume T's. The following function maps the event and convert that into some other event.
{% highlight java %}
fun map(from: Event<Any>, to: Event<Any>) {
    // Map the from event into to
}

val userId: Event<String>
val user: Event<User>
map(userId, user) // Error: because it expects (Event<Any>, Event<Any>)
{% endhighlight %}

`Event<T>` is invariant in T so `Event<String>` or `Event<User>` is not a subtype of `Event<Any>`. So to ensure that map function only read from `from` and doesn't do any bad, for example writing to `from`, we need to prevent it. It can be done by using **out variance on site** like below.
{% highlight java %}
fun map(from: Event<out Any>, to: Event<Any>) {
    // Map the from event into to
}
{% endhighlight %}
This approach is known as **type projection**. In a similar way, you can use **in variance** also to only write to that particular parameter.

# Use of Kotlin
Kotlin can be at various places where Java is used but two main usage of Kotlin to look upon are **Kotlin on Server side** and **Kotlin on Android**.

Server side programming includes a variety of applications like **web apps, backend for mobile apps and even microservices to communicate**. Kotlin is completely interoperable with java so no need to worry about working with existing codebase. Kotlin lets you use functional style programming to write code with <u>more confidence and concise syntax and full abstraction</u>. In addition to that, Kotlin also has **coroutines** to write code that works **asynchronously** which makes <u>writing microservices more easy</u>.

Kotlin language feature with a support plugin, turns **android development** into a much pleasurable experience and let you write less number of lines for code. Adding listeners to **view events**, **binding layouts** and **asynchronous programming**, all these can be done by Kotlin in very less code or even no sometimes. A library named [**Anko**](https://github.com/kotlin/anko) built by Kotlin team, can be used to <u>generate layout without using xmls</u>. All these combined together provides you a **nicer experience** for building android apps.

# Final Words
**Kotlin** is a powerful modern language which seamlessly work with a very old and popular language **Java**. The above mentioned features are only the key features of it, there are many more to describe but this post can not accomodate all of them. If you are a new **android developer**, you must start learning Kotlin as well because it's supported by Google so you can expect lots of new **tools** and **apis** built around Kotlin. If you are experienced with Java and Android Development, you should try Kotlin to **increase your productivity** and **write much more meaningful code**. 

Thanks for reading so far. I hope you have enjoyed this article. If you have any suggestions or query, kindly do comment below. 


