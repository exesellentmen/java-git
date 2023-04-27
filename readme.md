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














