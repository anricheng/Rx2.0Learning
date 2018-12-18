# Rx2.0Learning
## 1. subscribe 的几个重载的方法：
* subscribe(onNext);
* subscribe(onNext,onError)
* subscribe(onNext,onError,onComplete)
* subscribe(onNext,onError,onComplete,onSubscribe)

**完整的subscribe方法如下所示：**

```

      Observable.just("hello world")
                    .subscribe(new Consumer<String>() {
                        @Override
                        public void accept(String s) throws Exception {
                            System.out.println(s);
                        }
                    }, new Consumer<Throwable>() {
                        @Override
                        public void accept(Throwable throwable) throws Exception {
                            System.out.println(throwable.getMessage());
                        }
                    }, new Action() {
                        @Override
                        public void run() throws Exception {
                            System.out.println("complete");
                        }
                    }, new Consumer<Disposable>() {
                        @Override
                        public void accept(Disposable disposable) throws Exception {
                            System.out.println("onSubscribe");
                        }
                    });
                    
```

***The Result:***


------------

onSubscribe

hello world

onComplete

-----------


### Consumer 与 Action的区别如下：

    1. Action没有参数类型
    2. Consumer 有单一的参数

    
### 注意事项：
1. 在Rxjava2 中不在支持subscriber,必须使用observer;
2. Observable,Observer,subscribe 三者缺一不可，只有使用了subscribe方法，Observable才会开始发送数据；

## 2.几种Observable的比较

| 类型 | 描述 |
| --- | --- |
| Observable | 能够发送0或n个数据，并以成功或者失败事件终止 |
| Flowable | 与Observable 功能一样，增加对背压的支持，可以控制数据源发射数据的速度 |
| Single | 只发送单个数据或者错误事件，非常适合作为网络请求的数据源 |
| Completable | 不发送任何的数据，只发送成功或者失败，可以当做Rx当中的Runnable |
| Maybe | 能够发送0或者1个数据，要么成功，要么失败，类似于Optional |


## 3.do操作符：
它是对**Observable**的生命周期的各个阶段的一系列回调的监听，如下代码包含了对Observable的完整的生命周期：

```
 Observable.just("hello world")
                .doOnNext(new Consumer<String>() {
                    @Override
                    public void accept(String s) throws Exception {
                        System.out.println("doOnNext");
                    }
                }).doAfterNext(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                System.out.println("doAfterNext");
            }
        }).doOnComplete(new Action() {
            @Override
            public void run() throws Exception {
                System.out.println("doOnComplete");
            }
        }).doOnSubscribe(new Consumer<Disposable>() {
            @Override
            public void accept(Disposable disposable) throws Exception {
                System.out.println("doOnSubscribe");
            }
        }).doAfterTerminate(new Action() {
            @Override
            public void run() throws Exception {
                System.out.println("doAfterTerminate");
            }
        }).doFinally(new Action() {
            @Override
            public void run() throws Exception {
                System.out.println("doFinally");
            }
        }).doOnEach(new Consumer<Notification<String>>() {
            @Override
            public void accept(Notification<String> stringNotification) throws Exception {
                System.out.println(stringNotification.isOnComplete()?"onComplete":"onEach");
            }
        }).doOnLifecycle(new Consumer<Disposable>() {
            @Override
            public void accept(Disposable disposable) throws Exception {
                System.out.println("doOnLifeCycle:" + disposable.isDisposed());
                //这里也可以进行退订阅;
                //disposable.dispose();
            }
        }, new Action() {
            @Override
            public void run() throws Exception {
                System.out.println("doOnLifeCycleAction");
            }
        }).subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                System.out.println("收到的消息是："+s);
            }
        });
        
```

-------

**Result:**

doOnSubscribe
doOnLifeCycle:false
doOnNext
doOnEach
收到的消息是：hello world
doAfterNext
doOnComplete
doOnEach-onComplete
doFinally
doAfterTerminate

-------

### do操作符的用途如下：


| 操作符 | 用途 |
| --- | --- |
| doOnSubscribe | 一旦订阅立马被调用，也就是最先调用的生命周期 |
| doOnLifeCycle | 可以在订阅以后,可以设置是否取消 |
| doOnNext | 在OnNext（）之前进行调用 |
| doOnEach | 发射的任何数据都会进行调用，不仅是onNext还包括OnError以及OnComplete |
| doAfterOnNext | 在onNext()之后进行调用 |
| doOnComplete | 在onComplete()之后进行调用 |
| doFinally | 在Observable终止后进行调用 |
| doAfterTerminate | 在onError 或者onComplete之后进行调用 |









