### View事件分发机制

#### 事件分发三个重要方法：
dispatchTouchEvent(MotionEvent e)
用来进行事件分发，如果一个事件能够传递到当前View，那么该方法一定会被调用，该方法的返回结果受当前View的onTouchEvent(MotionEvent e)以及下级View的dispatchEvent(MotionEvent e)方法的影响，表示是否消耗当前事件。

onInterceptTouchEvent(MotionEvent e)
在上述方法内部调用，用来判断是否拦截某个事件，如果当前View拦截了某个事件，那么在同一个事件序列中，该方法不会再被调用。方法返回结果表示是否拦截当前事件。

onTouchEvent(MotionEvent e)
在dipatchTouchEvent(MotionEvent e)方法中被调用，用来处理事件，返回结果表示当前View是否消耗该事件，如果不消耗，那么当前事件的同一事件序列中，View将接受不到接下来的所有事件。

#### 事件分发伪代码

```java
public boolean dispatchTouchEvent(MotionEvent e){
    boolean consume = false;
    if(onInterceptTouchEvent(e)){
        consume = onTouchEvent(e);
    }else{
        consume = child.dispatchTouchEvent(e);
    }
    return consume;
}
```

注意：
1.如果View设置了OnTouchListener，OnTouchListener的优先级要比当前View的onTouchEvent()方法高，onTouchEvent()方法是否被调用要取决于OnTouchListener中onTouch()方法的返回结果。onTouch返回false，表示事件在onTouch()方法中没有被消费，继续调用onTouchEvent()方法；onTouch返回true，表示事件在onTouch中被消费，那么当前View将不再调用onTouchEvent()方法。

2.如果View设置了OnClickListener，那么OnClickListener中的onClick()方法会在onTouchEvent()方法中被调用，由此可见，OnClickListener的优先级要低于onTouchEvent()方法。

3.正常情况下，一个事件序列只能被一个View拦截且消耗，但是可以通过特殊手段来达到同一个事件序列被不同的View处理，比如一个View可以通过调用子View的onTouchEvent()方法强行将自己的事件传递给子View。如果某个View决定拦截某个事件，那么同一事件序列中接下来的事件都会交由该View处理，并且不再调用onInterceptTouchEvent()方法。

4.某个View在处理事件时，如果它不消耗ACTION_DOWN事件(onTouchEvent()方法返回false)，那么同一事件序列中的其他事件都不会交由它来处理，包括当前事件在内的接下来的所有事件都会交由父布局处理，即父布局的onTouchEvent()方法会被调用。当然不仅限于ACTION_DOWN事件了，如果某个ACTION_MOVE当前View不消耗，那么从这个ACTION_MOVE开始，接下来的所有事件都不会交由当前事件，它们会向上传递交由父布局处理。

5.如果View不消耗除ACTION_DOWN以外的其他事件，那么这个点击事件会消失，此时父布局的onTouchEvent()方法并不会被调用，当前View仍然可以接收到后续的事件，最后这些消失事件会传递给Activity处理。

6.ViewGroup默认不拦截任何事件，ViewGroup中的onInterceptTouchEvent()方法默认返回false。

7.View没有onInterceptTouchEvent()方法，一旦事件传递到View了并且View没有设置OnTouchListener，那么当前View的onTouchEvent()方法将被调用。

8.View的onTouchEvent默认都会消耗事件(返回true)，除非它是不可点击的(clickable和longClickable同时为false)，View的longClickable属性默认都为false，clickable属性要分情况了，比如Button的clickable属性默认为true，而TextView的clickable属性默认为false，所以说TextView默认不消耗任何事件。

9.View的enable属性不影响onTouchEvent的默认返回值，只要View的clickable和longClickable中有任何一个为true，那么View默认的onTouchEvent都会返回true。

10.onClick会被调用的前提是当前View是可点击的，即当前View收到了ACTION_DOWN和ACTION_UP事件。

11.事件传递是由外向内的，从父布局传递给子View，子View可以通过requestDisallowInterceptTouchEvent()方法干扰父布局对事件的分发，但是不能干扰ACTION_DOWN事件。


问题：
1.TouchTarget数据结构分析？


