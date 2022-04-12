## scala_learnings

Learning scala from Coursera EPFL

### Imperative languages vs Functional programming
In imperative languages you get things done by giving the computer a sequence of tasks and then it executes them. While executing them, it can change state. For instance, you set variable `a` to `5` and then do some stuff and then set it to something else. You have control flow structures for doing some action several times.

In purely functional programming you don't tell the computer what to do as such but rather you tell it what stuff is. The factorial of a number is the product of all the numbers from 1 to that number, the sum of a list of numbers is the first number plus the sum of all the other numbers, and so on. You express that in the form of functions.

You also can't set a variable to something and then set it to something else later. If you say that a is 5, you can't say it's something else later because you just said it was 5. What are you, some kind of liar? 

So in purely functional languages, a function has no side-effects. The only thing a function can do is calculate something and return it as a result. At first, this seems kind of limiting but it actually has some very nice consequences: if a function is called twice with the same parameters, it's guaranteed to return the same result. That's called referential transparency and not only does it allow the compiler to reason about the program's behavior, but it also allows you to easily deduce (and even prove) that a function is correct and then build more complex functions by gluing simple functions together.


### EPFL notes
To introduce a definition evaluated only when it is used, we use the keyword `def`: 
* `def` introduces a definition where the right hand side is evaluated on each use

Recursive functions in scala
* the return type can be omitted in non-recursive functions but it is required by the compiler for recursive functions

Tail recursive function
* calls itself as its last action - the last call of the function to itself is called tail call
* represents an iterative process - tail recursive functions represent processes where the same procedure is repeated with modified inputs such as the greatest common divisor
* can be annotated with @tailrec so that the compiler will succeed only if it can verify that the function is indeed tail recursive - The annotation verifies that a tail recursive function contains a tail - call to itself
* can be optimized by reusing the stack frame - this is the main advantage of preferring tail recursive functions: the memory overhead is reduced and the number of iterations is not limited by the stack size

Download slides from https://github.com/igorfyago/Coursera-Spec-Scala-Funtional-Programming-EPFL

Cheat sheet: https://www.coursera.org/learn/scala-functional-programming/supplement/Sauv3/cheat-sheet

Pure functions: https://mb21.github.io/blog/2021/01/23/pure-functional-programming-and-shared-mutable-state.html

Why Functional programming is becoming popular? https://madusudanan.com/blog/scala-tutorials-part-9-intro-to-functional-programming/
* Moore’s law now achieved by increasing # of cores not clock cycles

### Concurrency and Parallelism 
* Parallel programming: Execute programs faster on parallel hardware. 
* Concurrent programming: Manage concurrent execution threads explicitly. 
* Both are too hard!


### The Root of The Problem 

```scala
var x = 0 
async { x = x + 1 } 
async { x = x * 2 } // can give 0, 1, 2
```

* Non-determinism caused by concurrent threads accessing shared mutable state. 
* It helps to encapsulate state in actors or transactions, but the fundamental problem stays the same. 
* So, `non-determinism = parallel processing + mutable state`
* To get deterministic processing, avoid the mutable state! 
* Avoiding mutable state means programming **functionally**. 


### Scala is a Unifier 
* Agile, with lightweight syntax 
* Object-Oriented
* Functional 
* Safe and performant, with strong static typing

This makes it suitable for both sequential and parallel programming

### Different Tools for Different Purposes

#### Parallelism :

* Parallel Collections
* Collections
  - Distributed Collections
  - Parallel DSLs

#### Concurrency :
Akka
* Actors
* Software transactional memory
* Futures

### Class in Java vs Scala

#### Hello World

```java
public class HelloJava {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

```scala
object HelloScala {
    def main(args: Array[String]): Unit = {
        println("Hello World!")
    }
}
```

#### Simple list of Strings

```java
List<String> list = new ArrayList<String>();
list.add("1");
list.add("2");
list.add("3");
```

```scala
val list = List("1", "2", "3")
```

#### Converting list of strings to int

```java
List<Integer> ints = new ArrayList<Integer>();
for (String s : list) {
    ints.add(Integer.parseInt(s));
}
```

```scala
val ints = list.map(s => s.toInt)
```

#### Defining Person class

```java
public class Person {
  public final String name;
  public final int age;
  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
```

```scala
class Person(val name: String,
             val age: Int)
```

and its usage, splitting `people` into `minors` and `adults`

```java
import java.util.ArrayList;
...
Person[] people;
Person[] minors;
Person[] adults;
{
  ArrayList<Person> minorsList = new ArrayList<Person>();
  ArrayList<Person> adultsList = new ArrayList<Person>();
  for(int i = 0; i < people.length; i++)
    (people[i].age < 18 ? minorsList: adultsList).add(people[i]);
  minors = minorsList.toArray(people);
  adults = adultsList.toArray(people);
}
```

```scala
val people: Array[Person]
val (minors, adults) = people partition (_.age < 18)
```

Here Scala uses simple pattern match for `(minors, adults)`, an infix method call for `partition`, a function value for `_.age < 18`

If we want to process it parallely then we can just add `par`

```scala
val people: Array[Person]
val (minors, adults) = people.par partition (_.age < 18)
```

### Actors for Concurrent Programming

```scala
class Person(val name: String, val age: Int)

actor {
  receive {
  case people: Set[Person] =>
    val (minors, adults) = 
      people partition (_.age < 18)
    Facebook ! minors
    LinkedIn ! adults
  }
}
```

* Simple message-oriented programming model for multi-threading
* Serializes access to shared resources using queues and function passing
* Easier for programmers to create reliable concurrent processing
* Many sources of contention, races, locking and dead-locks removed


Read about **Companion objects**: A companion object is an object with the same name as a class or trait and is defined in the same source file as the associated file or trait. Ref: https://docs.scala-lang.org/overviews/scala-book/companion-objects.html


Todo: https://blog.tmorris.net/posts/scalaoption-cheat-sheet/
