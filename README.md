
---

# Patterns

**Что такое паттерны?**
Это конспект по книге «Design Patterns», созданный в процессе изучения паттернов.

Паттерн — это определённое решение задачи в конкретном контексте. В процессе разработки часто возникают повторяющиеся задачи, и паттерны помогают их решать, предоставляя своего рода **шаблон** решения.

---

## Основные элементы паттерна

* **Имя**
  Нужно для того, чтобы разрабатывать архитектуру на более высоком уровне абстракции. Также помогает легче обозначить проблему и задокументировать её.

* **Задача**
  Описывает, в каком случае применять данный паттерн.

* **Решение**
  Описание элементов дизайна, их отношений и функций. Это не конкретный дизайн, а именно шаблон.

* **Результаты**
  Последствия и плюсы/минусы применения паттерна.

---

## Категории паттернов

✅ **Порождающие**
✅ **Структурные**
✅ **Поведенческие**

---

## Порождающие паттерны

Порождающие паттерны абстрагируют процесс создания объектов:

* **Паттерны, порождающие классы** — используют наследование, чтобы изменять создаваемый класс.
* **Паттерны, порождающие объекты** — делегируют создание объектов другому объекту.

---

### 1. Абстрактная фабрика (Abstract Factory)

**Классификация:**
Порождающий паттерн (объекты).

**Назначение:**
Создаёт интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов, не определяя их конкретные классы.

**Применимость:**

* Система не зависит от того, как создаются и компонуются входящие в неё объекты.
* Семейство взаимосвязанных объектов должно использоваться вместе.
* Библиотека элементов, где скрыта реализация, а виден только интерфейс.

![image](https://github.com/user-attachments/assets/67e1b852-083d-4fda-a2e7-918300815848)

**Участники:**

* **AbstractFactory** — абстрактная фабрика, определяет интерфейсы.
* **ConcreteFactory** — конкретная фабрика, создаёт продукты.
* **AbstractProduct** — интерфейс для продукта.
* **ConcreteProduct** — конкретная реализация продукта.
* **Client** — использует только интерфейсы.

**Отношения:**

* Каждая конкретная фабрика создаёт конкретный объект.
* AbstractFactory делегирует создание объектов ConcreteFactory.

**Результаты:**
✅ Изолирует реализацию объектов.
✅ Упрощает смену семейства продуктов.
✅ Гарантирует сочетаемость продуктов.
⚠️ Сложно расширять — при расширении нужно обновлять все фабрики и интерфейсы.

**Пример кода:**

```csharp
// Абстрактные продукты
public interface IProductA { string GetName(); }
public interface IProductB { string GetDescription(); }

// Конкретные продукты
public class ProductA1 : IProductA { public string GetName() => "Product A1"; }
public class ProductA2 : IProductA { public string GetName() => "Product A2"; }
public class ProductB1 : IProductB { public string GetDescription() => "Product B1"; }
public class ProductB2 : IProductB { public string GetDescription() => "Product B2"; }

// Абстрактная фабрика
public interface IAbstractFactory
{
    IProductA CreateProductA();
    IProductB CreateProductB();
}

// Конкретные фабрики
public class Factory1 : IAbstractFactory
{
    public IProductA CreateProductA() => new ProductA1();
    public IProductB CreateProductB() => new ProductB1();
}

public class Factory2 : IAbstractFactory
{
    public IProductA CreateProductA() => new ProductA2();
    public IProductB CreateProductB() => new ProductB2();
}

// Клиент
class Program
{
    static void Main(string[] args)
    {
        IAbstractFactory factory = new Factory1();

        IProductA productA = factory.CreateProductA();
        IProductB productB = factory.CreateProductB();

        Console.WriteLine(productA.GetName());
        Console.WriteLine(productB.GetDescription());
    }
}
```

---

### 2. Строитель (Builder)

**Классификация:**
Порождающий паттерн (объекты).

**Назначение:**
Отделяет конструирование сложного объекта от его представления, так что в результате одного и того же процесса могут получаться разные представления объекта.

**Применимость:**

* Когда создание сложного объекта не должно зависеть от того, из каких частей он состоит.
* Когда нужно создавать различные представления создаваемого объекта.

![image](https://github.com/user-attachments/assets/5c3338d4-628a-49f3-bbc5-4e370fa836b5)

**Участники:**

* **Builder** — абстрактный интерфейс для создания частей объекта.
* **ConcreteBuilder** — реализует интерфейс, определяет внутреннее представление объекта.
* **Director** — управляет процессом пошагового создания.
* **Product** — создаваемый объект.

**Отношения:**

* Клиент создаёт объект Director и передаёт ему Builder.
* Director управляет процессом создания.
* Builder создаёт объект шаг за шагом.
* Готовый продукт возвращается клиенту.

![image](https://github.com/user-attachments/assets/48ea9856-c6ed-4f68-bd49-1eda02a69a5c)


**Результаты:**
✅ Позволяет изменять внутреннее представление объекта.
✅ Изолирует код создания.
✅ Дает гибкий пошаговый контроль.

**Пример кода:**

```csharp
// Продукт
public class Product
{
    public string Name { get; set; }
    public string Description { get; set; }
}

// Интерфейс строителя
public interface IProductBuilder
{
    void SetName(string name);
    void SetDescription(string description);
    Product Build();
}

// Конкретный строитель
public class ConcreteProductBuilder : IProductBuilder
{
    private Product _product = new Product();

    public void SetName(string name) => _product.Name = name;
    public void SetDescription(string description) => _product.Description = description;
    public Product Build() => _product;
}

// Директор (необязателен, но часто используется)
public class Director
{
    public Product Construct(IProductBuilder builder)
    {
        builder.SetName("Example Product");
        builder.SetDescription("This is a simple product.");
        return builder.Build();
    }
}

// Клиентский код
class Program
{
    static void Main(string[] args)
    {
        IProductBuilder builder = new ConcreteProductBuilder();
        Director director = new Director();

        Product product = director.Construct(builder);

        Console.WriteLine($"Name: {product.Name}");
        Console.WriteLine($"Description: {product.Description}");
    }
}
```

 3. Фабричный метод

Название и классификация:
Фабричный метод (Factory Method) — порождающий паттерн (классы).

**Назначение**:
Определяет интерфейс для создания объекта, но оставляет подклассам решение о том, какой класс инстанцировать. Таким образом, фабричный метод позволяет делегировать создание объектов дочерним классам.

**Известен также как:**
Virtual Constructor (виртуальный конструктор).

**Применимость:**
Используется, когда:

* Классу заранее неизвестно, объекты каких классов он должен создавать.
* Класс спроектирован так, чтобы позволять подклассам определять тип создаваемых объектов.
* Класс делегирует обязанности одному из нескольких вспомогательных подклассов, и вы хотите локализовать знание о том, какой класс принимает эти обязанности.

**Структура:**
![image](https://github.com/user-attachments/assets/2b7edc30-0266-4dc5-9cdd-2bb3b208f1db)

**Участники:**

* **Product** — общий интерфейс или абстрактный класс для объектов, которые будут созданы.
* **ConcreteProduct** — конкретная реализация интерфейса Product.
* **Creator** — объявляет фабричный метод, который возвращает объекты типа Product. Может иметь реализацию по умолчанию.
* **ConcreteCreator** — переопределяет фабричный метод, чтобы возвращать конкретные продукты.

**Отношения:**

* Creator полагается на подклассы в определении фабричного метода, который возвращает экземпляр нужного продукта.

**Результаты применения:**
✅ Избавляет класс от привязки к конкретным классам продуктов.
✅ Делает код более гибким и расширяемым — добавление нового типа продукта требует создания нового подкласса Creator.
✅ Упрощает поддержку и сопровождение — знание о создании объекта локализовано.
⚠️ Может привести к созданию большого числа дополнительных классов (по одному Creator для каждого ConcreteProduct).

---

### Пример кода (C#)

```csharp
// Абстрактный продукт
public interface IProduct
{
    void ShowInfo();
}

// Конкретный продукт A
public class ConcreteProductA : IProduct
{
    public void ShowInfo()
    {
        Console.WriteLine("Это продукт A.");
    }
}

// Конкретный продукт B
public class ConcreteProductB : IProduct
{
    public void ShowInfo()
    {
        Console.WriteLine("Это продукт B.");
    }
}

// Абстрактный создатель (Creator)
public abstract class Creator
{
    // Фабричный метод
    public abstract IProduct CreateProduct();

    // Некоторая логика, которая использует продукт
    public void SomeOperation()
    {
        IProduct product = CreateProduct();
        product.ShowInfo();
    }
}

// Конкретный создатель A
public class ConcreteCreatorA : Creator
{
    public override IProduct CreateProduct()
    {
        return new ConcreteProductA();
    }
}

// Конкретный создатель B
public class ConcreteCreatorB : Creator
{
    public override IProduct CreateProduct()
    {
        return new ConcreteProductB();
    }
}

// Клиентский код
class Program
{
    static void Main(string[] args)
    {
        // Можно легко изменить продукт, просто выбрав другой конкретный создатель
        Creator creatorA = new ConcreteCreatorA();
        creatorA.SomeOperation();

        Creator creatorB = new ConcreteCreatorB();
        creatorB.SomeOperation();
    }
}
```


---

### 4. Одиночка (Singleton)

**Классификация:**
Порождающий паттерн (объект).

**Назначение:**
Гарантирует, что у класса есть только один экземпляр, и предоставляет глобальную точку доступа к нему.

**Применимость:**

* Когда должен существовать ровно один экземпляр объекта, и он должен быть доступен из разных частей системы.
* Когда нужно обеспечить более строгий контроль над доступом к единственному экземпляру.

**Участники:**

* **Singleton** — определяет операцию `Instance`, которая позволяет получить доступ к единственному экземпляру. Скрывает конструктор, чтобы предотвратить создание новых объектов.
![image](https://github.com/user-attachments/assets/8e6be2a2-5eff-42b7-8e8a-262ff76fa221)

**Отношения:**

* Клиент получают доступ к Singleton только через Instance.

**Результаты:**
✅ Уменьшение числа имён
Singleton — шаг вперёд по сравнению с глобальными переменными. Позволяет избежать засорения пространства имён глобальными переменными, в которых хранятся уникальные экземпляры.

✅ Допускает уточнение операций и представления
От класса Singleton можно порождать подклассы, а приложение может быть сконфигурировано экземпляром расширенного класса. Это даёт возможность приложениям использовать экземпляр именно того класса, который нужен во время выполнения.

✅ Допускает переменное число экземпляров
Хотя Singleton предполагает только один экземпляр, паттерн позволяет легко изменить это ограничение, если в приложении нужно использовать несколько экземпляров. Необходимо изменить лишь один элемент — класс Singleton.

✅ Большая гибкость, чем у операций класса
Ещё один способ реализовать функциональность синглтона — использовать статические члены класса (например, функции-члены в C++ или методы класса в Smalltalk). Однако эти методы:

Не позволяют изменять реализацию (например, если нужны виртуальные функции в C++ — их нельзя объявить в статических методах).

Singleton позволяет более гибко управлять созданием экземпляров, заменять реализацию в подклассах и расширять функциональность..

**Пример кода:**

```csharp
public class Singleton
{
    private static Singleton _instance;

    // Закрытый конструктор, чтобы нельзя было создать напрямую
    private Singleton() { }

    // Статический метод для получения экземпляра
    public static Singleton Instance
    {
        get
        {
            if (_instance == null)
                _instance = new Singleton();
            return _instance;
        }
    }

    // Пример метода
    public void DoAction()
    {
        Console.WriteLine("Singleton works!");
    }
}

// Использование
class Program
{
    static void Main(string[] args)
    {
        Singleton singleton1 = Singleton.Instance;
        Singleton singleton2 = Singleton.Instance;

        singleton1.DoAction();
        Console.WriteLine(singleton1 == singleton2); // true, один и тот же объект
    }
}
```

---

### 5. Прототип (Prototype)

**Классификация:**
Порождающий паттерн (объект).

**Назначение:**
Определяет виды создаваемых объектов с помощью прототипа и создаёт новые объекты путем клонирования этого прототипа.

**Применимость:**

* Когда создание объекта напрямую слишком затратное (например, сложная инициализация).
* Когда нужно избежать создания подклассов (альтернатива абстрактной фабрике).
* Когда объекты должны быть независимыми от их классов.

**Участники:**

* **Prototype** — интерфейс с методом `Clone()`, который будет вызывать клиент.
* **ConcretePrototype** — конкретная реализация, которая клонирует себя.
![image](https://github.com/user-attachments/assets/e99749dd-0239-401e-a2f9-a6d308d7dcc7)



**Отношения:**

* Клиент вызывает `Clone()` для получения нового объекта на основе прототипа.

**Результаты:**
✅ Упрощает создание новых объектов.
✅ Изолирует код создания объекта.
⚠️ Может быть сложно, если у объекта много ссылок на другие объекты (глубокое копирование).

**Пример кода:**

```csharp
// Интерфейс прототипа
public interface IPrototype
{
    IPrototype Clone();
}

// Конкретный прототип
public class ConcretePrototype : IPrototype
{
    public string Data { get; set; }

    public IPrototype Clone()
    {
        // Поверхностное копирование
        return (IPrototype)this.MemberwiseClone();
    }
}

// Использование
class Program
{
    static void Main(string[] args)
    {
        ConcretePrototype prototype = new ConcretePrototype { Data = "Prototype Data" };
        ConcretePrototype clone = (ConcretePrototype)prototype.Clone();

        Console.WriteLine(prototype.Data); // Prototype Data
        Console.WriteLine(clone.Data);     // Prototype Data
        Console.WriteLine(prototype == clone); // false — это разные объекты
    }
}
```





                  
          








