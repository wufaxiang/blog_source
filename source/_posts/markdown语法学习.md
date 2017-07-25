
title: Markdown语法学习
category: 工具学习
tags: markdown
date: 2016-05-21 23:00:12
---


博文需要使用markdown语法来进行编写，于是就学习了一下markdown语法，很简单的，跟着打一遍就会啦。下面简要的进行说明下啦：

### 1. 斜体和粗体

    使用 * 和 ** 表示斜体和粗体。

example：
**input:** 

    这是 *斜体*，这是 **粗体**。

**output**: 这是 *斜体*，这是 **粗体**。
<!--more-->
### 2. 分级标题

使用 === 表示一级标题，使用 --- 表示二级标题。

example：

```
这是一个一级标题
============================

这是一个二级标题
--------------------------------------------------

### 这是一个三级标题
```

同事你也可以选择在行首加井号表示不同级别的标题 (H1-H6)，例如：# H1, ## H2, ### H3，#### H4。

### 3. 外链接

使用 \[描述](链接地址) 为文字增加外链接。

example：
**input:** 

    这是去往 [本人博客](http://www.wufaxiang.com) 的链接。

**output:**:
这是去往 [本人博客](http://www.wufaxiang.com) 的链接。

### 4. 无序列表

使用 *，+，- 表示无序列表。

example：
**input:** 

    * 无序列表项 一
    + 无序列表项 二
    - 无序列表项 二

**output:**
* 无序列表项 一
+ 无序列表项 二
- 无序列表项 二
### 5. 有序列表

使用数字和点表示有序列表。

example：

1. 有序列表项 一
2. 有序列表项 二
3. 有序列表项 三

### 6. 文字引用

使用 > 表示文字引用。

example：
**input:** 

    > 你好，markdown。

**output:**
> 你好，markdown。

### 7. 行内代码块

使用 \`代码` 表示行内代码块。

example：
**input:**

    让我们聊聊 `markdown`。


让我们聊聊 `markdown`。

### 8.  代码块

使用 四个缩进空格 表示代码块。

example：
    这是一个代码块，此行左侧有四个不可见的空格。

### 9.  插入图像

使用 \!\[描述](图片链接地址) 插入图像。

example：
**input:**

    ![我的头像](http://www.wufaxiang.com/img/head.jpg)

**output:**
![我的头像](http://www.wufaxiang.com/img/head.jpg)


### 10. 内容目录（hexo不兼容）

在段落中填写 `[TOC]` 以显示全文内容的目录结构。

![目录](http://www.wufaxiang.com/pic/TOC.png)

### 11. 标签分类

在编辑区任意行的列首位置输入以下代码给文稿标签：

    标签： java python markdown

或者

    Tags： java python markdown

### 12. 删除线

使用 ~~ 表示删除线。
**inpout:**

    ~~这是一段错误的文本。~~
**output:**
~~这是一段错误的文本。~~

### 13. 注脚（hexo不兼容）

使用 [^keyword] 表示注脚。
**input:**

    这是一个注脚[^footnote]的样例。

这是一个注脚[^footnote2]的样例。

### 14. LaTeX 公式（hexo不兼容）

$ 表示行内公式： 
**input:**

     $E=mc^2$
     
![mc](http://www.wufaxiang.com/pic/mc.png)

$$ 表示整行公式：
**input:**

    $$\sum_{i=1}^n a_i=0$$
    $$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$
    $$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$
    
![mc2](http://www.wufaxiang.com/pic/mc2.png)

可以访问 [MathJax](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference) 参考更多使用方法。

### 15. 加强的代码块

支持四十一种编程语言的语法高亮的显示，行号显示。

非代码示例：
**input:**
![代码](http://www.wufaxiang.com/pic/code.png)
**output:**
```python
def somefunc(param1='', param2=0):
    '''A docstring'''
    if param1 > param2: # interesting
        print 'Greater'
    return (param2 - param1 + 1) or None

class SomeClass:
    pass

>>> message = '''interpreter
... prompt'''
```

### 16. 流程图（hexo不兼容）
 example
![输入](http://www.wufaxiang.com/pic/flow_c.png)
**output:**
![输出](http://www.wufaxiang.com/pic/flow.png)
更多语法参考：[流程图语法参考](http://adrai.github.io/flowchart.js/)

```flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```

### 17. 序列图
 exiample 1
![序列代码](http://www.wufaxiang.com/pic/seq_c.png)
**output:**
![序列图](http://www.wufaxiang.com/pic/seq.png)


 更多语法参考：[序列图语法参考](http://bramp.github.io/js-sequence-diagrams/)

### 18. 表格支持

    | 项目        | 价格   |  数量  |
    | --------   | -----:  | :----:  |
    | 计算机     | \$1600 |   5     |
    | 手机        |   \$12   |   12   |
    | 管线        |    \$1    |  234  |
**output:**
![表格](http://www.wufaxiang.com/pic/table.png)

### 19. 定义型列表

名词 1
:   定义 1（左侧有一个可见的冒号和四个不可见的空格）

代码块 2
:   这是代码块的定义（左侧有一个可见的冒号和四个不可见的空格）

        代码块（左侧有八个不可见的空格）

### 20. Html 标签

支持在 Markdown 语法中嵌套 Html 标签 

### 21. 内嵌图标

有的图标系统会对外开放，例如在文档中输入

    <i class="icon-weibo"></i>

即显示微博的图标： <i class="icon-weibo icon-2x"></i>

替换 上述 `i 标签` 内的 `icon-weibo` 以显示不同的图标，例如：

    <i class="icon-renren"></i>

即显示人人的图标： <i class="icon-renren icon-2x"></i>

更多的图标和玩法可以参看 [font-awesome](http://fortawesome.github.io/Font-Awesome/3.2.1/icons/) 官方网站。

### 22. 待办事宜 Todo 列表

使用带有 [ ] 或 [x] （未完成或已完成）项的列表语法撰写一个待办事宜列表，并且支持子列表嵌套以及混用Markdown语法，例如：

    - [ ] **Cmd Markdown 开发**
        - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
        - [ ] 支持以 PDF 格式导出文稿
        - [x] 新增Todo列表功能 [语法参考](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments)
        - [x] 改进 LaTex 功能
            - [x] 修复 LaTex 公式渲染问题
            - [x] 新增 LaTex 公式编号功能 [语法参考](http://docs.mathjax.org/en/latest/tex.html#tex-eq-numbers)
    - [ ] **七月旅行准备**
        - [ ] 准备邮轮上需要携带的物品
        - [ ] 浏览日本免税店的物品
        - [x] 购买蓝宝石公主号七月一日的船票
        
对应显示如下待办事宜 Todo 列表：
        
![待办事项](http://www.wufaxiang.com/pic/todo.png)
        
 
### 23.结束语       
以上的这些语法是在cmd markdown上面学习的，并没有很复杂，多写写，应该很快就可以熟希啦，多多练习。。。


[^footnote2]: 这是一个 *注脚* 的 **文本**。