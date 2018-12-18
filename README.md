# Rx2.0Learning
##1. subscribe 的几个重载的方法：
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

##2.几种Observable的比较
| 类型 | 描述 |
| --- | --- |
| Observable | 能够发送0或n个数据，并以成功或者失败事件终止 |
| Flowable | 与Observable 功能一样，增加对背压的支持，可以控制数据源发射数据的速度 |
| Single | 只发送单个数据或者错误事件，非常适合作为网络请求的数据源 |
| Completable | 不发送任何的数据，只发送成功或者失败，可以当做Rx当中的Runnable |
| Maybe | 能够发送0或者1个数据，要么成功，要么失败，类似于Optional |




