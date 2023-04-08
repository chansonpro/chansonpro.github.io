---
title: Effective Java [1]创建和销毁对象
date: 2023-04-08 11:44:42
tags: java
---
## 第1条 用静态工厂方法代替构造器
``` text
静态工厂方法是一种替代构造器的方式。它是一个静态方法，
可以用于创建和返回类的实例。
与构造器不同的是，静态工厂方法可以有自己的名称，可以返回不同类型的对象，
并且可以缓存对象以提高性能。
```
### 1.静态工厂方法的<font color=red>优点</font>包括
1.名称可以清晰地表达返回的对象类型和含义。
2.可以隐藏构造器，使类的实现更加灵活。
3.可以返回缓存的对象，避免重复创建对象。
4.可以返回子类的对象，实现接口的多态性。
5.可以根据不同的参数返回不同的对象。
### 2.静态工厂方法的<font color=red>缺点</font>包括
1.静态工厂方法可能会与其他静态方法混淆，使代码难以理解
2.静态工厂方法可能会导致类的实现变得复杂。
3.静态工厂方法的名称可能不够直观，需要进行文档化说明。

### <font color=#008000>总的来说，静态工厂方法是一种有用的替代构造器的方式，可以使类的实现更加灵活和可维护。但是，是否使用静态工厂方法还需要根据具体情况进行判断。</font>

### 3.具体示例
假设我们有一个类叫做Person，我们可以使用静态工厂方法代替构造器：
``` java
public class Person {
    private String name;
    private int age;

    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public static Person createPerson(String name, int age) {
        return new Person(name, age);
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}

```
在上面的代码中，我们使用了一个名为createPerson的静态工厂方法来创建Person对象。这个方法接受一个name和age参数，并返回一个新的Person对象。我们也可以添加一些额外的逻辑，例如缓存对象或者返回子类的对象。
使用静态工厂方法创建Person对象的代码如下：
```java
Person person = Person.createPerson("Tom", 25);
String name = person.getName(); // 返回 "Tom"
int age = person.getAge(); // 返回 25
```
可以看到，使用静态工厂方法创建对象非常简单，而且可以使代码更加清晰和易于维护。
静态工厂方法在Java的类库中非常常见，以下是一些例子：
```text
java.util.Collections类中有很多静态工厂方法，例如emptyList()和singletonList()，它们用于创建空List和只包含一个元素的List。
java.nio.charset.Charset类中有一个静态工厂方法forName()，它用于获取Charset对象。
java.util.EnumSet类中有两个静态工厂方法of()和range()，它们用于创建枚举类型的集合。
java.util.regex.Pattern类中有一个静态工厂方法compile()，它用于编译正则表达式并返回一个Pattern对象。
java.security.KeyFactory类中有一个静态工厂方法getInstance()，它用于获取KeyFactory对象。
```
这些都是Java类库中使用静态工厂方法的例子，它们在实际开发中都非常有用。
### 4.QA
#### <font color=Blue>4.1 现在绝大多数对象都是由 spring 容器来管理了，静态工厂创建对象用的多吗？</font>
```text
1、Spring框架本身也使用了静态工厂方法来创建一些对象，例如RestTemplate和JdbcTemplate等。
2、在一些情况下，我们可能无法使用Spring容器来创建对象，例如在没有使用Spring的Java应用程序中创建对象。此时，我们可以使用静态工厂方法来创建对象。
3、在某些情况下，我们需要在运行时动态决定创建哪个类的对象。这时静态工厂方法也能胜任。
4、静态工厂方法可以隐藏对象创建过程中的复杂性，并将其抽象化，使得客户端代码更加简洁和易于维护。
5、静态工厂方法基于名称，可以返回不同的实现类对象，这样就可以在不改变客户端代码的情况下，灵活地切换不同的实现类对象。
```
总之，虽然Spring框架提供了强大的对象管理功能，但静态工厂方法还是有其使用场景和优点的。
#### <font color=Blue>4.2 有哪些类应该用静态工厂方法代替构造器？</font>
通常情况下，以下情况适合使用静态工厂方法代替构造器：
```text
1、当类需要有多个构造器时，使用静态工厂方法可以提供更加易于理解和使用的构造器名称。
2、当创建对象的过程比较复杂或需要进行复杂的初始化操作时，静态工厂方法可以将初始化逻辑封装起来，让客户端代码更加简洁和易于维护。
3、当需要动态切换不同实现类的实例时，静态工厂方法可以通过名称来进行实例的获取，而无需直接调用构造器。
4、当类需要进行缓存对象时，静态工厂方法可以通过缓存机制避免重复创建对象，提高性能。
5、当需要控制类实例的数量时，静态工厂方法可以通过限制实例数量来控制程序的资源消耗。
```
例如，Java中的Collections类就是通过静态工厂方法来创建List、Set、Map等集合类对象的。另外，Java中的Calendar类也通过静态工厂方法来创建对象，例如getInstance()方法。
#### <font color=Blue>4.3 有哪些类的实例应该缓存，而不用每次新建？</font>
以下是一些建议缓存实例的类：
```text
1、不可变对象：由于不可变对象在创建后就不会再改变，因此可以缓存实例，避免重复创建。
2、大量使用的对象：例如线程池、数据库连接池等对象，由于创建和销毁对象会造成较大的资源开销，因此可以缓存实例，重复利用。
3、昂贵创建的对象：例如网络连接、文件IO等操作，由于创建和销毁对象都需要时间和资源，因此可以缓存实例，重复利用。
4、单例模式的对象：由于单例模式的对象只有一个实例，因此可以缓存实例，避免重复创建。
```
需要注意的是，如果缓存的实例不再使用，需要及时清理，避免占用过多的内存空间。另外，为了保证缓存的实例的线程安全性，可以考虑使用线程安全的数据结构，如ConcurrentHashMap等。

####  <font color=Blue>4.4 有哪些代码应该用接口代替子类型，且子类型可以不对外暴露？</font>
以下是一些应该用接口代替子类型的情况，且子类型可以不对外暴露的代码：
```text
1、服务提供者接口（Service Provider Interface，SPI）：在实现SPI时，通常会定义接口和实现类，实现类不对外暴露，而是由其对应的接口进行暴露。
这样做可以让实现类的实现细节被封装，同时也可以让应用程序可以灵活地选择具体的实现类，而无需修改代码。
2、抽象工厂模式：在抽象工厂模式中，工厂类通过接口暴露创建对象的方法，具体的产品实现类不对外暴露。这样做可以让客户端代码只依赖于抽象接口，
而不依赖于具体实现类，从而实现了松耦合。
3、适配器模式：在适配器模式中，适配器实现了目标接口，并通过委托方式调用适配者的方法。在这种情况下，适配者不需要对外暴露，因为适配器已经将其封装起来了。
4、桥接模式：在桥接模式中，抽象部分（Abstraction）通过接口暴露行为，具体部分（Implementor）不对外暴露。这样做可以使得抽象部分与具体部分相互独立，从而可以灵活地进行组合。
```
需要注意的是，虽然子类型可以不对外暴露，但仍然需要保证其被正确地实现和使用。因此，在定义接口时，应该清楚地定义接口的规范和使用方式，以确保其正确性和可靠性。
具体示例如下：
<font color=gray>一个简单的图形库，其中包含图形（如圆形、矩形、三角形等）和绘制器（如画笔、填充器等）两个概念。
在这种情况下，可以定义一个图形接口和一个绘制器接口，具体的图形实现类和绘制器实现类不对外暴露，而是由其对应的接口进行暴露。</font>
例如，定义一个绘制圆形的接口：
```java
public interface Circle {
    void draw(DrawingTool tool);
}
```
其中，DrawingTool是一个绘制器接口，定义了绘制器的方法。
然后，定义一个具体的圆形实现类：
```java
class CircleImpl implements Circle {
    private int x, y, radius;

    public CircleImpl(int x, int y, int radius) {
        this.x = x;
        this.y = y;
        this.radius = radius;
    }
    @Override
    public void draw(DrawingTool tool) {
        tool.drawCircle(x, y, radius);
    }
}
```
其中，draw方法接受一个DrawingTool参数，用于绘制圆形。
最后，定义一个绘制器接口：
```java
public interface DrawingTool {
    void drawCircle(int x, int y, int radius);
    void drawRectangle(int x, int y, int width, int height);
    void drawTriangle(int x1, int y1, int x2, int y2, int x3, int y3);
}
```
在这种情况下，具体的绘制器实现类不需要对外暴露，因为它们的方法都是由DrawingTool接口定义的。客户端可以使用如下代码来绘制圆形：
```java
DrawingTool tool = new MyDrawingTool();
Circle circle = new CircleImpl(100, 100, 50);
circle.draw(tool);
```
其中，MyDrawingTool是一个具体的绘制器实现类，不需要对外暴露。
