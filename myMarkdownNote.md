# markdown 速查表

## 目录

- [一、VScode 插件](#vscode插件)
- [二、快捷键](#快捷键)
- [三、常用语法](#常用语法)
  - [1.标题](#1标题)
  - [2.粗体](#2快捷键)
  - [3.斜体](#3斜体)
  - [4.引用](#4引用)
  - [5.有序列表](#5有序列表)
  - [6.无序列表](#6无序)
  - [7.代码](#7代码)
  - [8.分割线](#8分隔线)
  - [9.链接](#9链接)
  - [10.图片](#10图片)
  - [11.表格](#11表格)
  - [12.代码块](#12代码块)
  - [13.删除线](#13删除线)
  - [14.任务列表](#14任务列表)
  - [15.公式语法](#15公式语法)

## VScode插件
1. Markdown preview enhanced
2. Markdown All In One
3. Markdown PDF

## 快捷键

- Ctrl + B	Toggle bold  切换粗体
- Ctrl + I	Toggle italic  切换斜体
- Alt + S	Toggle strikethrough  切换删除线
- Ctrl + Shift + ]	Toggle heading (uplevel)   标题（上升）
- Ctrl + Shift + [	Toggle heading (downlevel)   标题（下降）
- Alt + C	Check/Uncheck task list item   选表单切换
- Ctrl + d 选定多个相同的单词
- Ctrl + Alt + 上箭头/下箭头

## 常用语法

### 1.标题
   >
   > - **\#\#\#\#\#** 和普通的  **\*\*\*\*** 加粗效果相同，字体大小相等
   > - 语法： \#text
   >  
   ### H3
   #### H4
   ##### H5

### 2.粗体
    > - 快捷键Ctrl+B
    > - 语法：\*\*text\*\* 
    **bold text**

### 3.斜体
  > - 加粗倾斜，使用 \*\*\*text\*\*\* => ***text***
  > - 语法：\*text\* => *text*
  *italicized text*

### 4.引用
> 语法： \>
> blockquote
> > blockquote  
>
> blockquote
> blockquote


### 5.有序列表
> - 按**tab键** 即可更换编号优先级 1，2，3.../i,ii,iii,iv,.../a,b,c...
> - 另外可嵌套 **"-"**
> - 插件默认**tab 键**可以增加优先级，**回车键**可以降低优先级
> - 如果想跳出嵌套，可以**光标左移一格**再用**回车键**
> - 语法：
> > 1. 
> > 2. 
> > 3. 
---
   1. First item
        - test1
        - test2
   2. Second item
      1. test1
           - item1
             - item1.1 
             - item1.2
           - item2  
      2. test2
   3. Third item
      1. test1
         1. test1
         2. test2
      2. test2
      3. test3
      4. test 
---
### 6.无序列表
> 和有序列表相同，**tab键**继续向内嵌套，**删除键**跳出一级嵌套
> 
   - First item
     - item1
     - item2
   - Second item
   - Third item
### 7.代码
> - 如果想加粗 \` \` 中的代码，在反引号外面嵌套 \** **, 倾斜嵌套 * *，
> - 引用一个语法时是使用转义：\\或者\\\\
> - 语法：\` \` (反引号) 
>
`coding` is important
***`Java`** is the best language in the world*
**`damn`**
`**damn**`

### 8.分隔线
> 语法：\-\-\-
---

### 9.链接
> 语法：\[显示名]\(超链接地址 "title")

[my github page](https://github.com/xionghaotian525/ "title") 

### 10.图片
> 语法：\!\[图片alt]\(图片链接 "title")    

![alt text](/Res/images/集合接口继承关系和实现.png "title")
![alt text](/Res/images/并发编程知识库.png "title")

### 11.表格
> - 可以使用表格的HTML字符代码（\&#124;）在表中显示竖线（|）字符。在（|）字符之前添加转义（\\）也可以
> - 语法
> | text | text | text |
> \|:---|\:---\:|---\:| (依次表示：左对齐，居中对齐，右对齐)
> |text|text|text|
> |text|text|text|

| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

|hello|hello|hello|
| :---|:---:|---:|
|world|test|what|
|sorry|nonono|tommorow|
### 12.代码块
> 语法：
>   \```java    
>       code;
>   \``` 

```java
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```
### 13.删除线
> 语法： \~~ text \~~

~~The world is flat.~~

### 14.任务列表

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

### 15.公式语法

Markdown中使用LaTeX语法来表示数学公式。以下是一些常用的LaTeX语法和数学运算符号的示例：

1. **行内公式：** 使用一对美元符号（`$`）包裹公式，可以将公式嵌入到文本中，例如：$a^2 + b^2 = c^2$。

2. **块级公式：** 使用一对双美元符号（`$$`）包裹公式，可以单独显示为一行，例如：

$$\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$$。

3. **上标和下标：** 使用 **`^`** 表示上标，使用 **`_`** 表示下标，例如：$x^2$, $a_1$，$m^{i-1}$。

4. **分数：** 使用 **`\frac{分子}{分母}`** 表示分数，例如：$\frac{1}{2}$。

5. **根号：** 使用 **`\sqrt`** 表示根号，例如：$\sqrt{2}$。

6. **求和、积分：** 使用 **`\sum`** 表示求和，使用 **`\int`** 表示积分，例如：$\sum_{i=1}^{n} i$, $\int_{0}^{1} x^2 dx$。

7. **括号和大括号：** 使用 **`()`** 表示小括号，使用 **`[]`** 表示中括号，使用 **`{}`** 表示大括号，例如：$(a + b)$, $[x - y]$, $\{1, 2, 3\}$。

8. **矩阵：** 使用 **`\begin{matrix}`** 和 **`\end{matrix}`** 表示矩阵，例如：
$$
\begin{matrix}
1 & 2 \\
3 & 4
\end{matrix}
$$

9. **数学符号** 
   1. 大于等于：\geq $\geq$ 
   2. 小于等于：\leq
   3. 不等于：\neq
   4. 约等于：\approx
   5. 等价于：\equiv
   6. 属于：\in
   7. 不属于：\notin
   8. 无穷大：\infty
   9.  求和符号：\sum
   10. 积分符号：\int
   11. 微分符号：\frac{d}{dx}
   12. 空集：\emptyset
   13. 存在：\exists
   14. 全称：\forall
   15. 并集：\cup
   16. 交集：\cap
   17. 集合关系：\subseteq, \supseteq
   18. 集合运算：\setminus (集合减法)

这只是一小部分常用数学符号，LaTeX中还有很多其他符号和命令，可以根据具体需要查阅LaTeX文档。
