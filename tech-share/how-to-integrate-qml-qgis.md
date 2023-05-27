# QT6.5 Quick QML集成QGIS方案探索

## 使用场景
### 离线地图
### 支持QML
### GeoServer数据源

## 基于QtLocation
### （1）找到D:\app\QT\6.5.0\Src\qtlocation\src\plugins\geoservices\osm参考osm自定义地图插件
### （2）地图配置文件：osm_plugin.json
### （3）实现QGeoServiceProviderFactory接口，用于实例化地图管理引擎，例如：qgeoserviceproviderpluginosm.h/cpp
### （4）实现QGeoTiledMappingManagerEngine接口，用于定义地图配置，例如：qgeotiledmappingmanagerengineosm.h/cpp
### （5）实现QGeoTileFetcher接口，用于实现加载地图（网络/本地）瓦片图，例如：qgeotilefetcherosm.h/cpp
### （6）实现QGeoTiledMapReply接口，用于处理瓦片图请求响应，例如：qgeomapreplyosm.h/cpp


#### 官方参考文档：https://doc.qt.io/qt-6/qtlocation-geoservices.html
#### 开源参考项目1：https://github.com/Mud-Player/MudMap
#### 开源参考项目2：https://github.com/gongjianbo/MyQtLocation
#### 参考博客1：https://blog.csdn.net/zjgo007/article/details/123832500
#### 参考博客2：https://blog.csdn.net/zjgo007/article/details/123832500



## 基于QGIS Quick


## 参考资料
#### https://blog.csdn.net/zjgo007/article/details/123832500
#### https://blog.csdn.net/xt18971492243/article/details/122835980
#### https://blog.csdn.net/Mud_Player/article/details/110731857
#### https://api.qgis.org/api/qgsquick.html
#### https://www.zhihu.com/question/56705182?utm_id=0
#### https://github.com/gongjianbo/MyQtLocation
#### https://github.com/SindenDev/amap/blob/master/README.md
#### https://github.com/mguludag/pure-QML-Google-Maps
#### https://github.com/cwinston/GoogleMaps
