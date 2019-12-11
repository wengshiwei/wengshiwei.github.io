---
layout: post-cn
title: CSS Center Recap
category:
- css
- chinese
---

看到一篇详尽的讲解CSS居中的文章 [Centering in CSS: A Complete Guide (2014-09-12)](http://css-tricks.com/centering-css-complete-guide/)
这篇文章分类说明了CSS“水平”“垂直”“水平加垂直”三种居中的情况。我这篇Recap先是扼要重述了原文的观点，再补充几句闲话。

### 1 水平居中

对于行内元素(如a、span等)，水平居中方法是**为元素的container添加text-align: center**

对于单个块级元素(如div)，水平居中方法是**为元素添加margin: 0 auto**

对于多个块级元素(如在一行内居中显示多个div)，水平居中方法是**为元素的container添加text-align: center，同时为元素添加display: inline-block** 或者 **为元素的container添加display: flex和
justify-content: center**

怎么理解呢？行内元素不填充满其container的宽度，所以由其container设置text-align，将行内元素视作文本而居中。块级元素填充满其container的宽度，所以由行内元素自身的左右margin来补足元素与其container宽度的差。只要左右补足的margin相同，则元素本身在其container中间。

最后一条，其原理类似与将块级元素转化为行内元素。flex可理解为smart and pretty inline-block。关于flex具体，可见[A Complete Guide to Flexbox](http://css-tricks.com/snippets/css/a-guide-to-flexbox/)和[caniuse flexbox](http://caniuse.com/#search=flexbox)...

### 2 垂直居中

对于单行的行内元素(如a、span等)，垂直居中的方法是**为元素设置相同的上下padding**。如果设置padding的方法失效，那么可以**为行内元素添加相同的height和line-height**

对于多行的行内元素，同上。如果上面方法不生效，则可以**为元素的container添加display: table，为元素添加display: table-cell和vertical-align: middle**

注意vertical-align是一条神奇的属性。对于img等行内元素，它指的是img和这一行文字的baseline对齐的情况；对于table cell，它指的是cell内容垂直对齐的情况。上面一条方法就是利用了vertical-align: middle的table-cell的在table中垂直居中的默认属性实现的。

同样，将flexbox在这里也可以使用，方法是**为contanier添加display: flex; justify-content: center; flex-direction: column**

如果这些都不起作用，可以使用称作"ghost element"的CSS Hack，具体见[原文](http://css-tricks.com/centering-css-complete-guide/#vertical-inline-multiple)。

对于块级元素，如果知道元素的高度且高度不变，垂直居中的方法是**为元素container添加position: relative，为元素添加position: absolute; top: 50%; height: 100px; margin-top: -50px**。这种为container添加position: relative，为元素添加position: absolute的方法，下文简称为"relative-absolute hack"

对于块级元素，如果不知道元素的高度，垂直居中的方法是**relative-absolute hack，再为元素添加top: 50%; transform: translateY(-50%)**

同样可以使用flexbox，方法是**为container添加display: flex; flex-direction: column; justify-content: center**

对于垂直居中，无论是对于行内元素还是块级元素，flexbox是万能的解决方案。

### 3 水平加垂直居中

方法1加方法2，总有一款适合你。

如果元素的宽度和高度固定，那么居中的方法是**relative-absolute hack，再为元素添加top: 50%; left: 50%;(注意width和height也需要被指定)**，同理，如果元素的宽度和高度未知，那么居中的方法是**relative-absolute hack，再为元素添加top: 50%; left: 50%; transform: translate(-50%, -50%);**

当然，flexbox的方法依然有效，居中的方法是**为元素container添加display: flex; justify-content: center; align-items: center;**

### 4 闲话

上文简单recap了[Centering in CSS: A Complete Guide (2014-09-12)](http://css-tricks.com/centering-css-complete-guide/)这篇文章的内容。

对于居中问题，首先要看的是需要居中的元素是**行内元素**还是**块级元素**，其次是需不需要涉及到元素的container，再次是元素的宽高是否限定。有不需要知道宽高也能实现居中的方法，所以依赖具体像素大小的方法是不推荐的。

flexbox是解决居中问题的万灵药。




