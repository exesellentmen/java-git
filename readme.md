# Java Core
## Оглавление 

1. [Стримы](#Стримы)
2. [Многопоточность](#Многопоточность)


### 1. Стримы

### 2. Многопоточность
[:arrow_up:Оглавление](#Оглавление) 

**2.1 Что такое многопоточность(Multithreading)**<br />
**Многопоточность** - это процесс выполнения нескольких потоков подобна Ajax. Мы можем запсускать задачу на выполнение, параллельно с этим мы можем выводить результат на сайт, или притсупать к другим задачам либо мы можем разбивать задачу и запускать ее в несколько потоков. <br />
 <br /> 

**2.2 Что такое Поток(Thread)** <br />
**Поток(Thread)** - это легкий небольшой процесс вычесления, который может выполняться с другими потоками внутри процесса <br />

**Пример потока(Thread)** 
```java
Thread threadA = new Thread(() -> {
    System.out.println("Operation's completed");
});
threadA.start();
```

**2.3 Что такое Процесс(Process)** <br />
**Процесс(Process)** - это отдельный тяжеловесный процесс, который имеет свое пространство в памяти и передача данных между процессами долгая 

**2.4 Что такое Многопроцессорность(Multiprocessing)** <br />
**Многопроцессорность(Multiprocessing)** - это когда запускается выполнение нескольких процессов параллельно <br />
Подробнее: https://www.c-sharpcorner.com/article/a-complete-multithreading-tutorial-in-java/

**2.5 Что такое Планировщик(Scheduler)**<br />
**Планировщик(Scheduler)** - это система, которая выбирает поток, который будет выполняться в процессе. На его выбор может повлиять приоритет, который установлен, чем больше приоритет тем более важен, максимум 10 минимум 0 обычно 5 

**2.6 Что такое Многозадачность(Multitasking)** <br />
**Многозадачность(Multitasking)** - это когда необходимо выполнить несколько задач одновременно, можно выполнять с помощью многопоточности или многопроцессорности 

**2.7 Что такое Пул потоков(Thread Pool)** <br />
**Пул потоков(Thread Pool)** - это набор workers воркеров потоков, которые находятся в ожидании работы, которые могут выполняться много раз. Это дает возможность не затрачивать время на создание потоков, а использовать уже поток из ограниченного существующего набора потоков <br />

**Пример пул потоков(Thread Pool), будет выполняться по 5 потоков, остальные в очереди**
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

**2.8 Поточная синхронизация(Thread Synchronization)** <br />
**Поточная синхронизация(Thread Synchronization)** - блок, с помощью которого, можно ограничить доступ к ресурсу или к блоку кода только для 1-го потока(Thread) <br />

**Пример поточной синхронизации с помощью метода**
```java
synchronized void deposit(int amount){
    for (int i = 1; i <= amount; i++) {  
        System.out.println(i); 
    }  
}
```

**Пример поточной синхронизации с помощью блока**
```java
synchronized (this) {//synchronized block  
    for (int i = 1; i <= this.amount; i++) {  
        System.out.println(i); 
    }   
}  
```

**2.9 Межпотоковое общение Cooperation (interthread communication)** <br />
**Межпотоковое общение Cooperation (interthread communication)** - позволяет общаться между потоками. В данном случае поток приостанавливается и другой поток может войти в эту секцию <br />

**2.10 Semaphore(Симафор)** <br />
**Semaphore(Симафор)** - инструмент для ограничения доступа к ресурсом n-ым количеством потоков, т е одновременно с ресурсом может работать n-ое количество потоков, в то же время может использоваться как thread pool <br />

**Отличия от thread pool** - Thread pool ограничивает одновременное выполнение потоков(Threads) по типу воркеров, которые работают над очередью, а Semaphore может ограничить выполнение определенного участка кода потока, например выполнить скрипт фильтрации, и применение изменения(например добавление префикса) могут все потоки, а вот участок сохранения в базу данных ограничить только n-м количеством потоков.

**Параметры:** <br />
```java
new Semaphore(8); // Количество потоков которым разрешаем доступ 8
semaphore.acquire(); // Занимаем потоком участок кода
semaphore.release(); // Освобождаем потоком участок кода
```

**Пример:**
Задача получить список студентов и добавить им префикс, в этом случае создаем 40 потоков но выполняется только 8, например мы можем ограничить количество подключений к БД и так далее в участве
```java
semaphore.acquire();
//Здесь указываем код, доступ к выполнению которого ограничен Симафором
//...
semaphore.release();
```

**Пример:**

```java
public String multithreadingSemaphore(){
	//Устанавливаем параметры
	Semaphore semaphore = new Semaphore(8); // Создаем симафор с 8 потоками
    // Получаем список студентов
	ArrayList<Student> students = (ArrayList<Student>) this.studentRepository.findAll();

	for(int n = 0; n < 40; n++) {
		Thread thread = new Thread(){
        	public void run(){
        		try {
					semaphore.acquire(); // Участок в который могут заходить 8 потоков

					List<Student> studentsThread = students.stream()
                                .skip(1000 * n)
                                .limit(1000)
                                .peek(bIblockElement -> bIblockElement.setName(bIblockElement.getName() + " prefix1"))
                                .collect(Collectors.toList());
                    studentRepository.saveAll(studentsThread);

					semaphore.release(); // Освобождение ресурса потоком
				} catch (InterruptedException e) {
                        System.out.println("We already have very many threads for executing");
                }
			}
	    }
    }
}
```
<br />

**2.11 Мьютекса(MUTual EXclusion - взаимное исключение)** <br />
**Мьютекса(MUTual EXclusion - взаимное исключение)** - обеспечить такой механизм, чтобы доступ к объекту в определенное время был только у одного потока.<br /> 
Популярной аналогией мьютекса в реальной жизни можно считать «пример с туалетом».<br />
Замок на двери туалета — роль мьютекса, а очередь из людей снаружи — роль потоков <br />

**Примечания:**
* мьютекс есть у каждого объекта java<br />
* на самом деле мьютекс это одноместный Semaphore<br />

**Способы:**

Способ 1
```java
Synchronized(some object) // дает доступ к объекту
```
Способ 2
```java
synchronized return_type MethodName(parameters) {
  ...
}
```

**Пример:**
Synchronized<br />
Например у нас есть число и все потоки к нему получают доступ, например могут его менять, <br />
Поток 1 - получил доступ<br />
Поток 2 - получил доступ<br />
..
Но если мы добавим блок synchronized, тогда к числу может получить доступ только 1 поток<br />
Поток 1 - получил доступ<br />
Поток 1 - освободил доступ<br />
Поток 2 - получил доступ<br />
Поток 2 - освободил доступ<br />
..


```java
//Тестирование Mutex
@GetMapping("/multithreading-mutex")
public String multithreadingMutex(){
    Integer resource = 1;
    for (int i = 0; i < 8; i++){
        Thread thread = new Thread(){
            public void run(){
                System.out.println(Thread.currentThread().getName() + " начался");
                synchronized (resource){
                    System.out.println(Thread.currentThread().getName() + " получил доступ");
                    try {

                        System.out.println(Thread.currentThread().getName() + "  "+ resource);
                        Thread.sleep(100);

                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "освободил доступ");
                }
            }
        };
        thread.start();
    }
    return "test";
}

```

**ReentrantLock**
```java
public class SequenceGeneratorUsingReentrantLock extends SequenceGenerator {
    
    private ReentrantLock mutex = new ReentrantLock();

    @Override
    public int getNextSequence() {
        try {
            mutex.lock();
            return super.getNextSequence();
        } finally {
            mutex.unlock();
        }
    }
}
```

**2.12 Мониторы(Monitor)** <br />
**Мониторы(Monitor)** - необходим для мониторинга доступа к ресурсу только одним Thread, синхронизация базируется на Monitor.
![header](https://capsule-render.vercel.app/api?text=Hello%World!&fontColor=d6ace6)

**Примечания**
Каждый Thread неявно владеет одним монитором<br />
Применяется: с помощью synchronized<br />

**2.13 wait(), notify(), notifyAll()** <br />
**wait()** - когда у нас один поток вошел в synchronized и вызвал метод wait(), он останавливает работу с этим блоком и переходит в режим ожидания

**notify()** - до тех пор пока другой поток не вызовет метод notify()

**notifyAll()** - возобновляет работу всех потоков, у которых ранее был вызван метод wait()

**Пример:**
```java
// Класс Магазин, хранящий произведенные товары
class Store{
    private int product=0;
    public synchronized void get() {
        while (product<1) {
            try {
                wait();
            }
            catch (InterruptedException e) {
            }
        }
        product--;
        System.out.println("Покупатель купил 1 товар");
        System.out.println("Товаров на складе: " + product);
        notify();
    }
    public synchronized void put() {
        while (product>=3) {
            try {
                wait(); // Запускаем ожидание производителя и разрешаем есть потребителю
            }
            catch (InterruptedException e) {
            }
        }
        product++;
        System.out.println("Производитель добавил 1 товар");
        System.out.println("Товаров на складе: " + product);
        notify();
    }
}
// класс Производитель
class Producer implements Runnable{

    Store store;
    Producer(Store store){
        this.store=store;
    }
    public void run(){
        for (int i = 1; i < 6; i++) {
            store.put();
        }
    }
}
// Класс Потребитель
class Consumer implements Runnable{

    Store store;
    Consumer(Store store){
        this.store=store;
    }
    public void run(){
        for (int i = 1; i < 6; i++) {
            store.get();
        }
    }
}

// Тестируем wait(), notify(), notifyAll()
@GetMapping("/multithreading-wait-etc")
public String multithreadingWait(){

    Store store=new Store();
    Producer producer = new Producer(store);
    Consumer consumer = new Consumer(store);
    new Thread(producer).start();
    new Thread(consumer).start();

    return "test";
}
```




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
















