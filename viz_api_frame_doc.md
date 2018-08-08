# viz-api frame
## 1.RVIZ
rviz 是ROS中将机器人3D可视化的一款工具，几乎我们需要用到的所有机器人相关数据都可以在rviz中展现。但是由于机器人系统的需求不同，很多时候rviz中已有的一些功能就无法满足我们的需求了，这时就可以通过rviz的plugin机制，编写插件对rviz的功能进行扩展。
通过
```
rosrun rviz rviz
```
打开rviz，会看到如下界面：
![rviz_init](http://docs.ros.org/hydro/api/rviz/html/user_guide/_images/empty-rviz.png)
**Display**列表包括Global options以及显示在右方面板中的网格，marker，坐标轴等等。
通过**Add**可以选择添加marker，grid，axes等等。
## 2.RViz External APIs
RViz 的API主要使用以下几种库：

- **ros::NodeHandle** ROS主要使用的类之一，用于ROS中发布或者订阅话题；

- **ros::init()** 初始化函数，用于初始化ROS节点；

- **tf::MessageFilter** 用于过滤ROS的输入信息；
- **Ogre::SceneManager, Ogre::SceneNode, Ogre::Vector3, Ogre::Quaternion, Ogre::ManualObject, Ogre::Entity, and Ogre::Material** 是Ogre3D中的一些类。 

## 3.sparrow-visualization框架
### 3.1 头文件
**Entity.h**:通过color设置marker的颜色，position设置marker位置，rotation通过四元数设置marker的坐标变换（tf)，scale设置marker的大小，text设置marker的文本信息等等;
**BasicShapes.h**:主要用于声明一些基础marker和自定义模型类：Cube，Sphere，Text，Line，LineArray，Tractor，Truck等等。

### 3.2 launch

主要用于启动多个节点，设置RViz中的显示面板，地图加载，以及机器人模型参数。
**display.launch**:在RViz的Display中显示卡车模型节点；
**map.launch**:加载地图的接口；
**urdf_rviz.launch**：机器人参数设置接口；
### 3.3 map文件
外部已有的map文件，供接口调用。
### 3.4 urdf.rviz
用于定义机器人的环节（link），关节（joint），尺寸，运动参数等。


### 3.5 src
#### VehicleModel.cpp
生成车的模型并设置参数，如车的位置，轮速，头车和拖车之间的连接点的角度等。
#### vehicle_model_example.cpp
通过订阅话题将车的模型在rviz中展示出来。
#### rviz的一些plugin
1)vehicle_control_display.cpp
在rviz中用于控制展示的车的参数。
2)CarCreatorTool
rviz中的一个Tool，在rviz中点击生成一辆车，A和D用来旋转车辆。
3)OverlayPickerTool
rviz中的Tool，将octopus的overlay在rviz中。
4)DashboardDisplay
在rviz中展示车的控制面板。
5)FloatBoundedDisplay
在rviz中展示有界浮点数。
6)SpeedLimitDisplay
在rviz中展示车的速度限制。
7)SpeedDisplay
在rviz中展示车速。
#### api_test.cpp
rviz的api测试，可以订阅rostopic将一些marker显示在rviz中。
#### map.cpp
将map作为marker类展示在rviz中。

### 3.6 完成编译文件

为了编译成功，还需要完成一些编译文件的设置。
#### plugin_description.xml

在功能包的根目录下需要创建一个plugin的描述文件plugin_description.xml。
#### package.xml

然后在 package.xml文件里添加plugin_description.xml。

#### CMakeLists.txt

在CMakeLists.txt中也要加入相应的编译规则。