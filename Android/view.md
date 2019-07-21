## 1.view：相当于每一块的视图

1.View类通常位于android.view，而View的子类一般都位于android.widget中

2.view常用属性：

android:id="@+id/button  ：格式都是开头都是@+id 后面才是真正id

android:background="#3399cc  ：背景颜色

android:padding="16dp"  ：内边距，细分里面还有很多左边，右边距上边距下边距。

### 2.viewGroup：用来控制view子组件的摆放

通常使用他的两个子类。

1.viewGroup.LayoutParams类（控制布局高度和宽度）

```
android:layout_width="match_parent" (这个常量表示设置的值与父容器相同)
android:layout_height="wrap_content" （根据自身的内容来决定大小）
```

1.viewGroup.MarginLayoutParams类（主要用来控制外边距，主件四周的区域）

```
android:layout_marginLeft="118dp" (左边剧)
android:layout_marginTop="118dp" （上边距）
android:layout_marginBottom="118dp" （下边距）
```

![1555579947166](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1555579947166.png)

可以包含多个view和viewGroup.

# UI界面的设计

### 1.使用xml文件。默认在res/layout下的activity_main.xml文件中。文件名可随便改只要符合规则.	（简单不灵活）

(那么怎么使用呢？在java代码中通过setContentView(R.layout.activity_main); 加载界面)



#### layout文件夹存放布局文件：

默认是activity_main.xml

#### mipmap文件夹下:

放置图片 ，比如使用背景图片通过使用android:background="@mipmap/文件名" 使用

#### values文件夹下：

strings.xml :保存字符串资源（通过@string/变量名 调用）



### 2.通过java代码控制界面（灵活但复杂）

纯java设计，一大堆什么获取布局，视图啊，麻烦

### 3.使用xml和java混合（推荐）

大体框架通过xml设计，里面想要灵活，可以自己设计，通过getId，来获得需要的组件

可以通过自定义view来，增加功能。只需要自定义view，然后在mainActivity里中引用。



### Activity

1.Activity的四种状态

运行状态：正在运行

暂停状态：退出时弹出询问是否退出框，暂停的时候

停止状态：确定退出，此时停止状态

销毁状态：强制直接退出，比如：强制关闭。销毁状态

2..Activity生命周期

![1555643642156](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1555643642156.png)







