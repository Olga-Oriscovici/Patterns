# Patterns
Что такое паттерны?
Данная информация - это конспект по книги "Design patterns" , созданный в процессе изучения паттернов.

Паттерн предоставляет определенное решение задачи в определенном контексте. 
В процессе работы часто встречаются повторяющиеся задачи. Паттерны предоставляю своего рода шаблон решения повторяющихся задач. 

 Паттерны состоят из четырех основных элементов:
 
-Имя
Имя необходимо для того , чтобы разрабатывать архитектуру на более высоком уровне абстракции. Так же имя паттерна помогает легче обозначить проблему и задокументировать ее.

-Задача
Задача описывает в каком случае следует применять данный паттерн. 

-Решение
Описание элементов дизайна, отношений между ними , функции каждого элемента. Не предоставляет конкретный дизайн , а скорее шаблон. 

-Результаты 
Результаты применения паттерна. 

Паттерны разделяются на три категории:
-Порождающие 
-Структурные 
-Поведенческие 

Порождающие паттерны: 
Порождающие паттерны абстрагируют процесс инстанцирования. Паттерны порождающие классы используют наследования для того , чтобы изменять инстанцируемый класс, а паттерн порождающий объект делегирует инстанцирование другому объекту

1.Абстрактная фабрика

Название и классификация 
Абстрактная фабрика - паттерн порождающий объект

Назначение 
Абстрактная фабрика (Kit) - создаёт интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов , не определяя их конкретных классов 

Применимость 
-Если система не зависит от того , как создаются, компонуются и представляются входящие в нее данные
-Входящие в семейство взаимосвязаные объекты должны использоваться вместе
-Библиотека элементов , где реализация скрыта, виден только интерфейс

Структура

![image](https://github.com/user-attachments/assets/72c03087-1102-4799-bbcf-6ce796bec202)

Участники:
-AbstractFabrica- абстрактная фабрика, предоставляет интерфейс для классов определяющих объекты, делегирует создание объектов ConcreteFactory
-ConcreteFactory- реализует операции, создающие конкретные объекты-продукты
-AbstractProduct - объявляет интерфейс для типа объекта продукта
-ConcreteProduct - определяет конкретный объект продукт , который создаётся конкретной фабрикой 
-Client - клиент , который взаимодействует только с интерфейсами

Отношения:
1.Обычно конкретная фабрика создаёт конкретный объект продукт, чтобы получить другой , необходимо выбрать другую конкретную фабрику  в интерфейсе 
2.AbstractFactory предоверяет создание объектов своему подклассу ConcreteFactory

Результаты:
-Изолирование реализации обьектов
-Упрощает смену семейств продуктов , достаточно просто в интерфейсе выбрать другой вариант 
-Гарантирует сочетаемость продуктов, так как если объект используются вместе с другим , то важно реализовать механику использования исключительно одного объекта в момент времени 
-Сложно расширять систему  , так как при расширение системы необходимо расширять интерфейс абстрактной фабрики , а следовательно и подклассы

Пример кода:
// Абстрактные продукты

public interface IProductA
{
    string GetName();
}

public interface IProductB
{
    string GetDescription();
}

// Конкретные продукты

public class ProductA1 : IProductA
{
    public string GetName() => "Product A1";
}

public class ProductA2 : IProductA
{
    public string GetName() => "Product A2";
}

public class ProductB1 : IProductB
{
    public string GetDescription() => "Product B1";
}

public class ProductB2 : IProductB
{
    public string GetDescription() => "Product B2";
}

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
        // Выбираем фабрику
        
        IAbstractFactory factory = new Factory1();

        // Создаём продукты
        IProductA productA = factory.CreateProductA();
        IProductB productB = factory.CreateProductB();

        // Выводим информацию
        Console.WriteLine(productA.GetName());
        Console.WriteLine(productB.GetDescription());
    }
}


2.Builder

Название и классификация:
-Строитель: паттерны , порождающий объекты.

Назначение:
-Отделяет конструирование сложного объекта от его представления, так что в резуальтате одного и того же процесса конструирования могут получиться разные представления.

Применимость:
- Паттерн применяется в том случае , когда создание сложного объекта не должно зависеть от того из каких частей он создается и как они взаимодействуют между собой
- Паттерн применяется когда нужно создать различные представления,создаваемого объекта

Структура:
![image](https://github.com/user-attachments/assets/81ec2fb7-bbb0-4ae1-b5b1-449ab16d33b1)

Участники:
-Builder : создает абстрактный интерфейс для создания частей объекта Product;
-ConcreteBuilder :-создает части и собирает объект , реализуя интерфейс Builder;
                  -определяет представление и следит за ним;
                  -предоставляет интерфейс для доступа к продукту;

-Director : Конструирует объект
-Product: продук

Отношения:
-Клиент создает объект-распорядитель Director и конфигурирует его нужным объектом;
-Распорядитель уведомляет строителя, что нужно построить очередную часть продукта;
-Строитель обрабатывет запрос от распорядителя и создает нужные части;
-Клиент забирает продукт у строителя;

![image](https://github.com/user-attachments/assets/f122d18b-9db4-4ce7-9d7f-15c82f48ed69)

Результаты:
-представляет изменять внутренее представление продукта
-изолирует код
-дает более тонкий контроль над созданием продукта ,так как строитель создает объект шаг за шагом , до тех пор пока не завершит строительство распорядитель

 Пример кода:
 // Класс, который мы будем создавать с помощью строителя
 
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

    public void SetName(string name)
    {
        _product.Name = name;
    }

    public void SetDescription(string description)
    {
        _product.Description = description;
    }

    public Product Build()
    {
        return _product;
    }
}

// Директор (необязательно, но показывает пошаговое создание)

public class Director
{
    public Product Construct(IProductBuilder builder)
    {
        builder.SetName("Example Product");
        builder.SetDescription("This is a simple product.");
        return builder.Build();
    }
}

// Использование

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


 3. Фабричный метод

Название и классификация:
Фабричный метод (Factory Method) — порождающий паттерн (классы).

**Назначение:
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




                  
          








