---
layout: post
title: How to create a shadow under a div.
wordpress_id: 26
wordpress_url: http://johnweaver.zxdevelopment.com/2008/04/05/how-to-create-a-shadow-under-a-div/
date: 2008-04-05 12:52:01 -07:00
---
I thought that today I would show how to create a shadow beneath a div.

<img style="vertical-align: top;" alt="div element with shadow beneath it" title="div element with shadow beneath it" src="/wp-content/uploads/2008/04/sample1.jpg" />

First create an image of the shadow effect, and slice it into three pieces. The two corners should be the same size and should only be as wide as the curve of the corner necessitates. The third image will be a 1 pixel wide gradient that is as tall as the shadow effect you want is. This is the image that I used for the sample above, and for the dimensions in the CSS below.

<img style="vertical-align: middle;" alt="Shadow" title="Shadow" src="/wp-content/uploads/2008/04/shadow.jpg" />


The following HTML should be at the bottom and within the div that you want the shadow effect for:

```html
<div class="shadow-middle">
<div class="shadow-left"></div>
</div>
<div class="shadow-right"></div>
```

Then you will need the following CSS:

```css
.shadow-middle {
width: 100%;
height: 21px;
background: transparent url(middle.gif) repeat-x;
position: absolute;
bottom: -21px;
left: 0px;
}
.shadow-left {
width: 36px;
height: 21px;
background: transparent url(left.gif) no-repeat;
}
.shadow-right {
position: absolute;
background: transparent url(right.gif) no-repeat;
width: 36px;
height: 21px;
bottom: -21px;
right: 0px;
}
```

Also make sure that the enclosing div has a relative position, so that the above CSS forces the shadow just below the bottom of it. If you have a border around your enclosing div, make sure you adjust the bottom positioning to move the shadow below it too. Enjoy!


Update: I'm so glad you don't have to do this sort of thing any more.
