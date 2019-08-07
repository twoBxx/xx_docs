## java8

### Functional Interface

#### 函数式接口
指只有一个抽象方法的接口，用作Lambda表达式的类型。

使用 `@FunctionalInterface ` 用以检查是否符合函数式接口的标准。

| 接口              | 参数  | 返回类型 |     示例      |
| :---------------- | :---: | :------: | :-----------: |
| Predicate<T>      |   T   | boolean  | 是否已满18岁  |
| Consumer<T>       |   T   |   void   |  输出一个值   |
| Function<T,R>     |   T   |    R     | 由usb转type-c |
| Supplier<T>       | NONE  |    T     |   工厂方法    |
| UnaryOperator<T>  |   T   |    T     |    逻辑非     |
| BinaryOperator<T> | (T,T) |    T     |   两数相加    |

### 常用的流操作

#### 1. collect(toList()) 

由Stream里的值生成一个列表

``` java
List<String> collected = Stream.of("a", "b", "c") .collect(Collectors.toList()); 
assertEquals(Arrays.asList("a", "b", "c"), collected);  
```

#### 2. map

将一种类型的值转换成另一种类型

```java
List<String> collected = Stream.of("a","b","c")
               .map(s -> s.toUpperCase())
               .collect(Collectors.toList());
assertEquals(asList("A", "B", "C"), collected);

```

#### 3. flatMap

将多个Stream连接成一个Stream

```java
List<Integer> together = Stream.of(asList(1, 2), asList(3, 4))
                .flatMap(numbers -> numbers.stream())
                .collect(toList());
assertEquals(asList(1, 2, 3, 4), together);
```



#### 4. filter

从数据集中筛选出符合条件的数据

```java
List<String> beginningWithNumbers = Stream.of("a", "1abc", "abc1")
                .filter(value -> isDigit(value.charAt(0)))
                .collect(toList());
assertEquals(asList("1abc"), beginningWithNumbers);
```

####  5. reduce

从一组值中生成一个值，count、min、max都是基于reduce实现的

```java
//acc是累加器，element是元素
int count = Stream.of(1,2,3,4).reduce(0,(acc,element) -> a + b);
assertEquals(10, count);
```



 