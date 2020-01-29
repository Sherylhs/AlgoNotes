# Table of Contents
- [Interface vs Abstract Class](#interface-vs-abstract-class)
- [Topological Sorting](#topological-sorting)
- [Message Queue](#message-queue)


## Interface vs Abstract Class
1. One class can only have a superclass, but can have multiple interfaces.
2. One class need to implement all the methods in the interfaces.
3. Interface is used to be `implements`, while Class is used to `extends`.
4. Abstract Class can have implemented methods.

```java
public class Main {
    public static void main(String[] args) {
        Car audiCar = new Audi();
        audiCar.drive();

        Car altoCar = new Alto();
        altoCar.drive();
    }
}
```

Interface version:
```java
interface Car {
    void drive();
    String getColor();
}

class Audi implements Car {
    @Override
    public void drive() {
        System.out.println("Interface: Audi drive");
    }

    @Override
    public String getColor() {
        return "Black";
    }
}

class Alto implements Car {
    @Override
    public void drive() {
        System.out.println("Interface: Alto drive");
    }

    @Override
    public String getColor() {
        return "Red";
    }
}
```

Abstract Class version:
```java
abstract class Car {
    protected String color;
    private String manufacturer;

    public Car() {
        color = "black";
        manufacturer = getManufacturer();
    }

    public void drive() {
        System.out.println("Abstract Class: " + this.manufacturer + " drive");
    }

    public String getColor() {
        return this.color;
    }

    protected abstract String getManufacturer();
}

class Audi extends Car {
    public Audi() {}

    @Override
    protected String getManufacturer() {
        return "Audi";
    }
}

class Alto extends Car {
    public Alto() {
        color = "red";
    }

    @Override
    protected String getManufacturer() {
        return "Alto";
    }
}
```

## Topological Sorting
算法流程：
1. 统计所有点的入度，并初始化拓扑序列为空。
2. 将所有入度为 0 的点，也就是那些没有任何依赖的点，放到宽度优先搜索的队列中。
3. 将队列中的点一个一个的释放出来，放到拓扑序列中，每次释放出某个点 A 的时候，就访问 A 的相邻点（所有A指向的点），并把这些点的入度减去 1。
4. 如果发现某个点的入度被减去 1 之后变成了 0，则放入队列中。
5. 直到队列为空时，算法结束。


## Message Queue
- 使用场景: https://www.zhihu.com/question/34243607
- RabbitMQ: https://blog.csdn.net/whoamiyang/article/details/54954780

