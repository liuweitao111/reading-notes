# 技巧
* 页面某个模块的文字内容是动态的，可能是几个字，也可能是一句话。然后，希望文字少的时候居中显示，文字超过一行的时候居左显示。该如何实现？

解析：利用inline-block元素、浮动元素以及绝对定位元素都具有包裹性（元素尺寸由内部元素决定，但永远小于“包含块”容器的尺寸）。

[codepen](https://codepen.io/liuweitao/pen/MWKGKEX)

HTML
```html
<div class="box">
  <p class="content">页面某个模块的文字内容是动态的，可能是几个字，也可能是一句话。然后，希望文字少的时候居中显示，文字超过一行的时候居左显示。该如何实现？</p>
</div>
```
CSS
```css
.box {
  padding: 10px;
  width: 200px;
  background-color: #cd0000;
  text-align: center;
}
.content {
  display: inline-block;
  text-align: left;
}
```
* 实现任意高度元素的展开收起

解析： 可以添加一个隐藏的checkbox，然后通过设置选中的样式实现展开和收起。

[codepen](https://codepen.io/liuweitao/pen/NWxMxzw)

HTML
```html
<div class="box">
    <input id="check" type="checkbox">
    <p>标题</p>
    <div class="element">
        <p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
    </div>
    <label for="check" class="check-in">更多↓</label>
    <label for="check" class="check-out">收起↑</label>
</div>
```
CSS
```css
 /* 隐藏checkbox */ 
input[type="checkbox"] {
  position: absolute;
  clip: rect(0 0 0 0);
}
.element {
  display: none;
}
.check-out {
  display: none;
}  
:checked~.element {
  display: block; 
}
:checked~.check-out {
  display: block;
}
:checked~.check-in {
  display: none;
}
```
* 实现任意高度元素的展开收起（带有动画效果）

解析： 这里需要注意，如果给element设置height=0，:checked~.element中height=auto，也能实现展开和收起，但是没有动画效果。

HTML
```html
<div class="box">
    <input id="check" type="checkbox">
    <p>标题</p>
    <div class="element">
        <p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
    </div>
    <label for="check" class="check-in">更多↓</label>
    <label for="check" class="check-out">收起↑</label>
</div>
```
CSS
```css
 /* 隐藏checkbox */ 
input[type="checkbox"] {
  position: absolute;
  clip: rect(0 0 0 0);
}
.element {
  max-height: 0;
  overflow: hidden;
  transition: max-height .25s;
}
/* 设置一个很大的高度 */ 
:checked~.element {
  max-height: 999px; 
}
.check-out {
  display: none;
}  
:checked~.check-out {
  display: block;
}
:checked~.check-in {
  display: none;
}
```
* 基于 vertical-align 属性的水平垂直居中弹框

解析： 首先借助伪元素创建了一个和外部容器一样高的宽度为0的inline-block元素，设置vertical-align: middle使x的中心点在容器的垂直中心位置，再给dialog设置vertical-align: middle;

[codepen](https://codepen.io/liuweitao/pen/XWXqXBG)

HTML
```html
<div class="container">
  <div class="dialog">我是一个对话框</div>
</div>
```
CSS
```css
.container {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0,0,0,.5);
  text-align: center;
  font-size: 0;
  overflow: auto; /* 即便高度超出，也能正常显示*/
  z-index: 99;
}
.container:after{
  content: '';
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
.dialog {
  display: inline-block;
  vertical-align: middle;
  font-size: 16px;
  border-radius: 6px;
  background-color: #fff;
  padding: 20px 20px;
}
```