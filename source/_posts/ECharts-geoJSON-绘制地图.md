---
title: ECharts + geoJSON 绘制地图
date: 2025-01-03 11:47:04
---

## 获取geoJSON

1. 通过[地图选择器](https://datav.aliyun.com/portal/school/atlas/area_selector#&lat=35.88905007936091&lng=117.69653320312499&zoom=6)下载geoJSON数据，或者直接引用里面的地址。本例以洛阳市为例引用地址[`https://geo.datav.aliyun.com/areas/bound/410300_full.json`](https://geo.datav.aliyun.com/areas/bound/410300_full.json)

2. 在`arcgis`里面使用工具把`shp`数据处理成`geoJSON`数据。使用工具为`arcgis`工具箱 `Conversion Tools->JSON->Features To JSON`

## 使用ECharts创建地图

```` javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>洛阳市</title>
    <script src="scripts/lib/echarts.min.js"></script>
    <script src="scripts/lib/jquery.min.js"></script>
    <style>
        html,body,#main{
            padding: 0px;
            margin: 0px;
            height: 100%;
            overflow: hidden;
        }
    </style>
</head>
<body>
<div id="main"></div>
<script type="text/javascript">
    $.get("https://geo.datav.aliyun.com/areas/bound/410300_full.json",function(map){
 var myChart = echarts.init(document.getElementById('main'));
        echarts.registerMap("luoyang",map);
 var option = {
            series : [ {
                map : "luoyang",
                type : "map",
                aspectScale: 1.0,
                selectedMode: 'single',//选择类型,
                hoverable:false,//鼠标经过高亮
                roam: true,//鼠标滚轮缩放
                itemStyle: {
                    normal: {
                        borderWidth:1,
                        borderColor:'#ffffff',//区域边框色
                        areaColor: '#FFDAB9',//区域背景色
                        label: {
                            show: true,
                            textStyle: {
                                color: '#6495ED',//文字颜色
                                fontSize:18      //文字大小
                            }
                        }
                    },
                    emphasis: {                 // 选中样式
                        borderWidth:1,
                        borderColor:'#00ffff',
                        color: '#ffffff',
                        label: {
                            show: true,
                            textStyle: {
                                color: '#ff0000'
                            }
                        }
                    }
                }
            } ]
        };
        myChart.setOption(option);
    });
</script>
</body>
</html>
````

## 效果图如下

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20250103115020.png)