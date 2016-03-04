## map
java中的map其实就是以键值对形式的存放数据的容器，其常用的实现类主要是哈希map
例如：
Map map = new HashMap();
插入元素：map.put("key", obj); 
移除元素: map.remove("key");
清空: map.clear();

## Intager 与int
Integer i=0; 
i是一个对象 

int i=3; 
i是一个基础变量 

Integer i=0; 
这种写法如果没记错，在JAVA1.5之前是会报错的，自动的加解包是1.5的新特性 
必须写成 
Integer i= new Integer(0); 
i.intValue()才能提取i的值

使用场合，例如说
往ArrayList里面add，必须add的是Object
而int不是对象，就只能把Integer添加进去
## java 中的instanceof
java 中的instanceof 运算符是用来在运行时指出对象是否是特定类的一个实例
