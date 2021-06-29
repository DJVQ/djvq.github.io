---
title : "RxJava2.0使用心得"
tags : ["Android","学习"]
date : 2021-05-24
keywords : ["观察者模式","异步调用"]
---

## 关于RxJava

### **作用**

#### 1. 目的: 
异步调用

#### 2. 优势: 
简便,逻辑代码清晰,可读性强

#### 3. 使用情况
目前在Android项目中使用过,主要用于搜索功能，异步处理网络任务。

---
### **关于观察者模式**

> **Observable：** 被观察者

> **Observer：** 观察者，可接受Observable发送的数据

> **subscribe：** 订阅，观察者与被观察者通过subscribe()建立订阅关系

---

### **关于RxJava的观察者模式**

> **Scheduler:** 调度器，多线程时分配线程

使用举例：
```java
    //新建线程池
    private ExecutorService executorService;
    //新建调度器
    private Scheduler scheduler;
    //新建含threadsNum个线程的线程池
    executorService = Executors.newFixedThreadPool(threadsNum);
    //设置调度器
    scheduler = Schedulers.from(executorService);
```

> **ObservableEmitter：** 发射器，有三种发射方法，分别为:
```java
    /**
     * Signal a normal value.
     * @param value the value to signal, not null
     */
    void onNext(@NonNull T value);

    /**
     * Signal a Throwable exception.
     * @param error the Throwable to signal, not null
     */
    void onError(@NonNull Throwable error);

    /**
     * Signal a completion.
     */
    void onComplete();
```
以上三种方法分别对应三种状态：正常执行、发生错误、执行完成。发射器主要用于被观察者发送消息给观察者执行。通常在被观察者中传递接口参数和接口回调，在观察者中定义接口实现。

> **Observable的创建：** 可使用Observable.create()方法创建，创建时需要定义ObservableEmitter，即指明何种情况发射何种状态。举个例子如下：
```java
    public static Observable<String> getChapterContent(String url, final ReadCrawler rc) {
        String charset = rc.getCharset();
        url = NetworkUtils.getAbsoluteURL(rc.getNameSpace(), url);
        String finalUrl = url;
        return Observable.create(emitter -> {
            try {
                emitter.onNext(rc.getContentFormHtml(OkHttpUtils.getHtml(finalUrl, charset)));
            } catch (Exception e) {
                e.printStackTrace();
                emitter.onError(e);
            }
            emitter.onComplete();
        });
    }
```
上面相关代码是获取某个电子书网站的某本小说的某个章节内容的一个函数，由于网络操作耗时较久所以选择异步调用，使用RxJava2.0的观察者模式，当获取到内容或获取内容失败时在UI界面进行不同的展示。

> **Observer的创建：** 通过new来创建接口Observer<T>并实现其内部方法，其中，实现的方法包含被观察者发送的三种状态以及onSubscribe方法，onSubscribe为退订方法，一般调用Disposable.dispose()方法取消观察者与被观察者的订阅关系，示例代码如下：
```java
        //观察者
        Observer<String> reader=new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
                mDisposable=d;
                Log.e(TAG,"onSubscribe");
            }

            @Override
            public void onNext(String value) {
                if ("2".equals(value)){
                    mDisposable.dispose();
                    return;
                }
                Log.e(TAG,"onNext:"+value);
            }

            @Override
            public void onError(Throwable e) {
                Log.e(TAG,"onError="+e.getMessage());
            }

            @Override
            public void onComplete() {
                Log.e(TAG,"onComplete()");
            }
        };
```

> **订阅关系的创建：** 
```java
                observable//Observable对象
                    .subscribeOn(scheduler)//事件执行在scheduler控制的线程
                    .observeOn(AndroidSchedulers.mainThread())//事件回调在主线程
                    .subscribe(new Observer...);//订阅Observer对象
```

在上面的代码中，订阅关系的创建仅使用一行代码就能够解决，这体现了RxJava的简洁性。其中subscribeOn方法的参数可以为线程也可以为调度器，当为调度器时即可实现异步。