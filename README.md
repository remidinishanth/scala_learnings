## scala_learnings

Learning scala from Coursera EPFL


To introduce a definition evaluated only when it is used, we use the keyword `def`: 
* `def` introduces a definition where the right hand side is evaluated on each use

Recursive functions in scala
* the return type can be omitted in non-recursive functions but it is required by the compiler for recursive functions

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
