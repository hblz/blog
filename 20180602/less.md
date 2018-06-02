## less的简单使用介绍
less是一门使用广泛的css预处理语言，通过简单的语法和变量对css进行扩展，减少了很多css的代码量，使得css更易于维护和修改。
### 简单编译
使用NPM安装less：

```bash
npm install -g less
```
安装完成后可以在任何一个终端中使用“lessc”命令，来将.less文件转化成一个css文件

```bash
lessc style.less > style.css
```
### 语法
#### 变量
这是less的一个很重要的特性，你可以像普通编程语言一样创建变量，用变量来储存一些常用的值：比如颜色、字体大小等。

`.less`
```less
@background-color: #ffffff;
@text-color: #1A237E;
p{
  background-color: @background-color;
  color: @text-color;
  padding: 15px;
}
ul{
  background-color: @background-color;
}
li{
  color: @text-color;
}
```
`.css`
```css
p{
  background-color: #ffffff;
  color: #1A237E;
  padding: 15px;
}
ul{
  background-color: #ffffff;
}
li{
  color: #1A237E;
}
```
#### Mixins
你可以把一个已经存在的样式组应用到其他的选择器里面。

 `.less`
```less
#circle{
  background-color: #4CAF50;
  border-radius: 100%;
}
#small-circle{
  width: 50px;
  height: 50px;
  #circle
}
#big-circle{
  width: 100px;
  height: 100px;
  #circle
}
```
`.css`
```css
#circle {
  background-color: #4CAF50;
  border-radius: 100%;
}
#small-circle {
  width: 50px;
  height: 50px;
  background-color: #4CAF50;
  border-radius: 100%;
}
#big-circle {
  width: 100px;
  height: 100px;
  background-color: #4CAF50;
  border-radius: 100%;
}
```
mixins甚至可以接收参数
`.less`
```less
#circle(@size: 25px){
  background-color: #4CAF50;
  border-radius: 100%;
  width: @size;
  height: @size;
}
#small-circle{
  #circle
}
#big-circle{
  #circle(100px)
}
```
`.css`
```css
#small-circle {
  background-color: #4CAF50;
  border-radius: 100%;
  width: 25px;
  height: 25px;
}
#big-circle {
  background-color: #4CAF50;
  border-radius: 100%;
  width: 100px;
  height: 100px;
}
```
#### 嵌套
这是一个很好用的特性，可以帮助你讲html的页面结构和样式表的结构相同，减少代码。

`.less`
```less
ul{
  background-color: #03A9F4;
  padding: 10px;
  list-style: none;
  li{
    background-color: #fff;
    border-radius: 3px;
    margin: 10px 0;
  }
}
```
`.css`
```css
ul {
  background-color: #03A9F4;
  padding: 10px;
  list-style: none;
}
ul li {
  background-color: #fff;
  border-radius: 3px;
  margin: 10px 0;
}
```
#### Operations
less可以对数字和颜色做一些简单的数学计算。

`.less`
```less
@div-width: 100px;
@color: #03A9F4;
div{
  height: 50px;
  display: inline-block;
}
#left{
  width: @div-width;
  background-color: @color - 100;
}
#right{
  width: @div-width * 2;
  background-color: @color;
}
```
`.css`
```css
div {
  height: 50px;
  display: inline-block;
}
#left {
  width: 100px;
  background-color: #004590;
}
#right {
  width: 200px;
  background-color: #03a9f4;
}
```
