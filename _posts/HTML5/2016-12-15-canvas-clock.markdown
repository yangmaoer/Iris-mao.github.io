---
layout: post
title:  "Canvas绘制时钟 "
date:   2016-12-15 12:01:19 +0800
categories: jekyll HTML5
tags: HTML5
---

#### Canvas绘制时钟
* 背景图的绘制（大圆、数字、小圆点），掌握基础知识：圆的绘制（arc方法），关于圆的弧度的计算，数学中关于`sin cos`的用法

![哈哈](http://localhost:5001/source/my_pics/craft.png)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Canvas Clock</title>
</head>
<body>
<div style="margin:100px 100px;">
    <canvas id="clock" height="200" width="200"></canvas>
</div>
</body>
<script>
    var obj = document.getElementById('clock');
    var ctx = obj.getContext('2d');
    var width = ctx.canvas.width;
    var height = ctx.canvas.height;
    var r = width / 2;    //圆的半径为宽度的一半
    var rem = width / 200;  //比例，是为了在放大canvas大小的时候，其他的样式也一起放大，一开始做的时候是以200px为标准的

    //画圆
    function drawBackground() {
        ctx.save();
        ctx.translate(r, r);    //改变原点的位置
        ctx.beginPath();
        ctx.lineWidth = 10 * rem;
        //以0，0为原点，r为半径，0为起始角，2*Math.PI为结束角，顺时针画圆
        ctx.arc(0, 0, r - ctx.lineWidth / 2, 0, 2 * Math.PI, false);
        ctx.stroke();

        var hourNumber = [3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2];   //因为画圆是以3点的位置为起始点的，所以3放在第一个
        ctx.font = 18 * rem + 'px Arial';
        ctx.textAlign = 'center';   //将数字的位置摆正
        ctx.textBaseline = 'middle';
        //画出1-12的数字
        hourNumber.forEach(function (number, i) {
            var rad = 2 * Math.PI / 12 * i;
            var x = Math.cos(rad) * (r - 30 * rem);
            var y = Math.sin(rad) * (r - 30 * rem);
            ctx.fillText(number, x, y);
        });
        //画出秒针走动的60个点
        for (var i = 0; i < 60; i++) {
            var rad = 2 * Math.PI / 60 * i;
            var x = Math.cos(rad) * (r - 18 * rem);
            var y = Math.sin(rad) * (r - 18 * rem);
            ctx.beginPath();
            if (i % 5 === 0) {   //整点的圆圈是黑色的，非整点的部分是灰色的
                ctx.fillStyle = '#000';
                ctx.arc(x, y, 2 * rem, 0, 2, 2 * Math.PI, false);
            } else {
                ctx.fillStyle = '#ccc';
                ctx.arc(x, y, 2 * rem, 0, 2, 2 * Math.PI, false);
            }
            ctx.fill();
        }
    }

    drawBackground()；
</script>
```

* 时分秒针以及中心原点的绘制，其中时针和分针就是绘制一个直线即可，秒针则画一个梯形样式呈现又粗到细的效果，要掌握的基础知识：直线的绘制，旋转角度的控制（rotate以弧度为单位），时钟的动态走动效果，则用`setInterval`函数控制每一秒钟执行一次绘制时钟的函数。
```
//绘制时针
    function drawHour(hour, minute) {
        ctx.save();
        ctx.beginPath();
        var rad = 2 * Math.PI / 12 * hour;   //计算时钟转动的弧度
        var mrad = 2 * Math.PI / 12 / 60 * minute;   //计算分针转动的弧度
        ctx.rotate(rad + mrad);
        ctx.lineWidth = 6;
        ctx.lineCap = 'round';
        ctx.moveTo(0, 10 * rem);
        ctx.lineTo(0, -r / 2);
        ctx.stroke();
        ctx.restore();
    }

    //绘制分针
    function drawMinute(minute) {
        ctx.save();
        ctx.beginPath();
        var rad = 2 * Math.PI / 60 * minute;    //计算分针转动的弧度
        ctx.rotate(rad);
        ctx.lineWidth = 3 * rem;
        ctx.lineCap = 'round';
        ctx.moveTo(0, 10);
        ctx.lineTo(0, -r + 30 * rem);
        ctx.stroke();
        ctx.restore();
    }

    //绘制秒针
    function drawSecond(second) {
        ctx.save();
        ctx.beginPath();
        ctx.fillStyle = '#c14443';
        var rad = 2 * Math.PI / 60 * second;    //计算秒针转动的弧度
        ctx.rotate(rad);
        ctx.moveTo(-2, 20 * rem);
        ctx.lineTo(2, 20 * rem);
        ctx.lineTo(1, -r + 18 * rem);
        ctx.lineTo(-1, -r + 18 * rem);
        ctx.fill();
        ctx.restore();
    }

    //画时钟上的中心白色原点
    function drawDot() {
        ctx.beginPath();
        ctx.fillStyle = '#fff';
        ctx.arc(0, 0, 3 * rem, 0, 2 * Math.PI, false);
        ctx.fill();
    }

    function draw() {
        ctx.clearRect(0, 0, width, height);    //每一次都要将canvas清空一下，不然秒针就会一直显示在canvas上面
        var now = new Date();   //获得当前时间
        var hour = now.getHours();
        var minutes = now.getMinutes();
        var seconds = now.getSeconds();
        drawBackground();
        drawHour(hour, minutes);
        drawMinute(minutes);
        drawSecond(seconds);
        drawDot();
        ctx.restore();
    }

    setInterval(draw, 1000);   //每秒执行一次
```
* 完整代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Canvas Clock</title>
</head>
<body>
<div style="margin:100px 100px;">
    <canvas id="clock" height="200" width="200"></canvas>
</div>
</body>
<script>
    var obj = document.getElementById('clock');
    var ctx = obj.getContext('2d');
    var width = ctx.canvas.width;
    var height = ctx.canvas.height;
    var r = width / 2;
    var rem = width / 200;  //比例

    //画圆
    function drawBackground() {
        ctx.save();
        ctx.translate(r, r);
        ctx.beginPath();
        ctx.lineWidth = 10 * rem;
        //以0，0为原点，r为半径，0为起始角，2*Math.PI为结束角，顺时针画圆
        ctx.arc(0, 0, r - ctx.lineWidth / 2, 0, 2 * Math.PI, false);
        ctx.stroke();

        var hourNumber = [3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2];
        ctx.font = 18 * rem + 'px Arial';
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        //画出1-12的数字
        hourNumber.forEach(function (number, i) {
            var rad = 2 * Math.PI / 12 * i;
            var x = Math.cos(rad) * (r - 30 * rem);
            var y = Math.sin(rad) * (r - 30 * rem);
            ctx.fillText(number, x, y);
        });
        //画出秒针走动的60个点
        for (var i = 0; i < 60; i++) {
            var rad = 2 * Math.PI / 60 * i;
            var x = Math.cos(rad) * (r - 18 * rem);
            var y = Math.sin(rad) * (r - 18 * rem);
            ctx.beginPath();
            if (i % 5 === 0) {
                ctx.fillStyle = '#000';
                ctx.arc(x, y, 2 * rem, 0, 2, 2 * Math.PI, false);
            } else {
                ctx.fillStyle = '#ccc';
                ctx.arc(x, y, 2 * rem, 0, 2, 2 * Math.PI, false);
            }
            ctx.fill();
        }
    }

    //绘制时针
    function drawHour(hour, minute) {
        ctx.save();
        ctx.beginPath();
        var rad = 2 * Math.PI / 12 * hour;
        var mrad = 2 * Math.PI / 12 / 60 * minute;
        ctx.rotate(rad + mrad);
        ctx.lineWidth = 6;
        ctx.lineCap = 'round';
        ctx.moveTo(0, 10 * rem);
        ctx.lineTo(0, -r / 2);
        ctx.stroke();
        ctx.restore();
    }

    //绘制分针
    function drawMinute(minute) {
        ctx.save();
        ctx.beginPath();
        var rad = 2 * Math.PI / 60 * minute;
        ctx.rotate(rad);
        ctx.lineWidth = 3 * rem;
        ctx.lineCap = 'round';
        ctx.moveTo(0, 10);
        ctx.lineTo(0, -r + 30 * rem);
        ctx.stroke();
        ctx.restore();
    }

    //绘制秒针
    function drawSecond(second) {
        ctx.save();
        ctx.beginPath();
        ctx.fillStyle = '#c14443';
        var rad = 2 * Math.PI / 60 * second;
        ctx.rotate(rad);
        ctx.moveTo(-2, 20 * rem);
        ctx.lineTo(2, 20 * rem);
        ctx.lineTo(1, -r + 18 * rem);
        ctx.lineTo(-1, -r + 18 * rem);
        ctx.fill();
        ctx.restore();
    }

    //画时钟上的中心白色原点
    function drawDot() {
        ctx.beginPath();
        ctx.fillStyle = '#fff';
        ctx.arc(0, 0, 3 * rem, 0, 2 * Math.PI, false);
        ctx.fill();**_``_**
    }

    function draw() {
        ctx.clearRect(0, 0, width, height);
        var now = new Date();
        var hour = now.getHours();
        var minutes = now.getMinutes();
        var seconds = now.getSeconds();
        drawBackground();
        drawHour(hour, minutes);
        drawMinute(minutes);
        drawSecond(seconds);
        drawDot();
        ctx.restore();
    }

    setInterval(draw, 1000);   //每秒执行一次
</script>
```
![哈哈](http://localhost:5001/source/my_pics/result.png)