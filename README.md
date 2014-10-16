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

    * ####该服务器用来为gis2提供免费的地图底图支持，如gis2用到的
        *  Google Satellite
        *  autonavi(其实是google map)
    * ####服务端于`210.34.192.130|C:\Documents and Settings\Administrator\桌面\cpq\Portable Basemap Server v2.0.7`
        * 启动：`PortableBasemapServer.exe`
		* 配置文件：`CustomOnlineMaps.xml`已经配置了大量的免费地图服务包含gis2所用到的两个。
        * portable Basemap Server官网及具体使用说明：<http://geopbs.codeplex.com/>
        * 最终的服务通过端口映射提供为flexviewer服务以便增加前端灵活性。

* ###相关矢量数据生成及arcgis server服务发布  ###
	* ####新增道路分析
        * 数据来源：郑鸿杰
        * 矢量数据：`C:\arcgisserver\arcgisinput\zhj新增NEW_Export_Output.shp`
        * 数据配置文件：`210.34.192.130|C:\arcgisserver\arcgisinput\zhj新增\zhj.mxd`
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/zhjnew/MapServer>
    * ####道路震动数据
        * 数据来源：郑鸿杰 刘林
        * 矢量数据：`C:\arcgisserver\arcgisinput\破损道路\`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\破损道路\破损道路.mxd`
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/damage/MapServer/>
        
    * ####北京浮动车数据
        * 数据来源：邹老师
        * 矢量数据：`C:\arcgisserver\arcgisinput\beijing\`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\beijing\beijing.mxd`
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/beijing/MapServer/>
    * ####2015城运会重要poi
        * 数据来源：郑鸿杰
        * 矢量数据：`C:\arcgisserver\arcgisinput\zhj\poiNew`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\zhj\zhjpoi.mxd`
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/zhjpoi/MapServer/>
    * ####高德地图数据2014
        * 数据涟源：徐翔
        * 矢量数据：`C:\arcgisserver\arcgisinput\zhj新增`
        * 数据配置文件：`C:\arcgisserver\arcgisinput\zhj新增\zhj.mxd`
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/zhj/MapServer>
    * ####实时浮动车数据
        * 数据来源：udp2txt转发器udp转发
        * 启动接收：`python C:\arcgisserver\arcgisinput\car.py`
        * 矢量数据：`z:\carPoi.shp`(z盘为内存盘)
        * 数据配置文件：`C:\arcgisserver\arcgisinput\car.mxd`
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/carPoi2/MapServer/>
    * ####福州旧版地图数据
        * 数据来源：最早购买的福州地图数据
        * 矢量数据：
        * 数据配置文件：
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/oldfuzhou/MapServer>
    * ####实时路况数据
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
        * 服务地址：<http://210.34.192.119:8399/arcgis/rest/services/level/MapServer>
    * ####其他
        * argis server 使用 mxd文件来发布服务。最终的服务通过端口映射提供为flexviewer服务以便增加前端灵活性。
        * 重启相应的地图服务后应该先清除缓存，缓存管理地址：<http://210.34.192.130:8399/arcgis/rest/admin/> 账号密码与操作系统一致。登录后点击`clear cache options`,再点击`clear cache Now`即可完成清除缓存。
        * 有时候地图服务不可以用，进入系统服务，从其som，soc服务。
* ###python后端数据接口开发###
    * ####模块相关 
        * ######车翼行全球眼
            *  flexviewer post 
            *  由视频播放列表生成使用vlc控件的自动播放网页。
            *  生成脚本`python globalEye2\nameLocUrl2html\nameLocUrl2htmlactivex.py`
        
        * ######部分历史数据
            *  提取工具：`210.34.192.119/data4/gpsdata/2014/04filefilter.class`
            *  提取方法示例：`java -classpath . filefilter /data4/gpsdata/2014/04/20140401.txt ./zfm/20140401.txt -g 26.083,119.274-26.049,119.324 -c 41`
            *  `/data4/gpsdata/2014/04/20140401.txt`:原始二进制文件路径
            *  `./zfm/20140401.txt`：输出解码后的字符串格式文件
            *  `-g 26.083,119.274-26.049,119.324`：区域范围，需要严格按照大小顺序。
            *  `-c 41`：车辆类型为11
    
        * ######路径规划
            * gemfire路径规划
                * 现已停止而且准备由灵云智请的广州团队，在开发完gemfire路况计算后开始开发。
            * google api路径规划
                * 
    * #####与flexviewer通信协议
        * ######通信接口`http://gis2.fjut.edu.cn:8888/fjutgis`
        * ######flexviewer自身安全机制要求web通信时服务端放置跨域文件。由于我们使用bottle自带轻量web服务器所以这里使用get方法提供该跨域文件。
            * 代码位置：httpserver.py         
            ```
                @get('/crossdomain.xml')
                def Secure():
                    xml="<cross-domain-policy>"\
                	+"<allow-access-from domain=\"*\" to-ports=\"1025-9999\"/>"\
                	+"</cross-domain-policy>";
                	return xml
            ```
        * ######通信示例，这里用curl模拟flexviewer的post操作
            * history协议
                * 日期请求
                ```
                [root@FJUTGIS2 ~]# curl -d "type=history&cmd=getDateDir&data=null" http://210.34.192.119:8888/fjutgis 
                history|getDateDir:20130103
                ```
                * 车辆类型列表请求
                ```
                [root@FJUTGIS2 ~]# curl -d "type=history&cmd=getCarType&data=20130103" http://210.34.192.119:8888/fjutgis 
                history|getCarType:12 31 13 15 11 32 52 51 99 20 14 41 53
                ```
                * 车辆终端号列表请求
                ```
                [root@FJUTGIS2 ~]# curl -d "type=history&cmd=getMdid&data=20130103 41" http://210.34.192.119:8888/fjutgis 
                history|getMdid:1196928 1100145 1595270 1038003 1019884 999029 1523585 974925 1333533 1038016······
                ······
                ```
                * 车辆轨迹请求
                ```
                [root@FJUTGIS2 ~]# curl -d "type=history&cmd=getTrack&data=20130103 41 1196928" http://210.34.192.119:8888/fjutgis 
                history|getTrack:26.0798549,119.297378,2 26.079595,119.297265,6 26.077832,119.293667,5 26.073 974925 1333533 1038016······
                ······
                ```
            * grobalEye协议
                * 全部视频列表请求
                ```
                [root@FJUTGIS2 ~]# curl -d "type=globalEye2&cmd=getList&data=null" http://210.34.192.119:8888/fjutgis 
                globalEye2|getList:福州市！城南圆圈-82828,27.5342390,116.2235140 福州市018_五一路全景,26.0943930,119.297130 福州市1_!邮政小区顶楼天翼景象-28156,26.0745080,119.2964940 福州晋安二环路塔头路西出口,26.08797190,119.31719350
                ······
                ```
                * 所有视频url已经包含在使用vlc控件的html页面，页面标题名即上面返回的地点名，只需跳转到这个页面即可以播放相应摄像头。
            * routing 路径规划模块
                * 路径规划请求示例
                ```
                [root@FJUT-IPTV2 ~]#  curl -d "type=routing&cmd=getRoute&data=google 26.03423476,119.270503035 26.07897219,119.30186556" http://210.34.192.119:8888/fjutgis
                routing|getRoute:26.03346,119.27075 26.03346,119.27073 26.03335,119.2703 26.03325,119.26996 26.03317,119.26951 26.03314,119.26934 26.03314,119.26934 26.03306,119.26936 
                ······
                ```
* ###flexview前端开发###
widget插件开发，即顶部导航条的横栏按钮。fjutgis2 widget在项目中的目录为`fjlexviewer-2.3-src\srcwidgets\fjutgis\`，以下插件位置都位于该目录。
插件有与python数据通信接口通信的话都通过<http://210.34.192.119:8888/fjutgis>通信，所用插件共用一个接口地址配置文件`fjutgis\config\config.xml`
    * ###各模块详细说明
		* ####GlobalEye 模块
            * 插件名称`GlobalEye2` 
            * 功能：向python数据接口索取全球眼页面地址。全球眼页面需放到web服务器。
            * 协议：使用一下格式将数据post到http://210.34.192.119:8888/fjutgis

        * ####history 车辆历史轨迹播放模块
            * 插件名称`History`
            * 功能：向python数据接口索取指定车辆某一天数据进行播放
            * 协议：使用一下格式将数据post到http://210.34.192.119:8888/fjutgis
        * ####routing 路径规划模块
            * 插件名称`routing`
            * 功能：分别使用google与fjut两种方式进行路径规划
                * google  
                接口<https://developers.google.com/maps/documentation/directions/>这段时间google服务比较难访问。
                * fjut  
                向gemfire索取结果，这一块等灵云智请的广州开发团队开发完毕可以重新开始使用。
        * ####实时车辆数据
            * 插件名称`realCar2`
            * 功能：点击realCar2按钮后加载arcgis server发布的实时车辆图层并定时刷新。
            * 车辆实时图层地址：<http://210.34.192.119:8399/arcgis/rest/services/carPoi2/MapServe>
            
        * ####beijing浮动车数据
            * 插件名称`beijing`
            * 功能：点击beijing按钮后加载arcgis server发布的beijing浮动车图层。
            * 北京浮动车图层地址：<http://210.34.192.119:8399/arcgis/rest/services/beijing/MapServer/>
            
        * ####道路维护状态数据
            * 插件名称`damage`
            * 功能：点击realCar2按钮后加载arcgis server发布的实时车辆图层并定时刷新。
            * 道路维护状态图层地址：<http://210.34.192.119:8399/arcgis/rest/services/damage/MapServer/>
                                    
        * ####插件全局配置
            * 配置文件：`flexviewer-2.3-src/src/config.xml`
            * 配置的位置
                ```
                <widgetcontainer layout="float">
                            ······
                            ······
                            <widget label="History Track" 
                            icon="assets/images/fjut_history.png"
                            config="widgets/FjutGis/Config/fjutConfig.xml"
                            url="widgets/FjutGis/History/History.swf"/>
                            ······
                            ······
                </widgetcontainer>
                ```
    * ####图层配置
    这里的图层服务地址都经过210.34.192.119的端口映射，配置文件位于：`flexviewer-2.3-src/src/config.xml`
        * ######Portable Basemap Server(只能静态图层服务)
            * 静态图层
                * google satelite
                    ```
                    <layer label="Google Satellite" type="tiled" visible="false" url="http://210.34.192.119:7080/PBS/rest/services/GoogleMapsImagery/MapServer"/>
                    ```
                * autonavi(其实是google map)
                    ```
                    <layer label="autonavi" id="autonavi"  type="tiled" visible="true"
                url="http://210.34.192.119:7080/PBS/rest/services/GoogleMapsRoad/MapServer"/>
                    ```
        * #####arcgis server
            * 动态图层
                *  oldMap
                    ```
                    <layer label="oldMap" id="oldMap" type="dynamic" visible="false"
                url="http://210.34.192.119:8399/arcgis/rest/services/oldfuzhou/MapServer"/>
                    ```
                * navinfo
                    ```
                    <layer label="navinfo" id="swtx" type="dynamic" visible="false"
                url="http://210.34.192.119:8399/arcgis/rest/services/zhj/MapServer"/>
                    ```
                * new road for navinfo(新增道路图层)
                    ```
                    <layer label="new road for navinfo" type="dynamic" visible="false"
                url="http://210.34.192.119:8399/arcgis/rest/services/zhjnew/MapServer"/>
                    ```
                * road condition
                    ```
                    <layer label="road condition" type="dynamic" visible="false"
                url="http://210.34.192.119:8399/arcgis/rest/services/level/MapServer"/>
                    ```
    
    * ####系统账号密码管理
        *  账号密码目前写死在`src/(default/index.mxml)`根据需要修改这个文件的代码即可。
        


