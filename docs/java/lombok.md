## 1. 安装 Lombok

使用 Lombok 之前我们先要在所使用的 IDE 中进行集成安装，这里以 IDEA 为例，安装步骤十分简单：

*   前往 `File -> Settings -> Plugin -> Marketplace` ，搜索 Lombok

*   选择搜索结果 Lombok ，点击 Install 安装。

*   安装完成后重启即可。

> 基于 Eclipse 的 Lombok 插件安装方法这里就不详细描述了，官方也给了对应的文档说明：[eclipse-lombok安装](https://link.juejin.im?target=https%3A%2F%2Fprojectlombok.org%2Fsetup%2Feclipse)

在 IDE 安装了 Lombok 插件后，我们就可以在 `pom.xml` 文件中添加 Lombok 的依赖进行使用了。[其它build工具安装方式](https://www.projectlombok.org/setup/overview)
```xml
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.8</version>
        <scope>provided</scope>
    </dependency>
```

> 注意：`pom` 依赖设置 scope 为 `provided`，是为了让 Lombok 库不被打包进程序。

## 2. @Getter/@Setter

> `@Getter/@Setter` 注解就能帮我们省去`getter` 和 `setter` 方法的维护，由 Lombok 对 `User` 类自动生成 `getter` 和 `setter` 方法，两者最终的字节码时一样的。

> 如果 `@Getter` 注解修饰了 `boolean` 类型的变量，
其生成的 `getter` 方法签名是 `isXXX` 形式，而不是 `getXXX`形式。

> `@Getter/@Setter` 还提供访问权限控制的属性 `lombok.AccessLevel value()`, 默认为 `PUBLIC`，而其他选值都是枚举类型：`MODULE, PROTECTED, PACKAGE, PRIVATE`


```java
/**
* 作用于方法，表示针对这个类的非静态字段自动生成 `getter /setter` 方法
*/
@Getter
@Setter
public class User {
    private Integer id;
    private String username;
    private String password;
}

/**
* 作用于字段 表示针对该字段自动生成 `getter /setter` 方法
*/
@Getter
@Setter
private String password;
```

## 3. @NonNull

用于标记类中不能允许为 `null` 的字段或者参数上，任何使用该字段的地方都生成空指针判断代码，若`@NonNull` 标记的变量为 null，抛出 `NullPointException`  异常。
```java

public class User {
    private Integer id;
    private String username;
    private String password;

    public User(Integer id, @NonNull String username, @NonNull String password) {
        this.id = id;
        this.username = username;
        this.password = password;
    }
}

/**
* 编译后生成的代码
**/
public class User {
    private Integer id;
    private String username;
    private String password;

    public User(Integer id, @NonNull String username, @NonNull String password) {
        if (username == null) {
            throw new NullPointerException("username is marked non-null but is null");
        } else if (password == null) {
            throw new NullPointerException("password is marked non-null but is null");
        } else {
            this.id = id;
            this.username = username;
            this.password = password;
        }
    }
}
```

## 4. 构造器注解
Lombok 提供了三个不同构造器注解 `@NoArgsConstructor / @AllArgsConstructor / @RequiredArgsConstructor` 分别对用不同构造器方法处理方式。

*   `@NoArgsConstructor` 为实体类生成无参的构造器方法

*   `@AllArgsConstructor` 为实体类生成除了`static`修饰的字段之外带有各参数的构造器方法。

*   `@RequiredArgsConstructor` 为实体类生成指定字段的构造器方法，而这些字段需要被 `final`，或者 `@NonNull` 修饰。

```java
@RequiredArgsConstructor
public class User3 {
    private Integer id;
    private final String username;
    @NonNull
    private String password;
}
```
    编译成功后使用构造器方法时就是这样的效果：`User3 user3 = new User3("user3", "zxc123");`

## 5. @ToString

`@ToString` 会给类自动生成易阅读的 `toString` 方法，带上有所非静态字段的属性名称和值，这样就十分便于我们日常开发时进行的打印操作。
```java
@Getter
@Setter
@AllArgsConstructor
@ToString
public class User2 {
    private Integer id;
    private String username;
    private String password;
}

//反编译结果
public String toString() {
    return "User2(id=" + this.getId() + ", username=" + this.getUsername() + ", password=" + this.getPassword() + ")";
}
```



另外，注解 `@ToString` 还支持设置指定哪些字段的日志化输出，哪些不需要出现在 `toString` 方法中。使用属性 `@ToString.Exclude`排除不需要在 `toString` 中出现的字段，使用 `@ToString.Include`标记需要出现在 `toString` 中的字段
```java
@Getter
@Setter
@AllArgsConstructor
@ToString
public class User2 {
    @ToString.Exclude
    private Integer id;
    @ToString.Include
    private String username;
    @ToString.Include
    private String password;
}
```
打印 `User2` 对象的日志效果就是：`User2(username=user2, password=zcx123)`。

## 6. @EqualsAndHashCode

`@EqualsAndHashCode` 注解就是用于根据类所拥有的非静态字段自动重写 `equals` 方法和 hashCode 方法,方便我们用于对象间的比较。类似 `@ToString`，`@EqualsAndHashCode` 还可以使用需要作为比较的字段和排除不需要比较的字段
```java
@Getter
@Setter
@AllArgsConstructor
@ToString
@EqualsAndHashCode
public class User4 {
    @EqualsAndHashCode.Exclude
    private Integer id;
    @EqualsAndHashCode.Include
    private String username;
    @EqualsAndHashCode.Exclude
    private String password;
}
```


## 7 @Data/@Value

`@Data/@Value` 注解，提供了更综合的生成代码功能，等价于下面几个注解

    @Getter
    @Setter
    @RequiredArgsConstructor
    @ToString
    @EqualsAndHashCode


两个注解都只能使用在类上，与 `@Data` 不同， `@Value` 用来修饰不可变的类上。一般实体类没有特别的限制的话，通常可以直接使用 `@Data` 注解修饰。

## 8 @Builder

`@Builder` 是一个非常强大的注解，提供了一种基于建造者模式的构建对象的 API。使用 `@Builder` 注解为给我们的实体类自动生成 `builder()` 方法，并且直接根据字段名称方法进行字段赋值，最后使用 `build()`方法构建出一个实体对象。
```java
@Data
@Builder
public class User6 {
    private Integer id;
    private String username;
    private String password;
}

@Test
public void testBuilder() {
    User6 user6 = User6.builder().id(1).username("user6").password("zxc123").build();
    log.warn("testLog: {}", user6);
    // User6(id=1, username=user6, password=zxc123)
}
```
需要注意的是 `@Builder` 不支持父类字段的生成，当一个实体类存在父类时，`@Builder` 只能生成当前类的字段构建方法。若需要用到父类的字段方法时， Lombok 提供了新的注解 `@SuperBuilder` 来应对这种情况。
```java
@SuperBuilder
@Getter
@Setter
public class Parent {
   private int id;
   private String name;
}

@SuperBuilder
@Data
public class Child extends Parent {
    private String childName;
}
```

    调用示例：Child child = Child.builder().id(1).name("父类名称").childName("子类名称").build();
    System.out.println(child.getId());


> 由于 Lombok Plugin 还未更新支持`@SuperBuilder`，所以以上写法在 IDEA 下还会提示编译错误，无法找到 `builder()`方法。

也可以参考此文方式去处理继承的情况：[lomboks-builder-annotation-and-inheritance/](https://reinhard.codes/2015/09/16/lomboks-builder-annotation-and-inheritance/)

## 9. 日志注解

应对程序类中常见不同框架 Logger 对象，Lombok 也提供了注解，来自动生成 Logger 对象，Lombok 支持了多个日志框架，并且提供对应的注解如下：

*   `@CommonsLog` 等价效果： `private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog(LogExample.class);`

*   `@Flogger` 等价效果： `private static final com.google.common.flogger.FluentLogger log = com.google.common.flogger.FluentLogger.forEnclosingClass();`

*   `@JBosLog` 等价效果： `private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger(LogExample.class);`

*   `@Log` 等价效果： `private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());`

*   `@Log4j` 等价效果： `private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger(LogExample.class);`

*   `@Log4j2` 等价效果： `private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class);`

*   `@Slf4j` 等价效果： `private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);`

*   `@XSlf4j` 等价效果: `private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);`

## 10. @Cleanup

`@Cleanup` 用于标记需要释放清理操作的资源对象变量，如 `FileInputStream`, `FileOutputStream` 等，标记之后资源对象使用完毕后，就会被自动关闭和清理，实际上这里 Lombok 实现效果与 Java7 特性 `try with resource` 一样, 为我们屏蔽了关闭资源的模板代码。
```java
public class CleanupExample {
    public static void main(String[] args) throws IOException {
        @Cleanup InputStream in = new FileInputStream(args[0]);
        @Cleanup OutputStream out = new FileOutputStream(args[1]);
        byte[] b = new byte[10000];
        while (true) {
            int r = in.read(b);
            if (r == -1) {
                break;
            }
            out.write(b, 0, r);
        }
    }
}
/**
* 反编译之后代码
*/
public class CleanupExample {

    public static void main(String[] args) throws IOException {
        FileInputStream in = new FileInputStream(args[0]);
        try {
            FileOutputStream out = new FileOutputStream(args[1]);
            try {
                byte[] b = new byte[10000];
                while(true) {
                    int r = in.read(b);
                    if (r == -1) {
                        return;
                    }
                    out.write(b, 0, r);
                }
            } finally {
                if (Collections.singletonList(out).get(0) != null) {
                    out.close();
                }
            }
        } finally {
            if (Collections.singletonList(in).get(0) != null) {
                in.close();
            }
        }
    }
}
```

## 11 @SneakyThrows

`@SneakyThrows` 主要用于在没有 `throws` 关键字的情况下，隐蔽地抛出受检查异常，为我们平常开发中需要异常抛出时省去的 `throw` 操作。
```java
public class SneakyThrowsExample implements Runnable {
    @SneakyThrows(UnsupportedEncodingException.class)
    public String utf8ToString(byte[] bytes) {
    return new String(bytes, "UTF-8");
    }

    @SneakyThrows
    public void run() {
    throw new Throwable();
    }
}
/**
* 反编译结果
*/
public class SneakyThrowsExample implements Runnable {
    public SneakyThrowsExample() {
    }

    public String utf8ToString(byte[] bytes) {
        try {
            return new String(bytes, "UTF-8");
        } catch (UnsupportedEncodingException var3) {
            throw var3;
        }
    }

    public void run() {
        try {
            throw new Throwable();
        } catch (Throwable var2) {
            throw var2;
        }
    }
}
```
