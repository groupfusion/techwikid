---
title: penlayer gis 开发之地图
tag:  opengis openlayer 
date:
---

# openlayer gis 开发之地图

在进行webgis 开发中通常我们调用百度、高德地图api实现，也可使用openlayer组件实现。下面罗列了些可集成到openlayer的地图的瓦片图层。

### 关于openlayer
    OpenLayers是一个用于开发WebGIS客户端的JavaScript包，最初基于BSD许可发行。
    
    OpenLayers是一个开源的项目，其设计之意是为互联网客户端提供强大的地图展示功能，包括地图数据显示与相关操作，并具有灵活的扩展机制。
    
    1.支持瓦片图层
    OpenLayers支持从OSM、Bing、MapBox、Stamen和其他任何你能找到的XYZ瓦片资源中提取地图瓦片并在前端展示。同时也支持OGC的WMTS规范的瓦片服务以及ArcGIS规范的瓦片服务。
    
    2.支持矢量切片（或者矢量瓦片）
    OpenLayers也支持矢量切片的访问和展示，包括MapBox矢量切片中的pbf格式，或者GeoJSON格式和TopoJSON格式的矢量切片。
    
    3.支持矢量图层
    能够渲染GeoJSON、TopoJSON、KML、GML和其他格式的矢量数据，上面说的矢量切片形式的数据也可以被认为是在矢量图层中渲染。
    
    4.支持OGC规范
    OpenLayers支持OGC制定的WMS、WFS等GIS网络服务规范。
    
    5.运用前沿技术
    利用Canvas 2D、WebGL以及HTML5中其他最新的技术来构建功能。同时支持在移动设备上运行。
    
    6.易于定制和扩展
    可以直接调整CSS来为地图控件设计样式。而且可以对接到不同层级的API进行功能扩展，或者使用第三方库来定制和扩展。
    
    7.面向对象的思想
    最新版本的OpenLayers采用纯面向对象的ECMA Script 6进行开发，可以说，在OpenLayers中万物皆对象。
    
    8.优秀的交互体验
    OpenLayers实现了类似于Ajax的无刷新功能，可以结合很多优秀的JavaScript功能插件，带给用户更多丰富的交互体验。

### GIS底图-天地图
```
//http://www.arcgisonline.cn/arcgis/home/webmap/viewer.html 地图可参考这个链接
// 影像地图（球面墨卡托投影） 
// 天地图-影像注记（球面墨卡托投影）
https://t{0-7}.tianditu.gov.cn/cia_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=cia&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb
// 天地图-影像地图（球面墨卡托投影）
https://t{0-7}.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=img&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb
// 
// 矢量地图（球面墨卡托投影）
// 天地图-矢量注记（球面墨卡托投影）
https://t{0-7}.tianditu.gov.cn/cva_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=cva&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb
// 天地图-矢量地图（球面墨卡托投影）
https://t{0-7}.tianditu.gov.cn/vec_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=vec&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb
// 
// 天地图-矢量（含注记）（球面墨卡托投影）
// 天地图-矢量地图（球面墨卡托投影）
http://t{0-7}.tianditu.com/vec_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=vec&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb
// 天地图-矢量注记（球面墨卡托投影）
http://t{0-7}.tianditu.com/cva_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=cva&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb
```

```
//天地图［可用］［无需纠偏］
var tian_di_tuMapLayer = new ol.layer.Tile({
    title:'天地图卫星影像',
    source: new ol.source.XYZ({
        url:'http://t{1-7}.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=49ea1deec0ffd88ef13a3f69987e9a63'
    })
});
//天地图路网 (和注记一起使用)［可用］［无需纠偏］
var tian_di_tu_road_layer = new ol.layer.Tile({
    title: "天地图路网",
    source: new ol.source.XYZ({
        url: "http://t{1-7}.tianditu.com/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk=49ea1deec0ffd88ef13a3f69987e9a63"
    })
});
//天地图注记
var tian_di_tu_annotation = new ol.layer.Tile({
    title: "天地图文字标注",
    source: new ol.source.XYZ({
        url: 'http://t{1-7}.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=49ea1deec0ffd88ef13a3f69987e9a63'
    })
});
//天地图 ［无需纠偏］
var arcgisLayer = new ol.layer.Tile({
  title:'天地图卫星影像',
  source: new ol.source.XYZ({
    crossOrigin: 'anonymous',
    url:'https://t{1-7}.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=img&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb'
  })
});
var arcgis2Layer = new ol.layer.Tile({
  title: "天地图矢量地图",
  source: new ol.source.XYZ({
    crossOrigin: 'anonymous',
    url:'https://t{0-7}.tianditu.gov.cn/vec_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=vec&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb'
  })
});
var arcgis3Layer = new ol.layer.Tile({
  title:'天地图-矢量地图',
  source: new ol.source.XYZ({
    crossOrigin: 'anonymous',
    url:'https://t{0-7}.tianditu.gov.cn/cva_w/wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetTile&LAYER=cva&STYLE=default&FORMAT=tiles&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=4267820f43926eaf808d61dc07269beb'
  })
});
```

### GIS底图-高德地图
```
//高德矢量图［可用］ 需要纠偏
var gaodeMapLayer = new ol.layer.Tile({
    source: new ol.source.XYZ({
        url:'https://wprd0{1-4}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&style=7&x={x}&y={y}&z={z}'
    })
});
```


### GIS底图-捷泰地图
```
//arcgis 矢量图［可用］ 需要纠偏
var backLayer = new ol.layer.Tile({
  source: new ol.source.XYZ({
    crossOrigin: 'anonymous',
    url:'http://cache1.arcgisonline.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer/tile/{z}/{y}/{x}'
  })
});

//捷泰地图(灰) ［需要纠偏］
var geoq2Layer = new ol.layer.Tile({
  source: new ol.source.XYZ({
    crossOrigin: 'anonymous',
    url:'http://cache1.arcgisonline.cn/ArcGIS/rest/services/ChinaOnlineStreetGray/MapServer/tile/{z}/{y}/{x}'
  })
});
//捷泰地图（深蓝） ［需要纠偏］
var geoqLayer = new ol.layer.Tile({
  source: new ol.source.XYZ({
    crossOrigin: 'anonymous',
    url:'http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetPurplishBlue/MapServer/tile/{z}/{y}/{x}'
  })
});
```

### 使用示例
```
<!DOCTYPE html>
<html>
  <head>
    <title>Tiled ArcGIS MapServer</title>
    <link rel="stylesheet" href="https://openlayers.org/en/v5.3.0/css/ol.css" type="text/css">
    <!-- The line below is only needed for old environments like Internet Explorer and Android 4.x -->
    <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=requestAnimationFrame,Element.prototype.classList,URL"></script>

  </head>
  <body>
    <div id="map" class="map"></div>
    <script>
      import Map from 'ol/Map.js';
      import View from 'ol/View.js';
      import TileLayer from 'ol/layer/Tile.js';
      import {OSM, TileArcGISRest} from 'ol/source.js';

      var url = 'https://wprd0{1-4}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&style=7&x={x}&y={y}&z={z}';

      var layers = [
        new TileLayer({
          source: new OSM()
        }),
        new ol.layer.Tile({
                  	source: new ol.source.XYZ({
                    	url: url
                  	}),
                  isGroup: true,
                  name: '高德地图'
                 })
      ];
      var map = new Map({
        layers: layers,
        target: 'map',
        view: new View({
          center: [-10997148, 4569099],
          zoom: 4
        })
      });
    </script>
  </body>
</html>
```