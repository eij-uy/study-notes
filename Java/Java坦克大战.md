[toc]

# 坦克大战

## Java 绘图技术

- Component 类提供了两个和绘图相关最重要的方法
  1. `paint(Graphics g)`绘制组件的外观
  2. `repaint()`刷新组件的外观
- 当组件第一次在屏幕显示的时候，程序会自动的调用`paint()`方法来绘制组件
- 在以下情况`paint()`将会被调用
  1. 窗口最小化，再最大化
  2. 窗口的大小发生变化
  3. `repaint`函数被调用

### Graphics 类

**Graphics 类可以理解就是画笔，为我们提供了各种绘制图形的方法**

- 画直线 `drawLine(int x1,int y1, int x2, int y2)`

- 画矩形边框 `drawRect(int x, int y, int width, int height)`

- 画椭圆边框 `drawOval(int x, int y, int width, int height)`

- 填充矩形 `fillRect(int x, int y, int width, int height)`

- 填充椭圆 `fillOval(int x, int y, int width, int height)`

- 画图片 `drawImage(Image img, int x, int y, ...)`

  ~~~java
  // 1. 获取图片资源, hd.png 表示在该项目的根目录去获取 hd.png 的图片资源
  Image img = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/hd.png"));
  g.drawImage(img, 10, 10, 988, 617, this);
  ~~~

- 画字符串 `drawString(String str, int x, int y)`

- 设置画笔的字体 `setFont(Font font)`

  ~~~javascript
  // new Font(family，weght, size)
  // g.setFont(new Font("隶书", Font.BOLD, 50))
  ~~~

- 设置画笔的颜色 `setColor(color c)`

## Java 事件处理机制

### 基本说明

**Java 时间处理是采取 委派事件模型，当事件发生时，产生事件的对象，会把此信息传递给时间的坚挺着处理，这里所说的信息实际上就是 java.awt.event 事件类库里某个类所创建的对象，把它成为 事件的对象**

### 深入理解

1. 事件源：事件源是一个产生事件的对象，比如按钮，窗口等
2. 事件：事件就是承载事件源状态改变时的对象，比如当键盘事件、鼠标事件、窗口事件等等，会产生一个事件对象，该对象保存着当前事件很多信息，比如 KeyEvent 对象有含义被按下键的 Code 值。java.awt.event 包和 javax.swing.event 包中定义了各种事件类型

#### 事件类型

- ActionEvent 通常在按下按钮，或双击一个列表项或选中某个菜单时发生
- AdjustmentEvent 当操作一个滚动条时发生
- ComponentEvent 当一个组件隐藏，移动，改变大小时发送
- ContainerEvent 当一个组件从容器中加入或者删除时发生
- FocusEvent 当一个组件获得或是失去焦点时发生
- ItemEvent 当一个复选框或是列表项被选中时，当一个选择框或是选择菜单被选中
- KeyEvent 当从键盘的案件被按下，松开时发生
- MouseEvent 当鼠标被拖动，移动，点击，按下...
- TextEvent 当文本区和文本域的文本发生改变时发生
- WindowEvent 当一个窗口激活，关闭，失效，回复，最小化

#### 事件监听器接口

1. 当事件源产生一个事件，可以传送给事件监听者处理
2. 事件监听者实际上就是一个类，该类实现了某个事件监听器接口
3. 事件监听者接口有多重，不同的事件监听杰阔可以监听不同的事件，一个类可以实现多个监听接口
4. 这些接口在 java.awt.event 包和 javax.swing.event 包中定义。