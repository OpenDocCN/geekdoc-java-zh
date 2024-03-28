# 优化设计

> 原文：[`docs.oracle.com/javase/tutorial/uiswing/painting/refining.html`](https://docs.oracle.com/javase/tutorial/uiswing/painting/refining.html)

为了演示目的，将绘图逻辑完全包含在`MyPanel`类中是有意义的。但是，如果您的应用程序需要跟踪多个实例，您可以使用一种模式，将该代码提取到一个单独的类中，以便每个正方形可以被视为一个独立的对象。这种技术在 2D 游戏编程中很常见，有时被称为“精灵动画”。

点击“启动”按钮以使用[Java™ Web Start](http://www.oracle.com/technetwork/java/javase/javawebstart/index.html)运行 SwingPaintDemo4（[下载 JDK 7 或更高版本](http://www.oracle.com/technetwork/java/javase/downloads/index.html)）。或者，要自行编译和运行示例，请参考示例索引。

![启动 SwingPaintDemo4 示例](https://docs.oracle.com/javase/tutorialJWS/samples/uiswing/SwingPaintDemo4Project/SwingPaintDemo4.jnlp)

```java
package painting;

import javax.swing.SwingUtilities;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.BorderFactory;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics; 
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseMotionListener;
import java.awt.event.MouseMotionAdapter;

public class SwingPaintDemo4 {

    public static void main(String[] args) {

        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                createAndShowGUI(); 
            }
        });
    }

    private static void createAndShowGUI() {
        System.out.println("Created GUI on EDT? "+
        SwingUtilities.isEventDispatchThread());
        JFrame f = new JFrame("Swing Paint Demo");
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
        f.add(new MyPanel());
        f.setSize(250,250);
        f.setVisible(true);
    } 

}

class MyPanel extends JPanel {

    RedSquare redSquare = new RedSquare();

    public MyPanel() {

        setBorder(BorderFactory.createLineBorder(Color.black));

        addMouseListener(new MouseAdapter(){
            public void mousePressed(MouseEvent e){
                moveSquare(e.getX(),e.getY());
            }
        });

        addMouseMotionListener(new MouseAdapter(){
            public void mouseDragged(MouseEvent e){
                moveSquare(e.getX(),e.getY());
            }
        });

    }

    private void moveSquare(int x, int y){

        // Current square state, stored as final variables 
        // to avoid repeat invocations of the same methods.
        final int CURR_X = redSquare.getX();
        final int CURR_Y = redSquare.getY();
        final int CURR_W = redSquare.getWidth();
        final int CURR_H = redSquare.getHeight();
        final int OFFSET = 1;

        if ((CURR_X!=x) || (CURR_Y!=y)) {

            // The square is moving, repaint background 
            // over the old square location. 
            repaint(CURR_X,CURR_Y,CURR_W+OFFSET,CURR_H+OFFSET);

            // Update coordinates.
            redSquare.setX(x);
            redSquare.setY(y);

            // Repaint the square at the new location.
            repaint(redSquare.getX(), redSquare.getY(), 
                    redSquare.getWidth()+OFFSET, 
                    redSquare.getHeight()+OFFSET);
        }
    }

    public Dimension getPreferredSize() {
        return new Dimension(250,200);
    }

    public void paintComponent(Graphics g) {
        super.paintComponent(g);       
        g.drawString("This is my custom Panel!",10,20);

        redSquare.paintSquare(g);
    }  
}

class RedSquare{

    private int xPos = 50;
    private int yPos = 50;
    private int width = 20;
    private int height = 20;

    public void setX(int xPos){ 
        this.xPos = xPos;
    }

    public int getX(){
        return xPos;
    }

    public void setY(int yPos){
        this.yPos = yPos;
    }

    public int getY(){
        return yPos;
    }

    public int getWidth(){
        return width;
    } 

    public int getHeight(){
        return height;
    }

    public void paintSquare(Graphics g){
        g.setColor(Color.RED);
        g.fillRect(xPos,yPos,width,height);
        g.setColor(Color.BLACK);
        g.drawRect(xPos,yPos,width,height);  
    }
}

```

在这个特定的实现中，我们完全从头开始创建了一个`RedSquare`类。另一种方法是通过使`RedSquare`成为`java.awt.Rectangle`的子类来重用其功能。无论`RedSquare`如何实现，重要的是我们给该类一个接受`Graphics`对象的方法，并且该方法从面板的`paintComponent`方法中调用。这种分离使您的代码保持清晰，因为它基本上告诉每个红色正方形自己绘制。
