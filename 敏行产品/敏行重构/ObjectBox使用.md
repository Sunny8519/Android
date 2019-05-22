##### 实体类

通过`@Entity`和`@Id`定义一个实体类(这两个注解是必须的)，比如：

```java
// User.java
@Entity
public class User {
    @Id public long id;
    public String name;
}
```

##### 对象id的分配

实体对象id的分配默认是ObjectBox指定的，对于每个新对象，ObjectBox将会分配一个未使用的id，这个id的值将高于当前已存储的对象的最大的id值。

默认情况下只有ObjectBox才能分配id，如果你尝试通过手动指定id的方式存储一个比当前数据库中最大id还要大的对象，那么ObjectBox会抛出异常。

实体类必须有一个long类型的使用`@Id`注解的字段，可以用可空类型的Long类型字段，但是不推荐使用。

如果需要使用其他类型的id，比如服务器返回的字符串UID，可以把UID建模成普通字段，使用的时候通过下面这种方式去获取对应的实体对象：

```java
@Entity class StringIdEntity {
    @Id public long id;
    public String uid;
}

StringIdEntity entity = box.query()
    .equal(StringIdEntity_.uid, uid)
    .build().findUnique()
```

当存储一个没有指定id的实体对象时，默认情况下ObjectBox会为实体对象指定id，当然你也可以自己指定id，方法如下：

```java
@Id(assignable = true)
long id;
```

这样就可以为存储的实体对象设置自己指定的id了，**如果给id设置了0，那么ObjectBox会为实体对象分配新的id**。

不过，对于id，通常情况下我们都不会手动指定，因为手动指定id会中断ObjectBox的自动状态检测(即将要存储的实体对象与已经持久化的实体对象)，注：这里个人理解应该是ObjectBox要检测id的连续性，手动指定id的话会让ObjectBox跳过这种检测。这块需要详细了解：[updating relations](https://docs.objectbox.io/relations#updating-relations)

ObjectBox会把id为0或者null(如果id类型为Long)的实体对象认为是新的要存储到数据库中的实体对象

##### 实体类数据的访问

ObjectBox会访问我们定义的实体类的字段数据，比如ObjectBox需要生成Cursor对象供我们使用，我们可以通过Cursor直接拿到数据，为了ObjectBox能访问到这些字段数据，我们可以有两种做法：
- 让实体类字段的可见性至少是包可见，不可为private；
- 如果为private，就需要提供getter方法。

为了提升ObjectBox构造实体对象的性能，你可能需要提供一个全字段的构造函数。

##### @NameInDb注解

```java
@Entity
public class User {
    
    @NameInDb("USERNAME")
    private String name;
    
    @Transient
    private int tempUsageCount;
    
    ...
}
```

`@NameInDb`注解定义了该属性在数据库级别的字段名，因此添加了该注解的实体字段我们都是可以重命名而不会影响到数据库中的字段名。

##### @Transient注解

使用@Transient注解的属性不会被持久化。

##### @Index注解

@Index注解可以为指定的数据库字段创建索引，当查询该属性时会提升性能。
```java
@Index
private String name;
```

注意：byte[],float和double类型目前不支持@Index注解

数据库中的索引目的是为了更快速的查询。作为类比，在Java语言中，我们在List中存储对象，比如使用List<Person>存储人员信息，这时候你需要搜索具有特定姓名的人，你需要遍历List，这是一个O(n)操作，对于越来越的Person不能很好的扩展，为了更具有伸缩性，你可以引入第二种数据结构Map<String,Person>，并把姓名当做key，这样查找一个人的信息用时就是O(1)，这样做的缺点是它会占用更多的内存空间，但是查询时间缩短了，典型的空间换时间的做法。对比数据库索引，占用更多的则是磁盘空间。

ObjectBox2.0引入了索引类型，在此之前，每个索引都只能使用属性值进行索引查找，ObjectBox2.0之后也可以通过hash去构建索引，因为使用属性值构建索引会占用更多的空间，在ObjectBox2.0之后，索引的类型默认从基于属性值的索引变为了基于hash的索引。

你可以通过指定索引类型来指定ObjectBox为String属性使用基于值的索引：
```java
@Index(type = IndexType.VALUE)
private String name;
```

ObjectBox支持以下索引类型：
- Not specified or DEFAULT:使用基于属性类型的最佳索引(String为HASH，其他为VALUE);
- VALUE:使用属性值来构建索引，对于字符串类型的属性，这可能需要比基于散列的索引更多的存储空间;
- HASH:使用属性值的32位哈希来构建索引，偶尔可能会发生碰撞，通常选择HASH比HASH64更好，因为HASH需要更少的存储空间;
- HASH64:使用属性值的长哈希来构建索引，需要比HASH更多的存储空间，因此大多数情况下不会使用HASH64.

注意：基于哈希的索引的限制：哈希适用于相等性检查，但不适用于"starts with"开头的类型条件。如果经常使用它们，建议使用基于值的索引。

##### @Unique注解

使用`@Unique`注解的属性表示属性值是唯一的，如果

```java
@Unique
private String name;
```



#### [数据模型更新](https://docs.objectbox.io/advanced/data-model-updates)

大多数情况下ObjectBox会自动管理数据模型(我们定义的实体类)，当你添加一个或者删除一个实体或者实体类中的属性，ObjectBox自己会处理好这些更改而不需要我们做什么。但是对于一些其他的更改，比如重命名实体类或者属性/更改属性的类型，ObjectBox就需要更明确的信息去做一些处理，唯一ID和@Uid注解就可以提供这些信息。

##### UIDs

ObjectBox对实体类和属性的追踪是通过给他们分配唯一Id(UIDs)来做到的，这些UIDs存储在文件"objectbox-models/default.json"文件中，所以一般情况下我们需要把该文件跟随项目加入到版本控制系统中(比如git)，关于UIDs更深入的解释见[in-depth documentation on UIDs and concepts](https://docs.objectbox.io/advanced/meta-model-ids-and-uids).

##### 关于重命名实体类和属性名

为什么我们需要`@Uid`注解呢?如果只是重命名实体类，对于ObjectBox来说它只知道旧的实体类消失了，新的实体类是可用的，因此使用`@Uid`注解有两种原因：
- 重命名实体类，就相当于删除了旧的实体类，然后添加了一个新的实体类，对于ObjectBox来说，删除的实体类所对应的旧的数据会被丢弃。
- 但是我们仅仅只是给实体类重命名了，旧的数据不应该被无缘无故的丢弃。

因此，要告诉ObjectBox是重命名了实体类而不是丢弃旧的实体类和数据，我们需要确保ObjectBox知道重命名前后是同一个实体类而不是新实体，通过`@Uid`注解就是将内部的UID附加到实体类上，UID在重命名前后是不会变得，所以即使我们重命名了实体类，ObjectBox也会认为这是同一个实体类。

对于属性也是同样的道理。

##### 重命名实体类或属性名的步骤

1.为你想要重命名的实体类或属性添加`@Uid`注解；
```java
@Entity
@Uid
public class MyName { ... }
```
2.构建项目。构建将会失败，并且显示一条错误信息，该信息包含了实体类或者属性当前的UID：
```java
error: [ObjectBox] UID operations for entity "MyName": 
  [Rename] apply the current UID using @Uid(6645479796472661392L) -
  [Change/reset] apply a new UID using @Uid(4385203238808477712L)
```

3.将错误信息中重命名部分的UID值应用在想要重命名的实体类或者属性上
```java
@Entity
@Uid(6645479796472661392L)
public class MyName { ... }
```

4.最后我们就可以重命名实体类或者属性名了
```java
@Entity
@Uid(6645479796472661392L)
public class MyNewName { ... }
```

现在你就可以使用重命名后的实体类或者属性了，并且旧的数据仍然在数据库中。

注意：除了上面所说的方法外，你也可以在ObjectBox的default.json文件中找到实体类或者属性的UID，然后把UID值和`@Uid`注解一起添加到想要重命名的实体类或者属性上。

##### 更改属性类型

如果你想要更改属性的类型，你必须要告知ObjectBox在内部创建一个新的属性，这是因为ObjectBox不会迁移你的数据，因此简单的更改属性类型可能会导致数据丢失(不使用`@Uid`注解)。

如何做才能让数据不丢失呢？按下面步骤操作就行了：
1.给你想要更改类型的属性添加`@Uid`注解：
```java
@Uid
String year;
```

2.构建项目，构建将会失败，并且提示你一条错误信息，信息会提供给你一最新创建的UID值：
```java
error: [ObjectBox] UID operations for property "MyEntity.year": 
  [Rename] apply the current UID using @Uid(6707341922395832766L) -
  [Change/reset] apply a new UID using @Uid(9204131405652381067L)
```

3.使用[Chage/reset]部分的UID值加在你想要更改类型的属性上：
```java
@Uid(9204131405652381067L)
int year;
```

现在你就可以像使用新属性一样使用更改了类型后的属性了，并且旧的数据也不会丢失。

#### [Model file](https://docs.objectbox.io/getting-started#model-file)

ObjectBox在默认情况下会在`app/objectbox-models/`文件夹下生成`default.json`文件，每次更改实体或者对ObjectBox进行一些内部更改，此文件都会更改。

一般情况下该文件都会被提交到版本控制系统中，每次更改都需要提交。

`default.json`文件会跟踪分配给实体和属性的唯一ID，这可确保实体或者属性发生更改时，可以平滑的升级旧版本的数据库。而且这也可以保证当重命名实体或者属性时数据不丢失，或者当两个开发者同时做了一些更改，`default.json`能够解决冲突。

#### 核心类([Core Classes](https://docs.objectbox.io/getting-started#core-classes))

以下核心类是ObjectBox的基本接口：

MyObjectBox:基于实体类生成，提供了一个构建器(builder)来为应用程序设置BoxStore。

BoxStore:使用ObjectBox的入口点。BoxStore管理着所有Boxes并且是你与数据库交互的最直接的接口。

Box:一个持久化和查询实体的盒子(Box)，对于每个实体，都有一个对应的盒子(Box)，Box由BoxStore提供。

#### ObjectBox初始化([Core Initialization](https://docs.objectbox.io/getting-started#core-initialization))

BoxStore对象的初始化使用MyObjectBox的构建器builder来构建，类似于下面这样的帮助类：
```java
public class ObjectBox {
    private static BoxStore boxStore;

    public static void init(Context context) {
        boxStore = MyObjectBox.builder()
                .androidContext(context.getApplicationContext())
                .build();
    }

    public static BoxStore get() { return boxStore; }
}
```

初始化ObjectBox最好的时机是在app启动的时候，因此建议在`Application`中的`onCreate()`方法中进行初始化。
```java
public class ExampleApp extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        ObjectBox.init(this);
    }
}
```

现在你就可以在你的app中任何一个地方(通常是fragments,activities)获取BoxStore对象并访问特定的Box:
```java
notesBox = ObjectBox.get().boxFor(User.class);
```

拿到Box对象后，就可以向数据库中存储或检索数据了。

#### 基本的Box操作([Basic Box operations](https://docs.objectbox.io/getting-started#basic-box-operations))

Box对象可能是与我们打交道最多的类，如前所述，你可以通过`BoxStore.boxFor()`获取Box实例对象。Box实例可以使你访问特定类型的对象，比如有`User`和`Order`两个实体类，你需要两个Box实例分别交互：
```java
Box<User> userBox = boxStore.boxFor(User.class);
Box<Order> orderBox = boxStore.boxFor(Order.class);
```

以下是Box类提供的一些操作：
- put:插入一个新的或者更新一个已存在的具体相同id的对象，当插入并且存储完后新插入的对象会被分配一个ID;`put`支持放置多个对象，效率会更高。

- get,getAll:`get`可以获取指定id的对象，`getAll`获取盒子中所有对象。

- remove,removeAll:删除已存在盒子中的对象，`remove`也支持删除多个对象，效率也会更高。`removeAll`是删除盒子中的所有对象。

- count:返回盒子中存储的对象个数。

- query:构建查询以从盒子中返回符合特定条件的对象，详情见:[queries](https://docs.objectbox.io/queries)

#### 事务([Tansactions](https://docs.objectbox.io/getting-started#transactions))

ObjectBox提供了强大的事务处理能力，对于大多数应用来说，只要参考最基本的事务指南就足够了:
- 一个put操作是一个隐形事务；

- 如果可能，首选put批量的实体对象，比如在for循环中执行put操作，我们就要考虑是否可以把List中的数据批量通过put进Box中；

- 对于循环中的大量DB交互，请考虑使用显示事务，比如使用`runInTx()`

#### ObjectBoxLiveData的使用

ObjectBox附带了`ObjectBoxLiveData`，我们可以利用它来观察ObjectBox中数据的变化，从这一点来看，不管是使用性还是性能方面都可以和google的room orm数据库框架相比了，甚至更优秀。

使用示例：

```java
public class NoteViewModel extends ViewModel {
    
    private ObjectBoxLiveData<Note> noteLiveData;
    
    public ObjectBoxLiveData<Note> getNoteLiveData(Box<Note> notesBox) {
        if (noteLiveData == null) {
            // query all notes, sorted a-z by their text
            noteLiveData = new ObjectBoxLiveData<>(notesBox.query().order(Note_.text).build());
        }
        return noteLiveData;
    }
}
```

上面这个示例是把Box当做参数传给`getNoteLiveData()`方法的，这就意味着Box需要在ViewModel以外的地方生成出来，还有一种方式是NoteViewModel继承AndroidViewModel，然后通过`((App)getApplication()).getBoxStore().boxFor()`在ViewModel内部获取Box，但是这样不利于单元测试，因此建议使用上面示例的方法。

有了ViewModel之后，我们就可以在Activity或者Fragment中的onCreate()方法中实例化ViewModel了，并注册观察者，观察ObjectBox数据的变化：

```java
NoteViewModel model = ViewModelProviders.of(this).get(NoteViewModel.class);
model.getNoteLiveData(notesBox).observe(this, new Observer<List<Note>>() {
    @Override
    public void onChanged(@Nullable List<Note> notes) {
        notesAdapter.setNotes(notes);
    }
});
```

#### 查询([Queries](https://docs.objectbox.io/queries))

在ObjectBox查询方面有两个非常重要的类：`QueryBuilder`和`Query`，`QueryBuilder`主要职责就是指定查询的各种条件，条件指定完后，通过`QueryBuilder`实例化一个`Query`类，该类主要职责就是执行查询条件并返回查询结果。

**QueryBuilder**

`QueryBuilder`对象可以通过`Box.query()`实例化。

QueryBuilder提供了几种方法来定义实体属性的查询条件，指定查询某个属性，ObjectBox不使用字符串，而是使用在构建期间生成的元信息带下划线的类(比如User_)，实体类中的每个字段在元信息类中都有对象的静态字段(如User_firstName)，这样做的好处(猜测)：通过编译期检查来达到更安全的引用属性的目的，防止运行时错误，比如拼写错误等等。

示例：

```java
// 单条件查询:查询firstName为"Joe"的所有用户
List<User> joes = userBox.query().equal(User_.firstName, "Joe").build().find();

// 多条件查询:查询firstName为"Joe"开头的用户，这些用户出生于1970年以后，并且lastName以"O"开头
QueryBuilder<User> builder = userBox.query();
builder.equal(User_.firstName, "Joe")
        .greater(User_.yearOfBirth, 1970)
        .startsWith(User_.lastName, "O");
List<User> youngJoes = builder.build().find();
```

**值得我们注意的一些条件**

除了像`equal()`,`notEqual()`,`greater()`,`less()`这样的预期条件之外，还有以下一些条件也是我们经常用到的：
`isNull()`和`notNull`；
`between()`用于过滤给定两个值之间的值；
`in()`和`notIn()`可以过滤出给定数组中与之匹配的值；
`startsWith()`，`endsWith()`和`contains()`可以用于扩展字符串的过滤。

此外还有`and()`和`or()`来构建更复杂的条件组合。

`QueryBuilder`中的其他条件方法可以参考api文档(http://objectbox.io/files/objectbox-java/current/io/objectbox/query/QueryBuilder.html)

**有顺序的查询结果**

除了指定条件，我们还可以通过`order()`方法对返回的结果进行排序：

```java
userBox.query().equal(User_.firstName, "Joe")
    .order(User_.lastName) // in ascending order, ignoring case(按升序排列，忽略大小写)
    .find();
```

我们还可以通过标志位指定以降序排序，排序区分大小写或者专门处理空值。

```java
userBox.query().equal(User_.firstName, "Joe")
    .order(User_.lastName, QueryBuilder.DESCENDING | QueryBuilder.CASE_SENSITIVE)
    .find();
```

**Query**

Query实例是通过`QueryBuilder`的`build()`方法创建：

```java
Query<User> query = builder.build();
```

**查找对象**

```java
// return all entities matching the query(返回匹配到的所有查询结果)
List<User> joes = query.find();

// return only the first result or null if none(返回查询结果的第一个，如果没有结果返回null)
User joe = query.findFirst();

// return the only result or null if none, throw if more than one result(返回唯一的结果，没有结果返回null，有多个结果抛出异常)
User joe = query.findUnique();
```

**查询复用**




#### 数据浏览器([Data Browser](https://docs.objectbox.io/data-browser))

ObjectBox支持浏览器浏览数据库数据，具体设置如下：

```java
dependencies {
    debugImplementation "io.objectbox:objectbox-android-objectbrowser:$objectboxVersion"
    releaseImplementation "io.objectbox:objectbox-android:$objectboxVersion"
}

// apply the plugin after the dependencies block
apply plugin: 'io.objectbox'

// 权限
<!-- Required to provide the web interface -->
<uses-permission android:name="android.permission.INTERNET" />
<!-- Required to run keep-alive service when targeting API 28 or higher -->
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>

// 初始化
boxStore = MyObjectBox.builder().androidContext(this).build();
if (BuildConfig.DEBUG) {
    boolean started = new AndroidObjectBrowser(boxStore).start(this);
    Log.i("ObjectBrowser", "Started: " + started);
}
```

需要注意的是`apply plugin: 'io.objectbox'`需要在dependencies的后面，不然build会报错。

**浏览数据**
在android studio控制台过滤log(tag:ObjectBrowser)会有如下信息：

```java
I/ObjectBrowser: ObjectBrowser started: http://localhost:8090/index.html
I/ObjectBrowser: Command to forward ObjectBrowser to connected host: adb forward tcp:8090 tcp:8090
```

从这些信息上我们可以看到可以使用这个网址`http://localhost:8090/index.html`来访问ObjectBox数据库，但是在访问之前我们还要在终端上输入这个adb命令才行：`adb forward tcp:8090 tcp:8090`。

##### 参考

[ObjectBox官方文档](https://docs.objectbox.io/getting-started)

https://aosp.tuna.tsinghua.edu.cn/android/git-repo
