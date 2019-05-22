### [给初学者的RxJava2.0教程(一)](https://juejin.im/post/5848d96761ff4b0058c9d3dc)

### [给初学者的RxJava2.0教程(二)](https://juejin.im/post/5848dd11b123db0066030123)

### [给初学者的RxJava2.0教程(三)](https://juejin.im/post/5848dd3eac502e00691385c5)

1.map操作符
变换操作符，作用是暴露一个方法让上游的事件能够经过加工处理再向下游发布，这个加工处理的过程是我们控制的，比如我们可以把一个Integer类型的值转变成一个String类型的值向下游发布。

```java
    Observable.create(new ObservableOnSubscribe<Integer>() {
            @Override
            public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
                emitter.onNext(1);
                emitter.onNext(2);
                emitter.onNext(3);
            }
        }).map(new Function<Integer, String>() {
            @Override
            public String apply(Integer integer) throws Exception {
                return "This is result " + integer;
            }
        }).subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                Log.d(TAG, s);
            }
        });
```

2.flatMap操作符
也是一个变换操作符，但是他的变换能力跟map有所不同，map能够把一个事件加工处理变成另一个事件(本质上还是事件)，但是flatMap却是能够把一个事件转变成Observable(即事件发送源)，也就是说一个事件可能会产生更多个事件，最后把这些事件整合在一块形成一个新的Observable向下游发送，类似于一个水管的水流出来后经过了多个水管，然后再汇成一个水管，但是最后形成的水管中的事件顺序是未知的。

多层for循环嵌套应该也可以使用flatMap来实现。

```java
Observable.create(new ObservableOnSubscribe<Integer>() {
            @Override
            public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
                emitter.onNext(1);
                emitter.onNext(2);
                emitter.onNext(3);
            }
        }).flatMap(new Function<Integer, ObservableSource<String>>() {
            @Override
            public ObservableSource<String> apply(Integer integer) throws Exception {
                final List<String> list = new ArrayList<>();
                for (int i = 0; i < 3; i++) {
                    list.add("I am value " + integer);
                }
                return Observable.fromIterable(list).delay(10,TimeUnit.MILLISECONDS);
            }
        }).subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                Log.d(TAG, s);
            }
        });
```

3.concatMap操作符
flatMap操作符会导致最后发出来的事件顺序是乱序的，如果我们希望事件是按照顺序发送出来的，我们就可以使用concatMap操作符，该操作符与flatMap的区别就在于事件的顺序，其他相同。

4.doOnNext操作符
该操作符提供了一个对事件处理的方法，没有返回值，也就是说事件会流向他，但是他不会对事件产生什么影响(如果事件是同一个对象可能会产生影响，目前不确定，需要实际验证)。

总结：
flatMap和concatMap操作符能够解决接口嵌套的问题，如果我们使用Retrofit+RxJava，那么接口的嵌套也就变成了第一个接口请求(Observable)回来数据后作为一个事件再转换成第二个接口请求(Observable)，再把第二个接口请求回来的数据作为事件向下发送，当然了，我们仍然可以在第一个接口请求回来数据后针对该事件进行一些其他操作再进行第二个接口请求。

### [给初学者的 RxJava2.0 教程 (四)](https://juejin.im/post/584a6dd9128fe100589bf29d)

1.zip操作符
zip操作符和flapMap操作符有点点相反的味道，flapMap操作符是把一个水管(Observable)中的事件变成多个水管事件，然后再把多个水管事件合成一个水管中的事件发送出去，zip操作符就是把多个水管中的事件合在一个水管中发送出去，总是等多个水管中都有相对应的事件时才会发送，比如有三个水管，1号水管中有3个事件，2号水管中有个3个事件，3号水管中有2个事件，那么1号和2号水管中的第三个事件就不会被发送，因为缺少3号水管对应的事件来组合。

当多个水管是在同一个线程中执行的时候，总是一个水管中的事件发送完之后再发送下一个水管中的事件，当发送到最后一个水管中的事件时才会组合这些事件继续向下发送，因为同一个线程中的代码必须按照严格的顺序来执行。

当多个水管分别处于不同的线程时，多个水管之间事件的发送顺序就无法保证了，但是总是在多个水管分别发送一个事件之后才开始组合这些事件继续向下发送。

总结：
1.其实这里水管的意义就是一个个的Observable，事件都是通过Observable发送出来的；
2.zip操作符可以用于组合多个接口的请求，比如：某个页面的数据需要两个接口的数据都回来之后才能展示，但是我们不知道哪个接口的数据最后回来，那么zip操作符会是一个不错的选择，能够完美的解决这种问题。

### [给初学者的RxJava2.0教程(五)](https://juejin.im/post/584f76f48e450a006ad12ebf)

Backpressure的概念跟线程的同步异步有关系，上下游在同一个线程，那么上游事件的发送是必须等到下游事件被消费之后才能继续发送，这种情况不会导致上游事件发送太快事件发生积压的情况，但是如果上下游不在同一个线程，那么这种情况很可能发生，因为下游的线程根本不知道上游的线程当前在干啥，那么在RxJava中，在上下游处于异步时提供了一种“水缸”的机制，也就是用一个队列来暂时保存上游发送出来的事件，下游再在队列中取事件，这样上游如果发送事件太快，事件就会积压在队列中等着下游去取，如果下游取得太慢，那么队列很可能被干爆，出现OOM异常。

### [给初学者的RxJava2.0教程(六)](https://juejin.im/post/58510ec3ac502e0067cfa4a2)

1.filter操作符
过滤操作符，用于对上游发送出来的事件进行过滤，把符合条件的事件继续向下发送。

2.sample操作符
取样操作符，按照一定的时间频率取上游发送下来的事件发送给下游。

3.平衡上下游流速不均衡问题的解决方案有两种：

- 从数量上来减少发送到下游的事件，或者说发送到水缸中的事件；
- 从速度上来减慢事件的发送，让下游有足够的时间来消费水缸中事件。

总结：上下游事件流速不均衡的问题一般只会出现在线程切换的案例中，如果事件的产生和消费都是在同一个线程，那么就不存在事件积压的问题。

### [给初学者的RxJava2.0教程(七)](https://juejin.im/post/5857a5e48e450a006c752701)

1.Flowable和Subscriber
Flowable和Subscriber可以用来解决上下游流速不一致问题。

```java
Flowable<Integer> upstream = Flowable.create(new FlowableOnSubscribe<Integer>() {
            @Override
            public void subscribe(FlowableEmitter<Integer> emitter) throws Exception {
                Log.d(TAG, "emit 1");
                emitter.onNext(1);
                Log.d(TAG, "emit 2");
                emitter.onNext(2);
                Log.d(TAG, "emit 3");
                emitter.onNext(3);
                Log.d(TAG, "emit complete");
                emitter.onComplete();
            }
        }, BackpressureStrategy.ERROR); //增加了一个参数(背压策略)

        Subscriber<Integer> downstream = new Subscriber<Integer>() {

            @Override
            public void onSubscribe(Subscription s) {
                Log.d(TAG, "onSubscribe");
                s.request(Long.MAX_VALUE);  //注意这句代码
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "onNext: " + integer);
            }

            @Override
            public void onError(Throwable t) {
                 Log.w(TAG, "onError: ", t);
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "onComplete");
            }
        };

        upstream.subscribe(downstream);
```

这种模式中的request()方法代表着事件消费的一种能力，比如上面案例中的`s.request(Long.MAX_VALUE)`,就是下游的消费者告诉上游我可以消费Long.MAX_VALUE个事件，你发吧，然后上游就会把小于等于这么多个事件发送给下游，这是在上下游同步的情况下，上游必须要知道下游的消费能力才能够发送事件，如果这里不调用request()方法，那么会抛出MissingBackpressureException异常，因为上游不知道下游的消费能力，它不可能一直等待，如果这是主线程的话就会导致ANR了，所以干脆抛出异常以示提醒。

如果上下游不在同一个线程，比如下面这样：

```java
Flowable.create(new FlowableOnSubscribe<Integer>() {
            @Override
            public void subscribe(FlowableEmitter<Integer> emitter) throws Exception {
                Log.d(TAG, "emit 1");
                emitter.onNext(1);
                Log.d(TAG, "emit 2");
                emitter.onNext(2);
                Log.d(TAG, "emit 3");
                emitter.onNext(3);
                Log.d(TAG, "emit complete");
                emitter.onComplete();
            }
        }, BackpressureStrategy.ERROR).subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Subscriber<Integer>() {

                    @Override
                    public void onSubscribe(Subscription s) {
                        Log.d(TAG, "onSubscribe");
                        mSubscription = s;
                    }

                    @Override
                    public void onNext(Integer integer) {
                        Log.d(TAG, "onNext: " + integer);
                    }

                    @Override
                    public void onError(Throwable t) {
                        Log.w(TAG, "onError: ", t);
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "onComplete");
                    }
                });
```

因为在上游和下游之间有一个水缸的存在，那么上游不管你下游有没有消费能力，我都会发送我的事件，然后存储在水缸中，下游如果需要的话就去取就完了，所以这时候不调用request()方法程序也能正常运行，上游一样正常发送事件，只不过下游收不到事件而已。

但是呢，在上面这段代码案例中水缸的大小是有限制的，即128个事件大小，如果上游产生了超过128个事件且都没有被下游消费，那么也会抛出MissingBackpressureException异常。

### [给初学者的 RxJava2.0 教程 (八)](https://juejin.im/post/585b8f741b69e600560602d3)

### [给初学者的 RxJava2.0 教程 (九)](https://juejin.im/post/58807ef92f301e00697f6ad8)

### [Android RxJava 背压策略：图文 + 实例 全面解析](https://juejin.im/post/5a5e9bd25188256922631243)

### [如何形象的描述反应式编程中的背压(Backpressure)机制?](https://www.zhihu.com/question/49618581/answer/237078934)

