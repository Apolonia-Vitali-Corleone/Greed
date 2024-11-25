# OOP

## 四大特点

面向对象编程（Object-Oriented Programming，OOP）具有以下几个主要特点：



### 封装

**封装（Encapsulation）**：

- 封装是将数据（属性）和操作数据的方法（方法或函数）打包到一个单元中，即类。在类中，数据的访问受到控制，可以通过公共方法（接口）来访问和操作数据，而不必了解内部实现细节。这样可以提高安全性和代码的可维护性，同时隐藏了实现的复杂性。



### 继承

**继承（Inheritance）**：

- 继承允许一个类（子类或派生类）基于另一个类（父类或基类）的定义来构建。子类可以继承父类的属性和方法，并且可以扩展或修改这些属性和方法，同时可以添加新的属性和方法。继承支持代码重用，提高了代码的可扩展性和可维护性。



### 多态

我的说明

主要就这三个功能，overloading overload，overriding override，子类向上转型



**多态（Polymorphism）**：

- 多态性允许使用统一的接口来操作不同类型的对象，即同一个方法调用可以在不同对象上具有不同的行为。多态性通过虚函数和函数重载来实现。它增加了灵活性和可扩展性，使得代码可以处理不同类型和层次结构的对象。



#### C++

当涉及多态时，我们通常会使用继承和虚函数来展示其工作原理。下面是一个简单的C++示例，演示了多态的基本概念：

```c++
#include <iostream>

// 基类 Animal
class Animal {
public:
    // 虚函数，用于多态
    virtual void makeSound() {
        std::cout << "Animal makes a sound\n";
    }
};

// 派生类 Dog
class Dog : public Animal {
public:
    // 重写基类的虚函数
    void makeSound() override {
        std::cout << "Dog barks\n";
    }
};

// 派生类 Cat
class Cat : public Animal {
public:
    // 重写基类的虚函数
    void makeSound() override {
        std::cout << "Cat meows\n";
    }
};

// 函数以基类指针作为参数，展示多态的效果
void animalSound(Animal *animal) {
    animal->makeSound(); // 调用虚函数，根据实际对象的类型调用相应的函数
}

int main() {
    Animal *ptrAnimal; // 声明基类指针

    Dog dog; // 创建 Dog 对象
    Cat cat; // 创建 Cat 对象

    ptrAnimal = &dog; // 将基类指针指向派生类对象
    animalSound(ptrAnimal); // 调用函数，输出 Dog barks

    ptrAnimal = &cat; // 将基类指针指向另一个派生类对象
    animalSound(ptrAnimal); // 调用函数，输出 Cat meows

    return 0;
}
```

代码说明：

1. **Animal 类**：
   - 定义了一个虚函数 `makeSound()`，它被声明为 `virtual`，这表示它可以被派生类重写。
2. **Dog 类和 Cat 类**：
   - 分别从 Animal 类继承，并重写了 `makeSound()` 函数，每个派生类有自己特定的实现。
3. **animalSound 函数**：
   - 接受一个 Animal 类的指针作为参数，调用 `makeSound()` 函数。由于 `makeSound()` 是虚函数，所以在运行时会根据实际对象的类型来确定调用哪个版本的函数。
4. **main 函数**：
   - 创建了一个指向 Animal 对象的指针 `ptrAnimal`。
   - 分别将其指向 Dog 对象和 Cat 对象，然后调用 `animalSound()` 函数来展示多态性的效果。具体来说，当 `ptrAnimal` 指向 Dog 对象时，调用的是 Dog 类中的 `makeSound()` 函数；当指向 Cat 对象时，调用的是 Cat 类中的 `makeSound()` 函数。

这个示例展示了如何利用继承和虚函数来实现多态性，使得相同的函数调用可以根据实际对象的类型来执行不同的行为。



#### java

在 Java 中，多态性通过继承和方法重写（覆盖）来实现。下面是一个简单的示例，展示了如何使用多态性：

```java
// 基类 Animal
class Animal {
    // 定义一个方法 makeSound
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

// 派生类 Dog 继承自 Animal
class Dog extends Animal {
    // 重写（覆盖）基类的 makeSound 方法
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

// 派生类 Cat 继承自 Animal
class Cat extends Animal {
    // 重写（覆盖）基类的 makeSound 方法
    @Override
    public void makeSound() {
        System.out.println("Cat meows");
    }
}

// 测试类
public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog(); // 使用基类引用指向派生类对象
        Animal myCat = new Cat(); // 使用基类引用指向派生类对象
        
        myDog.makeSound(); // 调用 Dog 类中重写的 makeSound 方法
        myCat.makeSound(); // 调用 Cat 类中重写的 makeSound 方法
    }
}
```

代码说明：

1. **Animal 类**：
   - 定义了一个公共方法 `makeSound()`，在基类中实现了动物发出声音的基本行为。
2. **Dog 类和 Cat 类**：
   - 分别从 Animal 类继承，并重写了 `makeSound()` 方法。每个派生类提供了自己的实现，分别表示狗叫和猫叫的声音。
3. **Main 类**：
   - 在 `main` 方法中，创建了两个 Animal 类型的引用 `myDog` 和 `myCat`。
   - 分别将这些引用指向 Dog 和 Cat 的实例。这里利用了多态性，因为 `myDog` 和 `myCat` 是 Animal 类型的引用，但实际上指向了不同的派生类对象。
   - 调用 `makeSound()` 方法时，由于 Java 的动态绑定特性，实际上会调用对应派生类中重写的 `makeSound()` 方法，分别输出 "Dog barks" 和 "Cat meows"。

通过这种方式，可以看到基类引用可以根据实际指向的对象类型来调用不同类中的方法，实现了运行时的多态性。这种灵活性和扩展性是面向对象编程中多态性的关键优势之一。



### 抽象

**抽象（Abstraction）**：

- 抽象是简化复杂现象的过程，通过定义类的抽象数据类型和公共接口来实现。抽象可以隐藏不必要的细节，只显示关键信息，使得代码更易理解和使用。抽象是面向对象设计的基础，促使开发人员关注于问题领域的关键特征。





## 重载和重写

重载（Overloading）和重写（Overriding）是面向对象编程中的两个重要概念。它们都涉及方法的定义和使用，但适用的场景和目的不同。

### 重载（Overloading）

重载指的是在同一个类中，方法名相同，但参数列表不同（参数类型不同，参数个数不同，或者两者都不同）的多个方法。重载实现了编译时的多态性。

#### 示例代码（Java）

```java
class MathUtils {
    // 重载方法 add，接收两个整数参数
    public int add(int a, int b) {
        return a + b;
    }

    // 重载方法 add，接收三个整数参数
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // 重载方法 add，接收两个双精度浮点数参数
    public double add(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        MathUtils mathUtils = new MathUtils();
        
        System.out.println(mathUtils.add(1, 2));          
        // 输出: 3
        System.out.println(mathUtils.add(1, 2, 3));       
        // 输出: 6
        System.out.println(mathUtils.add(1.5, 2.5));      
        // 输出: 4.0
    }
}
```

### 重写（Overriding）

重写指的是在子类中重新定义父类中已经存在的方法。重写的方法必须具有与父类方法相同的方法名、参数列表和返回类型。重写实现了运行时的多态性。

#### 示例代码（Java）

```java
class Animal {
    // 父类方法 makeSound
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    // 子类重写父类方法 makeSound
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Animal();
        Animal myDog = new Dog();
        
        myAnimal.makeSound(); // 输出: Animal makes a sound
        myDog.makeSound();    // 输出: Dog barks
    }
}
```

### 总结

- 重载（Overloading）：
  - 在同一个类中，方法名相同，但参数不同。
  - 发生在编译时，实现编译时多态性。
- 重写（Overriding）：
  - 在子类中重新定义父类的方法，方法名和参数列表相同。
  - 发生在运行时，实现运行时多态性。







# JAVA

## 抽象类

首先这是类

然后无法直接实现，必须有子类继承。

必须有至少一个抽象方法

## 接口

和class本身一个级别

可以有variables，但是就是常量

全部是抽象方法

可以有default方法，但是是java8+



# C++



构造函数

析构函数

## 虚函数VS纯虚函数

我的说明

虚函数必须有实现，子类可以覆写也可以不覆写

纯虚函数必须没有实现，子类必须覆写



在 C++ 中，虚函数和纯虚函数的区别如下：

**虚函数（Virtual Function）**：

- 声明为 `virtual` 的函数，可以在派生类中被重写。
- 具有默认实现。

**纯虚函数（Pure Virtual Function）**：

- 用 `= 0` 声明，没有实现。
- 必须在派生类中实现。

### 示例代码

```c++
#include <iostream>

// 基类
class Base {
public:
    virtual void regularFunction() {
        std::cout << "Base regularFunction" << std::endl;
    }

    virtual void pureVirtualFunction() = 0; // 纯虚函数
};

// 派生类
class Derived : public Base {
public:
    void regularFunction() override {
        std::cout << "Derived regularFunction" << std::endl;
    }

    void pureVirtualFunction() override {
        std::cout << "Derived pureVirtualFunction" << std::endl;
    }
};

int main() {
    Derived d;
    Base* b = &d;

    b->regularFunction();       // 输出: Derived regularFunction
    b->pureVirtualFunction();   // 输出: Derived pureVirtualFunction

    return 0;
}
```

### 输出

```c++
Derived regularFunction
Derived pureVirtualFunction
```

### 解释

- `regularFunction` 是虚函数，在 `Derived` 类中被重写。
- `pureVirtualFunction` 是纯虚函数，`Base` 类中没有实现，必须在 `Derived` 类中实现。
- 在运行时，通过基类指针调用函数，会调用派生类的实现。

