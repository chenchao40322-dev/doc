## android note ##

### gravity  ###
- **android:gravity:** 是对view本身说的，元素本身的文本显示在什么地方靠着换个属性设置，不过不设置默认是在左侧的。
- **android:layout_gravity:** 是相对与它的父元素说的，说明元素显示在父元素的什么位置，只在 LinearLayout 和 FrameLayout 中有效。
- **对于LinearLayout何时生效的问题:**  
	- 当 android:orientation="vertical"  时， 只有水平方向的设置才起作用，垂直方向的设置不起作用。即：left，right，center_horizontal 是生效的。
	- 当 android:orientation="horizontal" 时， 只有垂直方向的设置才起作用，水平方向的设置不起作用。即：top，bottom，center_vertical 是生效的。

###inflate
**http://blog.csdn.net/xyz_fly/article/details/37932989**