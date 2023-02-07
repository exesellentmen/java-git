# Java Core
## Оглавление 

1. [Стримы](#Стримы)
2. [Многопоточность](#Многопоточность)


### 1. Стримы

### 2. Многопоточность
[:arrow_up:Оглавление](#Оглавление) 

**2.1 Что такое многопоточность(Multithreading)**<br />
[:arrow_up:Оглавление](#Оглавление) <br />
 <br /> 

**Многопоточность** - это процесс выполнения нескольких потоков подобна Ajax. Мы можем запсускать задачу на выполнение, параллельно с этим мы можем выводить результат на сайт, или притсупать к другим задачам либо мы можем разбивать задачу и запускать ее в несколько потоков. <br />
 <br /> 

**2.2 Что такое Поток(Thread)** <br />
[:arrow_up:Оглавление](#Оглавление)  <br />
**Поток(Thread)** - это легкий небольшой процесс вычесления, который может выполняться с другими потоками внутри процесса <br />

**Пример потока(Thread)** 
```java
Thread threadA = new Thread(() -> {
    System.out.println("Operation's completed");
});
threadA.start();
```
 <br /> 

**2.3 Что такое Процесс(Process)** <br />
[:arrow_up:Оглавление](#Оглавление)  <br />
**Процесс(Process)** - это отдельный тяжеловесный процесс, который имеет свое пространство в памяти и передача данных между процессами долгая <br />
 <br /> 

**2.4 Что такое Многопроцессорность(Multiprocessing)** <br />
[:arrow_up:Оглавление](#Оглавление)  <br />
**Многопроцессорность(Multiprocessing)** - это когда запускается выполнение нескольких процессов параллельно <br />
Подробнее: https://www.c-sharpcorner.com/article/a-complete-multithreading-tutorial-in-java/
 <br /> 

**2.5 Что такое Планировщик(Scheduler)**<br />
[:arrow_up:Оглавление](#Оглавление) <br />
**Планировщик(Scheduler)** - это система, которая выбирает поток, который будет выполняться в процессе. На его выбор может повлиять приоритет, который установлен, чем больше приоритет тем более важен, максимум 10 минимум 0 обычно 5 <br />

**2.6 Что такое Многозадачность(Multitasking)** <br />
[:arrow_up:Оглавление](#Оглавление)  <br />
**Многозадачность(Multitasking)** - это когда необходимо выполнить несколько задач одновременно, можно выполнять с помощью многопоточности или многопроцессорности <br /> 

**2.7 Что такое Пул потоков(Thread Pool)** <br />
[:arrow_up:Оглавление](#Оглавление)  <br />
**Пул потоков(Thread Pool)** - это набор workers воркеров потоков, которые находятся в ожидании работы, которые могут выполняться много раз. Это дает возможность не затрачивать время на создание потоков, а использовать уже поток из ограниченного существующего набора потоков <br />

**Пример пул потоков(Thread Pool), будет выполняться по 5 потоков, остальные в очереди** <br />
```java
ExecutorService executorService = Executors.newFixedThreadPool(5);
for(int n = 0; n < 40; n++){
    Integer i = n;
    Runnable runnable = () -> {
        System.out.println("Show something!");
    };
    executorService.execute(runnable);
}
executorService.shutdown();
```
 <br /> 













 <br /> <br /> <br /> <br /> <br />









**Переопределение Override** - это когда мы в классе наследнике переписываем реализацию метода на новую, при этом обратиться к методу родителя мы можем с помощью `super.название_метода()` Принято снабжать аннотацие `@Override`

**Пример Переопределения Override**

```java
public class Animal {
    
    public void voice() {
        System.out.println("Голос!");
    }
    
}

// Переопределяем метод в классе Cat
public class Cat extends Animal {

    @Override
    public void voice() {
        System.out.println("Мяу!"); //Мяу!
        //Вызываем родительский метод Animal.voice()
        super.voice(); //Голос!
    }
}
```

**Требования:**
- Должны принимать одинаковые аргументы
- Должны возвращать тот же тип переменных
- Не могут содержать уровень доступа более открытый
- Не могут выбрасывать новые exception

---

**Перегрузка Overload** - это когда метод может работать с разными параметрами, когда мы создаем один и тот же метод несколько раз с разными параметрами
Требования:
- Должны иметь новые аргументы
- Могут иметь другой тип результата
- Могут иметь новый уровень доступа
- Могут бросать новые exception

Статические методы нельзя переопределить, можно только перегрузить.

***Примеры Перегрузки Overload***
```java
// Количество параметров
public class Calculator {
    void calculate(int number1, int number2) { }
    void calculate(int number1, int number2, int number3) { }
}

// Типы параметров
public class Calculator {
    void calculate(int number1, int number2) { }
    void calculate(double number1, double number2) { }
}

// Порядок параметров
public class Calculator {
    void calculate(double number1, int number2) { }
    void calculate(int number1, double number2) { }
}
```

---

### 2. Виды полиморфизма, статический и динамический?

***Статический полиморфизм*** - это использование перегрузки методов

***Динамический полиморфизм*** - это когда мы используем переопределение методов

---

### 3. Абстракция
***Абстракция*** - Это черная коробка, когда мы можем пользоваться классами без погружения в детали.

Например есть библиотека, которую мы скачали, мы не знаем что внутри нее, как она работает, как взаимодействуют классы между собой внутри, и т д Но у нас есть например инструкция по использованию классов этой библиотеки.

В Java абстракция достигается с использованием абстрактных классов и интерфейсов.

***Абстрактный класс*** - это класс, на основе которого мы не можем создать объекты, но мы можем наследовать от него другой класс. Например животное(Абстрактный класс) и кошка(Класс наследник)
```java
// Абстрактный класс
abstract class Animal{
    public abstract void move();
    
    public void voice(){
        System.out.println("AAAA!!");
    }
}

// Класс наследник, в котором необходимо реализовать недостающие методы или переписать уже имеющиеся
class Cat extends Animal{

    //Добавляем необходимую реализацию для абстрактного класса, без нее будет ошибки
    @Override
    public void move() {
        System.out.println("The cat started to move");
    }
    
//    //Можем переписать метод если захотим, в данном случае комментируем его и используется метод Animal
//    @Override
//    public void voice(){
//        System.out.println("ББББ!!");
//        super.voice();
//    }
    
}
```


---

### 4. Композиция и Агрегация как вид связи

***Агрегация как связь (Aggregation)*** - это когда связь между объектами, один объект является частью другого объекта, но в тоже время основной объект может без него обойтись и функционировать

```java
import java.util.List;

//Объект Машина который имеет слабую связь с пользователями(список пользователей)
public class Car {
    User[] users;

    public void setUsers(User[] users) {
        this.users = users;
    }
}

//Пользователи
public class User {
    
}
```

***Композиция (Composition)*** - это жесткая связь между классами, когда объект не может функционировать без других объектов из которых он состоит, например машина и двигатель. Как правило внутри объекта мы создаем другой объект в конструкторе и используем его
```java
// Объект который будем использовать
public class Job {
    public void setSalary(long salary) {
        this.salary = salary;
    }
    public void setSalary(long salary) {
        this.salary = salary;
    }
}

// Объект который будет использовать Job для получения зп
public class Person {

    private Job job;

    public Person(){
        this.job=new Job();
        job.setSalary(1000L);
    }
    public long getSalary() {
        return job.getSalary();
    }
}

```

***Ассоцияция (Association)*** - это самая слабая связь, она говорит о том что объекты только знают друг о друге, например мать и ребенок. Также мы должны поддерживать вручную двухстороннюю связь
```java
class Child {
    Mother mother;
}

class Mother {
    List children;
}
```

---

### 6. Паттерны проектирования

Все паттерны можно посмотреть здесь
https://refactoring.guru/ru/design-patterns/builder/java/example#example-0

---
#### Фабрычный метод (Factory method) 

***Фабрычный метод(Factory method)*** - фабричный метод, это класс который вначале получает конфигурацию в виде
Строка -> класс
Название интерфейса -> класс

И с помощью метода getBean возвращает нужный объект
С помощью аннотации spring также может возвращать нужный объект

***Плюсы:***
- можно создавать несколько типов конструкторов с разными входными параметрами
- можно создать объект производного типа и при этом скрыть это внутри фабрики
- во время создания объекта можно производить какие-то действия, например регистрацию его в списке объектов
- можно сделать метод создания ассинхронным а через new не можем

***Пример простейшей фабрики***

```java

// Создаем интерфейс транспорт
interface Transport {
    void move();
}

// Первая реализация транспорта
class Car implements Transport{
    @Override
    public void move() {
        System.out.println("I'm going by car");
    }
}

// Вторая реализация транспорта
class Airplane implements Transport{
    @Override
    public void move() {
        System.out.println("I'm flying by plane");
    }
}

// Фабрика, которая возвращает необходимую реализацию, в этой фабрике можно поменять логику вывода объектов, обычно это делается с помощью загруженной в нее конфигурации или анализа кода с помощью аннотаций как в спринг @inject например @Autowired и @Qualifier("Airplane")..
class Factory{
    //Если мы запросили класс самолет то вернем его, если нет то во всех других случаях вернем объект машина
    public Transport getObject(String className){
        if(className.equals(Airplane.class.toString())){
            return new Airplane();
        }else{
            return new Car();
        }
    }
}

```

Более сложную фабрику разработанную мной можно посмотреть по ссылке
https://github.com/exesellentmen/Pat/tree/master/src/main/java/com/pavel/patterns/services/dependency/injection/container
















