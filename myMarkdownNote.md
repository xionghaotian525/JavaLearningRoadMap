# markdown 速查表

## 目录

- [一、VScode 插件](#VScode插件)
- [二、快捷键](#快捷键)
- [三、常用语法](#常用语法)
  - [1.标题](#标题)
  - [2.粗体](#快捷键)
  - [3.斜体](#斜体)
  - [4.引用](#引用)
  - [5.有序列表](#有序列表)
  - [6.无序列表](#无序)
  - [7.代码](#代码)
  - [8.分割线](#分隔线)
  - [9.链接](#链接)
  - [10.图片](#图片)
  - [11.表格](#表格)
  - [12.代码块](#代码块)
  - [13.删除线](#删除线)
  - [14.任务列表](#任务列表)

**VScode插件**
1. Markdown preview enhanced
2. Markdown All In One
3. Markdown PDF

##### 快捷键

- Ctrl + B	Toggle bold  切换粗体
- Ctrl + I	Toggle italic  切换斜体
- Alt + S	Toggle strikethrough  切换删除线
- Ctrl + Shift + ]	Toggle heading (uplevel)   标题（上升）
- Ctrl + Shift + [	Toggle heading (downlevel)   标题（下降）
- Alt + C	Check/Uncheck task list item   选表单切换
- Ctrl + d 选定多个相同的单词
- Ctrl + Alt + 上箭头/下箭头

##### 常用语法

1. ###### 标题
    >
    > - **\#\#\#\#\#** 和普通的  **\*\*\*\*** 加粗效果相同，字体大小相等
    > - 语法： \#text
    > 
    # H1
    ## H2
    ### H3
    #### H4
    ##### H5

2. ###### 粗体
    > - 快捷键Ctrl+B
    > - 语法：\*\*text\*\* 
    **bold text**

3. ###### 斜体
    > - 加粗倾斜，使用 \*\*\*text\*\*\* => ***text***
    > - 语法：\*text\* => *text*
    *italicized text*

4. ###### 引用
    > 语法： \>
> blockquote
> > blockquote  
>
> blockquote
> blockquote


5. ###### 有序列表
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
6. ###### 无序列表
> 和有序列表相同，**tab键**继续向内嵌套，**删除键**跳出一级嵌套
> 
   - First item
     - item1
     - item2
   - Second item
   - Third item
7. ###### 代码
> - 如果想加粗 \` \` 中的代码，在反引号外面嵌套 \** **, 倾斜嵌套 * *，
> - 引用一个语法时是使用转义：\\或者\\\\
> - 语法：\` \` (反引号) 
>
`coding` is important
***`Java`** is the best language in the world*
**`damn`**
`**damn**`

8. ###### 分隔线
> 语法：\-\-\-
---

9. ###### 链接
> 语法：\[显示名]\(超链接地址 "title")

[my github page](https://github.com/xionghaotian525/ "title") 

10. ###### 图片
> 语法：\!\[图片alt]\(图片链接 "title")    

![alt text](/Res/images/集合接口继承关系和实现.png "title")
![alt text](/Res/images/并发编程知识库.png "title")

11. ###### 表格
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
12.    ###### 代码块
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
13. ###### 删除线
> 语法： \~~ text \~~

~~The world is flat.~~

14. ###### 任务列表

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media



