<div align="center">

## Navigational


</div>

### Description

This is a navigational java applet menu that can be placed on a website.
 
### More Info
 
<applet code="Nav.class" align="baseline" width="165" height="145"

name="menu" id="menu" color="#000000">

<param name="Item1" value="Planet-Source-Code; ">

<param name="Item2" value="Rent A Coder;">

<param name="Item3" value="Applets; ">

<param name="Item4" value="Brewing; ">

<param name="Item5" value="Downloads; ">

<param name="Item6" value="Showcase; ">

<param name="Item7" value="Arcade; ">

<param name="target" value="_self">

</applet>


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[darocker22](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/darocker22.md)
**Level**          |Intermediate
**User Rating**    |4.0 (68 globes from 17 users)
**Compatibility**  |Java \(JDK 1\.2\)
**Category**       |[Menus](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/menus__2-89.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/darocker22-navigational__2-2703/archive/master.zip)





### Source Code

```
import java.applet.Applet;
import java.applet.AppletContext;
import java.awt.*;
import java.net.MalformedURLException;
import java.net.URL;
public class Nav extends Applet
  implements Runnable
{
  private Thread m_thrd;
  private Event m_evt;
  private String m_strNew;
  private String m_items[][];
  private Font m_font;
  private Font m_fontSel;
  private Color m_clrSel;
  private Color m_clrLight;
  private Color m_clrDark;
  private String m_strTarg;
  private int m_cx;
  private int m_cy;
  private int m_cySpace;
  private int m_iSel;
  private int m_iIn;
  private int m_nState;
  public void start()
  {
    if(m_thrd == null)
    {
      m_thrd = new Thread(this);
      m_thrd.start();
    }
    String s = getParameter("init");
    if(s != null)
      try
      {
        URL url = getDocumentBase();
        getAppletContext().showDocument(new URL(url, s), "init");
        return;
      }
      catch(MalformedURLException _ex) { }
  }
  public void stop()
  {
    if(m_thrd != null && m_thrd.isAlive())
    {
      m_thrd.stop();
    }
    m_thrd = null;
  }
  public Nav()
  {
    m_cy = 13;
    m_cySpace = 7;
    m_iIn = -1;
    setBackground(Color.black);
  }
  public boolean mouseExit(Event event, int i, int j)
  {
    if(m_nState == 0)
    {
      DrawButton(m_iIn, getGraphics(), 1);
      m_iIn = -1;
    }
    return true;
  }
  public boolean mouseMove(Event event, int i, int j)
  {
    Graphics g = getGraphics();
    int k = (j - 1) / (m_cy + m_cySpace);
    if(k >= m_items.length || j - 1 - k * (m_cy + m_cySpace) >= m_cy)
    {
      k = -1;
    }
    if(k != m_iIn)
    {
      if(m_iIn > -1)
      {
        DrawButton(m_iIn, g, 1);
      }
      if(k > -1)
      {
        DrawButton(k, g, 2);
      }
      m_iIn = k;
    }
    return true;
  }
  public boolean mouseDown(Event event, int i, int j)
  {
    if(m_iIn != -1)
    {
      m_nState = 1;
      DrawButton(m_iIn, getGraphics(), 3);
    }
    return true;
  }
  public boolean mouseDrag(Event event, int i, int j)
  {
    if(m_nState > 0)
    {
      int k = (j - 1) / (m_cy + m_cySpace);
      if(k != m_iIn || k >= m_items.length || i < 0 || i > m_cx || j - 1 - k * (m_cy + m_cySpace) >= m_cy)
      {
        k = -1;
      }
      if(m_nState == 1 && k != m_iIn)
      {
        m_nState = 2;
        DrawButton(m_iIn, getGraphics(), 2);
      }
      else if(m_nState == 2 && k == m_iIn)
      {
        m_nState = 1;
        DrawButton(m_iIn, getGraphics(), 3);
      }
    }
    return true;
  }
  public boolean mouseUp(Event event, int i, int j)
  {
    if(m_nState > 0)
      if(m_nState == 2)
      {
        DrawButton(m_iIn, getGraphics(), 1);
      }
      else
      {
        if(m_iIn != m_iSel)
        {
          int k = m_iSel;
          m_iSel = m_iIn;
          DrawButton(k, getGraphics(), 0);
          DrawButton(m_iSel, getGraphics(), 1);
        }
        m_iIn = -1;
        m_nState = 0;
        try
        {
          String s = m_items[m_iSel][2];
          if(s == null)
          {
            s = m_strTarg;
          }
          URL url = getDocumentBase();
          getAppletContext().showDocument(new URL(url, m_items[m_iSel][1]), s);
        }
        catch(MalformedURLException _ex) { }
      }
    return true;
  }
  protected final Color ClrFromStr(String s)
  {
    if(s == null || s.charAt(0) != '#' || s.length() != 7)
    {
      return Color.white;
    }
    else
    {
      return new Color(Integer.parseInt(s.substring(1, 7), 16));
    }
  }
  public void run()
  {
    do
    {
      try
      {
        Thread.currentThread();
        Thread.sleep(100L);
      }
      catch(InterruptedException _ex) { }
      postEvent(m_evt);
    } while(true);
  }
  public final void SetContext(String s)
  {
    m_strNew = s;
  }
  protected final void Draw3DRect(Graphics g, int i, int j, int k, int l, Color color, Color color1)
  {
    g.setColor(color);
    g.fillRect(i, j, k, 1);
    g.fillRect(i, j, 1, l);
    g.setColor(color1);
    g.fillRect(i, (j + l) - 1, k, 1);
    g.fillRect((i + k) - 1, j, 1, l);
  }
  protected final void DrawButton(int i, Graphics g, int j)
  {
    int k = i * (m_cy + m_cySpace) + 1;
    Font font;
    Color color;
    Color color1;
    Color color2;
    if(i == m_iSel)
    {
      g.setColor(m_clrSel);
      font = m_fontSel;
      color = Color.white;
      color1 = m_clrLight;
      color2 = m_clrDark;
    }
    else
    {
      if(j > 1)
      {
        g.setColor(Color.lightGray);
        color = Color.black;
      }
      else
      {
        g.setColor(Color.gray);
        color = Color.white;
      }
      font = m_font;
      color1 = Color.white;
      color2 = Color.gray;
    }
    if(j > 1)
    {
      g.fillRect(1, k, m_cx - 2, m_cy);
      if(j == 2)
      {
        Draw3DRect(g, 0, k - 1, m_cx, m_cy + 2, color1, color2);
      }
      else
      {
        Draw3DRect(g, 0, k - 1, m_cx, m_cy + 2, color2, color1);
      }
    }
    else
    {
      g.fillRect(0, k, m_cx, m_cy);
      if(j == 1)
      {
        g.setColor(getBackground());
        g.fillRect(0, k - 1, m_cx, 1);
        g.fillRect(0, k + m_cy, m_cx, 1);
      }
    }
    g.setColor(color);
    g.setFont(font);
    if(j == 3)
    {
      g.drawString(m_items[i][0], 11, 12 + k);
      return;
    }
    else
    {
      g.drawString(m_items[i][0], 10, 11 + k);
      return;
    }
  }
  public void init()
  {
    String s;
    int i;
    for(i = 1; (s = getParameter("Item" + Integer.toString(i))) != null; i++);
    if(i == 1)
    {
      return;
    }
    String as[][] = new String[i - 1][3];
    String s1;
    for(int j = 1; (s1 = getParameter("Item" + Integer.toString(j))) != null; j++)
    {
      int k = s1.indexOf(59, 0);
      if(k < 0)
      {
        return;
      }
      int i1 = s1.indexOf(59, k + 1);
      as[j - 1][0] = s1.substring(0, k);
      if(i1 < 0)
      {
        as[j - 1][1] = s1.substring(k + 1);
        as[j - 1][2] = null;
      }
      else
      {
        as[j - 1][1] = s1.substring(k + 1, i1);
        as[j - 1][2] = s1.substring(i1 + 1);
      }
    }
    String s2 = getParameter("sel");
    if(s2 != null)
    {
      m_iSel = Integer.parseInt(s2);
    }
    if(m_iSel < 0 || m_iSel >= as.length)
    {
      m_iSel = 0;
    }
    int l = 10;
    s2 = getParameter("fsize");
    if(s2 != null)
    {
      l = Integer.parseInt(s2);
    }
    s2 = getParameter("font");
    if(s2 != null)
    {
      m_font = new Font(s2, 0, l);
    }
    else
    {
      Graphics g = getGraphics();
      m_font = g.getFont();
      m_font = new Font(m_font.getName(), 0, l);
    }
    m_fontSel = new Font(m_font.getName(), 1, l);
    s2 = getParameter("selcolor");
    if(s2 == null)
    {
      m_clrSel = new Color(153, 0, 0);
    }
    else
    {
      m_clrSel = ClrFromStr(s2);
    }
    s2 = getParameter("ltcolor");
    if(s2 == null)
    {
      m_clrLight = new Color(255, 0, 0);
    }
    else
    {
      m_clrLight = ClrFromStr(s2);
    }
    s2 = getParameter("dkcolor");
    if(s2 == null)
    {
      m_clrDark = new Color(102, 0, 0);
    }
    else
    {
      m_clrDark = ClrFromStr(s2);
    }
    m_cx = size().width;
    m_strTarg = getParameter("target");
    m_evt = new Event(new Object(), 0, new Object());
    m_items = as;
  }
  public void paint(Graphics g)
  {
    Dimension dimension = size();
    g.setColor(getBackground());
    g.fillRect(0, 0, dimension.width, dimension.height);
    if(m_items == null)
    {
      return;
    }
    int i = m_items.length;
    for(int j = 0; j < i; j++)
    {
      DrawButton(j, g, 0);
    }
  }
  public final boolean handleEvent(Event event)
  {
    if(event == m_evt)
    {
      if(m_strNew != null)
      {
        if(!m_strNew.equalsIgnoreCase(m_items[m_iSel][1]))
        {
          int i = m_items.length;
          for(int j = 0; j < i; j++)
          {
            if(!m_strNew.equalsIgnoreCase(m_items[j][1]))
            {
              continue;
            }
            int k = m_iSel;
            m_iSel = j;
            Graphics g = getGraphics();
            DrawButton(k, g, 0);
            DrawButton(m_iSel, g, 0);
            g.setFont(m_font);
            break;
          }
        }
        m_strNew = null;
      }
      return true;
    }
    else
    {
      return super.handleEvent(event);
    }
  }
}
```

