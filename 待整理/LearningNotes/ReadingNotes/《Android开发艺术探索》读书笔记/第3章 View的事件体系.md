### 一.View的位置参数
View在父容器中的位置是有四个顶点的坐标决定的，在View类中有四个属性表示四个顶点的坐标，分别为mLeft,mTop,mRight,mBottom，他们都有对应的get/set方法。

在android3.0之后又增加了几个重要的关于位置的参数，分别为x,y,translationX,translationY，x,y表示的是View左上角的坐标，它和mLeft,mTop的不同应该是在View平移的过程中，mLeft，mTop的值不会变化，表示的是原始View所在的左上角坐标，但是x,y的值会随着平移不断变化，可以说x,y是View实时的左上角位置坐标，x,y也是相对于父容器的。

translationX,translationY表示View相对于父容器的偏移量，当View处于最原始位置的时候，这两个值都是0，当View发生平移后，这两个值就会随之变化。

mLeft,mTop,x,y,translationX,translationY的关系如下：
x = mLeft + translationX
y = mTop + translationY

**MotionEvent事件发生的坐标点**
getX和getY获取到的是当前手指触摸点相对于当前View左上角的坐标(那么这儿有个问题？事件都是从父容器向下传递给子View，当这个事件还在父容器中的时候，MotionEvent中getX和getY获取到的还是相对于子View左上角的坐标吗？？)。

getRawX和getRawY获取到的是当前手指触摸点相对于屏幕左上角的坐标(raw英文翻译叫原始,未加工的)。

**TouchSlop**
TouchSlop是系统能识别出的最小滑动距离，也就是说如果手指在屏幕上滑动距离小于这个值，系统就不认为这是一个滑动事件；这个常量值不同的设备可能不一样，系统也提供api供我们调用：`ViewConfiguration.get(context).getScaledTouchSlop();`它获取到的是一个像素值，该值在系统的资源文件中也有配置(/res/config.xml)：`<dimen name="config_viewConfigurationTouchSlop">8dp</dimen>`.

**VelocityTracker**
http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2012/1117/574.html
滑动速度追踪，可用来追踪手指在滑动过程中的速度，包括水平和竖直方向上的速度。

使用步骤：
```java
//声明变量
private VelocityTracker mVelocityTracker; 
@Override 
public boolean onTouchEvent(MotionEvent event) { 
    inal int action = event.getAction(); 
        
    //获得VelocityTracker类实例
    if (mVelocityTracker == null) {  
        mVelocityTracker = VelocityTracker.obtain();
    }
    //将事件加入到VelocityTracker类实例中
    mVelocityTracker.addMovement(ev);
        
    final VelocityTracker verTracker = mVelocityTracker; 
    switch (action) { 
        case MotionEvent.ACTION_DOWN: 
            //求第一个触点的id， 此时可能有多个触点，但至少一个 
            mPointerId = event.getPointerId(0); 
            break; 
  
        case MotionEvent.ACTION_MOVE: 
            //求伪瞬时速度 
            verTracker.computeCurrentVelocity(1000, mMaxVelocity); 
            final float velocityX = verTracker.getXVelocity(mPointerId); 
            final float velocityY = verTracker.getYVelocity(mPointerId);
            ...
            break; 
  
        case MotionEvent.ACTION_UP:
            releaseVelocityTracker(); 
            break; 
  
        case MotionEvent.ACTION_CANCEL: 
            releaseVelocityTracker(); 
            break; 
  
        default: 
            break; 
    } 
    return super.onTouchEvent(event); 
}

//释放VelocityTracker
private void releaseVelocityTracker() { 
    if(null != mVelocityTracker) { 
        mVelocityTracker.clear(); 
        mVelocityTracker.recycle(); 
        mVelocityTracker = null; 
    } 
}
```

**GestureDetector**

