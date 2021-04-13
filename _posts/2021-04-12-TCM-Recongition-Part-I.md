---


layout: post
title: TCM Recongition
subtitle: Part I
tags: [Project, Deep Learning]

---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>



## TCM Recongition Part I


TCM(Tradition Chinese Medicine) Recongition System 是本年度的创新创业大赛项目，旨在完成以下任务，中药识别，中药质量鉴定，中药真伪鉴定，中药产地鉴定。使用 Deep Learning Technology 来完成任务。大概画了一个导图。

![TCM1](/img/TCM1.png)


### 1.Qt
因为药材还在路上,所以先做准备工作，主要就是数据采集，这次打算做一个可视化工具做这个事情，按键拍照，选择类别，图像命名。
####1.1 设计
以前只用tkinter做过界面，有点简陋，这次就用qt来做。具体Qt的基础知识在这里就不说了，只说重要的。具体的设计如下，大概画了一下，主要就是显示照片，两个按钮，一个填类别的地方。

![TCM2](/img/TCM2.png)

想了想做UI没有价值，直接做成web app显然更好。

#### 1.2 实现

这个项目只要实现就ok,今晚想办法解决。用JS + Html结合实现。

```javascript
<!doctype html>
<html lang="en">
<head>
    <title>js调用摄像头拍照上传图片</title>
    <meta charset="utf-8">
</head>
<body>
<button onclick="openMedia()">开启摄像头</button>
<video id="video" width="500px" height="500px" autoplay="autoplay"></video>
<canvas id="canvas" width="500px" height="500px"></canvas>
<button onclick="takePhoto()">拍照</button>
<button onclick="SavePhoto('img','a.png')">保存</button>
<img id="imgTag" src="" alt="imgTag">
<button onclick="closeMedia()">关闭摄像头</button>
<script>
<!doctype html>
<html lang="en">
<head>
    <title>js调用摄像头拍照上传图片</title>
    <meta charset="utf-8">
</head>
<body>
<button onclick="openMedia()">开启摄像头</button>
<video id="video" width="500px" height="500px" autoplay="autoplay"></video>
<canvas id="canvas" width="500px" height="500px"></canvas>
药材<input  type="text" id="TCM">
<button onclick="takePhoto()">拍照</button>

<button onclick=SavePhoto('img',document.getElementById('TCM').value)>保存</button>
<img id="imgTag" src="" alt="imgTag">
<button onclick="closeMedia()">关闭摄像头</button>
<script>
    let mediaStreamTrack=null; // 视频对象(全局)
    function openMedia() {
        let constraints = {
            video: { width: 500, height: 500 },
            audio: true
        };
        //获得video摄像头
        let video = document.getElementById('video');
        let promise = navigator.mediaDevices.getUserMedia(constraints);
        promise.then((mediaStream) => {
            mediaStreamTrack = typeof mediaStream.stop === 'function' ? mediaStream : mediaStream.getTracks()[1];
            video.srcObject = mediaStream;
            video.play();
        });
    }

    // 拍照
    function takePhoto() {
        //获得Canvas对象
        let video = document.getElementById('video');
        let canvas = document.getElementById('canvas');
        let ctx = canvas.getContext('2d');
        ctx.drawImage(video, 0, 0, 500, 500);


        // toDataURL  ---  可传入'image/png'---默认, 'image/jpeg'
        let img = document.getElementById('canvas').toDataURL("image/png");
        // 这里的img就是得到的图片
        //console.log('img-----', img);
        document.getElementById('imgTag').src=img;
    }

function SavePhoto(img,name) {
  var a = document.createElement("a");

  a.href = document.getElementById('imgTag').src;
  a.download= name;
  a.click();
}

    // 关闭摄像头
    function closeMedia() {
        mediaStreamTrack.stop();
    }
</script>
</body>

```
![TCM3](/img/TCM3.jpg)

就是这样，拍照，然后呈现出来，合适的话就保存，不合适就继续拍，保存下来的图片用个批处理文件筛选就OK，数据采集就是这样。

由于还在等待药材，所以，这部分更新暂时停止。
