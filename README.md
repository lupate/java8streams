## Java 8 Streams API Examples
This repository contains a collection of examples that demonstrate how to use Java 8 Streams API.

## Introduction
Java 8 Streams API is a powerful tool for processing collections of data in a functional and efficient way. With Streams, you can perform complex data processing tasks with just a few lines of code, making it easier to write clean, readable, and maintainable code.

This repository contains a collection of examples that illustrate various aspects of the Java 8 Streams API, including:

- Filtering
- Mapping
- Reducing
- Sorting
- Parallel processing
Each example includes a comprehensive explanation of the code, making it easy to understand how the Streams API works and how to use it in your own projects.

## Getting Started
To get started with this project, follow these steps:

- Clone the repository: git clone https://github.com/lupate/java8streams.git
- Import the project into your IDE
- Open the src folder to find the examples
- Run the examples to see the Streams API in action

## Examples
This repository includes the following examples:

- Filtering Example
- Mapping Example
- Reducing Example
- Sorting Example
- Parallel Processing Example
- Each example includes a comprehensive explanation of the code, making it easy to understand how the Streams API works and how to use it in your own projects.

## Contributing
Contributions to this project are welcome. To contribute, follow these steps:

- Fork the repository
- Create a new branch: git checkout -b feature/your-feature-name
- Make your changes and commit them: git commit -am 'Add your commit message here'
- Push your changes to the branch: git push origin feature/your-feature-name
- Submit a pull request

## License
This project is licensed under the MIT License. See the LICENSE file for details.

## Conclusion
Java 8 Streams API is a powerful tool that can make your code more functional, efficient, and maintainable. By using the examples in this repository, you can learn how to use the Streams API to process collections of data in a more powerful and efficient way.

The Java Stream API provides a functional approach to processing collections of objects and it was added in Java 8 along with several other functional programming features.

We introduced two types of stream operations: intermediate and terminal. The intermediate operations are operations that transform or filter the elements in the stream. The terminal operations typically return a single value. Once the terminal operation is invoked on a **Stream**, the iteration of the **Stream** and any of the chained streams will get started. Once the iteration is done, the result of the terminal operation is returned.

We also took a closer look at some advanced concepts around Java stream including **Stream Specializations**, **Parallelized Streams**, **Short-Circuiting Operations**.

## Table of contents:
- Overview of Java streams API
- Creating a Stream
- Operations
- Stream Specializations
- Parallelized Streams
- Streams vs Collections
- Short-Circuiting Operations

## Overview of Java streams API
Nowadays, because of the tremendous amount of development on the hardware front, multicore CPUs are becoming more and more general. In order to leverage the hardware capabilities Java introduced Fork Join Framework. Java 8 Streams API supports many parallel operations to process the data, while completely abstracting out the low-level multithreading logic and letting the developer fully concentrate on the data and the operations to be performed on the data.

The Java 8 Streams concept is based on converting Collections to a Stream, processing the elements in parallel, and then gathering the resulting elements into a Collection.

Any Stream Operation can be divided into the following steps:

- Creating a Stream: Streams can be created from an existing collection or other ways of creating Streams.
- A Set of Intermediate Operations: Intermediate Operations process over a Stream and return Stream as a response.
- A Terminal Operation: Java 8 Streams Terminal Operation is the end of a Stream flow.

## Creating a Stream
Streams can be created from different element sources e.g. collection or array with the help of **stream()** and **of()** methods below or simply use **Stream.builder()**. A **stream()** default method is added to the Collection interface and allows creating a **Stream<T>** using any collection as an element source.

**Stream.empty()** creates an empty stream.
```bash  
  Stream<String> streamEmpty = Stream.empty();
```
**Stream.of( .. )** creates a stream from an array.
  
```bash  
  Stream<Integer> intStream = Stream.of(1, 2, 3);

  String[] stringArray = {"one", "two", "three"};
  Stream<String> stringStream = Stream.of(stringArray);
```
**stream()** method of the Collection interface.  

```bash
  String[] stringArray = {"one", "two", "three"};
  Stream<String> stringStream = Arrays.stream(stringArray);

  Collection<String> collection = Arrays.asList("a", "b", "c");
  Stream<String> streamOfCollection = collection.stream();
```
  
  Stream.builder() returns a builder for a Stream, and then we can build a stream using it. Syntax: **static <T> Stream.Builder<T> builder()**. Why Stream builder has accept and add?
  
```bash  
  // use accept
Stream.Builder<String> strStreamBuilder = Stream.builder();
strStreamBuilder.accept("one");
strStreamBuilder.accept("two");
strStreamBuilder.accept("three");
Stream<String> strStream = strStreamBuilder.build();

// use add
// Using Stream builder()
Stream.Builder<String> builder = Stream.builder();
Stream<String> strStream = builder.add("a").add("b").add("c").build();

// use add in a single line
Stream<String> strStream =
Stream.<String>builder().add("a").add("b").add("c").build();
```
**generate()** or **iterate()** creates an infinite stream.
  
```bash  
  Stream<String> streamGenerated = Stream.generate(() -> "element").limit(10);
  // element, element ... (10 elements)

  Stream<Integer> streamIterated = Stream.iterate(40, n -> n + 2).limit(20);
  // 40, 42, 44 ... (20 elements)
```
## Operations
Java 8 Stream has many operations which can be pipelined together to get the desired result. They are divided into intermediate operations (return Stream<T>) and terminal operations (return a result of definite type). It’s also worth noting that operations on streams don’t change the source (but they can indirectly change the data. e.g.

```bash  
  // this can change property of e but not e itself
  empList.stream().forEach(e -> e.salaryIncrement(10.0)); 
```
1. The main difference between intermediate and terminal operations is that intermediate operations return a stream as a result and terminal operations return non-stream values like primitive or object or collection or may not return anything.
2. As intermediate operations return another stream as a result, they can be chained together to form a pipeline of operations. Terminal operations can not be chained together.
3. The pipeline of operations may contain any number of intermediate operations, but there has to be only one terminal operation, that too at the end of pipeline.
4. Intermediate operations are lazily loaded. When you call intermediate operations, they are actually not executed. They are just stored in the memory and executed when the terminal operation is called on the stream.
5. Intermediate operations don’t give end results. They just transform one stream into another stream. On the other hand, terminal operations give end results.
  
## Intermediate Operations
Stream intermediate operations do not get executed until a terminal operation is invoked (even if we write them in different lines). All Intermediate operations are lazy, so they’re not executed until a result of processing is actually needed. The intermediate operations can be categorized int to Mapping, Filtering, and Sorting. 
  
# Mapping
To change the form of the elements in a stream. The map() and flatMap() are the most commonly used methods.

map() produces a new stream after applying a function to each element of the original stream. The new stream could be of a different type. The following code snippet obtains an Integer stream of employee ids from an array. Each Integer is passed to the function employeeRepository::findById() — which returns the corresponding Employee object; this effectively forms an Employee stream.
  
  ```bash  
  Integer[] empIds = { 1, 2, 3 };
    
  List<Employee> employees = Stream.of(empIds)
  .map(employeeRepository::findById)
  .collect(Collectors.toList());
```
flatMap() transforms each element of a stream into another form (just like a map), and generates sub-streams of the newly formed elements. Finally, it flattens all of the sub-streams into a single stream of elements. As the flatMap is a map type of function, it also takes a function and applies (maps) that function to each of the elements in the stream.  
  
  
  
  ```bash  
  List<List<String>> namesNested = Arrays.asList( 
  Arrays.asList("Jeff", "Bezos"), 
  Arrays.asList("Bill", "Gates"), 
  Arrays.asList("Mark", "Zuckerberg"));

  List<String> namesFlatStream = namesNested.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());

  System.out.println(namesFlatStream);
  // [Jeff, Bezos, Bill, Gates, Mark, Zuckerberg]

  // another example, Integer -> Array -> flat Stream
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
  List<Integer> flattened = numbers.stream()
    .flatMap(number - > Arrays.asList(number - 1, number, number + 1).stream())
    .collect(Collectors.toList());
  System.out.println(flattened); 
  // [0, 1, 2, 1, 2, 3, 2, 3, 4, 3, 4, 5]
```
  
# Filtering
To filter out elements from a stream. Let’s see how to use filter(), distinct(), limit(), skip(), and peek() operations.

filter() accepts a Predicate as an argument. A Predicate is a function that returns a boolean. The filter method returns a stream containing the elements matching the given predicate. 
  
  ```bash  
  // Only the students with score >= 60
  List<Student> studentList = students.stream()
    .filter(student - > student.getScore() >= 60)
    .collect(Collectors.toList());
```
distinct() does not take any argument and returns the distinct elements in the stream, eliminating duplicates. It uses the equals() method of the elements to decide whether two elements are equal or not 
  
  ```bash  
  List<Integer> intList = Arrays.asList(2, 5, 3, 2, 4, 3);
  List<Integer> distinctIntList = intList.stream()
    .distinct()
    .collect(Collectors.toList());

  assertEquals(distinctIntList, Arrays.asList(2, 5, 3, 4));
```
  
 limit() is used to limit the number of elements in a stream. The number of required elements is passed to the limit function as an argument. The limit is a short-circuiting operation, the stream is just skipped, once the limit condition is satisfied.
  
 
  ```bash  
  // List of first 3 students who have age > 20
  List<String> nameList = students.stream()
    .filter(s - > s.getAge() > 20)
    .map(Student::getName)
    .limit(3)
    .collect(Collectors.toList());
```
skip() is used to skip the given number of elements from the stream.
  
  ```bash  
  // List of all the students who have age > 20 except the first 3
  List<String> nameList = students.stream()
    .filter(s - > s.getAge() > 20)
    .map(Student::getName)
    .skip(3)
    .collect(Collectors.toList());
```
  
peek() performs the specified operation on each element of the stream and returns a new stream that can be used further. forEach() can be used similarly but it is a terminal operation.
  
  ```bash  
  List<Employee> list = empList.stream()
    .peek(e -> e.salaryIncrement(10.0))
    .peek(System.out::println)
    .collect(Collectors.toList());
```
  
# Sorting
To return the sorted stream of elements.
sorted() sorts the stream elements based on the comparator passed we pass into it. Note that short-circuiting will not be applied for sorted().
  
  ```bash  
  // sort by name
  List<String> nameList = students.stream()
    .map(Student::getName)
    .sorted()
    .collect(Collectors.toList());

  // another example
  List<Employee> employees = empList.stream()
    .sorted((e1, e2) -> e1.getName().compareTo(e2.getName()))
    .collect(Collectors.toList());
```
## Terminal Operations
A terminal operation is applied on a stream as the last step, they can be categorized as Looping, Matching, Finding, Reduction, Collecting, Comparing, and Counting.

# Looping
To loop the elements of the stream.
forEach() is the simplest and most common operation; it loops over the stream elements, calling the supplied function on each element.
  
  
```bash  
  nameList.stream().forEach(e -> System.out.println(e));
  employeList.stream().forEach(e -> e.increamentSalary(10.0));
```
 
# Matching
To determine if there are any elements that match or don’t match a condition.
anyMatch(), allMatch(), noneMatch(): These operations all take a predicate and return a boolean. Short-circuiting is applied and processing is stopped as soon as the answer is determined.
  
  
  ```bash  
  // Check if at least one student has got distinction
  Boolean hasStudentWithDistinction = students.stream()
      .anyMatch(student - > student.getScore() > 80);

  // Check if All of the students have distinction
  Boolean hasAllStudentsWithDistinction = students.stream()
      .allMatch(student - > student.getScore() > 80);

  // Return true if None of the students are over distinction
  Boolean hasAllStudentsBelowDistinction = students.stream()
      .noneMatch(student - > student.getScore() > 80);
```
# Finding
There are two methods for the finding purpose: findAny and findFirst. The findAny method returns any element from a given stream, while the findFirst returns the first element from the given stream.

findFirst(), findAny() examples:
  
  
  ```bash  
  // Returns any student that matches to the given condition 
  students.stream()
      .filter(student - > student.getAge() > 20) 
      .findAny(); 

  // Returns first student that matches to the given condition 
  students.stream()
      .filter(student - > student.getAge() > 20) 
      .findFirst();
```

# Reduction
A reduction operation (also called as fold) takes a sequence of input elements and combines them into a single summary result by repeated application of a combining operation. It is very useful for calculating the sum of all elements in the stream or calculating the max or min element out of a stream.

reduce(): the most common format of reduce() is T reduce(T identity, BinaryOperator<T> accumulator). where identity is the starting value and accumulator is the binary operation we repeatedly apply.

Key concepts in reduce():

- Identity — an element that is the initial value of the reduction operation and the default result if the stream is empty
- Accumulator — a function that takes two parameters: a partial result of the reduction operation and the next element of the stream
- Combiner — a function used to combine the partial result of the reduction operation when the reduction is parallelized (more on it later) or when there’s a mismatch between the types of the accumulator arguments and the types of the accumulator implementation.

  Java stream reduce example with identity, accumulator:
  
  ```bash  
  // Summing all elements of a stream 
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
  int result = numbers
    .stream()
    .reduce(
      0, // identity
      (subtotal, element) -> subtotal + element); // accumulator
  // result is 21

  // use method reference instead of a lambda expression
  int result = numbers.stream().reduce(0, Integer::sum); // 21
```
Java stream reduce example with identity, accumulator and combiner:
  
   ```bash  
  // when reduction is parallelized
  List<Integer> ages = Arrays.asList(25, 30, 45, 28, 32);
  int computedAges = ages.parallelStream().reduce(0, (a, b) -> a + b, Integer::sum);

  // when there's a mismatch between the types of the accumulator arguments and the types of the accumulator implementation
  List <String> numbers = Arrays.asList("one", "two", "three");
  int sum = numbers.stream()
  .reduce(0, (first, second) -> first + second.length(), Integer::sum);
```
If Integer::sum (combiner) is missing in the second example, the error error: no suitable method found for reduce(int,(first,sec[…] 0; }) will be thrown. This is because we have a stream of String objects, and the types of the accumulator arguments are Integer and String. However, the accumulator implementation is to return Integers, so the compiler just can’t infer the type of the second parameter. The combiner tells the compiler that the result is an integer sum.

# Collecting
To generate an array or collection from the stream.

toArray(), collect() examples:  
  
  ```bash  
  // convert to array
  Employee[] employees = empList.stream().toArray(Employee[]::new);

  // convert to list
  List<Employee> employees = empList.stream().collect(Collectors.toList());
```

# Comparison and Counting
min() and max() return the minimum and maximum element in the stream respectively, based on a comparator. They return an Optional since a result may or may not exist (due to, say, filtering):

min(), max(), count() examples:
  
  
  ```bash  
  Employee firstEmp = empList.stream()
    .min((e1, e2) -> e1.getId() - e2.getId())
    .orElseThrow(NoSuchElementException::new);

  // or use Comparator
  Employee maxSalEmp = empList.stream()
    .max(Comparator.comparing(Employee::getSalary))
    .orElseThrow(NoSuchElementException::new);

  // count element
  Long empCount = empList.stream()
    .filter(e -> e.getSalary() > 200000)
    .count();
```
**An advanced example using stream API and functional interface**
Imagine we’d like to implement a method to filter out some elements of a collection with the specified condition while logging the elements that are filtered and returning the collection containing those that are not filtered out.

Let’s see how we achieve it by using Stream API (predict, filter, peek) in Java to filter numbers greater than 5 using a custom filter method that accepts a Predicate and a Consumer. (Not familiar with Functional Interface? check out my other post!)  

  
```bash  
  public class StreamExample {
      public static void main(String[] args) {
          List<Integer> numbers = Arrays.asList(1, 7, 2, 9, 3, 5, 8);
          List<Integer> numbersGreaterThanFive = filter(
              numbers,
              num -> num > 5, // lambda impl of Predicate Functional Interface
              num -> System.out.println("filtered out: " + num) // lambda impl of Consumer Functional Interface
          );
          System.out.println("Number greater than 5: " + numbersGreaterThanFive);
      }

      public static List<Integer> filter(List<Integer> collection, Predicate<Integer> predicate, Consumer<Integer> negateProcess) {
          return collection.stream()
                .peek(n -> {if (predicate.negate().test(n)) {negateProcess.accept(n);}}) // for logging
                .filter(predicate)
                .collect(Collectors.toList());
      }
  }

  /*
  filtered out: 1
  filtered out: 2
  filtered out: 3
  filtered out: 5
  Number greater than 5: [7, 9, 8]
  */
```
Here’s how the code works:

1. The filter method takes three arguments: the collection to filter (numbers), a Predicate to define the filtering logic (in this case, num -> num > 5), and a Consumer to define what to do with each number that is filtered out (in this case, print "filtered out: " and the number itself).
2. The filter method uses the Stream API to filter the collection based on the provided Predicate and collect the filtered results into a list. It does this by first calling peek to execute a given action on each element of the stream before it is filtered. In this case, the peek method checks if the negate() of the provided Predicate returns true (i.e. if numbers ≤ 5 ) for the current element. If it does, it means that the current element did not meet the filtering criteria and should be consumed by the Consumer. If the current element passes the Predicate test, then it is allowed to pass through the stream.
3. The filtered numbers are collected into a list using the collect method and then returned by the filter method.
This code demonstrates how you can use peek in a customized filter to log the elements which you cannot achieve by using foreach. (as foreach and collect are both terminal operations)

# Stream Specializations
From what we discussed so far, Stream is a stream of object references. However, there are also the IntStream, LongStream, and DoubleStream — which are primitive specializations for int, long and double respectively. These are quite convenient when dealing with a lot of numerical primitives.

These specialized streams do not extend Stream but extend BaseStream on top of which Stream is also built.

As a consequence, not all operations supported by Stream are present in these stream implementations. For example, the standard min() and max() take a comparator, whereas the specialized streams do not.

# Creation
We can use mapToxxx or range to create anxxxStream.  
   
```bash  
  Integer latestEmpId = empList.stream()
    .mapToInt(Employee::getId)
    .max()
    .orElseThrow(NoSuchElementException::new);

  IntStream intStream = IntStream.range(1, 3);
  LongStream longStream = LongStream.rangeClosed(1, 3);

  // Note the following creates Stream<Integer> not IntStream
  Stream<Integer> intStream = Stream.of(1, 2, 3);
```
  
# Operations
Specialized streams provide additional operations as compared to the standard Stream — which are quite convenient when dealing with numbers. For example sum(), average(), range() etc 
  
  
  ```bash  
  Double avgSal = empList.stream()
    .mapToDouble(Employee::getSalary)
    .average()
    .orElseThrow(NoSuchElementException::new);
```
## Parallelized Streams
We have practiced with the stream in a sequential manner — each operation is applied to each element of the stream sequentially. Any stream in Java can easily be transformed from sequential to parallel. We can achieve this by adding the parallel method to a sequential stream or by creating a stream using the parallelStream method of a collection. Parallel streams enable us to execute code in parallel on separate cores. The final result is the combination of each individual outcome. 
  
  ```bash  
  // use parallel
  List<Integer> listOfNumbers = Arrays.asList(1, 2, 3, 4);
  listOfNumbers.stream().parallel().forEach(number -> System.out.println(number));

  // use parallelStream
  List<Integer> listOfNumbers = Arrays.asList(1, 2, 3, 4);
  listOfNumbers.parallelStream().forEach(number ->
      System.out.println(number + " " + Thread.currentThread().getName())
  );
```
Since we cannot control the order of execution when parallelized streams are used, the output above may differ in each execution. Remember the following when using parallelized streams:

1. Ensure the code is thread-safe. Special care needs to be taken if the operations performed in parallel modify shared data.
2. Ensure the order in which operations are performed or the order returned in the output stream doesn’t matter.
3. Ensure that it is worth making the code execute in parallel as they can be overkill if the operations applied to the stream aren’t expensive, or the number of elements in the stream is small.

  Let’s take a look at a case of reduce() on parallelized streams.  

  
  ```bash  
  List<Integer> listOfNumbers = Arrays.asList(1, 2, 3, 4);
  int sum = listOfNumbers.parallelStream().reduce(5, Integer::sum); // the result isn't 15
```
In a sequential stream, the result of the above operation would be 15. But since the reduce operation is handled in parallel, the number five actually gets added up in every worker thread. The actual result might differ depending on the number of threads used in the common fork-join pool. In order to fix this issue, the number five should be added outside of the parallel stream:
  
  ```bash  
  int sum = listOfNumbers.parallelStream().reduce(0, Integer::sum) + 5;
```
## Streams vs Collections
Collections and Streams, both are conceptually two different things that are used for two different purposes. If the collections are used to store the data then the streams are used to perform operations on that data.
  
  ![Screenshot 2023-06-03 161000](https://github.com/lupate/java8streams/assets/9717058/a4e43687-efd6-4778-b3cd-947716530b88)


## Short-Circuiting Operations
Java 8 short-circuiting operations are just like boolean short-circuit evaluations in Java.

In boolean short-circuiting logic, for example, firstBoolean && secondBoolean, if firstBoolean is false then the remaining part of the expression is ignored (the operation is short-circuited) because the remaining evaluation will be redundant.

Java 8 Stream short-circuit operations are not limited to boolean types. There are pre-defined short-circuiting operations. Short-circuiting operations allow computations on infinite streams to complete in finite time:
  
  ```bash  
  Stream<Integer> infiniteStream = Stream.iterate(2, i -> i * 2);
  List<Integer> collect = infiniteStream
    .skip(3) // skip first 3 elements
    .limit(5) // limit to 5 elements
    .collect(Collectors.toList());

  assertEquals(collect, Arrays.asList(16, 32, 64, 128, 256));
```
Java 8 stream intermediate and terminal operations both can be short-circuiting. As we have seen in previous examples, the following are the most commonly used short-circuiting operations.
  
![Screenshot 2023-06-03 161141](https://github.com/lupate/java8streams/assets/9717058/0abc6fb3-a080-4c57-8b84-9304ecfb496a)
  
The END.  
