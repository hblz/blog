css3 动画之坑
============================
前段时间用到了css3的animation 实现图片的上下移动，这个本来是个很简单的动画，正是这简单的动画，让我踩了两个坑，这边记录下。
### Android卡顿和以及IOS抖动现象
现象：在Android机下 图片上下左右移动会出现卡顿，即移动是一步一步移过去而不是很顺畅的滑行；在IOS下动画对应的元素下 如果有包裹子元素，子元素会出现抖动。代码如下：
```html
<div class="man-heart man-heart-active">
    <img class="photo-icon" id="man-user" style="" src="xxx">
    <img class="photo-img" id="man-user-img" style="" src="xx" onerror="this.onerror='';src='xxx'">
    <img class="photo-icon" id="man-defalut-img" style="display: none" src="xxg">
</div>
```
```css
 .man-heart {
        min-height: 80px;
        top: 23%;
        left: 12%;
        z-index: 20;
        position: absolute;
        width: 39%;
        backface-visibility: hidden;
        -webkit-backface-visibility: hidden;

        .photo-icon {
            width: 100%;
            z-index: 21;
            position: absolute;
        }

        .photo-img {
            margin-top: 21.5%;   
            border-radius: 100%;
            width: 49px;
            margin-left: 29.5%;
        }

        @media (width:414px) and (height: 736px){
            .photo-img {
                margin-top: 22.5%;
                width: 54px;
                margin-left: 30%;
            }
        }
    }


    .man-heart-active {
        -webkit-animation: heartmove-and 2s ease-in-out infinite alternate;
        animation: heartmove-and 2s ease-in-out infinite alternate;
    }

  @keyframes heartmove-and {
    from {
        top: 23%;
    }
    to {
        top: 26%;
    }
  }

  @-webkit-keyframes heartmove-and {
      from {
          top: 23%;
      }
      to {
          top: 26%;
      }
  }
```
经过排查以及查询相关文档发现上面的两个现象都是由于使用了from to的移动导致。改成使用transform 的translate来实现移动，就好规避上面的两个现象，修改如下：
```css
@keyframes heartmove-and {
    0% {
        transform: translate(0, 0); 
    }
    100% {
        transform: translate(0, 26%); 
    }
}

@-webkit-keyframes heartmove-and {
    0% {
        -webkit-transform: translate(0, 0); 
    }
    100% {
        -webkit-transform: translate(0, 26%); 
    }
}
```

### 设置display由隐藏到显示 会破坏animation transform的动画，使其动画无法播放,解决方法如下：
```html
<div class="man-heart">
    <img class="photo-icon" id="man-user" style="" src="xxx">
    <img class="photo-img" id="man-user-img" style="" src="xx" onerror="this.onerror='';src='xxx'">
    <img class="photo-icon" id="man-defalut-img" style="display: none" src="xxg">
</div>
```
```css
 .man-heart {
        min-height: 80px;
        top: 23%;
        left: 12%;
        z-index: 20;
        position: absolute;
        width: 39%;
        backface-visibility: hidden;
        -webkit-backface-visibility: hidden;

        .photo-icon {
            width: 100%;
            z-index: 21;
            position: absolute;
        }

        .photo-img {
            margin-top: 21.5%;   
            border-radius: 100%;
            width: 49px;
            margin-left: 29.5%;
        }

        @media (width:414px) and (height: 736px){
            .photo-img {
                margin-top: 22.5%;
                width: 54px;
                margin-left: 30%;
            }
        }
    }


    .man-heart-active {
        -webkit-animation: heartmove-and 2s ease-in-out infinite alternate;
        animation: heartmove-and 2s ease-in-out infinite alternate;
    }
  @keyframes heartmove-and {
      0% {
          transform: translate(0, 0); 
      }
      100% {
          transform: translate(0, 26%); 
      }
  }

  @-webkit-keyframes heartmove-and {
      0% {
          -webkit-transform: translate(0, 0); 
      }
      100% {
          -webkit-transform: translate(0, 26%); 
      }
  }
```
```js

  $('#man-user').show();
  $('#man-user-img').show();
  // 在相应的节点显示出来后，在给其加上动画的样式
  $('.man-heart').addClass('man-heart-active');
```
