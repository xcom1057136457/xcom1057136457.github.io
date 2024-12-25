---
title: HTML5多媒体标签
date: 2019-09-20 14:20:26
---
**在HTML5问世之前，要在网络上展示视频，音频，动画，除了使用第三方自主开发的播放器哦之外，使用得最多的工具就是Flash，但是需要在浏览器上安装各种插件，并且有时速度很慢。HTML5新增了两个与媒体相关的标签，让开发人员不必依赖任何插件就能在网页中嵌入跨浏览器的音频和视频内容，这两个标签是 `<audio>` `<video>`**



> 注意：需要使用流媒体文件格式（如mp4、webm、ogg等）



## 1. 嵌入式/音频



### video

`<video src = "文件路径" id = "" width = "" height = "">如果视频无法播放则会显示这段话</video>`



### audio

`<audio src = "文件路径" id = "" width = "" height = "">如果音频无法播放则会显示这段话</audio>`



## 2. source,指定不同媒体来源



**并不是所有的浏览器都支持所有的媒体格式，可以指定多个不同的媒体来源。由于不同的浏览器支持不同的编解码器，一定要指定多种格式的媒体来源**



### src：指播放媒体的URL地址



### type：媒体类型，属性值为播放文件的MIME类型，该属性值中的codes参数表示所使用的媒体的编码格式



### 举例

``` html
<video id = "video_1">
	<source src = "sample.ogv" type = "video/ogg">
	<source src = "sample.mov" type = "video/quicktime">
	视频播放器无法使用
</video>
```

``` html
<audio id = "audio_1">
	<source src = "" type = "" >
    <source src = "" type = "">
    音频播放器无法使用
</audio>
```

### 属性

|       属性值        |                描述                |
| :-----------------: | :--------------------------------: |
|    width、height    |        播放控件的宽度和高度        |
|      controls       |       默认播放控制条是否显示       |
|      autoplay       |              自动播放              |
|       preload       |               预加载               |
|       poster        | 视频预览图（视频不用或者不可用时） |
|        loop         |              循环播放              |
|     currentTime     |            当前播放位置            |
|      duration       |            文件总的播放            |
|       played        |             是否播放中             |
|       paused        |              是否暂停              |
|        ended        |            是否播放完毕            |
| defaultPlaybackRate |              播放速度              |
|    playbackRate     |           设置播放速度1            |
|       volume        |              音量 0-1              |
|        muted        |          静音 true/false           |



### 方法



**play()：播放**

**pause()：暂停**



## 3.自定义媒体播放器



### 自定义视频播放器

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        #myVideo{
            position: relative;
            width: 640px;
        }
        #p{
            width: 50px;
            height: 50px;
            border:3px solid skyblue; 
            border-radius: 50%;
            background: none;
            font-size:24px;
            font-weight: bold;
            color: skyblue; 
            position: absolute;
            top:50%;
            left: 50%;
            margin-left: -25px;
            margin-top: -25px;
        }
    </style>
</head>
<body>
    <div id="myVideo">
        <video id="video1">
            <source src="./a1.mp4" type="video/mp4"/>
            <source src="./a1.webm" type="video/webm"/>
            <source src="./a1.ogg" type="video/ogg"/>
            您的浏览器不支持该插件!
        </video>
        <input type="button" value=">" id="p" onclick="pl()" />
        <div id="showTime"></div>
        <input type="range" min="0" max="1" step="0.1" id="ra" 		oninput="getVolume()"/>
        <input type="button" value=">>" onclick="up()" />
    </div>
    
    <script>
        var v = document.getElementById('video1');//媒体元素
        var p = document.getElementById('p');//播放按钮
        var showTime = document.getElementById('showTime');//显示时间DIV
        var ra = document.getElementById('ra');//滑块
        //P按钮移入移出作业
        function pl(){
            if(v.paused){
                v.play();
                p.value='||';
            }else{
                v.pause();
                p.value='>';
            }
        }
        //获取媒体总时间时，必须文档装载完毕后，所以必须在onload事件后获取
        window.onload=function(){
            s = parseInt(v.duration);
        }
        //获取当前播放的时间
        setInterval('current()',1000);
        function current(){
            //把当前播放时间显示到对应div
            showTime.innerHTML = parseInt(v.currentTime) + '/' +s;
        }
        //将滑块的值赋值给媒体元素
        function getVolume(){
            v.volume = ra.value;
        }
        //快放
        function up(){
            v.playbackRate = v.playbackRate*2;
        }
    </script>
</body>
</html>
```



### 自定义音频播放器

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <audio id="audio1" controls>
        <source src="./1.mp3" type="audio/mp3"/>
        <source src="./1.cda" type="audio/cda"/>
        <source src="./1.flac" type="audio/flac"/>
        您的浏览器不支持该插件！
    </audio>
    <input type="button" onclick="next()" value="下一曲" />
    <script>
        var MyAudio = document.getElementById('audio1');
        var data = ['1.mp3','2.mp3'];//保存我的歌曲库
        var index = 0;//歌曲当前的索引位置
        var dataLength = data.length;//获取歌曲总数量
        //制作上一曲
        function next(){
            index++;
            if(index>=dataLength){
                index=0;
            }
            var getdata = data[index];
            MyAudio.src = getdata;
        }
    </script>
</body>
</html>
```









