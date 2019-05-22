## 第1章 Activity的生命周期和启动模式

### 1. 典型情况下的Activity生命周期

onCreate(Bundle)：onCreate(Bundle)是Activity生命周期的第一个方法，表示Activity正在被创建，在该方法中我们可以做一些初始化操作，比如使用setContentView()设置显示界面或者初始化数据等。

onRestart():表示Activity正在重新启动