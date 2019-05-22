#### 1.RxJava1.x和RxJava2.x不能共存
a.在gradle中同时引用RxJava1.x和RxJava2.x会导致编译异常；
b.app中引用了RxJava2.x，引用的第三方库中如果引用了RxJava1.x会导致编译异常；

建议：项目如果使用RxJava，那么版本统一使用RxJava1.x或者RxJava2.x

#### 2.RxAndroid和RxJava
如果项目使用了RxAndroid，在引用依赖的时候不能省去对相应版本RxJava的依赖。

官方解释：
> Because RxAndroid releases are few and far between, it is recommended you also
explicitly depend on RxJava's latest version for bug fixes and new features.
因为RxAndroid发布版本较少，所以也推荐你明确依赖RxJava的最新版本来修复错误和新功能。

#### 3.新增Flowable
RxJava1.x对于backpressure支持不良好，在RxJava2.x中新增一种类型Flowable来支持backpressure。

有关backpressure概念详见：
[关于RxJava最友好的文章——背压（Backpressure）](https://juejin.im/post/582d413c8ac24700619cceed)
[给初学者的RxJava2.0教程(五)：背压（Backpressure）](https://mp.weixin.qq.com/s?__biz=MzIwMzYwMTk1NA==&mid=2247484422&idx=1&sn=c1dfb14d9221d8919c3e7188b47c0a70&chksm=96cda54ba1ba2c5dec956a56c6edb003867e0d4c94fb8758bbe73b551dc9d6ded5ceca1e7969#rd)

#### 4.ActionN和FuncN改名
Action0改名为Action，Action1改名为Consumer，Action2改名为BiConsumer，而Action3~Action9都不再使用了，ActionN使用Consumer<Object[]>代替(泛型替换原来的多参)。

Func改名为Function，Func2改名为BiFunction，Func3~Func9改名为Function3~Function9，FuncN改名为Function<Object[],R>.

#### 5.Observable.onSubscribe类改为了ObservableOnSubscribe类
RxJava1.x写法：
```
Observable.create(new Observable.OnSubscribe<String>() {
            
            @Override
            public void call(Subscriber<? super String> subscriber) {
                subscriber.onNext("hello");
            }

        }).subscribe(new Action1<String>() {

            @Override
            public void call(String s) {
                System.out.println(s);
            }
        });
```

RxJava2.x写法
```
 Observable.create(new ObservableOnSubscribe<String>() {

            @Override
            public void subscribe(ObservableEmitter<String> e) throws Exception {
                e.onNext("hello");
            }

        }).subscribe(new Consumer<String>() {

            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });
```

#### 6.ObservableOnSubscribe中使用ObservableEmitter发送数据
在RxJava1.x中使用的Subscriber发送事件，RxJava2.x使用ObservableEmitter发送事件，可以把ObservableEmitter理解为发射器，观察者的事件都是由他发送出去的。

ObservableEmitter可以发送三种类型的事件，onNext发送next事件，onComplete发送complete事件，onError发送error事件。

注意：onComplete发送完complete事件后，Consumer不会再接收到接下来next的事件。

#### 7.Observable.Transformer类改为了ObservableTransformer
RxJava1.x写法：
```
/**
* 跟compose()配合使用,比如ObservableUtils.wrap(obj).compose(toMain())
* @param <T>
* @return
*/
public static <T> Observable.Transformer<T, T> toMain() {
    return new Observable.Transformer<T, T>() {
        @Override
        public Observable<T> call(Observable<T> tObservable) {
            return tObservable
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread());
        
    };
}
```
RxJava2.x写法：
```
/**
* 跟compose()配合使用,比如ObservableUtils.wrap(obj).compose(toMain())
* @param <T>
* @return
*/
public static <T> ObservableTransformer<T, T> toMain() {
    return new ObservableTransformer<T, T>() {
        @Override
        public ObservableSource<T> apply(Observable<T> upstream) {
            return upstream.subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread());
        }
    };
}
```

由于新增了Flowable类型，所以也增加了FlowableTransformer

#### 8.Subscription改名为Disposable
由于RxJava2.x中已经有org.reactivestreams.Subscription类了，为了不使命名冲突把原来的Subscription改为了Disposable。

Disposable可以翻译为一次性的，即用完就要销毁。

#### 9.first用法改变
**暂不知具体用法**

#### 10.toBlocking().y被blockingY()取代
**暂不知具体用法**

#### 11.PublishSubject
**暂不知具体用法**

#### 参考
[RxJava1 升级到 RxJava2 所踩过的坑](https://www.jianshu.com/p/6d644ca1678f)


