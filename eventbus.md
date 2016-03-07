## eventbus ##

###What is EventBus
- eventbus是一个发布/订阅类型的消息总线，以消息为媒介，eventbus为内核，实现消息发布者和消息定约占的解耦
- 事件传递既可用于 Android 四大组件间通讯，也可以用户异步线程和主线程间通讯等等。
- 被大量商业化使用
- 消息的处理线程支持多种模型


###使用   
**1：添加eventbus依赖：**     
`compile 'org.greenrobot:eventbus:3.0.0`

**2：定义事件类型：**   
`public class MyEvent {}`

**3：定义事件处理方法**  
`public void onEventMainThread`

**4：发送事件**  
`EventBus.getDefault().post(new MyEvent())`

###核心概念 
**1：event：**  
event类似于message，可以是任何类型的对象  

**2：发布者：**
任何调用EventBus的post(Object)的对象，可以在任何线程中发布消息

**3：eventbus实例：**
不是很准确的理解，eventbus实例就是一个能把发布者和订阅者关联起来，并把发布者发布大消息转发给订阅者的worker；可以有多个master（eventbus实例），一般情况下用默认提供的：EventBus.getDefault()  

**4：ThreadMode：**  通过注解设置：@Subscribe(threadMode = ThreadMode.MAIN)  
**--PostThread**     
事件的处理在和事件的发送在相同的进程，所以事件处理时间不应太长，不然影响事件的发送线程，而这个线程可能是UI线程。对应的函数名是*onEvent*。 
 
**--MainThread**   
事件的处理会在UI线程中执行。事件处理时间不能太长，这个不用说的，长了会ANR的，对应的函数名是*onEventMainThread*。  

**--BackgroundThread**     
事件的处理会在一个后台线程中执行，虽然名字是BackgroundThread，事件处理是在后台线程，但事件处理时间还是不应该太长，因为如果发送事件的线程是后台线程，会直接执行事件，如果当前线程是UI线程，事件会被加到一个队列中，由一个线程依次处理这些事件，如果某个事件处理时间太长，会阻塞后面的事件的派发或处理.对应的函数名是*onEventBackgroundThread*

**--Async**     
事件处理会在单独的线程中执行，主要用于在后台线程中执行耗时操作，每个事件会开启一个线程（有线程池），但最好限制线程的数目。对应的函数名是*onEventAsync*

**5：sticky Events：**当event没有被删除，始终缓存最后一个event.  
sticky事件就是指在EventBus内部被缓存的那些事件。EventBus为每个类（class）类型保存了最近一次被发送的事件——sticky。后续被发送过来的相同类型的sticky事件会自动替换之前缓存的事件。当一个监听者向EventBus进行注册时，它可能会去请求这些缓存事件。这时，所有已缓存的事件就会立即自动发送给这个监听者，就象这些事件又重新刚被发送了一次一样。这就意味着，一个监听者可以收到在它注册之前就已经被发送到EventBus中的事件（甚至是在这个监听者的实例被创建出来前，这一点是不是很奇妙）。

**6：优先级：**  
订阅者存在优先级，优先级高的订阅者可以取消事件继续向优先级低的订阅者分发，默认所有订阅者优先级都为 0。