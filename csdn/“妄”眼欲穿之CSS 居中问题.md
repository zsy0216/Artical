> 妄：狂妄；
>
> 不会的东西只有怀着一颗狂妄的心，假装能把它看穿吧。
>
> 作为一个什么都不会的小白，为了学习，特别在拿来主义之后写一些对于某些css布局的总结，进一步加深对知识的记忆。知识是人类的共同财富，中华大地之上，皆是华夏儿女的智慧结晶。如心中也怀有拿来主义，可慢慢窃走吧，这不是偷，只是窃罢了。我们的孔大人说过的，窃书罢了，不叫偷。

# CSS居中问题

## 1      text-align：center

文本居中问题（图片）

## 2      align-content

此属性只适用于多行的flex容器，并且当侧轴上有多余空间使flex线对齐

使用前提：flex布局；

> （见“妄”眼欲穿-CSS之flex布局和边框阴影）

任意布局：display:flex;

行内元素：display:inline-flex;         

## 3      居中总结

### 3.1    水平居中

#### 	3.1.1行内元素居中（文本（text）、图片（img）、按钮等）

text-align:center;

#### 	3.1.2块状元素居中

1.  定宽块状元素居中

满足宽度（块状元素的宽度width为固定值）和块状两个条件的元素可以通过左右margin值为auto来实现居中。

```html
.div1{
　　　　 width:200px;
　　　　 border:1px solid red;
        margin:0 auto;
     }
<div class="div1">Hello world</div>
```

2. 不定宽块状元素居中

在实际工作中我们会遇到需要为“不定宽度的块状元素”设置居中，比如网页上的分页导航，因为分页的数量是不确定的，所以我们不能通过设置宽度来限制它的弹性。(不定宽块状元素：块状元素的宽度width不固定。)

​        i. 改变块级元素的display为inline类型（设置为行内元素显示），然后使用text-align实现居中效果。

​       ii. 通过给父元素设置float，然后给父元素设置position：relative和left：50%，子元素设置position：relative和left：-50%来实现水平居中。

### 3.2    垂直居中

1. 父元素高度确定的单行文本

   ​	通过设置父元素的height和line-height高度一致来实现的（height为元素的高度，line-height为行高，指在文本中，行与行之间的基线间的距离）。

2. 父元素高度确定的多行文本

   ​	使用插入table（包括body、tr、td）标签，同时设置vertical-align：middle。

### 3.3    水平垂直居中

```
#login_box {
      width: 300px;
      height: 150px;
      border: 1px solid #ccc;
      position: absolute;
      left: 50%;top: 50%;
      margin-left: -150px;
      margin-top: -75px;
    }
```

这几行在#login_box中的目的是使其在浏览器中水平居中显示。（无论浏览器被如何拉伸，该login_box始终保持居中）

原理（对齐浏览器的中心点与#login_box的中心点，这样就使得.foot在浏览器中水平居中了）：

left:50%;  使#login_box左边框距离浏览器左边50% ，所以login_box整体并不是位于浏览器中间，

需要使用margin-left: -150px;  使#login_box向左移动150像素（即login_box自身宽度的一半）。这样box整体就位于浏览器中间了。

top: 50%;   margin-top: -75px 的道理同上。 