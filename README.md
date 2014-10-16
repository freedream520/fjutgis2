#fjut校园网高码率iptv建设文档#
 * author:nbsky  2014-10-15
 * Email:<chpeiqi@gmail.com>

##架构图##
![](https://raw.githubusercontent.com/nbsky/fjutgis2/master/gis2%E7%BB%93%E6%9E%84%E5%9B%BE.jpg)
##模块说明##
主要分为4大块内容：  
1.python后端数据接口开发   
2.相关矢量数据生成及arcgis server服务发布  
3.flexviewer前端开发  
4.portable basemap server配置。


* ###Portable Basemap Server配置###

    * 该服务器用来为gis2提供免费的地图底图支持，如gis2用到的
        *  Google Satellite
        *  autonavi(其实是google map)
    * 服务端于`210.34.192.130|C:\Documents and Settings\Administrator\桌面\cpq\Portable Basemap Server v2.0.7`
        * 启动：`PortableBasemapServer.exe`
		* 配置文件：`CustomOnlineMaps.xml`已经配置了大量的免费地图服务包含gis2所用到的两个。
        * portable Basemap Server官网及具体使用说明：<http://geopbs.codeplex.com/>
        * 最终的服务通过端口映射提供为flexviewer服务以便增加前端灵活性。

* ###相关矢量数据生成及arcgis server服务发布  ###
	* 新增道路分析
        * 数据来源：鸿杰
        * 矢量数据：`C:\arcgisserver\arcgisinput\zhj新增NEW_Export_Output.shp`
        * 数据配置文件：`210.34.192.130|C:\arcgisserver\arcgisinput\zhj新增\zhj.mxd`

    * 道路震动数据
        * 数据来源：郑鸿杰 刘林
        * 矢量数据：`C:\arcgisserver\arcgisinput\破损道路\`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\破损道路\破损道路.mxd`
        
    * 北京浮动车数据
        * 数据来源：邹老师
        * 矢量数据：`C:\arcgisserver\arcgisinput\beijing\`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\beijing\beijing.mxd`

    * 2015城运会重要poi
        * 数据来源：郑鸿杰
        * 矢量数据：`C:\arcgisserver\arcgisinput\zhj\poiNew`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\zhj\zhjpoi.mxd`
    
    * 实时浮动车数据
        * 数据来源：udp2txt转发器udp转发
        * 启动接收：`python C:\arcgisserver\arcgisinput\car.py`
        * 矢量数据：`z:\carPoi.shp`(z盘为内存盘)
        * 数据配置文件：`C:\arcgisserver\arcgisinput\car.mxd`
        
    * 实时路况数据
        * 数据来源：
            * udp2txt转发器udp转发,每隔10分钟生成数据： 
                * 启动接收：`python C:\Documents and Settings\Administrator\桌面\cpq\newlukuang\carDataRec\car4condition.py`
                * 每隔10分钟刷新数据：`C:\Documents and Settings\Administrator\桌面\cpq\newlukuang\car_cur.txt`
            * 高德地图网格化后的文本数据：C:\Documents and Settings\Administrator\桌面\cpq\newlukuang\`
                * FJ_OFFSET.txt
                * fidnode.txt
                * gridfidnode.txt
            * 启动路况计算并生成路况矢量数据：`python C:\Documents and Settings\Administrator\桌面\cpq\newlukuang\main.py` 
            * 路况计算程序定时刷新：`C:\arcgisserver\arcgisinput\test\line.shp`
        * 矢量数据：`C:\arcgisserver\arcgisinput\test\line.shp`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\level.mxd`
        
    * argis server 使用 mxd文件来发布服务。最终的服务通过端口映射提供为flexviewer服务以便增加前端灵活性。
    
* ###pyton后端数据接口开发###
    * 车翼行全球眼数据
        *  
    * 部分历史数据
    * 路径规划
        * gemfire路径规划
        * google api路径规划
* ###flexview前端开发###
	* widget插件开发，即顶部导航条的横栏按钮。
		* `浙江卫视`
    * 图层配置。 
        * google satelite
        * autonavi(其实是google map)
    * 系统账号密码管理
    *  




history
    client                                      `type|cmd|data`                                     server  
    获取时间列表                    ->          `history|getDateDir:null`                 ->  
    接收时间列表                    <-          history|getDateDir:dateDir1 dateDir2    <-  
    根据时间获取车辆类型            ->          history|getCarType:dateDir              -      
    获取时间列表下的车辆类型        <-          history|getCarType:CarType1 CarType2    <-  
    根据时间和类型获取车辆列表      ->          history|getMdid:dateDir CarType         ->  
    接收车辆列表                    <-          history|getMdid:mdid1 mdid2             <-  
    根据时间车辆类型车辆获取车辆轨迹->          history|getTrack:dateDir cartype mdid   ->  
    接收车辆轨迹                    <-          history|getTrack:lat,lng lat1,lng1      <-  

