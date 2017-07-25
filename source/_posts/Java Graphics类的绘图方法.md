---
title: Java Graphics类的绘图方法
date: 2015-04-30 21:32:12
tags: 
  - java
category: java
---

Graphics类提供基本的几何图形绘制方法，主要有：画线段、画矩形、画圆、画带颜色的图形、画椭圆、画圆弧、画多边形等。  
  
** 1\. 画线 **   
在窗口画一条线段，可以使用Graphics类的drawLine()方法：  
drawLine(int x1,int y1,int x2,int y2)  
例如，以下代码在点（3,3）与点（50,50）之间画线段，在点（100,100）处画一个点。  
<pre>
g.drawLine(3,3,50,50);//画一条线段  
g.drawLine(100,100,100,100);//画一个点。
</pre>
  <!-- more -->
** 2. 画矩形 **   
有两种矩形：普通型和圆角型。  
(1) 画普通矩形有两个方法：

  * drawRect(int x,int y,int width,int height)：画线框围起来的矩形。其中参数x和y指定左上角的位置，参数width和height是矩形的宽和高。 
  * fillRect(int x,int y,int width,int height)：是用预定的颜色填充一个矩形，得到一个着色的矩形块。 
以下代码是画矩形的例子： 
<pre>
g.drawRect(80,100,40,25);//画线框  
g.setColor(Color.yellow);g.fillRect(20,70,20,30);//画着色块  
</pre>
(2)画圆角矩形也有两个方法：

  * drawRoundRect(int x,int y,int width, int height, int arcWidth, int arcHeight)：是用线围起来的圆角矩形。其中参数x和y指定矩形左上角的位置；参数width和heigth是矩形的宽和高；arcWidth和arcHeight分别是圆角弧的横向直径和圆角弧的纵向直径。 
  * fillRoundRect(int x,int y,int width,int height,int arcWidth,int archeight)：是用预定的颜色填充的圆角矩形。各参数的意义同前一个方法。 
以下代码是画矩形的例子：
<pre>
g.drawRoundRect(10,10,150,70,40,25);//画一个圆角矩形  
g.setColor(Color.blue); g.fillRoundRect(80,100,100,100,60,40);//涂一个圆角矩形块  
g.drawRoundRect(10,150,40,40,40,40);//画圆  
g.setColor(Color.red); g.fillRoundRect(80,100,100,100,100,100);//画圆块 
</pre>
可以用画圆角矩形方法画圆形，当矩形的宽和高相等，圆角弧的横向直径和圆角弧的纵向直径也相等，并等于矩形的宽和高时，画的就是圆形。参见上述例子中的注释，前一个是
画圆，后一个是涂圆块。  
  
** 3. 画三维矩形 **   
画三维矩形有两个方法：

  * draw3DRect(int x,int y,int width,int height, boolean raised)：画一个突出显示的矩形。其中x和y指定矩形左上角的位置，参数width和height是矩形的宽和高，参数raised是突出与否。 
  * fill3DRect(int x,int y,int width,int height,boolean raised)：用预定的颜色填充一个突出显示的矩形。 
以下代码是画突出矩形的例子：  
g.draw3DRect(80,100,40,25,true);//画一个线框  
g.setColor(Color.yellow); g.fill3DRect(20,70,20,30,true);//画一个着色块  
  
** 4.画椭圆形 **   
椭圆形由椭圆的横轴和纵轴确定。画椭圆形有两个方法：

  * drawOval(int x,int y,int width,int height)：是画用线围成的椭圆形。其中参数x和参数y指定椭圆形左上角的位置，参数width和height是横轴和纵轴。 
  * fillOval(int x,int y,int width,int height)：是用预定的颜色填充的椭圆形，是一个着色块。也可以用画椭圆形方法画圆形，当横轴和纵轴相等时，所画的椭圆形即为圆形。 
以下代码是画椭圆形的例子：
<pre>
g.drawOval(10,10,60,120);//画椭圆  
g.setColor(Color.cyan);g.fillOval(100,30,60,60);//涂圆块  
g.setColor(Color.magenta);g.fillOval(15,140,100,50);//涂椭圆  
</pre>
** 5\. 画圆弧 **   
画圆弧有两个方法：

  * drawArc(int x,int y,int width,int height,int startAngle, int arcAngle)：画椭圆一部分的圆弧线。椭圆的中心是它的外接矩形的中心，其中参数是外接矩形的左上角坐标(x,y)，宽是width，高是heigh。参数startAngle的单位是 “度”，起始角度0度是指3点钟方位.参数startAngle和arcAngle表示从startAngle角度开始，逆时针方向画arcAngle度的弧，约定，正值度数是逆时针方向，负值度数是顺时针方向，例如-90度是6点钟方位。 
  * fillArc(int x,int y,int width, int height, int startAngle, int arcAngle)：用setColor()方法设定的颜色,画着色椭圆的一部分。 
以下代码是画圆弧的例子：  
<pre>
g.drawArc(10,40,90,50,0,180);//画圆弧线  
g.drawArc(100,40,90,50,180,180);//画圆弧线  
g.setColor(Color.yellow); g.fillArc(10,100,40,40,0,-270);//填充缺右上角的四分之三的椭圆  
g.setColor(Color.green); g.fillArc(60,110,110,60,-90,-270);//填充缺左下角的四分之三的椭圆  
</pre>
** 6. 画多边形 **   
多边形是用多条线段首尾连接而成的封闭平面图。多边形线段端点的x坐标和y坐标分别存储在两个数组中，画多边形就是按给定的坐标点顺序用直线段将它们连起来。以下是画
多边形常用的两个方法：

  * drawPolygon(int xpoints[],int yPoints[],int nPoints)：画一个多边形 
  * fillPolygon(int xPoints[],int yPoints[],int nPoints)：用方法setColor()设定的颜色着色多边形。其中数组xPoints[]存储x坐标点，yPoints[]存储y坐标点，nPoints是坐标点个数。 
  
注意，上述方法并不自动闭合多边形，要画一个闭合的多边形，给出的坐标点的最后一点必须与第一点相同.以下代码实现填充一个三角形和画一个八边形。
```JAVA
int px1[]={50,90,10,50};//首末点相重,才能画多边形  
int py1[]={10,50,50,10};  
int px2[]={140,180,170,180,140,100,110,140};  
int py2[]={5,25,35,45,65,35,25,5};  
g.setColor(Color.blue);  
g.fillPolygon(px1,py1,4);  
g.setColor(Color.red);  
g.drawPolygon(px2,py2,9);  
```
也可以用多边形对象画多边形。用多边形类Polygon创建一个多边形对象，然后用这个对象绘制多边形。Polygon类的主要方法：

  * Polygon()：创建多边形对象，暂时没有坐标点。 
  * Polygon(int xPoints[],int yPoints[],int nPoints)：用指定的坐标点创建多边形对象。 
  * addPoint()：将一个坐标点加入到Polygon对象中。 
  * drawPolygon(Polygon p)：绘制多边形。 
  * fillPolygon(Polygon p)：和指定的颜色填充多边形。 
  
例如,以下代码，画一个三角形和填充一个黄色的三角形。注意，用多边形对象画封闭多边形不要求首末点重合。  
```JAVA
int x[]={140,180,170,180,140,100,110,100};  
int y[]={5,25,35,45,65,45,35,25};  
Polygon ponlygon1=new Polygon();  
polygon1.addPoint(50,10);  
polygon1.addPoint(90,50);  
polygon1.addPoint(10,50);  
g.drawPolygon(polygon1);  
g.setColor(Color.yellow);  
Polygon polygon2 = new Polygon(x,y,8);  
g.fillPolygon(polygon2);  
```
** 7\. 擦除矩形块 **   
当需要在一个着色图形的中间有一个空缺的矩形的情况，可用背景色填充一矩形块实现，相当于在该矩形块上使用了 “橡皮擦”.实现的方法是：  
clearRect(int x,int y, int width,int height)：擦除一个由参数指定的矩形块的着色。  
例如，以下代码实现在一个圆中擦除一个矩形块的着色：  
<pre>
g.setColor(Color.blue);  
g.fillOval(50,50,100,100);g.clearRect(70,70,40,55);  
</pre>
** 8. 限定作图显示区域 **   
用一个矩形表示图形的显示区域，要求图形在指定的范围内有效，不重新计算新的坐标值，自动实现超出部分不显示。方法是clipRect(int x,int
y,int width,int height)，限制图形在指定区域内的显示，超出部分不显示。多个限制区有覆盖时，得到限制区域的  交集区域  。例如，代码： 
<pre>
g.clipRect(0,0,100,50);
g.clipRect(50,25,100,50); 
</pre>
相当于  
<pre>
g.clipRect(50,25,50,25);  
</pre>
  
** 9\. 复制图形 **   
利用Graphics类的方法copyArea()可以实现图形的复制,其使用格式是：  
<pre>
copyArea(int x,int y,int width,int height, int dx, int dy)，
</pre>
dx和dy分别表示将图形粘贴到原位置偏移的像素点数，正值为往右或往下偏移是，负值为往左或往上偏移量。位移的参考点是要复制矩形的左上角坐标。  
  
例如，以下代码示意图形的复制,将一个矩形的一部分、另一个矩形的全部分别自制。  
<pre>
g.drawRect(10,10,60,90);  
g.fillRect(90,10,60,90);  
g.copyArea(40,50,60,70,-20,80);  
g.copyArea(110,50,60,60,10,80);  
</pre>

【例12-3】小应用程序重写update()方法,只清除圆块，不清除文字，窗口显示一个不断移动的红色方块

    
```JAVA    
    import java.applet.*;
    import java.awt.*;
    public class Example7_3 extends Applet{
        int i=1;
        public void init(){
            setBackground(Color.yellow);
        }
        public void paint(Graphics g){
            i = i+8; if(i>160)i=1;
            g.setColor(Color.red);g.fillRect(i,10,20,20);
            g.drawString("我正学习update()方法",100,100);
            try{
                Thread.sleep(100);
            }
            catch(InterruptedException e){}
            repaint();
        }
        public void update(Graphics g){
            g.clearRect(i,10,200,100);//不清除"我正在学习update()方法"
            paint(g);
        }
    }
```
  
一般的绘图程序要继承JFrame，定义一个JFrame窗口子类，还要继承JPanel，定义一个JPanel子类。在JPanel子类
中重定义方法paintComponent()，在这个方法中调用绘图方法,绘制各种图形。  
  
【例12-4】使用XOR绘图模式的应用程序

    
    <pre name="code" class="java">import javax.swing.*;
    import java.awt.*;
    public class Example7_4 extends JFrame{
        public static void main(String args[]){
            GraphicsDemo myGraphicsFrame = new GraphicsDemo();
        }
    }
    class ShapesPanel extends JPanel{
    	ShapesPanel(){
            setBackground(Color.white);
        }
        public void paintComponent(Graphics g){
            super.paintComponent(g);
            setBackground(Color.yellow); //背景色为黄色
            g.setXORMode(Color.red); //设置XOR绘图模式,颜色为红色
            g.setColor(Color.green);
            g.fillRect(20, 20, 80, 40); //实际颜色是green + yellow的混合色=灰色
            g.setColor(Color.yellow);
            g.fillRect(60, 20, 80, 40); //后一半是yellow+yellow=read,前一半是yellow+灰色
            g.setColor(Color.green);
            g.fillRect(20, 70, 80, 40); //实际颜色是green+yellow的混合色=灰色.
            g.fillRect(60, 70, 80, 40);
            //前一半是(green+yellow)+gray =背景色,后一半是green+yellow = gray
            g.setColor(Color.green);
            g.drawLine(80, 100, 180, 200); //该直线是green+yellow = gray
            g.drawLine(100, 100, 200, 200); //同上
            /*再绘制部分重叠的直线.原直线中间段是灰色+灰色=背景色,延长部分是green+yellow=gray.*/
            g.drawLine(140, 140, 220, 220);
            g.setColor(Color.yellow); //分析下列直线颜色变化,与早先的力有重叠
            g.drawLine(20, 30, 160, 30);
            g.drawLine(20, 75, 160, 75);
        }
    }
    
    class GraphicsDemo extends JFrame{
        public GraphicsDemo(){
            this.getContentPane().add(new ShapesPanel());
            setTitle("基本绘图方法演示");
            setSize(300, 300);
            setVisible(true);
        }
    }

转载自： [ http://www.weixueyuan.net/view/6073.html
](http://www.weixueyuan.net/view/6073.html)  
  

