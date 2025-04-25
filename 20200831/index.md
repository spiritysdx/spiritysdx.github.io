# 创建新线程(通用)


### 创建并使用多线程

```python
print('主线程执行代码') 

# 从 threading 库中导入Thread类
from threading import Thread
from time import sleep

# 定义一个函数，作为新线程执行的入口函数
def threadFunc(arg1,arg2):
    print('子线程 开始')
    print(f'线程函数参数是：{arg1}, {arg2}')
    sleep(5)
    print('子线程 结束')


# 创建 Thread 类的实例对象， 并且指定新线程的入口函数，此时并没有执行
thread = Thread(target=threadFunc,
                args=('参数1', '参数2')
                )

#target=threadFunc对应执行的函数threadFunc
#args=('参数1', '参数2')这样新进程添加参数
# 执行start 方法，就会创建新线程，
# 并且新线程会去执行入口函数里面的代码。
# 这时候这个进程有两个线程了。↓
thread.start()

# 主线程的代码执行 子线程对象的join方法，
# 就会等待子线程结束，才继续执行下面的代码
thread.join()
print('主线程结束')
```

运行该程序，解释器执行到下面代码时

```python
thread = Thread(target=threadFunc,
                args=('参数1', '参数2')
                )
```

创建了一个Thread实例对象，其中，Thread类的初始化参数 有两个

target参数 是指定新线程的 入口函数， 新线程创建后就会 执行该入口函数里面的代码，

args 指定了 传给 入口函数threadFunc 的参数。 线程入口函数 参数，必须放在一个元组里面，里面的元素依次作为入口函数的参数。

注意，上面的代码只是创建了一个Thread实例对象， 但这时，新的线程还没有创建。

要创建线程，必须要调用 Thread 实例对象的 start方法 。也就是执行完下面代码的时候
```python
thread.start()
```

新的线程才创建成功，并开始执行 入口函数threadFunc 里面的代码。


有的时候， 一个线程需要等待其它的线程结束，比如需要根据其他线程运行结束后的结果进行处理。

这时可以使用 Thread对象的 join 方法

```python
thread.join()
```

如果一个线程A的代码调用了 对应线程B的Thread对象的 join 方法，线程A就会停止继续执行代码，等待线程B结束。 线程B结束后，线程A才继续执行后续的代码。

```就是等所一线程执行完毕才继续运行运行下面的程序```

所以主线程在执行上面的代码时，就暂停在此处， 一直要等到 新线程执行完毕，退出后，才会继续执行后续的代码。

```python
#错误示例！！！！
thread = Thread(target=threadFunc('参数1', '参数2'))
```

↑如果这样写无法创建新线程并执行，这样target传入不是函数，传入的是运行结果```(null)```，而且是在主线程运行完了，并不是在子线程里运行。

### 共享数据的访问控制

做多线程开发，经常遇到这样的情况：多个线程里面的代码需要访问```同一个```公共的数据对象。

这个公共的数据对象可以是任何类型， 比如一个列表、字典、或者自定义类的对象。

有的时候，程序需要防止线程的代码同时操作公共数据对象。否则，就有可能导致数据的访问互相冲突影响。

请看一个例子。

我们用一个简单的程序模拟一个银行系统，用户可以往自己的帐号上存钱。

对应代码如下：
```python
from threading import Thread
from time import sleep

bank = {
    'byhy' : 0
}

# 定义一个函数，作为新线程执行的入口函数
def deposit(theadidx,amount):
    balance =  bank['byhy']
    # 执行一些任务，耗费了0.1秒
    sleep(0.1)
    bank['byhy']  = balance + amount
    print(f'子线程 {theadidx} 结束')

theadlist = []
for idx in range(10):
    thread = Thread(target = deposit,
                    args = (idx,1)
                    )
    thread.start()
    # 把线程对象都存储到 threadlist中
    theadlist.append(thread)

for thread in theadlist:
    thread.join()

print('主线程结束')
print(f'最后我们的账号余额为 {bank["byhy"]}')
```

上面的代码中，一起执行

开始的时候， 该帐号的余额为0，随后我们启动了10个线程， 每个线程都deposit函数，往帐号byhy上存1元钱。

可以预期，执行完程序后，该帐号的余额应该为 10。

然而，我们运行程序后，发现结果如下
```
子线程 0 结束
子线程 3 结束
子线程 2 结束
子线程 4 结束
子线程 1 结束
子线程 7 结束
子线程 5 结束
子线程 9 结束
子线程 6 结束
子线程 8 结束
主线程结束
```
最后我们的账号余额为 1    
为什么是 1 呢？ 而不是 10 呢？

如果在我们程序代码中，只有一个线程，如下所示

```python
from time import sleep

bank = {
    'byhy' : 0
}

# 定义一个函数，作为新线程执行的入口函数
def deposit(theadidx,amount):
    balance =  bank['byhy']
    # 执行一些任务，耗费了0.1秒
    sleep(0.1)
    bank['byhy']  = balance + amount

for idx in range(10):
    deposit (idx,1)

print(f'最后我们的账号余额为 {bank["byhy"]}')
```

代码都是串行执行的。不存在多线程同时访问bank对象的问题，运行结果一切都是正常的。

现在我们程序代码中，有多个线程，并且在这个几个线程中都会去调用```deposit```，就有可能同时操作这个bank对象，就有可能出一个线程覆盖另外一个线程的结果的问题。

这时，可以使用threading库里面的锁对象```Lock```去保护。

我们修改多线程代码，如下：
```python
from threading import Thread,Lock
from time import sleep

bank = {
    'byhy' : 0
}

bankLock = Lock()

# 定义一个函数，作为新线程执行的入口函数
def deposit(theadidx,amount):
    # 操作共享数据前，申请获取锁
    bankLock.acquire()
    
    balance =  bank['byhy']
    # 执行一些任务，耗费了0.1秒
    sleep(0.1)
    bank['byhy']  = balance + amount
    print(f'子线程 {theadidx} 结束')
    
    # 操作完共享数据后，申请释放锁
    bankLock.release()

theadlist = []
for idx in range(10):
    thread = Thread(target = deposit,
                    args = (idx,1)
                    )
    thread.start()
    # 把线程对象都存储到 threadlist中
    theadlist.append(thread)

for thread in theadlist:
    thread.join()

print('主线程结束')
print(f'最后我们的账号余额为 {bank["byhy"]}')
```
执行一下，结果如下
```
子线程 0 结束
子线程 1 结束
子线程 2 结束
子线程 3 结束
子线程 4 结束
子线程 5 结束
子线程 6 结束
子线程 7 结束
子线程 8 结束
子线程 9 结束
主线程结束
最后我们的账号余额为 10
```
正确了。

```Lock```对象的acquire方法是申请锁。

每个线程在操作共享数据对象之前，都应该申请获取操作权，也就是调用该共享数据对象对应的锁对象的acquire方法。

如果线程A执行如下代码，调用acquire方法的时候，
```python
bankLock.acquire()
```
别的线程B已经申请到了这个锁，并且还没有释放，那么线程A的代码就在此处```等待```线程B释放锁，不去执行后面的代码。

直到线程B执行了锁的```release```方法释放了这个锁，线程A才可以获取这个锁，就可以执行下面的代码了。

如果这时线程B又执行这个锁的acquire方法，就需要等待线程A执行该锁对象的release方法释放锁，否则也会等待，不去执行后面的代码。

### daemon线程

```python
from threading import Thread
from time import sleep

def threadFunc():
    sleep(2)
    print('子线程 结束')

thread = Thread(target=threadFunc)
thread.start()
print('主线程结束')
```

可以发现，主线程先结束，要过个2秒钟，等子线程运行完，整个程序才会结束退出。

因为：

Python程序中当所有的 `非daemon线程` 结束了，整个程序才会结束
主线程是非daemon线程，启动的子线程缺省也是```非daemon线程```线程。

所以，要等到```主线程和子线程```都结束，程序才会结束。


我们可以在创建线程的时候，设置daemon参数值为True，如下
```python
from threading import Thread
from time import sleep

def threadFunc():
    sleep(2)
    print('子线程 结束')

thread = Thread(target=threadFunc,
                daemon=True # 设置新线程为daemon线程
                )
thread.start()
print('主线程结束')
```
再次运行，可以发现，只要主线程结束了，整个程序就结束了。因为只有主线程是非daemon线程。

