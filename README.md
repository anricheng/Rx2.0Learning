# Rx2.0Learning
## subscribe 的几个重载的方法：
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


