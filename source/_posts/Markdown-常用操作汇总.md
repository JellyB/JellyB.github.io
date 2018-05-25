title: Markdown 常用操作汇总
author: jelly
tags:
  - markdown
categories:
  - markdown
date: 2018-05-25 15:56:00
---
#### 1 标题
使用符号 `#` 表示

\# 一级标题效果
# 一级标题效果
\## 二级标题效果
## 二级标题效果
\### 三级标题效果
### 三级标题效果
\#### 四级标题效果
#### 四级标题效果
\##### 五级标题效果
##### 五级标题效果
\###### 六级标题效果
###### 六级标题效果


#### 2 粗体字 & 斜体字
粗体字（使用两个星号），斜体字（使用一个星号）

`**我是粗体字**`
**我是粗体字**

`我是正常字`
我是正常字

`*我是斜体字*`
*我是斜体字*

#### 3 有序列表
（点后有一个空格，此处写了11. 直接回车生成了12)

`11. 测试，并回车`
`12. 回车自动生成12号有序列表`

11. 测试，并回车
12. 回车自动生成12号有序列表

#### 4 无序列表
使用  `-` ，`*` 表示，回车不会自动生成 123 序号

`- 测试，并回车demo`
`- 自动生成了当前列表`

- 测试，并回车demo
- 自动生成了当前列表

`* 测试，并回车demo2`
`* 自动生成了当前列表`

* 测试，并回车demo2
* 自动生成了当前列表

`埋个点：`
  🐸 <span id="comeHere">`<span id="comeHere">`

#### 5 引用
使用 `>` 表示

\> 鲁迅说："世上本没有路，走的人多了，便有了套路。"
> 鲁迅说："世上本没有路，走的人多了，便有了套路。"

#### 6 分隔线
使用三个星表示，`***`

\*\*\*
哈哈我们搞独立了!\0/
\*\*\*

***
哈哈我们搞独立了!\0/
***

#### 7 代码 | 脚本
使用三个 \`\`\` 表示

\`\`\`java
System.out.println("markdown 从入门到精通~~");
\`\`\`

``` java
System.out.println("markdown 从入门到精通~~");
```

\`\`\`bash
sudo sh zkServer start
\`\`\`

``` bash
sudo sh zkServer start
```

#### 8 表格
使用 | 表示，注意 | 与 -，： 中间有一个空格
| --: | 居右
| :-- | 居左
| :--: | 居中

`| name | age | address | job |`
`| --: | --: | :---: | --- |`
`| zhangsna| 20 | 中关村 | java攻城狮 |`
`| liusi | 30 | 望京 | python 攻城狮 |`
`| wangwu | 40 | 家里蹲 | 三人CEO |`


| name | age | address | job |
| --: | --: | :---: | --- |
| zhangsna| 20 | 中关村 | java攻城狮 |
| liusi | 30 | 望京 | python 攻城狮 |
| wangwu | 40 | 家里蹲 | 三人CEO |


####  9 转义字符
使用反斜杠 `\`，比如想要转义 \`，使用 \\\`

\\\`我的衣服呢\\\`
\`我的衣服呢\`

#### 10 网址链接
`使用  [显示的内容](url)  表示`

\[我的博客](https://jellyb.github.io/)
[我的博客](https://jellyb.github.io/)

#### 11 页内跳转
首先需要定义一个`<span id="comeHere">`，见上面小🐸

\[显示内容](#comeHere)
[送我去见小蛤蟆](#comeHere)

#### 12 引用图片
`使用 ![显示的内容](图片url)  表示`
\!\[小手一转](http://p97p9srfe.bkt.clouddn.com/give%20me%20a%20little.JPG)

![小手一转](http://p97p9srfe.bkt.clouddn.com/give%20me%20a%20little.JPG)

#### 13 图片添加超链接
{插入图片-->配置文字链接}=>图片插入超链接，只要最后一步即可：
插入图片：
\!\[my blog](http://p97p9srfe.bkt.clouddn.com/my%20wx%20pic.JPG "欢迎光临我的博客！")配置文字链接：
\[我的博客](https://jellyb.github.io/)
图片插入超链接（最后只保留这一条即可实现效果）：
\[\!\[my blog](http://p97p9srfe.bkt.clouddn.com/my%20wx%20pic.JPG "欢迎光临我的博客！")](https://jellyb.github.io/)
效果如下：

[![my blog](http://p97p9srfe.bkt.clouddn.com/my%20wx%20pic.JPG "欢迎光临我的博客！")](https://jellyb.github.io/)

推荐一款mac版markdown编辑软件MWEB，使用command + R 快捷键可以再编辑及预览页面切换



