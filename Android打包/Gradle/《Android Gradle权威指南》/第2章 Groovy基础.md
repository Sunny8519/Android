### 闭包

```groovy
task task_testClosure_01 {
    customEach { k, v, e, r ->
        println "i = $k/$v/$e/$r"
    }
}

// 定义一个方法，参数为一个闭包对象
def customEach(closure) {
    for (int i : 1..10) {
//        closure(String.valueOf(i), i)
        closure.call(String.valueOf(i), i, "8767", "83948934")
    }
}
```
上面的代码中我们定义了一个方法，方法的参数是一个闭包对象，闭包对象的类型其实是Closure，在groovy的方法参数中可以简写为closure。

`closure(String.valueOf(i),i)`和`closure.call(String.valueOf(i),i)`作用是一样的，个人理解应该是前面的写法最终会调用第二种写法的call方法。经过测试call方法可以传很多参数，但是从第三个参数开始好像不能传递int类型的值了，只能是字符串。

