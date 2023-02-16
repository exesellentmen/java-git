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

**2.14 Гонка данных(Data Race)** <br />
**Состояния гонки(Data Race)** - случай когда с одним ресурсом работает несколько потоков и в результате они могут получить одно и тоже значение и одновременно выполнить операцию и мы получим например за 1000 иттераций i++ не i = 1000, а например i = 997

**Источники**<br />
https://javascopes.com/java-mutex-5903ae13/

**2.15 Состояния гонки (Condition Race)** <br />
**Состояния гонки(Condition Race)** - часто путают с Data Race, это проблема, которая возникает в результате когда время или порядок влияют на результат программы

**Источники**<br />
https://medium.com/german-gorelkin/race-8936927dba20

**Решение:**<br />
Иногда можно добиться с помощью синхронизации
```java
Пример:
Thread thA = new Thread(){
    public void run(){
        for(int i = 0; i < 10000; i++){
            System.out.println("Thread A");
        }
    }
};

Thread thB = new Thread(){
    public void run(){
        for(int i = 0; i < 10000; i++){
            System.out.println("Thread B");
        }
    }
};

thA.start();
thB.start();
```
**Результат:**<br />
Thread A<br />
Thread B<br />
Thread A<br />
Thread B<br />
Thread A<br />
Thread A<br />


**2.15 Lock Concept in Multithreading** <br />
**Lock Concept in Multithreading** - альтернатива блока synchronized

Классы блокировок реализуют интерфейс Lock

**Методы:**
lock() - блокирует доступ
unlock() - разблокирует доступ
boolean tryLock() - пытается получить блокировку, если не получается не ожидает ее получения как в lock
Condition newCondition() - возвращает объект condition, который соответствует блокировке

**ReentrantLock** - реализует интерфейс Lock

Для использования создаем ReentrantLock locker и передаем его всем потокам, если он блокирует, то только тот поток который заблочил блок может им пользоваться остальные ждут
```java
locker.lock();
//...
locker.unlock();
```

**ReadWriteLock** - это высокоуровневый инструмент блокировки потоков. Он позволяет различным потокам читать определенный ресурс, но позволяет только одному записывать его одновременно.

**Пример:**
```java
ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
readWriteLock.readLock().lock();
 //... блокировка только 1 может читать
readWriteLock.readLock().unlock();

readWriteLock.writeLock().lock();
 //... Только для чтения можно войти в этот блок
readWriteLock.writeLock().unlock();
```

**2.16 Thread Deadlock** <br />
**Thread Deadlock** - Взаимная блокировка - ситуция, которая часто возникает в многопоточности, когда у нас Поток-1 пытается получить доступ к (synchronized)блоку занимаемому Потоком-2, а Поток-2 пытается получить доступ к (synchronized)блоку занимаемый Потоком-1 в результате взаимная блокировка

**Источник:** https://javarush.com/groups/posts/1422-vzaimnaja-blokirovkadeadlock-v-java-i-metodih-borjhbih-s-ney


**Пример DeadLock:**
Например у нас есть задача мы переводим из аккаунта А деньги на счет аккаунта Б и в тоже время мы переводим деньги со счета Б на счет А в результате 1-ый поток блокирует счет А и начинает проводить операции а в то же время 2-ой поток блокирует счет, в результате взаимная блокировка.

```java
// Пример DeadLock, в результате не будет выполнен перевод, т к 2 потока взаимно заблокировали друг друга
@GetMapping("/multithreading-deadlock")
public String multithreadingDeadLock(){

    Account accauntA = new Account();
    accauntA.count = 10;
    Account accauntB = new Account();
    accauntB.count = 10;
    Bank bank = new Bank();

    Thread threadA = new Thread(){public void run(){
        try {
            bank.transferMoney(accauntA, accauntB, 1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }};
    threadA.start();
    Thread threadB = new Thread(){public void run(){
        try {
            bank.transferMoney(accauntB, accauntA, 1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }};
    threadB.start();

    return "Test";
}

class Account{
    public int count;
}

class Bank{
    public void transferMoney(Account fromAccount, Account toAccount, int amount) throws InterruptedException {
        synchronized (fromAccount) {
            Thread.sleep(100);
            synchronized (toAccount) {
                fromAccount.count = fromAccount.count - amount;
                toAccount.count = toAccount.count + amount;
                System.out.println("Operation's completed");
            }
        }
    }
}
```


**Решение DeadLock** (плохое т к hashcode может быть одинаковым, можно формировать порядок по id или вносить доп переменную):
```java
class SmartBank{
    public void transferMoney(Account fromAccount, Account toAccount, int amount) throws InterruptedException {
        Account firstAccount, secondAccount;
        if(fromAccount.hashCode() > toAccount.hashCode()){
            firstAccount = fromAccount;
            secondAccount = toAccount;
        }else {
            firstAccount = toAccount;
            secondAccount = fromAccount;
        }

        synchronized (firstAccount) {
            Thread.sleep(100);
            synchronized (secondAccount) {
                fromAccount.count = fromAccount.count - amount;
                toAccount.count = toAccount.count + amount;
                System.out.println("From Smart Bank Operation's completed");
            }
        }
    }
}
```

**Решение с помощью Lock**
```java
class SuperSmartBank{
    public void transferMoney(Account fromAccount, Account toAccount, int amount) throws InterruptedException {
        while(true) {
            if (fromAccount.lock.tryLock()) {
                Thread.sleep(100);
                if (toAccount.lock.tryLock()) {
                    try {
                        //do something
                        fromAccount.count = fromAccount.count - amount;
                        toAccount.count = toAccount.count + amount;
                        System.out.println(Thread.currentThread().getName()+" From Smart Bank Operation's completed");
                        break;
                    } finally {
                        toAccount.lock.unlock();
                        fromAccount.lock.unlock();
                    }
                } else {
                    fromAccount.lock.unlock();
                }
            }
            System.out.println("Attempt");
        }
    }
}
```

**Проблема состоит в том что из нелегко найти и нужно проводить хороший Code review**

Для решения можно сравнивать по id или хеш коду и давать доступ наименьшему или ввести дополнительный объект
Пусть у нас есть пара объектов реализующих интерфейс Lock и нам необходимо занять их мониторы так, чтоб избежать взаимной блокировки. Реализовать это можно так:

**Примечания DeadLock**
- JVM позволяет диагностировать взаимные блокировки отображая их в дампах потоков.
- Также может помочь lockInterruptibly интерфейса Lock он прирвет поток, который занимает монитор

https://cdn.javarush.com/images/comment/0085a60a-91b4-4219-9050-5fc9066fb3ec/original.jpg

недетерминированности

**2.17 Состояния потока(states of thread)** <br />
**Состояния потока(states of thread):** <br />
- new - при создании нового thread но до его запуска с помощью start() <br />
- runnable - после запуска start но планировщик(scheduler) еще не выбрал его для выполнения <br />
- running - когда thread начал выполняться <br />
- Non-Runnable (Blocked) - если thread заблокирован для выполнения <br />
- Terminated - завершение, когда thread завершил свое выполнение <br />

**Способы создания потока:** <br />
1 Создать класс расширенный от класа Thread и реализовать метод run() <br />
2 Создать класс реализующий интерфейс Runnable <br />


**Методы для Thread:**
```java
Thread.sleep(1000); // ожидание 1 секунда, если thread спит планировщик берет другой поток и работает с ним 
t1.join();  // Для ожидания завершения потока, пока поток не выполнится следующие не запустятся
Thread t = Thread.currentThread();  // Получаем текущий thread внутри метода
Thread.getName(); // возвращает имя thread
Thread.setName(String name); // Устанавливает имя для thread
Thread.currentThread().getPriority(); // получить приоритетность thread
s.setPriority(8);  // Установить приоритетность thread

try {  
    t1.join();  
} catch (Exception e) {  
    System.out.println(e);  
}  
```

**Примечания:**
- Thread не может быть запущен 2 раза, после первого запуска второй раз будет выводить исключение
- Если запустить поток через run а не через start тогда наша программа будет выполняться не в многопоточном режиме а последовательно


### 3 Примеры многопоточности:

**3.1 Создать класс расширенный от класа Thread и реализовать метод run()** <br />

```java
public class ThreadConstructor extends Thread {  
    public void run() {  
        System.out.println("This is example of Thread Constructor");  
    }  
  
    public static void main(String args[]) {  
        ThreadConstructor t = new ThreadConstructor();  
        t.start();  
    }  
} 
```


**3.2 Создать класс реализующий интерфейс Runnable** <br />

```java
public class ThreadConstructor2 implements Runnable {  
  
    public void run() {  
        System.out.println("This is the example of Thread(Runnable target) constructor");  
    }  
  
    public static void main(String args[]) {  
        ThreadConstructor2 s = new ThreadConstructor2();  
        Thread t = new Thread(s);  
        t.start();  
    }  
} 
```

**3.3 Как запустить поток в анонимном классе(Anonymous class) через Thread** <br />

Более длинный вариант
```java
Thread t2 = new Thread() {  
    public void run() {  
        System.out.println("Task two is start now...");  
    }  
};  

t2.start(); 
```
Более короткий вариант
```java
new Thread() {  
    public void run() {  
        c.withdraw(15000);  
    }  
}.start();
```

**3.4 Как запустить поток в анонимном классе(Anonymous class) через Runnable** <br />
т е мы не создаем отдельный класс для выполнения кода

```java
Runnable r1 = new Runnable() {  
    public void run() {  
        System.out.println("Task one is start now...");  
    }  
};  
Thread t1 = new Thread(r1);  
t1.start();  
```

**3.5 Создание пула threads** <br />

```java
import java.util.concurrent.ExecutorService;  
import java.util.concurrent.Executors;  
  
public class ThreadPoolTest {  
    public static void main(String[] args) {  
        ExecutorService e = Executors.newFixedThreadPool(3);//creating a pool of 3 threads  
        for (int i = 0; i <= 6; i++) {  
            Runnable t = new ThreadPoolExample("" + i);  
            e.execute(t);  
        }  
        e.shutdown();  
        while (!e.isTerminated()) {  
        }  
        System.out.println("All threads are finish");  
    }  
} 
```

**3.6 Создание группы потоков в одном классе** <br />
Описание

```java
public class ThreadGroupExample implements Runnable {  
    public void run() {  
        System.out.println(Thread.currentThread().getName());  
    }  
  
    public static void main(String[] args) {  
        ThreadGroupExample obj = new ThreadGroupExample();  
        ThreadGroup tg = new ThreadGroup("Main ThreadGroup");  
        Thread t1 = new Thread(tg, obj, "thread1");  
        t1.start();  
        Thread t2 = new Thread(tg, obj, "thread2");  
        t2.start();  
        Thread t3 = new Thread(tg, obj, "thread3");  
        t3.start();  
        System.out.println("Name of ThreadGroup : " + tg.getName());  
        tg.list();  
    }  
}  
```

**3.7 Многопоточность через аннотации Async для методов** <br />

```java
// Класс AsyncTester.java:
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.Async;
import org.springframework.scheduling.annotation.EnableAsync;

@Configuration
@EnableAsync
public class AsyncTester {
    @Async
    public void saveTest(int i) throws InterruptedException {
        Thread.sleep(1000 - i*100);
        System.out.println(i);
    }
}

// Пример использования:
@Autowired
ApplicationContext context;

// Тестирую асинхронность и многопоточность через аннотацию @Async
@GetMapping("/async")
public String mathodeAsync() throws InterruptedException {
    AsyncTester asyncTester = this.context.getBean(AsyncTester.class);
    for (int i = 1; i < 10; i++){
            asyncTester.saveTest(i);
    }
    return "test";
}
```
**Результат в обратном порядке:**
10<br />
9<br />
8<br />
7<br />
6<br />
5<br />
4<br />
3<br />
2<br />
1<br />



 

**3.8 Работа с Thread через лямбду** <br />

```java
Thread threadA = new Thread(() -> {
    // Some code....
});
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
















