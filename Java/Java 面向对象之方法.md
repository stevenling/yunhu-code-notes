## 一、方法
方法类似 `c++` 中的函数概念。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/726118/1649140197659-80b74fca-62c6-491a-b255-0c0e63695bb3.png#clientId=uf8b00f36-0248-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=170&id=u73efe68f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=340&originWidth=831&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86066&status=done&style=none&taskId=ueb54a05b-1c7b-47aa-8a79-501796e4c20&title=&width=415.5)
### 1.1 实现方法
封装是面向对象的一个基本特性，可以隐藏信息。将字段的访问设为 `private` 私有的，就可以拒绝外部的访问。
```java
class Person {
    private String name;
    private int age;
}
```
通过方法来对字段进行设置值和获取值的操作。
```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("Xiao Ming"); // 设置name
        ming.setAge(12); // 设置age
        System.out.println(ming.getName() + ", " + ming.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 100) {
            throw new IllegalArgumentException("invalid age value");
        }
        this.age = age;
    }
}
```
在方法内部，可以设置值的范围，比如 `setAge()` 方法。
一个类通过定义方法，就可以给外部代码暴露一些操作的接口。
### 1.2 private 方法
```java
public class learn1 {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setBirth(2008);
        // ming.calcAge(2019); // 不可以 java: calcAge(int) 在 Person 中是 private 访问控制
        System.out.println(ming.getAge());
    }
}

class Person {
    private String name;
    private int birth;

    public void setBirth(int birth) {
        this.birth = birth;
    }

    public int getAge() {
        return calcAge(2019); // 调用private方法
    }

    // private方法:
    private int calcAge(int currentYear) {
        return currentYear - this.birth;
    }
}
```

第 5 行 `java: calcAge(int)` 在 `Person` 中是 `private` 访问控制，外部不可以直接调用。

这边 `getAge()` 方法获取 `age`，不用管 `Person` 内部到底有没有 `age` 字段，那是类的具体实现细节，封装就是会隐藏，调用方只管接口就好。
## 二、方法的参数
### 2.1 普通参数
普通参数有 `0` 个或 `n` 个，按照顺序和类型一一传递就好。
### 2.2 可变参数
可变参数相当于数组类型
```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}

Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```
## 三、参数绑定
### 3.1 基本类型参数传递
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 15; // n的值为15
        p.setAge(n); // 传入n的值
        System.out.println(p.getAge()); // 15
        n = 20; // n的值改为20
        System.out.println(p.getAge()); // 15 
    }
}

class Person {
    private int age;

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```
`n` 是一个外部的局部变量，`getAge()` 获取的是对象的 `age`，不是同一个数据。
### 3.2 引用参数传递
`java` 数组作为方法的参数时，传递的是地址。因此是会指向同一个位置。
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String[] fullname = new String[] { "Homer", "Simpson" };
        p.setName(fullname); // 传入fullname数组
        System.out.println(p.getName()); // "Homer Simpson"
        fullname[0] = "Bart"; // fullname数组的第一个元素修改为"Bart"
        System.out.println(p.getName()); // 输出 Bart Simpson"
    }
}

class Person {
    private String[] name;

    public String getName() {
        return this.name[0] + " " + this.name[1];
    }

    public void setName(String[] name) {
        this.name = name;
    }
}
```

