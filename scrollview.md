# scrollview

1、ScrollView和HorizontalScrollView是为控件或者布局添加滚动条

2、上述两个控件只能有一个孩子，但是它并不是传统意义上的容器

3、上述两个控件可以互相嵌套

4、滚动条的位置现在的实验结果是：可以由layout_width和layout_height设定

5、ScrollView用于设置垂直滚动条
，HorizontalScrollView用于设置水平滚动条：

需要注意的是，有一个属性是    scrollbars 可以设置滚动条的方向:

但是ScrollView设置成horizontal是和设置成none是效果同，HorizontalScrollView设置成vertical和none的效果同。
