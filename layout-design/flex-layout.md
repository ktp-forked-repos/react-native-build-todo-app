# Flexbox布局

为实现上面的布局，我们使用[CSS弹性盒子](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes)\(Flexbox\)，这是一种布局方式。Flexbox可以使页面适应不同的屏幕大小以及设备类型，确保界面元素拥有更恰当的排布方式。React Native采用Flexbox进行布局，使得App开发和Web开发采用相同的技术，这也是React Native吸引开发者的原因之一。

看一下我们的App布局：

![](/assets/flex_layout.png)

* header固定高度，而且里面的元素排列方向是横向，上下居中，和外边框有一定的距离
* footer固定高度，里面的元素横向排列
* content弹性高度，里面的列表纵向排列，每一行里面的元素又是横向排列

下面是详细的分解标注图

![](/assets/flexbox_layout_desc.png)

