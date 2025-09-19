---
title: markdown语法
date: 2024-12-25 15:22:36
tags: 代码分享
cover: /img/02.jpg
---
# *markdown* 基础语法
当我们刚开始接触编程时或许并不了解编程语言都有哪些，总是把markdown语法当成一种编程语言，严格意义上其实并不是这样的，那么它究竟是什么，为什么总是被广泛地运用？
## 什么是 *markdown* 语法 ？
Markdown 是一种轻量级标记语言，常用于编写文档、格式化文本，尤其是在代码库、博客、论坛等场景下。它语法简单，易于书写和阅读，同时可以转换为HTML等格式，广泛应用于技术文档和说明文件中（例如README文件）。
## 我们该如何使用 *markdown* 语法 ？
_________________
### 标题的创建和使用
#### 使用方法
标题分为一级标题，二级标题和三级标题，使用时在想要当作标题的文字前打上井号键。
| markdown语法 | 
|-------------|
| ```# 这是一级标题 ```| 
| `## 这是二级标题 `| 
| `### 这是三级标题 `| 

剩下四级五级标题依此类推即可，我们也可以通过在文字后换行添加任意数量等号来设置一级标题，任意数量----为二级标题。
#### 注意事项
切记在 # 号后添加空格，不然无法达成标题的效果。
| ✅  Do this | ❌  Don't do this |
|-------------|----------|
| `# austin我要给你爆金币 `| `#austin我要给你爆金币` |
_________________
### 段落语法
#### 使用方法
要创建段落，请使用空白行将一行或多行文本进行分隔。
| markdown语法 | 预览效果 |
|-------------|----------|
| ```The stars above, a gentle glow,A quiet world where dreams can grow.```<br><br>```The breeze that whispers, soft and light,Carries hope into the night.```| The stars above, a gentle glow,A quiet world where dreams can grow.<br><br> The breeze that whispers, soft and light,Carries hope into the night.|
#### 注意事项
我们在使用时并不需要在每一个段落前添加缩进，或者空格。
| ✅  Do this | ❌  Don't do this |
|-------------|----------|
| ```The stars above, a gentle glow,A quiet world where dreams can grow.```<br><br>```The breeze that whispers, soft and light,Carries hope into the night.```|  &nbsp;&nbsp;&nbsp;&nbsp;The stars above, a gentle glow,A quiet world where dreams can grow.<br><br>&nbsp;&nbsp;&nbsp;&nbsp;The breeze that whispers, soft and light,Carries hope into the night. |
_________________
### 强调语法
#### 粗体使用方法
在使用加粗语法时我们在需要强调的文字周围添加两个*号或者_达成理想的效果。
| markdown语法 | 预览效果 |
|-------------|----------|
|```you **are** the best ```|you <strong> are </strong> the best  |
| ```you __are__  ```|you <strong> are </strong>  |
#### 粗体注意事项
由于各种markdown编译器的不同__的方法并不能很好地适配，因此在句中时尽量使用*号。
| ✅  Do this | ❌  Don't do this |
|-------------|----------|
| ```嗨嗨，**austin**我要给你爆金币 ```| ```嗨嗨，__austin__我要给你爆金币``` |
#### 斜体使用方法
在使用加粗语法时我们在需要强调的文字周围添加一个*号或者_达成理想的效果，中间不需要带空格。
| markdown语法 | 预览效果 |
|-------------|----------|
|```you *are* the best ```|you <em> are </em> the best  |
| ```you _are_  ```|you <em> are </em>  |
#### 两者的同时使用
要同时用粗体和斜体突出显示文本，请在单词或短语的前后各添加三个星号或下划线。要加粗并用斜体显示单词或短语的中间部分，请在要突出显示的部分前后各添加三个星号，中间不要带空格。
| markdown语法 | 预览效果 |
|-------------|----------|
|```you ***are*** the best ```|you <strong> are </strong> the best  |
| ```you __are__  ```|you <strong> are </strong>  |
_________________
### 引用语法
当我们需要引用某句话时，我们需要用引用语法，即在引用语句前加下划线的方式进行引用。
```> 示例```
渲染效果如下所示
> 示例
#### 多个段落的块引用
块引用可以包含多个段落。为段落之间的空白行添加一个 > 符号。
```
> 1
> 2
> 3
```
渲染效果
> 1
> 2
> 3
#### 嵌套块引用
块引用可以嵌套。在要嵌套的段落前添加一个 >> 符号。
```
> 第一层
>> 第二层1
>> 第二层2
```
渲染效果
> 第一层
>> 第二层1
>> 第二层2
#### 带有其它元素的块引用
块引用可以包含其他 Markdown 格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效。
```
>  The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
>
>  *Everything* is going according to **plan**.
```
实际渲染效果如下
>  The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
    >
    >  *Everything* is going according to **plan**.
> - _________________

### 代码语法
#### 使用语法
我们想要将代码添加到markdown语法中，就必须使用反引号包裹在它的周围。
| markdown语法 | 预览效果 |
|-------------|----------|
| python :`` `import pandas`  ``|python : <code>`import pandas`</code>  |
#### 转义用法
如果你要表示为代码的单词或短语中包含一个或多个反引号，则可以通过将单词或短语包裹在双反引号(``)中。
| markdown语法 | 预览效果 |
|-------------|----------|
|``` ``import `pandas` `` ``` |``import `pandas` ``   |
#### 围栏式代码块
> ````
> ```
> import pandas
> print('hello world')
> ```
> ````
预览效果如下
 ```
 import pandas
 print('hello world')
 ```
_________________
### 图片语法
插入图片Markdown语法代码：``` ![图片alt](图片链接)。```
例如 ``` ![这是一张示例图](/img/zqs.png) ```
渲染效果如下：
![这是一张示例图](/img/zqs.png)
#### 给图片增加链接
给图片增加链接，请将图像的Markdown 括在方括号中，然后将链接添加在圆括号中。
例如 ``` [![这是一张示例图](/img/zqs.png)](https://austinnumber2.xin) ```
渲染效果如下：
[![这是一张示例图](/img/zqs.png)](https://austinnumber2.xin)
_________________
## 链接语法
```这是一个链接 [austin blog](https://austinnumber2.xin)。```
实际渲染效果:这是一个链接 [austin blog](https://austinnumber2.xin)。
如果我们需要给链接进行一个备注，就在括号内用引号加上备注即可。
```这是一个链接 [austin blog](https://austinnumber2.xin "上大皇帝的blog")。```
实际渲染效果：[austin blog](https://austinnumber2.xin "上大皇帝的blog")。(鼠标放在文字上)

## 总结
以上是使用markdown最最基础的语法，对于撰写个人博客来说绰绰有余，markdown本身只是一个传达思想的工具，语法背后的内容是否有价值，我想才是最重要的。












