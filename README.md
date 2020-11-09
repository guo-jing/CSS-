# CSS 等比缩放元素

今天被翔总问了个问题，如何让一个元素以16:9的宽高比例进行等比缩放（不能用 js）。



当时完全不知道怎么做，回来在网上查了一下，解决问题的关键是 **padding 属性的百分比都是基于包含块的 width 值**（今天也被问了包含块的定义的问题，也说不清楚🤣）。



解决思路如下：

1. 首先目标元素的宽度和高度要始终保持同一比例。
2. 通过 padding-top 和 padding-left 属性的百分比都是基于包含块的 width 值这一特点，使 padding-top 和 padding-left 具有相同的基数。设置 padding-left: 100%; padding-top: calc(100% * 9 / 16);，可使 padding-left 和 padding-top 比例始终保持 16:9。
3. 将上面描述的元素放到一个宽度自适应的 div 里面，元素的 padding-top 就可以把父元素的高度撑开，padding-left 和 padding-top 的比例刚好是 16:9：

```html
<style>
    #box1 {
        border: 1px solid red;
        width: 100%;
    }
    #box2 {
        padding-left: 100%;
        padding-top: calc(100% * 9 / 16);
    }
</style>
<div id="box1">
    <div id="box2"></div>
</div>
```

![image-20201110034004781](/Users/guojing/Library/Application Support/typora-user-images/image-20201110034004781.png)

4. 现在已经有一个等比缩放的元素了，我们只需要在这个元素里生成一个 width: 100%; height: 100%; 的子元素用来展示内容就 OK 了：

   ```html
   <style>
       #box1 {
           position: relative;
           width: 100%;
       }
       #box2 {
           padding-left: 100%;
           padding-top: calc(100% * 9 / 16);
       }
       #box3 {
           position: absolute;
           top: 0;
           width: 100%;
           height: 100%;
           border: 10px solid green;
           box-sizing: border-box;
       }
   </style>
   <div id="box1">
       <div id="box2"></div>
       <div id="box3"></div>
   </div>
   ```

5. 最后再优化一下代码：

   ```html
   <style>
       #parent {
           position: relative;
           width: 100%;
       }
       #parent:before {
           content: '';
           display: block;    
           padding-left: 100%;
           padding-top: calc(100% * 9 / 16);
       }
       #box {
           position: absolute;
           top: 0;
           width: 100%;
           height: 100%;
           border: 10px solid green;
           box-sizing: border-box;
       }
   </style>
   <div id="parent">
       <div id="box"></div>
   </div>
   <script>
       const domBox = document.getElementById('box');
       window.onresize = () => {
           domBox.innerHTML = '<p>offsetWidth: ' + domBox.offsetWidth + '</p>' +
               '<p>offsetHeight: ' + domBox.offsetHeight + '</p>' +
               '<p>offsetWidth / offsetHeight: ' + domBox.offsetWidth / domBox.offsetHeight + '</p>'
       }
   </script>
   ```

6. 演示：![Kapture 2020-11-10 at 04.08.03](/Users/guojing/Movies/Kaptures/Kapture 2020-11-10 at 04.08.03.gif)



# 总结

1. 翔总，yyds！
2. 该看看 content block 了。
