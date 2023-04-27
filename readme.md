# Java Core
## Оглавление 

1. [Стримы](#Стримы)
2. [Многопоточность](#Многопоточность)


### 1. Стримы

**1.1 Что такое stream api**

**stream** - созданы для того чтобы удобно работать с данными, сортировать, фильтровать, ограничивать, преобразовывать и так далее

**Существуют следующие методы Stream:**

**Промежуточные операции** - Возвращают тип Stream<br>

**1.2 filter - метод для фильтрации данных**<br>
Пример:
Получить элементы id которых меньше 5
```java
arrayList.stream().filter(el -> el.id < 5);
```

**1.3 map - медот для преобразование одного массива в другой массив**<br>
Получить все id элементов
```java
arrayList.stream().map(el -> el.id);
```

**1.4 flatmap - расширенный метод map для получение всех элементов из вложенных списков**<br>
Получить все папки которым принадлежат элементы (el - товар, folders - список разделов)
```java
arrayList.stream().flatMap(el -> el.folders.stream())
```

**1.5 sorted - для сортировки элементов**<br>
Получить отсортированный список объектов по имени
```java
arrayList.stream().sorted((o1, o2)-> o1.name.compareTo(o2.name));
```

**1.6 skip - пропустить количетво элементов сначала, т е обрезать список**<br>
Получить список начиная с 4го элемента
```java
arrayList.stream().skip(3)
```

**1.7 limit - ограничить список до определенного количества**<br>
Получить список только из 3х элементов
```java
arrayList.stream().limit(3)
```

**1.8 peek - перебрать элементы, которые сейчас в stream, для отладки и так далее**<br>
Вывести именя элементов вернет stream
```java
arrayList.stream().peek(el -> System.out.println(el.name))
```

**1.9 distinct - получить уникальные элементы, убирает дубли**<br>
Получить уникальные элементы
```java
arrayList.stream().distinct()
```

**Терминальные:** - конечные методы, не возвращают тип Stream

**1.10 collect - преобразует стрим в выбранный тип**<br>
Получить из stream тип List
```java
arrayList.stream().collect(Collectors.toList());
```

**1.11 forEach - перебрать элементы, не возвращает результат, можно использовать для вывода сообщений или для суммы и т д**<br>
Получить сумму всех id элемента el
```java
int[] cc = new int[1];
arrayList.stream().forEach(el -> cc[0] += el.id);
```

**1.12 count - подсчет количество элементов**<br>

**1.13 min - минимальный элемент**<br>
```java
Optional<Product> product = arrayList.stream().min((e1, e2) -> e1.name.compareTo(e2.name));
```

**1.15 findFirst() - получить первый элемент для поиска**<br>

**1.16 Проверка соответствия всех элементов значению**
```java
.allMatch(e -> e > 0) - проверить все ли элементы соответствуют условиям
.noneMatch(e -> e < 0) - все элементы не соответствуют заданным условиям
.anyMatch(el -> el.id == 3) - есть ли хоть 1 элемент, который соответствует условиям
```

**1.17 Получение результата из всех элементов**<br>
reduce - получить результат, например сумму всех элементов или произведение
```java
Stream.of(1,2,3,4,5,6).reduce((x,y)->x*y)
// ИЛИ
Stream.of(1,2,3,4,5,6).reduce(0, (x,y)->x*y)
```

**1.18 Работа с Iterable, применение действия ко всем элементам**
```java
StreamSupport.stream(bIblockElements.spliterator(),false)
    .forEach(bIblockElement -> bIblockElement.setName(bIblockElement.getName() + " prefix1"));
```









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




### 4. Все методы объекта Object

**Все методы:**
- toString() method - получение строкового представления объекта
- hashCode() method - получение hashCode схож с уникальным идентификатором
- equals(Object obj) method - сверение по значению объекта, необходимо переопределять
- finalize() method - используется редко, выполняется автоматически при освобождении Object
- getClass() method - получение объекта Class, который содержит большой функционал
- clone() method - клонирование объекта, нужно переопределять и реализовывать интерфейс
- wait(), notify() notifyAll() methods - методы для работы с многопоточностью


**Описание:**


**4.1 toString() method** - метод объекта Object, который возвращает строковое представление объекта по умолчанию это: <br />
Полный адрек класса + @ + беззнаковое шаснадцетиричное представление hashCode<br />
com.paul.startclass.TestObject@21d6dc81

Алгоритм создания строки
```java
getClass().getName() + "@" + Integer.toHexString(hashCode());
```
т е toString вызывает хеш код по умолчанию

<br />

**4.2 hashCode() method** - метод объекта Object, Возвращает хеш код объекта, как правило для того чтобы можно было быстро сравнивать объекты, обычно это сумма всех свойств переведенные в формат int умноженное на нечетное число 31 или 29 в промежуточных операциях.<br />

**По умолчанию возвращает:** - идентификационный хеш (identity hash code), скрипт получения которого является nativ, нативным, написан на другом языке. для его получения в случае переопределения используется 

```java
System.identityHashCode(o)
```

Пример генерации hashCode:
```java
@Override
public int hashCode() {
    int result = model == null ? 0 : model.hashCode();
    result = 31 * result + this.maxSpeed;
    result = 31 * result + this.size;
    return result;
}
```

<br />

**4.3 equals()** - метод объекта Object,сравнение по состоянию объектов (т е соответствуют ли свойства 1-го объекта другому), тогда как оператор (equel singns) == проверяет ссылаются ли переменные на один и тот же объект. <br />

**Источник:**
https://javarush.com/groups/posts/equals-java-sravnenie-strok <br />
https://javarush.com/groups/posts/2179-metodih-equals--hashcode-praktika-ispoljhzovanija <br />


**Требования equals**
- Рефлексивность. Если один и тот же объект, то получаем true
- Симметричность. Объекты должны быть равны друг другу симметрично.  Как 
a.equals(b) = true => b.equals(a) = true
- Транзитивность. Если 2 объекта равны 3-му то и между сабой они тоже должны быть равны
(a.equals(c) = true, b.equals(c) = true) => a.equals(b) = true
- Постоянность. Результат должен зависить только от входящих аргументов, а не от сторонних значений
- Неравенство с null. Любой объект не должен быть равен null

**Примечания**
- equals метод класса Object и изначально его механизм такой же как и оператора (equals signs) ==, но для работы его нужно переопределить в созданном классе

**Пример написания метода:**
```java
@Override
public boolean equals(Object o){
    if (this == o) return true; // Проверяем, может это один и тот же объект
    if (o == null || getClass() != o.getClass()) return false; // проверяем соответствует ли класс и то что объект не равен null
    Car that = (Car) o;
    if (this.maxSpeed != that.maxSpeed) return false;
    return (this.model.equals(that.model)) ;
}
```

**Пример использования кода:**
```java
class Car{
    public String model;
    public int maxSpeed;
    @Override
    public boolean equals(Object o){
        if (this == o) return true; // Проверяем, может это один и тот же объект
        if (o == null || getClass() != o.getClass()) return false; // проверяем соответствует ли класс и то что объект не равен null
        Car that = (Car) o;
        if (this.maxSpeed != that.maxSpeed) return false;
        return (this.model.equals(that.model)) ;
    }
}

@SpringBootApplication
public class EqualsApplication {
    public static void main(String[] args){
        Car car1 = new Car();
        car1.model = "Ferrari";
        car1.maxSpeed = 300;

        Car car2 = new Car();
        car2.model = "Ferrari";
        car2.maxSpeed = 300;

        System.out.println(car1 == car2); // Результат false
        System.out.println(car1.equals(car2)); // Результат equals true
    }
}
```

<br />

**4.4 finalize() method** - метод объекта Object, данный метод используется очень редко т к у него есть много ньансов, а вызывается он когда объект освобождается и становится доступным для утилизации с помощью garbage collector<br />

**Примечания**
- Данный метод не обязательно будет вызван, например в случае выключения программы
- Его можно вызывать для высвобождения служебных систем, например закрыть подключение к БД внутри объекта
- Еще один минус в том что если метод реализован, то объект не утилизируется сразу а встает в очередь на утилизацию, где занимает ресурсы
- Лучше использовать другие методы для обработки высвобождения объекта
- Исключения, которые будут вызваны этим методом не будут обработаны

<br />

**4.5 getClass() method** - метод объекта Object, возвращает экземляр класса Class, который содержит информацию о классе на основе которого был создан объект из которого был вызван метод getClass().
Необходим в случае разработки библиотек или например контейнеров зависимостей<br />


**class Class** - это класс, который позволяет возвращать различные данные о классе, такие как:
- имя класса
```java
classTest.getSimpleName();
```
- полное имя класса
```java
classTest.getName();
```
- набор методов
```java
classTest.getMethods();
try {
    Method setName = classTest.getMethod("setName", String.class);
} catch (NoSuchMethodException e) {
    e.printStackTrace();
}
```
- набор свойств
- интерфейсы, которые он реализует
- супер классы (родительские классы)


**class Field** - это класс, который позволяет работать со свойством класса и с объектами, которые принадлежат данному классу. Позволяет проводить такие действия как:
- получение Fields из объекта
```java
Field fields = testObject.getClass().getDeclaredFields();
```
- получать или устанавливать значения для свойств объекта
```java
String name = (String) fields[0].get(testObject);
String name = (String) fields[0].set(testObject, "test value");
```
- узнавать доступно ли свойство для редактирования
```java
boolean accessField = fields[1].canAccess(testObject);
```
- изменять доступность свойств, например с private на public и обратно
```java
fields[1].setAccessible(true);
```
- устанавливать значения в случаи примитивных значений свойства
```java
fields[0].setInt(testObject,3);
```

<br />

**4.6 clone()** - метод объекта Object, создание клона объекта<br />

**Источник:**
https://www.techiedelight.com/ru/clone-method-in-java/

**Примечание:**
- В Java мы присваеваем не объект, а ссылку на него, чтобы создать копию необходимо пользоваться механизмом clone()
- Все объекты, которые мы можем клонировать должны реализовывать Cloneable интерфейс и метод clone() должен возвращать объект типа Object
- При клонировании объекты также должны внутри вызывать метод super.clone(), чтобы клонировать состояние родителя тоже
- Есть мелкое и глубокое копирование (Deep and Shallow Copy)
- Если мы имеем поле с непримитивным типом, мы должны его создать по умолчанию
Test c = new Test();

Синтаксис
```java
protected Object clone() throws CloneNotSupportedException // метод clone
```
**Shallow copy(Поверхностного копирования)** - выводит только копию объекта но не копирует ссылочные типы внутри объекта. т е Если у нас есть свойство типом объект, то объект будет тот же мы скопируем только ссылку на него

Пример Shallow copy(Поверхностного копирования):
```java
@SpringBootApplication
public class CloneApplication {
    public static void main(String[] args) throws CloneNotSupportedException {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(StartclassApplication.class, args);

        Test2 t1 = new Test2();
        t1.a = 10;
        t1.b = 20;
        t1.c.x = 30;
        t1.c.y = 40;

        Test2 t2 = (Test2)t1.clone();

        t2.c.x = 999;

        System.out.println("Test");

    }
}

class Test {
    int x, y;
}

class Test2 implements Cloneable {
    int a;
    int b;
    Test c = new Test();
    public Object clone() throws CloneNotSupportedException
    {
        return super.clone();
    }
}
```

**Пример Deep copy(Поверхностного копирования)** - намного сложнее Shallow Copy потому что необходимо копировать все вложенные объекты всех уровней.
Глубокое клонирование включает рекурсивное копирование всех изменяемых типов.
Иногда для этих задач используют сериализацию и специальные библиотеки Apache Commons Ланг

**Пример Deep Copy(Глубокого копирования):**
```java
@SpringBootApplication
public class DeepCloneApplication {
    public static void main(String[] args) throws CloneNotSupportedException {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(StartclassApplication.class, args);

        TestDeep2 t1 = new TestDeep2();
        t1.a = 10;
        t1.b = 20;
        t1.c.x = 30;
        t1.c.y = 40;

        TestDeep2 t3 = (TestDeep2)t1.clone();
        t3.a = 100;

        t3.c.x = 300;

        System.out.println("Test");

    }
}

class TestDeep {
    int x, y;
}

class TestDeep2 implements Cloneable {
    int a, b;

    TestDeep c = new TestDeep();

    public Object clone() throws CloneNotSupportedException
    {
        TestDeep2 t = (TestDeep2)super.clone();

        t.c = new TestDeep();
        t.c.x = c.x;
        t.c.y = c.y;

        return t;
    }
}
```

<br />

**4.7 wait(), notify() notifyAll() methods** - методы объекта Object, все эти методы вызываются только из синхронизированного контекста - синхронизированного блока или метода.<br />


Данные методы нужны когда мы синхронизируемся с определенным объектом 
objectTest.wait() - Останавливает текущий поток в блоке synchronized (objectTest), и передает инициативу другим, пока не будет вызван objectTest.notify(); или objectTest.notifyAll();

```java
package com.paul.startclass;

public class ThreadApplication {
    public static void main(String[] args) {
        String test = "111";
        Thread th = new Thread(() -> {
            ThreadApplication.testMethode(test);
        });
        th.start();

        Thread th1 = new Thread(() -> {
            ThreadApplication.testMethode(test);
        });
        th1.start();
    }

    public static void testMethode(String testStr){
        synchronized (testStr){
            for(int i = 0; i<10; i++){
                if(i == 5) {
                    testStr.notify();
                    try {
                        testStr.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                System.out.println(Thread.currentThread().getName() + " "+ i);
            }
        }
    }
}
```



### 5. Колекции и Map

**5.1 java.util** - коллекции и структуры данных

**5.2 Источники:**
- https://it.wikireading.ru/32696
- https://www.bestprog.net/ru/2022/03/23/java-introduction-to-java-collections-framework-ru/#q03
- https://java-online.ru/java-util.xhtml

**5.3 Картинка:**
https://storage.yandexcloud.net/wr4img/376517_15_14-1.png


**5.4 Типы структур:**
https://i.pinimg.com/originals/c8/c4/66/c8c466b3cfb59a2a209e59c2972db3c0.png
https://dz2cdn1.dzone.com/storage/temp/13795578-java-collection-framework-hierarchy.jpg
https://book.huihoo.com/oreilly/java/fclass/figs/jfc_1701.gif
<br />

**5.5 Все классы и интерфейсы.**

Iterable<T> - интерфейс <br />
    Iterator<T> - интерфейс <br />
        ListIterator<T> - интерфейс <br />
    Collection<T> - интерфейс <br />
        List<T> - интерфейс <br />
            ArrayList<T> - класс <br />
            LinkedList<T> - класс <br />
            Vector(sync) - класс <br />
                Stack - класс <br />
        Set<T> - интерфейс <br />
            HashSet - класс <br />
            LinkedHashSet - класс <br />
            SortedSet<T> - интерфейс <br />
                TreeSet(SortedSet) - класс <br />
                NavigableSet<T> - интерфейс <br />
        Queue<T> - интерфейс <br />
            PriorityQueue - класс <br />
            Deque<T> - интерфейс <br />
                ArrayDeque(Deque) - класс <br />
                LinkedList(Deque) - класс <br />
 <br />
Map<K,V> - интерфейс <br />
    SortedMap<K,V> - интерфейс <br />
        NavigableMap<K,V> - интерфейс <br />
            HashMap - класс <br />
            HashLinkedMap - класс <br />
            HashTable(sync) - класс <br />
            TreeMap(SortedMap) - класс <br />

Краткое описание
https://www.youtube.com/watch?v=0sWpjUvbGcQ
<br />


**5.6 Iterable<T>** - интерфейс предоставляет методы для обхода элементов коллекций(bypass collection elements) с помощью forEach, for, iterator

**Методы:**<br />
- обход массивов(iterator.next()), <br />
- проверка есть ли следующий элемент(iterator.hasNext()), <br />
- более лаконичный обход массивов(iterator.forEachRemaining((element) -> {System.out.println(element);}))<br />
- удаление текущего элемента(iterator.remove())<br />

**Пример:**<br />
```java
//Interface Iterator - необходим для обхода массива, получение следующего элемента, или определение есть ли следующий
//Interface Iterator - requiered for collection traversing, getting a next element, checking if there is a next element, deleting a curent element
@Test
void iteratorInterface() {
    ArrayList<String> arrayList = new ArrayList<>();
    arrayList.add("1");
    arrayList.add("2");
    arrayList.add("3");

    Iterator<String> iterator = arrayList.iterator();

    //Проверяем есть ли следующий элемент(Checking, if there is the next element )
    boolean hasNext = iterator.hasNext(); // true

    //Мы получаем следующий элемент(Getting the next element)
    String str = iterator.next(); // 1

    iterator.next();

    // Удаляет текущий элемент, в данный момент 2ой, также и в самой коллекции ArrayList(Deleting the current element)
    iterator.remove(); // iterator = {"1", "3"}

    // Обход элементов массива более лаконичным способом(more convenient array traversing)
    arrayList.iterator().forEachRemaining((element) -> {
        System.out.println(element);
    });
}
```
<br />

**5.7 Collection<T>** - базовый интерфейс для всех коллекций и других интерфейсов коллекций, который включает такие методы как добавить(add(), addALL()), удалить(remove(), removeAll, removeIf(), retainAll()), проверить на наличие элементов(containsAll(),contains())

**Методы:**<br />
add()<br /> 
addALL()<br /> 
remove()<br />
removeAll<br /> 
removeIf()<br /> 
retainAll()<br /> 
containsAll()<br /> 
contains()<br /> 


**Пример:**<br />
```java
@Test
void collectionInterface(){
    // Создаем коллекцию из значений
    Collection<String> collection = new ArrayList<>(Arrays.asList("1","2","3"));

    // Добавление элемента(Adding an element)
    collection.add("4");

    // Добавление нескольких элементов(Adding multiple elements)
    collection.addAll(List.of("5", "6", "7"));

    // Получение количество элементов(Getting the numbers of elements)
    Collection<String> collection1 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    int size = collection1.size(); // 5

    // Определяем есть ли данный элемент(determining whether there is a given element)
    Collection<String> collection2 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    boolean containsElement = collection2.contains("5"); // true
    boolean containsElementfalse = collection2.contains("10"); // false

    //Определяем есть ли несколько элементов в коллекции
    Collection<String> collection3 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    boolean containsAll = collection3.containsAll(List.of("1", "3", "4"));
    boolean containsAllfalse = collection3.containsAll(List.of("1", "3", "41")); // false

    //Проверяет наличие элементов(checking whether there the elements)
    Collection<String> collection4 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    boolean empty = collection4.isEmpty(); // true

    //Удаление элемента из коллекции (deleting an element from collection)
    Collection<String> collection5 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    collection5.remove("2"); // collection {"1","3","4","5"}

    //Удаление нескольких элементов из коллекции(Deleting multiple elements from a collection)
    Collection<String> collection6 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    collection6.removeAll(List.of("1","4")); // collection {"2","3","5"}

    // Удаление элементов соответствующих условиям(Deleting the elements that match the conditions )
    Collection<String> collection7 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    collection7.removeIf(elem -> elem.equals("3") || elem.equals("4"));  // collection {"1","2","5"}

    // Удаление элементов не совпадающие(Deleting the elements that dont mach the conditions)
    Collection<String> collection8 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    collection8.retainAll(List.of("1","2","3")); // collection {"1","2","3"}

    // Удаление всех элементов (Deleting all elements)
    Collection<String> collection9 = new ArrayList<>(Arrays.asList("1","2","3","4","5"));
    collection9.clear(); // collection {}
}
```
<br />

**5.8 List<T>** - интерфейс, который наследует все методы collection

**Методы:**<br />
- set(index, value)<br /> 
- addAll(index, collection) - добавление элементов с заданой коллекции<br /> 
- remove(index) - удаление элемента по индексу<br /> 
- indexOf(value) - поиск индекса элемента по его значению<br /> 
- lastIndexOf(value) - поиск последнего вхождения по значению<br /> 
- listIterator() - получение ListIterator, аналог Iterator, но позволяет работать с дополнительными методами <br /> 
- replaceAll(e -> e.toLowerCase()) - позволяет приминить действия ко всем элементам List<br /> 
- sort((e,w)-> e.compareTo(w)) - позволяет отсортировать все элементы по условию<br /> 
- subList(2, 4) - получить под массив с индекса 2 до индекса 4(2 элемента)<br /> 
- spliterator() - получение spliterator, который работает параллельно как итератор<br /> 

**Пример:**<br />
```java
//Interface Iterator - необходим для обхода массива, получение следующего элемента, или определение есть ли следующий
//Interface Iterator - requiered for collection traversing, getting a next element, checking if there is a next element, deleting a curent element
@Test
void iteratorInterface() {
    ArrayList<String> arrayList = new ArrayList<>();
    arrayList.add("1");
    arrayList.add("2");
    arrayList.add("3");

    Iterator<String> iterator = arrayList.iterator();

    //Проверяем есть ли следующий элемент(Checking, if there is the next element )
    boolean hasNext = iterator.hasNext(); // true

    //Мы получаем следующий элемент(Getting the next element)
    String str = iterator.next(); // 1

    iterator.next();

    // Удаляет текущий элемент, в данный момент 2ой, также и в самой коллекции ArrayList(Deleting the current element)
    iterator.remove(); // iterator = {"1", "3"}

    // Обход элементов массива более лаконичным способом(more convenient array traversing)
    arrayList.iterator().forEachRemaining((element) -> {
        System.out.println(element);
    });
}
```
<br />

**5.9 Time complexity - Временая сложность** - временая сложность, показывает количество шагов для выполнения задачи

**Термины:**<br />
n - number of element - номер элемента<br />
O(n) - линейное время, чем больше размер ввода, тем больше потребуется времени для выполнения. (Пример: Поиск наименьшего или наибольшего элемента в неотсортированном массиве)<br />
O(1) - константное время<br />
O(log2(n))- логарифмическое время(Пример: Двоичный поиск в отсортированном массиве)<br />
O(n^2)- квадратичное время(Пример: Сортировка пузырьком, сортировка вставками)<br />
O(n * log2(n)) - линейно-логарифмическое время(Самая быстрая сортировка сравнением)<br />
log a под основанием b - это значит что в какую степень нужно возвести b чтобы получить a

Например двоичный поиск при 32, время выполнения
log2(32) = 5
Итого 5 шагов

Онлайн калькулятор:
https://umath.ru/calc/vychislenie-logarifma-chisla-onlajn/
<br />

**5.10 ArrayList<T>** - класс, который реализует динамический массив

**Источники:**<br />
https://docs.oracle.com/javase/6/docs/api/java/util/ArrayList.html<br />
https://www.baeldung.com/java-collections-complexity<br />

**Заметки:**<br />
- Для оптимизации, если известно что массив будет содержать большое количество элементов, лучше сразу указать размер, так как при каждом добавлении будет создаваться новый элементв в массиве что замедляет работу<br />
```java
ArrayList<String> list2 = new ArrayList<>(10000);
```
- Сложность при удалении элемента, но можно решить:<br />
Решение для ArrayList:<br />
Записывал вместо удаления null, а в конце просто убрал бы все null элементы или при итерировании игнорировал бы null значения.<br />

**Временая сложность ArrayList (Time complexity for ArrayList):**<br />
O(1) - Постоянное количество времени (Constant Time):<br />
size() - размен массива<br />
isEmpty() - пустой массив<br />
get(i) - получить элемент<br />
set(i, x) - установить значение для элемента<br />
iterator() - получить итератор<br />
listIterator() - получить продвинутый лист Итератор

O(n) - Линейное время (Linear time):<br />
indexof(x) - поиск элемента по значению, необходимо проверять весь массив<br />
clear() - очистить, необходимо удалять каждый элемент<br />
remove(x) - необходимо найти элемент, а потом уменьшить размер, обойдя все элементы<br />
remove(i) - необходимо удалить, а потом перебрать все элементы для уменьшения размера<br />
<br />


**5.11 LinkedList<T>** - это реализация двусвязного списка / очереди<br />
состоит на основе ссылок, каждый элемент содержит значение и ссылку на следующий элемент<br />
Редкие ситуации когда можно использовать.<br />
В основном когда нужно удалять элементы из середины списка, т к в ArrayList приходиьтся смещать все элементы<br />

**Решение для ArrayList:**<br />
Записывал вместо удаления null, а в конце просто убрал бы все null элементы или при итерировании игнорировал бы null значения.
Еще выгодно когда можно использовать Iterator

**Временая сложность LinkedList (Time complexity for LinkedList):**<br />
O(1) - Постоянное количество времени (Constant Time):<br />
add(e) - добавление элемента в конец списка<br />
remove(i) - удаление<br />
O(n) - Линейное время (Linear time):<br />
get(i) - получение элемента, в среднем = n/4, нужно обойти все элементы<br />
add(i, e) - добавление элемента в середину, в среднем = n/4<br />
remove(e) - удаление, нужно обойти все элементы, в среднем = n/4<br />
remove(i) - удаление, нужно обойти все элементы, в среднем = n/4<br />
contains(e) -нужно обойти все элементы<br />
indexof(x) - поиск элемента по значению, необходимо проверять весь массив<br />

**Пример работы с LinkedList:**
```java
// Список
@Test
void linkedListTest() {
    LinkedList<String> linkedList = new LinkedList<>(Arrays.asList("1", "2", "3", "4", "5", "3"));

    // Итератор в обратном порядке
    Iterator<String> iterator = linkedList.descendingIterator(); //Iterator {"3","5","4"..}

    linkedList.addFirst("11"); // Добавление в начало, выгода в том что добавляет только 1 ссылку
    linkedList.addLast("22"); //Добавляет в конец без ссылки

    String element = linkedList.element(); // Получение текущего элемента

    // Получение первого и последнего элемента
    String first = linkedList.getFirst();
    String last = linkedList.getLast();

    linkedList.offer("1"); // добавление элемента в конец при этом возвращает bool
    linkedList.offerLast("2"); // добавление элемента в конец списка при этом возвращает bool
    linkedList.offerFirst("22"); // добавление элемента в начало списка при этом возвращает bool
}
```
<br />



**5.12 Сравнение LinkedList и ArrayList** - интерфейс предоставляет методы для обхода элементов коллекций(bypass collection elements) с помощью forEach, for, iterator


**Заключение: более выгодно использовать ArrayList:**<br />
- добавление в середину списка время в обоих случаях O(n), к тому же метод добавления у ArrayList nativ, который компенсирует скорость чтения у LinkedList<br />
- удаление из середины быстрее у LinkedList, но можно ускорить ArrayList, если не удалять, а присваивать Null, а потом их игнорировать, или удалять все null в конце цикла например<br />

**Описание и источники:**<br />
https://2.bp.blogspot.com/-dFutRoKLpfw/V5Pf3VMjSZI/AAAAAAAAmQ4/HEHCWd9vtzICTy0IDSSMKw1kAKCs0yt2gCLcB/s1600/Image%2B687.png
https://rukovodstvo.net/posts/id_776/<br />


**LinkedList выгодно использовать:**<br />
- для удаления элемента из середины<br />
- для вставки элемента в середину(Хотя по некоторым исслодованием можно увидеть обратное т к операция nativ для ArrayList и она происходит быстрее)
https://habr.com/ru/post/262943/<br />

**В остальных случаях ArrayList**

https://rukovodstvo.net/posts/id_776/

**Заключение**<br />
ArrayList и LinkedList - две разные реализации интерфейса List У них есть свои различия, которые важно понимать, чтобы правильно их использовать.<br />

Какую реализацию следует использовать, зависит от конкретных вариантов использования. Если элементы будут извлекаться часто, не имеет смысла использовать LinkedList поскольку выборка выполняется медленнее по сравнению с ArrayList . С другой стороны, если требуются вставки с постоянным временем или если общий размер заранее неизвестен, предпочтительнее использовать LinkedList<br />
<br />




**5.13 Stack(стек)** - структура данных подобна магазину в пистолете, первый зашел последним выйдет.

**Места применения:**
- например для кнопки назад в браузере<br />
- может потребоваться при написании алгоритмов для задачик<br />
- например при создании карточной игры<br />

**Пример:**
```java
// Стек
@Test
void stackTest() {
    Stack<String> stack = new Stack<>();
    stack.addAll(Arrays.asList("1", "2", "3", "4", "5", "3"));

    stack.push("1"); // Добавить элемент вверх стека

    String pop = stack.pop(); // получить верхний элемент стека и удалить его из стека

    String peek = stack.peek(); //Вернуть верхний элемент стека но не удалять его
}
```


**5.14 Vector(sync)** - класс, структура данных, хранит данные линейно, как и массивы, которая позволяет эффективно изменять размер структуры данных во время выполнения программы. Похож на ArrayList, но имеет несколько отличий

**Отличия Vector перед ArrayList:**<br />
- Vector - синхронизируется, т е в один момент времени к Vector имеет доступ только один поток, что позволяет не поврежать данные<br />
- Увеличение емкости, Vector удваивает свой размер, ArrayList увеличивает в полтора раза<br />
- Vector имеет Enumeration в отличие от ArrayList(Преимущества Enumeration: синхронизированы, элементы в колекции защищены, не удаляются, не заменяются и не добавляются, )<br />
- Vectore имеет устаревшие функции, которые есть только в Vectore<br />

**Пример работы:**
```java
// Vector
@Test
void vectorTest(){
    Vector<String> vector = new Vector<>(List.of("1","2","3","4","5"));

    // Размер Vector
    int capacity = vector.capacity();

    // Удаляет лишнюю емкость, сохраняет место только для текущих элементов
    vector.trimToSize();

    // Получение Enumeration
    Enumeration<String> elements = vector.elements();

    // Увеличение емкости на 22 позиции
    vector.ensureCapacity(22);

    //Фиксирует размер Vectore, при этом лишние элементы удаляет, а если больше элементов, заполняет их null
    vector.setSize(5);

    // Возвращает массив элементов из Vectore
    String[] objects = (String[]) vector.toArray();

}
```



**5.15 ListIterator** - расширенная структура данных от iterator.

**Отличия от Iterator:**<br />
- Vector - синхронизируется, т е в один момент времени к Vector имеет доступ только один поток, что позволяет не поврежать данные<br />
- Увеличение емкости, Vector удваивает свой размер, ArrayList увеличивает в полтора раза<br />
- Vector имеет Enumeration в отличие от ArrayList(Преимущества Enumeration: синхронизированы, элементы в колекции защищены, не удаляются, не заменяются и не добавляются, )<br />
- Vectore имеет устаревшие функции, которые есть только в Vectore<br />

**Примечания:**<br />
- listIterator взаимосвязан с изначальным ArrayList, при изменении элементов в listIterator, меняется и сам List<br />


**Пример работы:**
```java
// listIterator
@Test
public void listIteratorTest(){
    ArrayList<String> arrayList = new ArrayList<>(List.of("1","2","3","4","5"));

    ListIterator<String> listIterator = arrayList.listIterator();

    // listIterator взаимосвязан с изначальным ArrayList, при изменении элементов в listIterator, меняется и сам ArrayList
    listIterator.next();
    listIterator.remove();

    // listIterator переход к следующего/предыдущего
    listIterator.next();
    listIterator.previous();

    // listIterator проверка существование следующего/предыдущего
    boolean hasNext = listIterator.hasNext();
    boolean hasPrevious = listIterator.hasPrevious();

    // listIterator получение индекса следующего/предыдущего
    int nextIndex = listIterator.nextIndex();
    int previousIndex = listIterator.previousIndex();

    // listIterator обход всех элементов и их изменение
    listIterator.forEachRemaining(e -> e = e + "prefix");


    //Проверка CRUD

    // Добавление элемента
    ListIterator<String> listIterator1 = Arrays.asList("1","2","3","4","5").listIterator();
    listIterator1.add("6");

    // Удаление элемента
    ListIterator<String> listIterator2 = Arrays.asList("1","2","3","4","5").listIterator();
    while (listIterator2.hasNext()){
        if(listIterator2.next().equals("3")){
            listIterator2.remove();
            break;
        }
    }

    // Изменение элемента
    ListIterator<String> listIterator3 = Arrays.asList("1","2","3","4","5").listIterator();
    while (listIterator3.hasNext()){
        if(listIterator3.next().equals("4")){
            listIterator3.set("44");
            break;
        }
    }

    // Чтение
    ListIterator<String> listIterator4 = Arrays.asList("1","2","3","4","5").listIterator();
    String read = listIterator4.next();
    
}
```


**5.15 Set** - коллекция элементов, не допускающих дублирования.

**Реализации:**
java.util.HashSet<br />
java.util.TreeSet<br />
java.util.LinkedHashSet<br />

**Пример:**
```java
// Set - коллекция, список, кторая не содержит дубликаты
@Test
void setTest(){
    Set<String> set = new HashSet<>();

    // Попытка добавить дубли, но не получится
    set.add("1");
    set.add("1");
}
```
<br />



**5.16 HashSet** - List который не имеет дублей, при этом нету порядка элементов.

**Примечания:** 
- Предоставляет быстрый доступ к элементам, но только при условии отсутствия коллизий, когда HashCode правильно настроен и не возвращает одинаковые значения<br />
- При этом порядка элементов нету<br />
- В классе элементов HashSet, hashCode метод должен быть переопределен<br />

Методы:<br />
public Iterator iterator() - Получение Iterator<br />
public int size() - размер множества<br />
public boolean isEmpty() - проверка на пустое множество<br />
public boolean contains(Object o) - есть ли элемент в множестве<br />
public boolean add(Object o) - добавить элемент в множество<br />
public boolean addAll(Collection c) - добавить элементы множества в множ.<br />
public Object[] toArray() - преобразовать в массив<br />
public boolean remove(Object o) - удалить элемент из множества<br />
public boolean removeAll(Collection c) - удалить несколько элементов коллекции<br />
public boolean retainAll(Collection c) - удалить отсутствующие элемнеты из c<br />
public void clear() - очистить<br />
public Object clone() - клонировать<br />
<br />


**5.17 LinkedHashSet**  - связанный список, который поддерживает порядок и который исключает дубли
<br />



**5.18 TreeSet** - отсортированный список, для хранения применяется дерево, объекты сохраняются в отсортированном порядке, который основан на красно-черном дереве(тип бинарных деревьев).

**Источники:**
https://www.codeflow.site/ru/article/java-tree-set

**Красное-черное дерево** - это сбалансированное бинарное дерево поиска, которое имеет следующие характеристики:<br />
- Каждый узел окрашен либо в красный, либо в черный цвет (в структуре данных узла появляется дополнительное поле – бит цвета).<br />
- Корень окрашен в черный цвет.<br />
- Листья(так называемые NULL-узлы) окрашены в черный цвет.<br />
- Каждый красный узел должен иметь два черных дочерних узла. Нужно отметить, что у черного узла могут быть черные дочерние узлы. Красные узлы в качестве дочерних могут иметь только черные.<br />
- Пути от узла к его листьям должны содержать одинаковое количество черных узлов(это черная высота).<br />

**Картинка:**<br />
https://habrastorage.org/r/w1560/web/ae2/9ed/b02/ae29edb02c724c209d25ec3ee48724f5.png

**Данная балансировка позволяет выполнять операции:**<br />
- вставки<br />
- удаления<br />
- выборки<br />
за O(log n)<br />

**Алгоритм заполнения красно черного дерева:**<br />
https://habr.com/ru/post/330644/

Вставка в красно-черное дерево начинается со вставки элемента, как в обычном бинарном дереве поиска. Только здесь элементы вставляются в позиции NULL-листьев. Вставленный узел всегда окрашивается в красный цвет. Далее идет процедура проверки сохранения свойств красно-черного дерева $1-5$.<br />

Свойство 1 не нарушается, поскольку новому узлу сразу присваивается красный цвет.<br />

Свойство 2 нарушается только в том случае, если у нас было пустое дерево и первый вставленный узел (он же корень) окрашен в красный цвет. Здесь достаточно просто перекрасить корень в черный цвет.<br />

Свойство 3 также не нарушается, поскольку при добавлении узла он получает черные листовые NULL-узлы.<br />

**В основном встречаются 2 других нарушения:**<br />

1) Красный узел имеет красный дочерний узел (нарушено свойство $4$).<br />

2) Пути в дереве содержат разное количество черных узлов (нарушено свойство $5$).<br />

Подробнее о балансировке красно-черного дерева при разных случаях (их пять, если включить нарушение свойства $2$) можно почитать на wiki.<br />

**Идея работы:**<br />
В начале мы вставляем узлы в дерево, а потом оно провидит балансировку<br />
Бинарное дерево(Red-black tree, RB tree, binary tree, B-деревьями)<br />
TreeMap()<br />

**Примечания:**<br />
- По умолчанию сортировка производится привычным способом, но можно изменить это поведение через интерфейс Comparable.<br />
- Реализует интерфейс: SortedSet<br />
- TreeSet использует самобалансирующееся двоичное дерево поиска, а точнее красно-черное дерево<br />
- несинзронизироват по дефолту, но можно синхронизировать с помощью synchronized<br />
- TreeSet в отличие от ArrayList всегда содержит элементы в отсортированном виде<br />
<br />

**Дополнительные методы**<br />
Кроме стандартных методов Set у интерфейса есть свои методы.<br />

Comparator comparator()<br />
subSet(Object fromElement, Object toElement)<br />
tailSet(Object fromElement)<br />
headSet(Object toElement)<br />
Object first()<br />
Object last()<br />


**Вывод**<br />
TreeSet является структурой данных с большей локальностью, поэтому мы можем сделать вывод в соответствии с Принципом локальности, что мы должны отдавать предпочтение TreeSet , если у нас мало памяти и если мы хотим получить доступ к элементам, которые находятся относительно близко друг другу в соответствии с их естественным порядком.<br />

В случае, если данные должны быть прочитаны с жесткого диска (который имеет большую задержку, чем данные, прочитанные из кеша или памяти), тогда предпочитайте TreeSet , поскольку он имеет большую локальность<br />





**5.19 Queue<T>** - интерфейс для работы с очередями, по принципу первый зашел, первый вышел<br />

**Основное примечания:**

**Источники:**
https://betacode.net/13451/java-queue<br />
https://raw.githubusercontent.com/wanglizhi/wanglizhi.github.io/master/img/2016-06-22/Java-Collections_API-Queue.jpg - полная схема интерфейсов и реализаций очередей<br />

**Расширяет интерфейсы:**
- Collection<br />

**Очереди:**
Queue
    BlockingQueue(Interface) - заставляют потоки ожидать очереди<br />
        LinkedBlockingQueue(LinkedBlockingDeque) - потокобезопасные<br />
        ArrayBlockingQueue - потокобезопасные<br />
        PriorityBlockingQueue - потокобезопасные<br />
        DelayQueue - потокобезопасные<br />
        SynchronousQueue - потокобезопасные<br />
        TransferQueue(Interface)<br />
            LinkedTransferQueue - потокобезопасные<br />
    Deque(Interface) - как из колоды карт можно взять с начала и с конца<br />
        ArrayDeque<br />
		LinkedList<br />
		ConcurrentLinkedDeque<br />
		LinkedBlockingDeque<br />

PriorityQueue<br />
BlockingDeque<E><br />
ArrayDeque<br />
LinkedList<br />
ConcurrentLinkedQueue<br />

**Дополнительные вопросы:**
FIFO

**Реализации:**
PriorityQueue - класс<br />
Deque<T> - интерфейс<br />
    ArrayDeque(Deque) - класс<br />
    LinkedList(Deque) - класс<br />

**Область применения очередей:**
- Для отложенных операций, когда операции тяжеловесные и требуют много времени, а пользователь ждать не хочет, мы помещаем операцию в очередь, пока какой-нибудь worker не возьмет ее в работу<br />
- Для обработки пиковых нагрузок, когда мы получили большой объем задач, мы можем их распределить и равномерно выполнять<br />
- Масштабируемость, за счет этого мы можем поднять несколько сервисов, которые будут брать задачи из очереди и выполнять задачи быстрее<br />
- Для java актуально использовать для многопоточности, когда есть очередь и несколько потоков ее обрабатывает, берут из нее задачи<br />

**Заметки:**
- Чтобы получить доступ к элементу, в начале нужно удалить его предшественников<br />
- Элементы можно вставить куда-то в очередь, не обязательно в конец, в зависимости от типа очереди и приоритета элемента<br />

**Методы:**
boolean add(E e); // Вставляет элемент в очередь, если нет места - Exception если успех - true<br />
boolean offer(E e); // Пытается вставить элемент в очередь если нету места и по другим причинам возвращает false<br />
<br />
E remove(); // Возвращает элемент и удаляет его, если элементов больше нет, возвращает исключение<br />
E poll(); // Возвращает элемент и удаляет его, если элементов больше нет, возвращает null<br />
<br />
E element(); //  Возвращает элемент но не удаляет его, если элементов больше нет, возвращает исключение<br />
E peek(); // Возвращает элемент но не удаляет его, если элементов больше нет, возвращает null<br />
<br />
**Основные реализации Queue:**<br />
- LinkedList - стандартная очередь, также реализует List<br />
- Deque - можно получить как с начала, так и с конца очереди<br />
- PriorityQueue - очередь на основе приоритетов, сортирует элементы согласно правилу Comparator. Есть стандартная сортировка<br />
- Concurrentlink Queue - очередь для многопоточности<br />
- LinkedBlockingQueue - очередь для многопоточности, необязательно ограничена по размеру<br />
- ArrayBlocking Queue - очередь для многопоточности<br />
- SynchronousQueue - очередь для многопоточности<br />

**Источник:**<br />
https://www.baeldung.com/java-queue<br />
https://translated.turbopages.org/proxy_u/en-ru.ru.5ee42d6e-63f9d44a-e39b2a87-74722d776562/https/www.baeldung.com/java-queue<br />
https://www.baeldung.com/java-queue-linkedblocking-concurrentlinked<br />

**Интерфейсы:**<br />
- BlockingQueue<br />
- TransferQueue<br />
<br />



**5.20 LinkedList<T>** - стандартная очередь, также реализует List

**Пример работы с linkdeList как с queue:**
```java
// Работа с LinkedList как с queue
@Test
void linkedListQueueTest(){
    Queue<Integer> linkedList = new LinkedList<>();
    for (int i = 0; i < 1000; i++) {
        linkedList.add(i);
    }

    //Getting an element without removing, if queue is empty returns exception
    Integer element = linkedList.element();

    //Getting an element without removing, if queue is empty returns null
    Integer peek = linkedList.peek();

    //Additing an element if it's possible else false
    linkedList.offer(1001);

    //Additing an element if it's possible else exception
    linkedList.add(1002);

    // Getting an element with deleting from queue, if empty returns null
    Integer poll = linkedList.poll();

    //Bypass all elements without deleting
    for (Integer i : linkedList) {
        System.out.println(i);
    }

    Queue<Integer> linkedList2 = new LinkedList<>();
    //With deleting elements array traversing
    while (!linkedList.isEmpty()){
        linkedList2.add(linkedList.poll());
    }

}
```
<br />

**5.21 Deque<T>** - можно получить как с начала, так и с конца очереди<br />


**5.22 PriorityQueue<T>** - очередь на основе приоритетов, сортирует элементы согласно правилу Comparator. Есть стандартная сортировка<br />


**5.23 PriorityQueue<T>** - очередь на основе приоритетов, сортирует элементы согласно правилу Comparator. Есть стандартная сортировка<br />



**5.24 LinkedBlockingQueue** - очередь для многопоточности(потокобезопасная), характерная тем, что может быть неограничена по размеру, и обладает блокирующим характером, т е, если очередь пуста и поток пытается получить, он замораживается, а если очередь наполняется, поток возобновляет работу, тоже самое и с переполнением очереди. Работает на связанных узлах<br />
Если мы уткнемся в ограничение времени, вернет исключение OutOfMemoryError<br />

**Реализует BlockingQueue interface**<br />

**Пример:**<br />
BlockingQueue<Integer> boundedQueue = new LinkedBlockingQueue<>(100);<br />
BlockingQueue<Integer> unboundedQueue = new LinkedBlockingQueue<>(); // неограниченная по размеру<br />

**Ресурс блокируется если:**<br />
- блокируется если заканчиваются элементы в очереди, если элементы появились потоки запускаются<br />
- блокируется если очередь заполнена, если место появвилось поток запускается<br />
- блокируется если поток получает элемент<br />
- блокируется если поток добавляет элемент<br />

**Методы для многопоточности:**<br />
boundedQueue.take(); // получение потока, если элементов нет, замораживается потоки, но если появится опять, продолжет выполнение<br />
boundedQueue2.put(elem); // добавляет элемент, если очередь полная, поток останавливается, когда место появляется поток возобновляется<br />


В результате при работе с потоком мы не получим колизей и так далее, как например при работе с очередью LinkedList. В результате перемещение элементов из одной очереди в другую мы получим одинаковые элементы, которых не было:<br />
283 = 281 !duplicate<br />
284 = 282<br />
285 = 281 !duplicate<br />

**Пример работы с потоками и очередями**<br />
```java
//Пример работы с потоками
BlockingQueue<Integer> boundedQueue = new LinkedBlockingQueue<>();
BlockingQueue<Integer> boundedQueue2 = new LinkedBlockingQueue<>();

for(int i = 0; i < 40000; i++){
    boundedQueue.add(i);
}

ExecutorService executorService1 = Executors.newFixedThreadPool(8);
for(int i = 0; i < 8; i++){
    Runnable runnable = () -> {
        while (boundedQueue.peek() != null){
            Integer elem = boundedQueue.poll();
            if(elem != null){
                boundedQueue2.add(elem);
            }
        }
    };
    executorService1.execute(runnable);
}
```


**5.25 ConcurrentLinkedQueue** - неограниченная, потокобезопасная и не блокирующая очередь. Неограниченная по размеру, возможность работать с многопоточностью, не блокирует потоки в случае опустошения очереди возвращает null<br />

Если мы запустим поток, который будет постоянно считывать из очереди элементы, он будет получать null если нету элементов, но если элементы появляются, то он начнет получать элементы, результат примерно такой:<br />
```java
null
null
OurElement
null
null
null
null
OurElement
....
```

**Пример:**
```java
// concurrentLinkedQueue - демонстрация работы
@Test
void concurrentLinkedQueueTest(){
    // Создаем очереди concurrentLinkedQueue
    Queue<Integer> concurrentLinkedQueue1 = new ConcurrentLinkedQueue<>();
    Queue<Integer> concurrentLinkedQueue2 = new ConcurrentLinkedQueue<>();

    // Запускаем производителей, которые каждые 100 млс будут производить
    ExecutorService producers = Executors.newFixedThreadPool(4);
    for (int i = 0; i < 4; i++){
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Runnable producer = () -> {
            while (true){
                concurrentLinkedQueue1.add(22222222);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        producers.execute(producer);
    }

    // Запускаем потребителей, которые будут возвращать null, но когда будет номер будет возвращать номер
    ExecutorService customers = Executors.newFixedThreadPool(4);
    for (int i = 0; i < 4; i++){
        Runnable customer = () -> {
            while (true){
                System.out.println(concurrentLinkedQueue1.poll());
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        customers.execute(customer);
    }

    try {
        TimeUnit.SECONDS.sleep(20);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```


**5.26 ArrayBlockingQueue** - очередь потокобезопасная, блокирующая, ограниченая по размеру<br />

**Пример:**<br />
BlockingQueue<Integer> arrayBlockingQueue = new ArrayBlockingQueue<>(100);

**Конструкторы:**<br />
ArrayBlockingQueue(int capacity) // размер
ArrayBlockingQueue(int capacity, boolean fair) // размер и политика доступа
ArrayBlockingQueue(int capacity, boolean fair, Collection<? extends E> c) // размер, политика доступа и массив элементов



**5.27 priorityBlockingQueue** - потокобезопасная блокирующая очередь поддерживающая приоритетность<br />

**Детали:**<br />
- не поддерживает изначальный порядок добавления элементов в очередь, сразу сортирует по указанному порядку<br />
- можно кастомизировать порядок, согласно переданного Comparator или лямбды<br />
PriorityBlockingQueue<Integer>(10000, (o1, o2) -> ((Integer) o2).compareTo(((Integer) o1)));
- реализует интерфейс BlockingQueue<br />
- Схож с PriorityQueue, но имеет блокирующий интерфейс, что позволяет предоставлять свойства блокировки<br />
- В случаях окончания элементов или заполнение автоматически блокирует потоки, до благоприятных условий при которых их разблокирует<br />
- может быть очередью с неорграниченной вместимостью<br />
- повторяющиеся элементы разрешены<br />
- null не разрешен<br />

**Конструкторы:**<br />
```java
public PriorityBlockingQueue()
public PriorityBlockingQueue(int initialCapacity)  
public PriorityBlockingQueue(int initialCapacity, Comparator<? super E> comparator)  
public PriorityBlockingQueue(Collection<? extends E> c) 
```

**Пример**<br />
```java
@Test
void priorityBlockingQueue() throws InterruptedException {
    BlockingQueue<Integer> priorityBlockingQueue = new PriorityBlockingQueue<Integer>(10000, (o1, o2) -> ((Integer) o2).compareTo(((Integer) o1)));
    BlockingQueue<Integer> blockingQueue = new LinkedBlockingQueue<>();

    //Производители, которые добавляют случайные числа от 1-го до 10
    ExecutorService producers = Executors.newFixedThreadPool(4);
    for (int i = 0; i < 4; i++) {
        Runnable producer = ()->{
            while (true){
                int randomNum = ThreadLocalRandom.current().nextInt(1, 10);
                try {
                    priorityBlockingQueue.put(randomNum);
                    Thread.sleep(5);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

        };
        producers.execute(producer);
    }
    TimeUnit.SECONDS.sleep(5);
    producers.shutdown();
    TimeUnit.SECONDS.sleep(5);


    //Производители, которые добавляют случайные числа от 1-го до 10
    ExecutorService customers = Executors.newFixedThreadPool(4);
    for (int i = 0; i < 4; i++) {
        Runnable customer = ()->{
            while (true) {
                try {
                    Integer elem = priorityBlockingQueue.take();
                    System.out.println(elem);
                    blockingQueue.put(elem);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        customers.execute(customer);
    }

    TimeUnit.SECONDS.sleep(10);
    customers.shutdown();

}
```


**5.28 SynchronousQueue** - очередь для многопоточности<br />


**5.29 Deque** - интерфейс для Очередей в которой можно получать элементы как с начала так и с начала очереди<br />

**Реализации:**<br />
- ArrayDeque(неограниченная, двухсторонняя)<br />
- LinkedList(неограниченная, двухсторонняя) <br />
- ConcurrentLinkedDeque(потокобезопасная, неограниченная, двухсторонняя)<br />
- LinkedBlockingDeque(потокобезопасная, блокирующая, неограниченная, двухсторонняя)<br />

**Методы:**<br />
peek - получение элемента без удаление, возвращает element/null<br />
pool - получение элемента с удалением, возвращает element/null<br />
get - получение элемента без удаление, возвращает element/exception<br />
add - добавление элемента, возвращает true/exception<br />
offer - добавление элемента, возвращает true/false<br />

**Примеры работы:**
```java
// Все очереди с deque
@Test
void duqueTest() throws InterruptedException {

    ArrayList<Integer> arrayList = new ArrayList<>();
    for (int i = 0; i < 1000; i++) {
        arrayList.add(i);
    }

    //Deque Initialization
    Deque<Integer> arrayDeque = new ArrayDeque<>(arrayList);
    Deque<Integer> linkedList = new LinkedList<>(arrayList);

    // MultiThreading
    Deque<Integer> concurrentLinkedDeque = new ConcurrentLinkedDeque<>(arrayList);
    BlockingDeque<Integer> linkedBlockingDeque = new LinkedBlockingDeque<>(arrayList);

    //Additing at first or at last. If possible - true else exception
    arrayDeque.addLast(9999);
    arrayDeque.addFirst(8888);

    //Additing an elements at first or at last. If possible - true else - false
    arrayDeque.offerFirst(2222);
    arrayDeque.offerLast(1111);

    // Getting from first or from end without removing. If possible - true else - exception
    Integer getFirst = arrayDeque.getFirst();
    Integer getLast = arrayDeque.getLast();

    // Getting from first or from end without deleting. If it's empty returns null
    Integer peekFirst = arrayDeque.peekFirst();
    Integer peekLast = arrayDeque.peekLast();

    // Getting from first or from end with deleting. If it's empty returns null
    Integer pollFirst = arrayDeque.pollFirst();
    Integer pollLast = arrayDeque.pollLast();

    // Getting elements from beginning and ending
    ExecutorService executorService = Executors.newFixedThreadPool(8);
    for (int i = 0; i < 4; i++) {
        Runnable runnableFirst = () -> {
            while (true){
                try {
                    System.out.println(linkedBlockingDeque.takeFirst());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        executorService.execute(runnableFirst);

        Runnable runnableLast = () -> {
            while (true) {
                try {
                    System.out.println(linkedBlockingDeque.takeLast());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        executorService.execute(runnableLast);
    }
    TimeUnit.SECONDS.sleep(3);
    executorService.shutdown();
}
```


**5.30 DelayQueue** - потокобезопасная, блокирующая, неограниченная очередь, основана на выдачи элементов по истечению времени. 
Реализуют интерфейс BlockingQueue<br />

**Пример**<br />
```java
BlockingQueue<DelayObject> delayQueue = new DelayQueue<>();
```

Элементы должны реализовывать интерфейс Delayed<br />

Для реализации DelayObject мы должны реализовать методы:<br />
public long getDelay(TimeUnit unit) - должен возвращать отрицательное значение, сколько времени в миллисекундах будет задержка
Например:
-3000 // 3 секунды будет задержка

**Применение:**<br />
Например можно вполнять задачи по расписанию запускать воркеры в ожидания задач, при это воркеры не занимаю вычесление т к BlockingQueue их замораживает 
Пример Delayed, отсчет начнется после создания элемента<br />

**Пример кода**
```java
class DelayObject implements Delayed {

    private String name;
    private long time;

    // Constructor of DelayObject
    public DelayObject(String name, long delayTime)
    {
        this.name = name;
        this.time = System.currentTimeMillis()
                + delayTime;
    }

    // Implementing getDelay() method of Delayed
    @Override
    public long getDelay(TimeUnit unit)
    {
        long diff = time - System.currentTimeMillis();
        return unit.convert(diff, TimeUnit.MILLISECONDS);
    }

    // Implementing compareTo() method of Delayed
    @Override
    public int compareTo(Delayed obj)
    {
        if (this.time < ((DelayObject)obj).time) {
            return -1;
        }
        if (this.time > ((DelayObject)obj).time) {
            return 1;
        }
        return 0;
    }
}


@Test
void delayQueueTest() throws InterruptedException {
    BlockingQueue<DelayObject> delayQueue = new DelayQueue<>();

    delayQueue.add(new DelayObject("Test 500",500L));
    delayQueue.add(new DelayObject("Test 3000",3000L));
    delayQueue.add(new DelayObject("Test 100",100L));
    delayQueue.add(new DelayObject("Test 2000",2000L));
    delayQueue.add(new DelayObject("Test 6000",6000L));
    delayQueue.add(new DelayObject("Test 2000",2000L));

    Thread thread = new Thread(()->{
        while (true){
            try {
                System.out.println(delayQueue.take().name);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    });
    thread.start();
    TimeUnit.SECONDS.sleep(10);
}

@Test
void delayQueueSecondTest() throws InterruptedException {

    DelayObject delayObject = new DelayObject("Test 500",500L);

    BlockingQueue<DelayObject> blockingQueue = new DelayQueue<>();
    blockingQueue.add(new DelayObject("Test 500",500L));
    blockingQueue.add(new DelayObject("Test 2000",2000L));
    blockingQueue.add(new DelayObject("Test 1000",1000L));
    blockingQueue.add(new DelayObject("Test 5000",5000L));

    DelayQueue<DelayObject> delayQueue = new DelayQueue<>();

    while (true){
        System.out.println(blockingQueue.take().name);
    }

}
```
<br />

**5.31 SynchronousQueue** - потокобезопасная, блокирующая, неограниченная очередь с размером в 1 элемент, альтернатива атомарным переменным, но более удобна, когда производитель кладет элемент он замораживается пока потребитель не вытащит элемент. Можно работать по принцыпу производитель - потребитель
https://www.codeflow.site/ru/article/java-synchronous-queue<br />

Данная очередь сильно оптимизированна в отличии от LinkedBlockingQueue<br />
Она имеет размер 1 элемент<br />

**Пример:**
```java
@Test
void synchronousQueueTest(){
    ExecutorService executorService = Executors.newFixedThreadPool(2);
    SynchronousQueue<Integer> synchronousQueue = new SynchronousQueue<>();

    Runnable producer = () -> {
        Integer producedElement = 3333;
        try {
            synchronousQueue.put(producedElement); // замораживается поток, пока потребитель не снимет элемент из очереди
            synchronousQueue.put(producedElement);
            synchronousQueue.put(producedElement);
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
    };

    Runnable consumer = () -> {
        try {
            Integer consumedElement = synchronousQueue.take();
            System.out.println(consumedElement);
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
    };
    executorService.execute(consumer);
    executorService.execute(producer);
}
```


**5.32 Использование общих переменных(Shared Variable) в потоках** - для этих задач можно использовать АТОМАРНЫЕ переменные, т е переменные оснащенные методами для реализации атомарности, например такие как:<br />
```java
AtomicInteger.compareAndSet(current,value);
```
<br />
**Атомарные операции** - операции которые совершаются полностью или не совершаются вовсе, например i++ это 3 операции, чтение, увеличение значение и запись. Во время этого другие потоки могут также изменить это значение, в результате мы получим неверное значение.<br />

АТОМАРНЫЕ переменные позволяют с помощью атомарной операции проверить было ли изменено значение current или нет, если нет, то мы устанавливаем ему новое значение в обратном случае возвращает false.<br />
Далее мы этот код помещаем в бесконечный цикл, который будет пытаться изменить значение и когда отдаст true мы выходим из цикла:<br />
```java
int current, value;
do{
    current = atomicInteger.get();
    // И другие операции: Этот блок будет атомарным, т е либо выполнится полностью либо не выполнится вообще
    value = current + 1; // integer.incrementAndGet();
    value = value + 1;

} while (!atomicInteger.compareAndSet(current,value));
```


**Атомарность** - операция, которая выполняется полностью либо не выполняется вообще. Атомарность очень актуально в многопоточных приложениях например i++, это с точки зрения JVM несколько операций, чтение и запись, но если данная операция не атомарна, она может привести к ошибке, например 2 потока увеличивают это значение i++, в результате они могут прочитать значение одновременно и увеличить значение на 1 одновременно, а мы будем ожидать увеличение 2 раза.
Для решения данной проблемы и существует атомарность, блокировка и синхронизация.<br />

**Писимистический подход общих переменных**<br />
Блокировка, блокирует все потоки как бутылочное горлышко, пока 1 поток не завершит операцию. <br />
Минусы:<br />
- Возможность взаимной блокировки - это заблокирует несколько потоков, без возможности разблокировки<br />
- С ресурсом(переменной) в один момент времени может работать только 1 поток<br />

**Оптимистический подход общих переменных**<br />
Если поток видет что переменная изменилась другим потоком, он меняет переменную уже исходя из нового значения<br />


**Механизм оптимистической блокировки** - мы получаем значение переменной, проводим расчеты, получаем новый результат этой переменной, проверяем поменялась ли переменная за это время, если нет, то меняем, если да, проводим иттерацию с начала, это работает с атомарными переменными через метод:
compareAndSet() вернет false если значение было изменено<br />
приведет к новой итерации цикла while в методе getAndAdd.<br />

**Классы:**<br />
https://java-online.ru/concurrent-atomic.xhtml<br />
AtomicInteger - класс для работы с Integer внутри потока, позволяет не быть final<br />


**Пример:**
```java
// Пример использование атомарных переменных и атомарных операций
@Test
void sharedVariableThird() throws InterruptedException {
    AtomicInteger atomicInteger = new AtomicInteger(); //Создаем атомарную переменную
    atomicInteger.set(0);
    Runnable runnable = ()->{
        for (int i = 0; i < 50000; i++) {
            int current, value;
            do{
                current = atomicInteger.get(); // получаем текущее значение
                // И другие операции: Этот блок будет атомарным, т е либо выполнится полностью либо не выполнится вообще
                value = current + 1; // integer.incrementAndGet();
                value = value + 1;

            } while (!atomicInteger.compareAndSet(current,value)); // Проверяем, изменялась ли переменная другими потоками, если нет то изменяем значение

            // Или можем использовать уже готовые функции AtomicInteger
            //integer.incrementAndGet();
            //integer.incrementAndGet();
        }
        System.out.println(Thread.currentThread().getName() + " is finished");
    };
    (new Thread(runnable)).start();
    (new Thread(runnable)).start();
    (new Thread(runnable)).start();
    (new Thread(runnable)).start();
    TimeUnit.SECONDS.sleep(1);
    System.out.println(atomicInteger.get()); // Результат должен быть 400 000
}

// Многопоточность БЕЗ АТОМАРНОСТИ с ошибками. Если мы запустим изменение переменной в 4х циклах мы получим случаи значение отличное от ожидаемого, так как нет блокировки ресурса, и потоки работают одновременно
@Test
void sharedVariableWithoutBlocking() throws InterruptedException {
    AtomicInteger integer = new AtomicInteger();
    integer.set(0);
    Runnable runnable = ()->{
        for (int i = 0; i < 50000; i++) {
            integer.set(integer.get() + 1);

        }
        System.out.println(Thread.currentThread().getName() + " is finished");
    };
    (new Thread(runnable)).start();
    (new Thread(runnable)).start();
    (new Thread(runnable)).start();
    (new Thread(runnable)).start();
    TimeUnit.SECONDS.sleep(2);
    System.out.println(integer.get());
}
```
<br />

**5.33 LinkedTransferQueue** - потокобезопасная, блокирующа, неограниченная очередь с некоторыми аспектами. Поток имеет метод transfer() который передает только по одному элементу и про втором вызове замораживает поток для ожидания потребителя<br />

**Логика:**<br />
- Если добавляем через add() или put(), то добавляет сколько угодно элементов как обычная очередь<br />
```java
linkedTransferQueue.put(11);
linkedTransferQueue.add(12);
```
- Если добавляем через transfer(), элемент передается в поток и идет дальше, но если вызываем второй раз transfer(), поток Производитель замораживается<br />
```java
linkedTransferQueue.transfer(1);
```
- Если Потребитель получает элемент через take() он выполняет код с ним, а только потом выполняется остальной код Производителя<br />
```java
linkedTransferQueue.take();
transfer(); // Второй вызов transfer() замораживает поток<br />
```

TransferQueue - интерфейс<br />
BlockingQueue - интерфейс<br />
    LinkedTransferQueue - реализация<br />

**Методы:**<br />
put(e); // кладет элементы в очередь, элемент, который находится в очереди больше всего будет взят первым<br />
transfer(e); // кладет элемент в очередь если потребитель ожидает, в обратном случае будет блокирован, переведен в режим ожидания, по логике SynchronousQueue<br />


**Не гарантируется что массовые операции будут выполненны полностью(Атомарно):**<br />
addAll()<br />
removeAll()<br />
retainAll()<br />
containsAll()<br />
equals()<br />
toArray()<br />


**Пример:**
```java
@Test
void transferQueueTest() throws InterruptedException {
    TransferQueue<Integer> linkedTransferQueue = new LinkedTransferQueue();
    
    Runnable producer = ()->{
        try {
            linkedTransferQueue.transfer(1);
            System.out.println("1 Added");
            linkedTransferQueue.transfer(2);
            System.out.println("2 Added");
            linkedTransferQueue.transfer(3);
            System.out.println("3 Added");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

    };

    Runnable consumer = ()->{
        try {
            System.out.println(linkedTransferQueue.take());
            System.out.println(linkedTransferQueue.take());
            System.out.println(linkedTransferQueue.take());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    };
    ExecutorService executorService = Executors.newFixedThreadPool(2);
    executorService.execute(producer);
    TimeUnit.SECONDS.sleep(1);
    executorService.execute(consumer);
}
```













































=========================================


**5. Iterable<T>** - интерфейс предоставляет методы для обхода элементов коллекций(bypass collection elements) с помощью forEach, for, iterator

**Методы:**<br />
- обход массивов(iterator.next()), <br />
- проверка есть ли следующий элемент(iterator.hasNext()), <br />
- более лаконичный обход массивов(iterator.forEachRemaining((element) -> {System.out.println(element);}))<br />
- удаление текущего элемента(iterator.remove())<br />

**Пример:**<br />
```java
//Interface Iterator - необходим для обхода массива, получение следующего элемента, или определение есть ли следующий
//Interface Iterator - requiered for collection traversing, getting a next element, checking if there is a next element, deleting a curent element
@Test
void iteratorInterface() {
    ArrayList<String> arrayList = new ArrayList<>();
    arrayList.add("1");
    arrayList.add("2");
    arrayList.add("3");

    Iterator<String> iterator = arrayList.iterator();

    //Проверяем есть ли следующий элемент(Checking, if there is the next element )
    boolean hasNext = iterator.hasNext(); // true

    //Мы получаем следующий элемент(Getting the next element)
    String str = iterator.next(); // 1

    iterator.next();

    // Удаляет текущий элемент, в данный момент 2ой, также и в самой коллекции ArrayList(Deleting the current element)
    iterator.remove(); // iterator = {"1", "3"}

    // Обход элементов массива более лаконичным способом(more convenient array traversing)
    arrayList.iterator().forEachRemaining((element) -> {
        System.out.println(element);
    });
}
```














