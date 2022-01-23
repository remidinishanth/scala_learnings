## scala_learnings

Learning scala from Coursera EPFL


To introduce a definition evaluated only when it is used, we use the keyword `def`: 
* `def` introduces a definition where the right hand side is evaluated on each use

Download slides from https://github.com/igorfyago/Coursera-Spec-Scala-Funtional-Programming-EPFL

Cheat sheet: https://www.coursera.org/learn/scala-functional-programming/supplement/Sauv3/cheat-sheet

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
