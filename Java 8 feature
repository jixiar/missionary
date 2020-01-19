Table of Contents
Default Methods for Interfaces
Lambda expressions
Functional Interfaces
Method and Constructor References
Lambda Scopes
Accessing local variables
Accessing fields and static variables
Accessing Default Interface Methods
Built-in Functional Interfaces
Predicates
Functions
Suppliers
Consumers
Comparators
Optionals
Streams
Filter
Sorted
Map
Match
Count
Reduce
Parallel Streams
Sequential Sort
Parallel Sort
Maps
Date API
Clock
Timezones
LocalTime
LocalDate
LocalDateTime
Annotations
Where to go from here?

https://github.com/winterbe/java8-tutorial#parallel-streams

**Default Methods for Interfaces**
接口默认方法

Java 8 enables us to add non-abstract method implementations to interfaces by utilizing the default keyword. This feature is also known as virtual extension methods.
Java 8 让我们可以在接口中利用default关键字添加非抽象方法，这个特性也是作为一个虚拟扩展方法。

Here is our first example:

```
interface Formula {
    double calculate(int a);

    default double sqrt(int a) {
        return Math.sqrt(a);
    }
}
```

Besides the abstract method calculate the interface Formula also defines the default method sqrt. Concrete classes only have to implement the abstract method calculate. The default method sqrt can be used out of the box.
除了抽象方法计算外Formula接口还定义了默认方法sqrt，构造类只需要实现抽象方法calculate，默认方法sqrt可以开箱即用。

```
Formula formula = new Formula() {
    @Override
    public double calculate(int a) {
        return sqrt(a * 100);
    }
};

formula.calculate(100);     // 100.0
formula.sqrt(16);           // 4.0
```

The formula is implemented as an anonymous object. The code is quite verbose: 6 lines of code for such a simple calculation of sqrt(a * 100). As we'll see in the next section, there's a much nicer way of implementing single method objects in Java 8.
这个formula是个匿名类继承Formula接口，这段代码是非常冗余的，6行代码只是为了实现一个简单的sqrt(a*100)的简单calculation方法，不过我们会在下个目录看到，有一个非常好的方式通过Java 8实现单个方法对象。

**Lambda expressions**

Let's start with a simple example of how to sort a list of strings in prior versions of Java:
我们先开始通过以前的Java版本实现一个的怎么为字符串数组排序的简单例子。

```
List<String> names = Arrays.asList("peter", "anna", "mike", "xenia");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});
```

The static utility method Collections.sort accepts a list and a comparator in order to sort the elements of the given list. You often find yourself creating anonymous comparators and pass them to the sort method.
这个静态工具方法Collections.sort接受一个list参数和一个为了给list参数排序的comparator对象，你经常发现你自己实现一个匿名的comparator类并且作为参数传递给排序方法。

Instead of creating anonymous objects all day long, Java 8 comes with a much shorter syntax, lambda expressions:
除了整天实现匿名类，Java 8 实现一个非常简短的语法，lambda表达式。

```
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
```

As you can see the code is much shorter and easier to read. But it gets even shorter:
你可以发现这样的代码非常简短和容易阅读，并且它可以甚至更简短：

```
Collections.sort(names, (String a, String b) -> b.compareTo(a));
```

For one line method bodies you can skip both the braces {} and the return keyword. But it gets even shorter:
通过一行代码你可以跳过{}方法体和return关键字，但是它可以再简短：

names.sort((a, b) -> b.compareTo(a));

List now has a sort method. Also the java compiler is aware of the parameter types so you can skip them as well. Let's dive deeper into how lambda expressions can be used in the wild.
List类现在有一个sort方法，Java编译器可以识别参数类型所以你可以跳过参数类型描述，让我们更深层的疯狂探索怎么使用lambda表达式。

**Functional Interfaces**

How does lambda expressions fit into Java's type system? Each lambda corresponds to a given type, specified by an interface. A so called functional interface must contain exactly one abstract method declaration. Each lambda expression of that type will be matched to this abstract method. Since default methods are not abstract you're free to add default methods to your functional interface.
lambda表达式怎么再Java的类型系统中使用呢，每个lambda表达式都由接口指定的类型，一个所谓的方法接口必须包含至少一个抽象方法定义，每个lambda表达式都会匹配这个接口类型的抽象方法，由于默认方法不是抽象的你可以自由的在函数接口中增加默认方法。

We can use arbitrary interfaces as lambda expressions as long as the interface only contains one abstract method. To ensure that your interface meet the requirements, you should add the @FunctionalInterface annotation. The compiler is aware of this annotation and throws a compiler error as soon as you try to add a second abstract method declaration to the interface.
我们可以使用任意的接口作为lambda表达式的参数类型，只要这个接口只有一个抽象方法，为保证你的接口符合要求，你应该添加@FunctionalInterface 注解，编译器会识别这个注解并且如果你尝试在接口中定义第二个抽象方法时会抛出编译异常。

Example:

```
@FunctionalInterface
interface Converter<F, T> {
    T convert(F from);
}
Converter<String, Integer> converter = (from) -> Integer.valueOf(from);
Integer converted = converter.convert("123");
System.out.println(converted);    // 123
```

Keep in mind that the code is also valid if the @FunctionalInterface annotation would be omitted.
注意如果@FunctionalInterface注解被遗漏这段代码仍然有效。

**Method and Constructor References**
方法和构造器引用

The above example code can be further simplified by utilizing static method references:
上面的代码例子可以通过静态方法引用更简洁。

```
Converter<String, Integer> converter = Integer::valueOf;
Integer converted = converter.convert("123");
System.out.println(converted);   // 123
```

Java 8 enables you to pass references of methods or constructors via the :: keyword. The above example shows how to reference a static method. But we can also reference object methods:
Java 8 允许你可以通过::关键字传递方法引用或则构造器，上面的例子展示了怎么引用一个静态方法，而且我们同样可以引用一个对象方法。

```
class Something {
    String startsWith(String s) {
        return String.valueOf(s.charAt(0));
    }
}
Something something = new Something();
Converter<String, String> converter = something::startsWith;
String converted = converter.convert("Java");
System.out.println(converted);    // "J"
```

Let's see how the :: keyword works for constructors. First we define an example class with different constructors:
让我们看下::关键字怎么运用在构造器上，首先我们定义一个有不同构造器的类：

```
class Person {
    String firstName;
    String lastName;

    Person() {}

    Person(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

Next we specify a person factory interface to be used for creating new persons:
然后我们指定一个用于创建person对象的person工厂接口：

```
interface PersonFactory<P extends Person> {
    P create(String firstName, String lastName);
}
```

Instead of implementing the factory manually, we glue everything together via constructor references:
和手动实现工厂接口不同，我们通过使用构造器来完成任何事情。

```
PersonFactory<Person> personFactory = Person::new;
Person person = personFactory.create("Peter", "Parker");
```

We create a reference to the Person constructor via Person::new. The Java compiler automatically chooses the right constructor by matching the signature of PersonFactory.create.
我们通过Person::new创建了对Person构造器的引用，Java编译器通过匹配PersonFactory.create的方法签名自动选择正确的构造器。

**Lambda Scopes**

Accessing outer scope variables from lambda expressions is very similar to anonymous objects. You can access final variables from the local outer scope as well as instance fields and static variables.
允许lambda表达式使用外部变量的方式和匿名类非常类似，你可以允许本地的外部final变量和实例字段和静态变量。

**Accessing local variables**

We can read final local variables from the outer scope of lambda expressions:
我们可以读取lambda表达式的外层final本地变量：

```
final int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

stringConverter.convert(2);     // 3
```

But different to anonymous objects the variable num does not have to be declared final. This code is also valid:
但是和匿名类不同的是num变量不一定非要定义为final，这段代码也是有效的：

```
int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

stringConverter.convert(2);     // 3
```

However num must be implicitly final for the code to compile. The following code does not compile:
但是对于要编译的代码num必须是隐式的final，下面的代码不编译：

```
int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);
num = 3;
```

Writing to num from within the lambda expression is also prohibited.
lambda表达式中修改num也是被禁止的

**Accessing fields and static variables**

In contrast to local variables, we have both read and write access to instance fields and static variables from within lambda expressions. This behaviour is well known from anonymous objects.
相对于局部变量，我们对实例的字段和静态变量在lambda表达式中有写和读的权限，这种行为在匿名类中很常见。

```
class Lambda4 {
    static int outerStaticNum;
    int outerNum;

    void testScopes() {
        Converter<Integer, String> stringConverter1 = (from) -> {
            outerNum = 23;
            return String.valueOf(from);
        };

        Converter<Integer, String> stringConverter2 = (from) -> {
            outerStaticNum = 72;
            return String.valueOf(from);
        };
    }
}
```

**Accessing Default Interface Methods**

Remember the formula example from the first section? Interface Formula defines a default method sqrt which can be accessed from each formula instance including anonymous objects. This does not work with lambda expressions.
记得在第一节的formula类例子吗，接口Formula定义一个可以被任意formula对象包括匿名类对象访问的默认方法sqrt，这个跟lambda表达式不相关。

Default methods cannot be accessed from within lambda expressions. The following code does not compile:
默认方法不能通过lambda表达式访问，下面的代码编译不通过

Formula formula = (a) -> sqrt(a * 100);

**Built-in Functional Interfaces**
构建函数接口

The JDK 1.8 API contains many built-in functional interfaces. Some of them are well known from older versions of Java like Comparator or Runnable. Those existing interfaces are extended to enable Lambda support via the @FunctionalInterface annotation.
JDK 1.8 API包含许多函数接口，一些是我们知道的老版本中的Comparator或则Runnable接口，这些已经存在的接口通过@FunctionalInterface注解扩展成了可以支持lambda表达式的函数接口。

But the Java 8 API is also full of new functional interfaces to make your life easier. Some of those new interfaces are well known from the Google Guava library. Even if you're familiar with this library you should keep a close eye on how those interfaces are extended by some useful method extensions.
但是Java 8 API为了让程序更丰富也创建了许多新的函数接口，一些新的接口来自我们熟知的Google Guava包，即使你熟悉这些库，你也应该密切关注这些接口如何通过一些有用的方法来扩展的。

**Predicates**
预测

Predicates are boolean-valued functions of one argument. The interface contains various default methods for composing predicates to complex logical terms (and, or, negate)
预测是一个参数值并返回布尔值的函数，这接口包含各种将谓词（与，或，非）组合成复杂逻辑的默认方法。

```
Predicate<String> predicate = (s) -> s.length() > 0;

predicate.test("foo");              // true
predicate.negate().test("foo");     // false

Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();
```

**Functions**

Functions accept one argument and produce a result. Default methods can be used to chain multiple functions together (compose, andThen).
方法接口接受一个参数并且返回一个结果，接口中的默认方法可以多个方法链接到一起处理（组合，转折）。


```
Function<String, Integer> toInteger = Integer::valueOf;
Function<String, String> backToString = toInteger.andThen(String::valueOf);

backToString.apply("123");     // "123"
```

**Suppliers**

Suppliers produce a result of a given generic type. Unlike Functions, Suppliers don't accept arguments.
生产者提供一个通用类型的结果，不像函数接口，生产者不接受参数。

```
Supplier<Person> personSupplier = Person::new;
personSupplier.get();   // new Person
```

**Consumers**

Consumers represent operations to be performed on a single input argument.
消费者提供操作行为通过提供一个输入参数。

```
Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);
greeter.accept(new Person("Luke", "Skywalker"));
```

**Comparators**

Comparators are well known from older versions of Java. Java 8 adds various default methods to the interface.
比较器在Java老版本中被熟知，Java 8 在Comparators接口中添加各种各样的默认方法。

```
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);

Person p1 = new Person("John", "Doe");
Person p2 = new Person("Alice", "Wonderland");

comparator.compare(p1, p2);             // > 0
comparator.reversed().compare(p1, p2);  // < 0
```

**Optionals**

Optionals are not functional interfaces, but nifty utilities to prevent NullPointerException. It's an important concept for the next section, so let's have a quick look at how Optionals work.
选择器不是函数接口，但是是一个防止空指针异常的很好的工具，在下一节会有一个重要的介绍，那么我们现在快速的查看选择器是怎么工作的。

Optional is a simple container for a value which may be null or non-null. Think of a method which may return a non-null result but sometimes return nothing. Instead of returning null you return an Optional in Java 8.
选择器是一个可以是空或则非空的值的简单容器，想象下一个可以返回非空值的方法在有些时候却什么都不返回，在Java 8中你可以通过返回Optional来代替返回null。

```
Optional<String> optional = Optional.of("bam");

optional.isPresent();           // true
optional.get();                 // "bam"
optional.orElse("fallback");    // "bam"

optional.ifPresent((s) -> System.out.println(s.charAt(0)));     // "b"
```

**Streams**

A java.util.Stream represents a sequence of elements on which one or more operations can be performed. Stream operations are either intermediate or terminal. While terminal operations return a result of a certain type, intermediate operations return the stream itself so you can chain multiple method calls in a row. Streams are created on a source, e.g. a java.util.Collection like lists or sets (maps are not supported). Stream operations can either be executed sequentially or parallely.
一个数据流工具类可以让一个元素序列在一个或则多个操作方法中被处理，数据流操作可以是中间操作也可以是终止操作，如果是终止操作就返回一个有确定类型的结果，中间操作就返回数据流自身，所以你可以在一行代码中链接调用多个方法。数据流可以某一个集合数据源上创建，如列表和集合（不支持映射map）,数据流可以序列化执行也可以并行执行。

Streams are extremely powerful, so I wrote a separate Java 8 Streams Tutorial. You should also check out Sequency as a similiar library for the web.
数据流非常有用，所以我单独写了一个Java 8数据流教程，你也可以像网络图书馆那样按顺序检索。

Let's first look how sequential streams work. First we create a sample source in form of a list of strings:
让我们看连续的数据流是怎么工作的，第一步我们以字符串列表的形式创建一个简单的数据源：

```
List<String> stringCollection = new ArrayList<>();
stringCollection.add("ddd2");
stringCollection.add("aaa2");
stringCollection.add("bbb1");
stringCollection.add("aaa1");
stringCollection.add("bbb3");
stringCollection.add("ccc");
stringCollection.add("bbb2");
stringCollection.add("ddd1");
```

Collections in Java 8 are extended so you can simply create streams either by calling Collection.stream() or Collection.parallelStream(). The following sections explain the most common stream operations.
在Java 8 中集合被扩展了，所以你可以通过调用Collection.stream()或则Collection.parallelStream()方法简单的创建数据流对象，下面章节解释了大部分常用的数据流操作。

**Filter**

Filter accepts a predicate to filter all elements of the stream. This operation is intermediate which enables us to call another stream operation (forEach) on the result. ForEach accepts a consumer to be executed for each element in the filtered stream. ForEach is a terminal operation. It's void, so we cannot call another stream operation.
过滤器接受一个预测对象来过滤数据流中的所有元素，这个操作是中间操作可以让我们在结果集后调用其他数据流操作（遍历），遍历方法接受一个消费者对象用来执行每个过滤后的数据流的元素，遍历是一个终止操作，它返回空，所以我们不能调用其他的数据流操作。

```
stringCollection
    .stream()
    .filter((s) -> s.startsWith("a"))
    .forEach(System.out::println);

// "aaa2", "aaa1"
```

**Sorted**

Sorted is an intermediate operation which returns a sorted view of the stream. The elements are sorted in natural order unless you pass a custom Comparator.
排序是一个中间操作可以返回一个排序后的数据流，如果你传递一个比较器消费者，这些元素会按照自然顺序进行排序。

```
stringCollection
    .stream()
    .sorted()
    .filter((s) -> s.startsWith("a"))
    .forEach(System.out::println);

// "aaa1", "aaa2"
```

Keep in mind that sorted does only create a sorted view of the stream without manipulating the ordering of the backed collection. The ordering of stringCollection is untouched:
请记住这种排序只是创建了数据流的排序试图，并不是对实际的集合进行了手动排序，这个集合的元素顺序并没有改变。

```
System.out.println(stringCollection);
// ddd2, aaa2, bbb1, aaa1, bbb3, ccc, bbb2, ddd1
```

**Map**

The intermediate operation map converts each element into another object via the given function. The following example converts each string into an upper-cased string. But you can also use map to transform each object into another type. The generic type of the resulting stream depends on the generic type of the function you pass to map.
中间操作map映射通过给定的方法把每一个元素转换成另外一个元素，下面的例子是把每一个字符串元素转换成一个大写的字符串，而且你也可以通过使用map映射把每一个元素转换成另外一种类型元素，结果流的泛型类型取决于你传递给map映射的泛型类型。

```
stringCollection
    .stream()
    .map(String::toUpperCase)
    .sorted((a, b) -> b.compareTo(a))
    .forEach(System.out::println);

// "DDD2", "DDD1", "CCC", "BBB3", "BBB2", "AAA2", "AAA1"
```

**Match**

Various matching operations can be used to check whether a certain predicate matches the stream. All of those operations are terminal and return a boolean result.
各种匹配操作可以用来检查某个谓词是否匹配数据流，这些操作都是终止操作并且返回一个布尔值。

```
boolean anyStartsWithA =
    stringCollection
        .stream()
        .anyMatch((s) -> s.startsWith("a"));

System.out.println(anyStartsWithA);      // true

boolean allStartsWithA =
    stringCollection
        .stream()
        .allMatch((s) -> s.startsWith("a"));

System.out.println(allStartsWithA);      // false

boolean noneStartsWithZ =
    stringCollection
        .stream()
        .noneMatch((s) -> s.startsWith("z"));

System.out.println(noneStartsWithZ);      // true
```

**Count**

Count is a terminal operation returning the number of elements in the stream as a long.
统计是一个终止操作，返回代表数据流元素个数的long类型结果

```
long startsWithB =
    stringCollection
        .stream()
        .filter((s) -> s.startsWith("b"))
        .count();

System.out.println(startsWithB);    // 3
```

**Reduce**

This terminal operation performs a reduction on the elements of the stream with the given function. The result is an Optional holding the reduced value.
这是一个终止操作使用给定的函数来给元素进行减操作，结果是一个包含减值的Optional结果。

```
Optional<String> reduced =
    stringCollection
        .stream()
        .sorted()
        .reduce((s1, s2) -> s1 + "#" + s2);

reduced.ifPresent(System.out::println);
// "aaa1#aaa2#bbb1#bbb2#bbb3#ccc#ddd1#ddd2"
```

**Parallel Streams**

As mentioned above streams can be either sequential or parallel. Operations on sequential streams are performed on a single thread while operations on parallel streams are performed concurrently on multiple threads.
就像上面提的数据流即可以是一个串行执行也可以并行执行，串行操作实在一个单线程上执行，并行操作是在多线程中执行。

The following example demonstrates how easy it is to increase the performance by using parallel streams.
下面例子示范了怎么样通过使用数据流的并行执行轻松提高性能。

First we create a large list of unique elements:
首先我们创建一个大的唯一元素的列表

```
int max = 1000000;
List<String> values = new ArrayList<>(max);
for (int i = 0; i < max; i++) {
    UUID uuid = UUID.randomUUID();
    values.add(uuid.toString());
}
```

Now we measure the time it takes to sort a stream of this collection.
现在我们计算为这个集合的数据流排序需要多长时间。

**Sequential Sort**
串行排序

```
long t0 = System.nanoTime();

long count = values.stream().sorted().count();
System.out.println(count);

long t1 = System.nanoTime();

long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("sequential sort took: %d ms", millis));

// sequential sort took: 899 ms
```

**Parallel Sort**
并行排序

```
long t0 = System.nanoTime();

long count = values.parallelStream().sorted().count();
System.out.println(count);

long t1 = System.nanoTime();

long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("parallel sort took: %d ms", millis));

// parallel sort took: 472 ms
```

As you can see both code snippets are almost identical but the parallel sort is roughly 50% faster. All you have to do is change stream() to parallelStream().
就像你看到的这两个代码片段几乎完全一样，但是并行排序快了将近50%，而你所要做的就是将stream()方法换成parallelStream()方法。

**Maps**

As already mentioned maps do not directly support streams. There's no stream() method available on the Map interface itself, however you can create specialized streams upon the keys, values or entries of a map via map.keySet().stream(), map.values().stream() and map.entrySet().stream().
尽管之前已经提到映射集不直接支持数据流操作，Map接口本身没有可用的stream()方法，但是你可以通过键值，value或则map的entries集创建指定的数据流，如：map.keySet().stream(), map.values().stream() and map.entrySet().stream().

Furthermore maps support various new and useful methods for doing common tasks.
另外映射支持各种用于执行常见任务的新的有用的方法。

```
Map<Integer, String> map = new HashMap<>();

for (int i = 0; i < 10; i++) {
    map.putIfAbsent(i, "val" + i);
}

map.forEach((id, val) -> System.out.println(val));
```

The above code should be self-explaining: putIfAbsent prevents us from writing additional if null checks; forEach accepts a consumer to perform operations for each value of the map.
以上代码需要自我解释：putIfAbsent方法给我们提供了在写入操作的时候检查是否为null，forEach允许一个消费者执行每一个map的元素。

This example shows how to compute code on the map by utilizing functions:
这个例子给我们展示了怎么样通过函数式编程写map操作的代码。

```
map.computeIfPresent(3, (num, val) -> val + num);
map.get(3);             // val33

map.computeIfPresent(9, (num, val) -> null);
map.containsKey(9);     // false

map.computeIfAbsent(23, num -> "val" + num);
map.containsKey(23);    // true

map.computeIfAbsent(3, num -> "bam");
map.get(3);             // val33
```

Next, we learn how to remove entries for a given key, only if it's currently mapped to a given value:
下面，我们学习怎么样通过一个给定的key移除entries集，只有它当前映射到一个给定的值时：

```
map.remove(3, "val3");
map.get(3);             // val33

map.remove(3, "val33");
map.get(3);             // null
Another helpful method:

map.getOrDefault(42, "not found");  // not found
```

Merging entries of a map is quite easy:
合并一个映射集的entries是非常容易的：

```
map.merge(9, "val9", (value, newValue) -> value.concat(newValue));
map.get(9);             // val9

map.merge(9, "concat", (value, newValue) -> value.concat(newValue));
map.get(9);             // val9concat
```

Merge either put the key/value into the map if no entry for the key exists, or the merging function will be called to change the existing value.
合并不仅在当前key对应的entry不存在时把键值对放进映射集中，还有这个合并函数被调用来改变当前存在的值。

**Date API**

Java 8 contains a brand new date and time API under the package java.time. The new Date API is comparable with the Joda-Time library, however it's not the same. The following examples cover the most important parts of this new API.
Java 8在java.time包中包含了一些新的日期和时间的接口，新的日期接口可以和Joda-Time包相媲美，但是又不完全一样，下面这个例子包含了这些新的API的最重要的一部分。

**Clock**

Clock provides access to the current date and time. Clocks are aware of a timezone and may be used instead of System.currentTimeMillis() to retrieve the current time in milliseconds since Unix EPOCH. Such an instantaneous point on the time-line is also represented by the class Instant. Instants can be used to create legacy java.util.Date objects.
Clock类提供了访问当前日期和时间的方式，时钟类是各种各样的时间戳可以被用来替换System.currentTimeMillis()方法来检索距离Unix纪年法的当前时间的毫秒数，这个在时间线上的瞬时点也通过Instant类呈现，Instant类可以用来创建遗留的java.util.date对象。

```
Clock clock = Clock.systemDefaultZone();
long millis = clock.millis();

Instant instant = clock.instant();
Date legacyDate = Date.from(instant);   // legacy java.util.Date
```

**Timezones**

Timezones are represented by a ZoneId. They can easily be accessed via static factory methods. Timezones define the offsets which are important to convert between instants and local dates and times.
时区时间是通过ZoneId来呈现的，他们可以通过静态工厂方法被轻松的访问，时区时间定义了在Instant对象和本地日期和时间对象之间的转换标准。

```
System.out.println(ZoneId.getAvailableZoneIds());
// prints all available timezone ids

ZoneId zone1 = ZoneId.of("Europe/Berlin");
ZoneId zone2 = ZoneId.of("Brazil/East");
System.out.println(zone1.getRules());
System.out.println(zone2.getRules());

// ZoneRules[currentStandardOffset=+01:00]
// ZoneRules[currentStandardOffset=-03:00]
```

注：也就是Instant可以生成老的Date对象，Timezones类可以把Instant 转换成Time对象，反之亦然。

**LocalTime**

LocalTime represents a time without a timezone, e.g. 10pm or 17:30:15. The following example creates two local times for the timezones defined above. Then we compare both times and calculate the difference in hours and minutes between both times.
LocalTime没有时区的时间如：10pm或则17:30:15，下面的示例为上面时区时间创建了两个本地时间，然后我们比较了两个时间，并且计算了两个时间在小时和分钟上的差异。

```
LocalTime now1 = LocalTime.now(zone1);
LocalTime now2 = LocalTime.now(zone2);

System.out.println(now1.isBefore(now2));  // false

long hoursBetween = ChronoUnit.HOURS.between(now1, now2);
long minutesBetween = ChronoUnit.MINUTES.between(now1, now2);

System.out.println(hoursBetween);       // -3
System.out.println(minutesBetween);     // -239
LocalTime comes with various factory methods to simplify the creation of new instances, including parsing of time strings.

LocalTime late = LocalTime.of(23, 59, 59);
System.out.println(late);       // 23:59:59

DateTimeFormatter germanFormatter =
    DateTimeFormatter
        .ofLocalizedTime(FormatStyle.SHORT)
        .withLocale(Locale.GERMAN);

LocalTime leetTime = LocalTime.parse("13:37", germanFormatter);
System.out.println(leetTime);   // 13:37
```

**LocalDate**

LocalDate represents a distinct date, e.g. 2014-03-11. It's immutable and works exactly analog to LocalTime. The sample demonstrates how to calculate new dates by adding or subtracting days, months or years. Keep in mind that each manipulation returns a new instance.


LocalDate today = LocalDate.now();
LocalDate tomorrow = today.plus(1, ChronoUnit.DAYS);
LocalDate yesterday = tomorrow.minusDays(2);

LocalDate independenceDay = LocalDate.of(2014, Month.JULY, 4);
DayOfWeek dayOfWeek = independenceDay.getDayOfWeek();
System.out.println(dayOfWeek);    // FRIDAY
Parsing a LocalDate from a string is just as simple as parsing a LocalTime:

DateTimeFormatter germanFormatter =
    DateTimeFormatter
        .ofLocalizedDate(FormatStyle.MEDIUM)
        .withLocale(Locale.GERMAN);

LocalDate xmas = LocalDate.parse("24.12.2014", germanFormatter);
System.out.println(xmas);   // 2014-12-24

**LocalDateTime**

LocalDateTime represents a date-time. It combines date and time as seen in the above sections into one instance. LocalDateTime is immutable and works similar to LocalTime and LocalDate. We can utilize methods for retrieving certain fields from a date-time:

LocalDateTime sylvester = LocalDateTime.of(2014, Month.DECEMBER, 31, 23, 59, 59);

DayOfWeek dayOfWeek = sylvester.getDayOfWeek();
System.out.println(dayOfWeek);      // WEDNESDAY

Month month = sylvester.getMonth();
System.out.println(month);          // DECEMBER

long minuteOfDay = sylvester.getLong(ChronoField.MINUTE_OF_DAY);
System.out.println(minuteOfDay);    // 1439
With the additional information of a timezone it can be converted to an instant. Instants can easily be converted to legacy dates of type java.util.Date.

Instant instant = sylvester
        .atZone(ZoneId.systemDefault())
        .toInstant();

Date legacyDate = Date.from(instant);
System.out.println(legacyDate);     // Wed Dec 31 23:59:59 CET 2014
Formatting date-times works just like formatting dates or times. Instead of using pre-defined formats we can create formatters from custom patterns.

DateTimeFormatter formatter =
    DateTimeFormatter
        .ofPattern("MMM dd, yyyy - HH:mm");

LocalDateTime parsed = LocalDateTime.parse("Nov 03, 2014 - 07:13", formatter);
String string = formatter.format(parsed);
System.out.println(string);     // Nov 03, 2014 - 07:13
Unlike java.text.NumberFormat the new DateTimeFormatter is immutable and thread-safe.

For details on the pattern syntax read here.

**Annotations**

Annotations in Java 8 are repeatable. Let's dive directly into an example to figure that out.

First, we define a wrapper annotation which holds an array of the actual annotations:

@interface Hints {
    Hint[] value();
}

@Repeatable(Hints.class)
@interface Hint {
    String value();
}
Java 8 enables us to use multiple annotations of the same type by declaring the annotation @Repeatable.

**Variant 1: Using the container annotation (old school)**

@Hints({@Hint("hint1"), @Hint("hint2")})
class Person {}

**Variant 2: Using repeatable annotations (new school)**

@Hint("hint1")
@Hint("hint2")
class Person {}
Using variant 2 the java compiler implicitly sets up the @Hints annotation under the hood. That's important for reading annotation information via reflection.

Hint hint = Person.class.getAnnotation(Hint.class);
System.out.println(hint);                   // null

Hints hints1 = Person.class.getAnnotation(Hints.class);
System.out.println(hints1.value().length);  // 2

Hint[] hints2 = Person.class.getAnnotationsByType(Hint.class);
System.out.println(hints2.length);          // 2
Although we never declared the @Hints annotation on the Person class, it's still readable via getAnnotation(Hints.class). However, the more convenient method is getAnnotationsByType which grants direct access to all annotated @Hint annotations.

Furthermore the usage of annotations in Java 8 is expanded to two new targets:

@Target({ElementType.TYPE_PARAMETER, ElementType.TYPE_USE})
@interface MyAnnotation {}

**Where to go from here?**

My programming guide to Java 8 ends here. If you want to learn more about all the new classes and features of the JDK 8 API, check out my JDK8 API Explorer. It helps you figuring out all the new classes and hidden gems of JDK 8, like Arrays.parallelSort, StampedLock and CompletableFuture - just to name a few.

I've also published a bunch of follow-up articles on my blog that might be interesting to you:

[Java 8 Stream Tutorial](http://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/)
[Java 8 Nashorn Tutorial](http://winterbe.com/posts/2014/04/05/java8-nashorn-tutorial/)
[Java 8 Concurrency Tutorial: Threads and Executors](http://winterbe.com/posts/2015/04/07/java8-concurrency-tutorial-thread-executor-examples/)
[Java 8 Concurrency Tutorial: Synchronization and Locks](http://winterbe.com/posts/2015/04/30/java8-concurrency-tutorial-synchronized-locks-examples/)
[Java 8 Concurrency Tutorial: Atomic Variables and ConcurrentMap](http://winterbe.com/posts/2015/05/22/java8-concurrency-tutorial-atomic-concurrent-map-examples/)
[Java 8 API by Example: Strings, Numbers, Math and Files](http://winterbe.com/posts/2015/03/25/java8-examples-string-number-math-files/)
[Avoid Null Checks in Java 8](http://winterbe.com/posts/2015/03/15/avoid-null-checks-in-java/)
[Fixing Java 8 Stream Gotchas with IntelliJ IDEA](http://winterbe.com/posts/2015/03/05/fixing-java-8-stream-gotchas-with-intellij-idea/)
[Using Backbone.js with Java 8 Nashorn](http://winterbe.com/posts/2014/04/07/using-backbonejs-with-nashorn/)
You should follow me on Twitter. Thanks for reading!


