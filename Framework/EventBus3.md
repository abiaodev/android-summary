# EventBus 3.1.1 源码分析
## 简介
EventBus是一个适用于Android和Java的事件**发布/订阅**总线框架，可用于Activity、Fragment、Service、Threads等等之间的通讯。该框架使用观察这模式的思想实现，
![EventBus-Publish-Subscribe](https://github.com/GitABiao/AndroidSummary/tree/master/images/EventBus-Publish-Subscribe.png)
---

## 简单使用
### 1、定义Event
```
public class MessageEvent {

    public final String message;

    public MessageEvent(String message) {
        this.message = message;
    }
}
```
### 2、准备订阅者
```
// This method will be called when a MessageEvent is posted (in the UI thread for Toast)
@Subscribe(threadMode = ThreadMode.MAIN)
public void onMessageEvent(MessageEvent event) {
    Toast.makeText(getActivity(), event.message, Toast.LENGTH_SHORT).show();
}

// This method will be called when a SomeOtherEvent is posted
@Subscribe
public void handleSomethingElse(SomeOtherEvent event) {
    doSomethingWith(event);
}
```
### 3、注册/注销EventBus
```
@Override
public void onStart() {
    super.onStart();
    EventBus.getDefault().register(this);
}

@Override
public void onStop() {
    EventBus.getDefault().unregister(this);
    super.onStop();
}
```

### 4、发送事件
```
EventBus.getDefault().post(new MessageEvent("Hello everyone!"));
```
---
## 源码分析
分析源码之前，我想先抛出几个问题，然后根据这几个问题对源码进行分析
* EventBus的具体工作流程是怎样的
* EventBus是怎么处理线程的切换的
* EventBus是怎么执行订阅的方法的

### EventBus的具体工作流程
#### EventBus注册流程
源码如下
```
public void register(Object subscriber) {
        Class<?> subscriberClass = subscriber.getClass();
        //1、通过SubscriberMethodFinder找到当前注册的类的订阅方法
        List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriberClass);
        synchronized (this) {
            for (SubscriberMethod subscriberMethod : subscriberMethods) {
                //2、遍历订阅这些方法
                subscribe(subscriber, subscriberMethod);
            }
        }
    }
private void subscribe(Object subscriber, SubscriberMethod subscriberMethod) {
    Class<?> eventType = subscriberMethod.eventType;
    //激活方法
    Subscription newSubscription = new Subscription(subscriber, subscriberMethod);
    CopyOnWriteArrayList<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
    if (subscriptions == null) {
        subscriptions = new CopyOnWriteArrayList<>();
        subscriptionsByEventType.put(eventType, subscriptions);
    } else {
        if (subscriptions.contains(newSubscription)) {
            throw new EventBusException("Subscriber " + subscriber.getClass() + " already registered to event "
                    + eventType);
        }
    }
    //将Subscription放到合适的优先级
    int size = subscriptions.size();
    for (int i = 0; i <= size; i++) {
        if (i == size || subscriberMethod.priority > subscriptions.get(i).subscriberMethod.priority) {
            subscriptions.add(i, newSubscription);
            break;
        }
    }
    //添加事件并保存到
    List<Class<?>> subscribedEvents = typesBySubscriber.get(subscriber);
    if (subscribedEvents == null) {
        subscribedEvents = new ArrayList<>();
        typesBySubscriber.put(subscriber, subscribedEvents);
    }
    subscribedEvents.add(eventType);
    //粘性事件
    if (subscriberMethod.sticky) {
        if (eventInheritance) {
            // Existing sticky events of all subclasses of eventType have to be considered.
            // Note: Iterating over all events may be inefficient with lots of sticky events,
            // thus data structure should be changed to allow a more efficient lookup
            // (e.g. an additional map storing sub classes of super classes: Class -> List<Class>).
            Set<Map.Entry<Class<?>, Object>> entries = stickyEvents.entrySet();
            for (Map.Entry<Class<?>, Object> entry : entries) {
                Class<?> candidateEventType = entry.getKey();
                if (eventType.isAssignableFrom(candidateEventType)) {
                    Object stickyEvent = entry.getValue();
                    checkPostStickyEventToSubscription(newSubscription, stickyEvent);
                }
            }
        } else {
            Object stickyEvent = stickyEvents.get(eventType);
            checkPostStickyEventToSubscription(newSubscription, stickyEvent);
        }
    }
}
```
以上是EventBus的整个注册流程，有几个类值得注意我们这里先简单说一下。
* SubscriberMethodFinder 通过类名寻找订阅方法
* SubscriberMethod 用于存储订阅方法的实体类
* Subscription 订阅实体类，主要是激活订阅方法

流程描述：
1. 通过**SubscriberMethodFinder#findSubscriberMethods**找到当前类声明的订阅方法。
2. 然后通过**Subscription**激活并使用全局变量**EventBus#subscriptionsByEventType**缓存,订阅方法准备调用。如果是粘性（sticky）事件，则直接触发checkPostStickyEventToSubscription。
3. 将事件保存到**EventBus#typesBySubscriber**缓存
#### EventBus注销流程
源码如下：
```
// Unregisters the given subscriber from all event classes.
public synchronized void unregister(Object subscriber) {
    //1、获取当前类的已经注册且状态是激活的事件
    List<Class<?>> subscribedTypes = typesBySubscriber.get(subscriber);
    if (subscribedTypes != null) {
        for (Class<?> eventType : subscribedTypes) {
            //2、冻结激活的事件
            unsubscribeByEventType(subscriber, eventType);
        }
        //3、移除
        typesBySubscriber.remove(subscriber);
    } else {
        logger.log(Level.WARNING, "Subscriber to unregister was not registered before: " + subscriber.getClass());
    }
}
// Only updates subscriptionsByEventType, not typesBySubscriber! Caller must update typesBySubscriber.
private void unsubscribeByEventType(Object subscriber, Class<?> eventType) {
    List<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
    if (subscriptions != null) {
        int size = subscriptions.size();
        for (int i = 0; i < size; i++) {
            Subscription subscription = subscriptions.get(i);
            if (subscription.subscriber == subscriber) {
                //设置激活状态为冻结
                subscription.active = false;
                //移除
                subscriptions.remove(i);
                i--;
                size--;
            }
        }
    }
}
```
流程描述：
1. 获取当前类的已经注册且状态是激活的事件EventBus#typesBySubscriber
2. 将获取到的事件冻结


#### 发送事件流程
源码
```
public void post(Object event) {
   PostingThreadState postingState = currentPostingThreadState.get();
   List<Object> eventQueue = postingState.eventQueue;
   eventQueue.add(event);

   if (!postingState.isPosting) {
       postingState.isMainThread = isMainThread();
       postingState.isPosting = true;
       if (postingState.canceled) {
           throw new EventBusException("Internal error. Abort state was not reset");
       }
       try {
           while (!eventQueue.isEmpty()) {
               postSingleEvent(eventQueue.remove(0), postingState);
           }
       } finally {
           postingState.isPosting = false;
           postingState.isMainThread = false;
       }
   }
}

private void postSingleEvent(Object event, PostingThreadState postingState) throws Error {
    Class<?> eventClass = event.getClass();
    boolean subscriptionFound = false;
    if (eventInheritance) {
        List<Class<?>> eventTypes = lookupAllEventTypes(eventClass);
        int countTypes = eventTypes.size();
        for (int h = 0; h < countTypes; h++) {
            Class<?> clazz = eventTypes.get(h);
            subscriptionFound |= postSingleEventForEventType(event, postingState, clazz);
        }
    } else {
        subscriptionFound = postSingleEventForEventType(event, postingState, eventClass);
    }
    if (!subscriptionFound) {
        if (logNoSubscriberMessages) {
            logger.log(Level.FINE, "No subscribers registered for event " + eventClass);
        }
        if (sendNoSubscriberEvent && eventClass != NoSubscriberEvent.class &&
                eventClass != SubscriberExceptionEvent.class) {
            post(new NoSubscriberEvent(this, event));
        }
    }
}

private boolean postSingleEventForEventType(Object event, PostingThreadState postingState, Class<?> eventClass) {
    CopyOnWriteArrayList<Subscription> subscriptions;
    synchronized (this) {
        subscriptions = subscriptionsByEventType.get(eventClass);
    }
    if (subscriptions != null && !subscriptions.isEmpty()) {
        for (Subscription subscription : subscriptions) {
            postingState.event = event;
            postingState.subscription = subscription;
            boolean aborted = false;
            try {
                postToSubscription(subscription, event, postingState.isMainThread);
                aborted = postingState.canceled;
            } finally {
                postingState.event = null;
                postingState.subscription = null;
                postingState.canceled = false;
            }
            if (aborted) {
                break;
            }
        }
        return true;
    }
    return false;
}
private void postToSubscription(Subscription subscription, Object event, boolean isMainThread) {
    switch (subscription.subscriberMethod.threadMode) {
        case POSTING:
            invokeSubscriber(subscription, event);
            break;
        case MAIN:
            if (isMainThread) {
                invokeSubscriber(subscription, event);
            } else {
                mainThreadPoster.enqueue(subscription, event);
            }
            break;
        case MAIN_ORDERED:
            if (mainThreadPoster != null) {
                mainThreadPoster.enqueue(subscription, event);
            } else {
                // temporary: technically not correct as poster not decoupled from subscriber
                invokeSubscriber(subscription, event);
            }
            break;
        case BACKGROUND:
            if (isMainThread) {
                backgroundPoster.enqueue(subscription, event);
            } else {
                invokeSubscriber(subscription, event);
            }
            break;
        case ASYNC:
            asyncPoster.enqueue(subscription, event);
            break;
        default:
            throw new IllegalStateException("Unknown thread mode: " + subscription.subscriberMethod.threadMode);
    }
}

```
