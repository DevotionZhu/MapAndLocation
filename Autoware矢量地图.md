

## Autoware Vector Map Builder Guide

## 1、基本操作

### 1.1 注册Tier.iv账号

使用VectorMap及相关服务需要注册T4账号，注册地址：<https://account.tier4.jp/>
![img](https://tools.tier4.jp/static/img/2.png)

进入在线标注工具页面地址 ( <http://maps.tier4.jp/>).

### ![img](https://tools.tier4.jp/static/img/1.png)1.2 编辑界面简介

1. Header menu
2. Toolbox ( add / edit tools for various objects )
3. Change the camera button
4. Project panel ( Data being read list panel )
5. Edit panel ( Detailed and Edit panel of the selected object )
6. VectorMap visualization area

#### 1.Header menu

在此菜单下，您可以加载、导出和创建新项目。

##### ◆ Setting function

FPS设置，增加每秒帧数(FPS)将使屏幕刷新更频繁，将消耗计算机资源，默认值是20。如果你遇到一些问题，或者你的电脑没有专门的GPU，试着降低这个值来改善你的体验，最大允许值是60，将提供一个平滑的可视化。

![img](https://tools.tier4.jp/static/img/5.png)

![img](https://tools.tier4.jp/static/img/6.png)



##### ◆ User Guide function

单击Help / UserGuide将显示当前用户帮助手册。

![img](https://tools.tier4.jp/static/img/7.png)

#### 2.Toolbox 

在这个工具箱中，您将找到向VectorMap添加对象所需的按钮，在选项卡中单击时可以切换几个类别。

#### 3.Camera POV

 vector map可以在二维和三维两种模式下进行可视化。3D模式下，可以在单击和拖动可视化区域的同时移动相机，相机的高度可以通过改变垂直移动(3D)参数来调整。

#### 4.Project panel

在此面板(ADAS Map、Waypoints、PointCloud和数据列表)下，可以显示或隐藏以前加载的数据。在同一项目中加载多个ADAS Map时，一次只能显示一个。要选择要显示哪一个，单击地图名称旁边的option按钮。你还可以使用下图的复选按钮选择要显示地图的哪个子区域。

![img](https://tools.tier4.jp/static/img/8.png)

> 1. Create / Edit switch ( checkbox ) 
>
> - 允许在编辑和创建模式之间切换。
>
> 2. Show / hide ( eye icon)
>
> - 用于隐藏显示在左侧的对象实例
>
> 3. Setting function ( gear icon)
>
> - Delete / Focus function
> - Delete: delete map data
> - Focus: camera focus
>
> 4. Select - deselect function
>
> - 选定/取消 子对象
>
> 5. Show - hide function
>
>    - 显示/隐藏所有的子对象

#### 5.Edit panel

显示所选对象的详细信息，它还允许更改object的原有参数。

#### 6.Main page

##### ◆ Focus function

此功能允许将坐标轴移动到所需位置。这可以通过单击action菜单上的focus按钮，然后将其拖动到新的选定位置。

![img](https://tools.tier4.jp/static/img/9.png)

![img](https://tools.tier4.jp/static/img/10.png)

##### ◆ Shortcuts key

按下Delete键可以将Object从可视化区域中删除。vector和pole对象可以使用复制/粘贴组合进行复制。

当窗口中显示“复制对象”时，复制的对象将在相同的位置生成。

![img](https://tools.tier4.jp/static/img/11.png)

如果出现“此对象不支持复制”消息，则无法使用此方法复制所选对象。

![img](https://tools.tier4.jp/static/img/12.png)

##### ◆ Arranging objects

![img](https://tools.tier4.jp/static/img/13.png)

可以移动的Objects将显示如图所示的箭头。若要沿着所需轴移动对象，请单击并拖动箭头到所需位置。当拖动显示在对象中心的小矩形时，对象也可以沿平面移动。

## 2、创建编辑ADASMap

### 2.1 创建ADASMap

从顶部标题菜单的Create选项卡中单击Create ADASMap。

![img](https://tools.tier4.jp/static/img/14.png)

为新Map编写一个名称，选择一个平面参考编号(见下图)，然后单击Create按钮。

![img](https://tools.tier4.jp/static/img/15.png)

参考编号是指根据下面链接查询到的位置确定。

["わかりやすい平面直角座標系｜国土地理院" - http://www.gsi.go.jp/sokuchikijun/jpc.html](http://www.gsi.go.jp/sokuchikijun/jpc.html)

![img](https://tools.tier4.jp/static/img/16.png)

通过点击顶部栏头菜单中的“导入PCD…”按钮加载PCD文件(点云数据)。

![img](https://tools.tier4.jp/static/img/17.png)

点击Browse…按钮选择PCD文件夹/文件。

![img](https://tools.tier4.jp/static/img/18.png)

选择“读取文件并设置最大强度”复选框后，单击“导入”按钮。最大Intensity初始设定值为10。

![img](https://tools.tier4.jp/static/img/19.png)

### 2.2、ADASMap编辑

#### 2.2.1 导入

- 从顶部的标题菜单中点击“导入ADASMap…”按钮，选择要读取的ADASMap文件夹。

- 选择包含要读取的CSV文件列表的文件夹。

- 使用Import按钮导入map文件。

#### 2.2.2 编辑 (包括路网图 )

- 从右上角的项目面板中，选择要编辑的数据(例如，在编辑路网时选择道路区域)

- 选择要编辑的节点和车道

- 一旦选中，可以在右上角的编辑面板中更改参数。

- 可以从工具箱中添加新的节点和车道。  

#### 2.2.3 导出

- 选择要导出的ADASMap，然后从顶部的标题菜单单击“导出ADASMap…“按钮。

- 将zip文件下载到本地

## 3、ADASMap规范中的道路元素

### 3.1 基本道路元素

#### Road

・Node、Lane （车道、节点）
・Way area（可行驶区域）
・Dtlane：（目前不支持）

#### Road Shape

・Curb
・Roadedge（路边缘）
・Gutter（侧水沟）
・Intersection（路口）

#### Road Surface（路面）

・Whiteline（白线）
・Stopline（停车线）
・Zebrazone（斑马线）
・Crosswalk（人行横道）
・RoadSurfaceMark（路面标识）

#### Roadside

・Guardrail（护栏）
・Sidewalk（人行道）

#### Structure

・Pole（杆）
・Utilitypole（电线杆）
・Roadsign（标识）
・Signaldata（信号灯）
・Streetlight（路灯）
・Curvemirror
・Wall
・Fence（围栏）
・RailroadCrossing（铁路道口）

### 3.2 道路元素的编辑方法

#### 3.2.1 Road

Road的目的是利用节点、车道、Dtlane和路域来表达道路线和路网信息，(当前版本不支持Dtlane)。请确保使用项目面板中的复选框选择道路网络，以编辑相应的Road数据。

■ Node, Lane

Lane用来定义车辆行驶的车道。它由两个node组成，一般公路的默认长度为1米，高速公路为5米。

![img](https://tools.tier4.jp/static/img/20.png)

![img](https://tools.tier4.jp/static/img/21.png)

①点击”add Node“按钮添加一个节点。

②点击“Add Node with Lane”添加一个车道节点，并选择结束节点。

③输入Dtlane ID的字段。(Dtlane当前版本不支持)。

④在JCT中指定当前车道分支/合并模式 (正常(0),branch to the left (1)branch to the right(2),合并到左车道(3)合并到右车道(4)))

⑤不包含BLID，在BLID2-4 字段指定要合并到当前车道的Lane ID。(0表示没有合并lane)

⑥包含FLID，在FLID2-4 中指定要分流出去的车道LaneID。(当车道没有分支时，选择0)

⑦Crossing ID。(只使用在十字路口,输入CrossingID,否则值为0)

⑧LCnt中指定车道数。(内部交叉是0)

⑨车道在Lno field的数量。从左边的车道开始编号，内部交叉是0。

⑩LimitVel 设定的最大速度。

⑪RefVel 中指定目标运动速度(驾驶期间估计速度)。

⑫在LaneType指定车道类型。（ 0: 直行, 1: 左转, 2:  右转 ）

⑬在RoadSecID 中指定路段。(在十字路口是0,相邻车道通常具有相同的RoadSecID)

⑭LaneChgFG中指定是否车道允许变道。(0:允许,1:不允许(0)在十字路口)

⑮在LinkWAID中指定Way area ID。

⑯重复②⑮

■ Lanes

Lane元素有两个主要功能

**◯ Reverse Direction 反向**

该功能允许通过替换BNID和FNID来改变车道方向。选择想要改变方向的车道后，点击编辑面板中的“Change direction”按钮。

![img](https://tools.tier4.jp/static/img/22.png)

　![img](https://tools.tier4.jp/static/img/23.png)　↓　　



##### ◯ Lane Split 车道分割

此函数允许分割大于1m的车道。显示[Split Lane]按钮，选择一个大于1米的车道，选择要分割的车道后，点击“Split Lane”按钮。

　　![img](https://tools.tier4.jp/static/img/24.png)↓　　

![img](https://tools.tier4.jp/static/img/25.png)

##### ■ Wayarea

Wayarea定义了车辆可以行驶的道路区域。

![img](https://tools.tier4.jp/static/img/25.png)

① 点击 “Add Wayarea” 按钮添加Wayarea
② 点击 “Add line of Area” 按钮扩展 Wayarea。（Dot lane 是一条连接起点到终点的虚线 ) 

③ Repeat ②.

##### ■ Dtlane

Dtlane是为道路或车道中心线添加了线性元素和纵向横向坡度的数据，承担着中心线性数据的作用。暂时不支持。

#### 3.2.2 Road Shape



Road shape的目的是利用曲线、道路边缘、侧水沟和交叉口来确定道路的形状。需要确认在项目面板中选择道路形状复选框以编辑道路形状数据。最后，为每个元素指定链接ID。

##### ■ Curb

![img](https://tools.tier4.jp/static/img/25.png)

![img](https://tools.tier4.jp/static/img/25.png)

①点击“Add Curb”按钮添加路边Curb。
②在Height中设置curb的高度。
③使用宽度参数定义ciurb的宽度。(宽度方向设置使用Dir参数,下面解释)
④使用Dir参数设置新创建的curb的方向(右:1、左:0)
⑤选择车道后，在LinkID指定距离最近的车道ID。
⑥扩大curb区域后,点击“添加curb”按钮选择结束节点。
⑦重复②到⑥。

##### ■ RoadEdge

![img](https://tools.tier4.jp/static/img/28.png)

①点击“Add RoadEdge”按钮，添加一个RoadEdge元素。
②选择车道后，在LinkedID中指定距离最近的车道ID。
③点击“Add RoadEdge”按钮，选择结束节点后，扩大RoadEdge区域。
④重复②和③。

##### ■ Gutter

①点击“Add Gutter”按钮添加Gutter。
②选择车道后，在LinkedID中设置最近的LaneID。
③指明Gutter使用类型参数的类型。（ 0 : No lid（无盖子）, 1 : With lid, 2 : Grating（光栅） ）
④通过点击“Add line of Area”按钮扩大Gutter。
⑤重复④。

##### ■ Intersection（十字路口）

十字路口范围以stopline和road edge确定，圆周线通常与路边缘线相同。

![img](https://tools.tier4.jp/static/img/30.png)

①点击“Add intersection”添加一个十字路口。
②使用LinkID指定最近的Lane ID。
③通过点击“Add line of Area”按钮扩大交叉区域。
④重复③。

#### 3.2.3 Road Surface

路面的作用是用白线、停车线、斑马线、人行横道、路面标识来表达道路上的线条、标志等。需要确认在项目面板中选择路面复选框以编辑路面数据，你必须指定链接ID到所有元素。

##### ■ Whiteline

![img](https://tools.tier4.jp/static/img/31.png)

![img](https://tools.tier4.jp/static/img/32.png)

①通过点击“Add White line”按钮，添加一条白线。
②在Width中设置线的宽度。
③Color中指定线的颜色参数。(白:W,黄色:Y）
④在Type中指定线的类型。(0:实线,1:虚线,2:空白虚线)
⑤使用LinkID指定最近的车道ID。
⑥点击添加白线后，选择“add Whiteline”扩展白线。
⑦重复②到⑥。

##### ■ Stopline

![img](https://tools.tier4.jp/static/img/33.png)
①点击“Add Stopline”按钮，添加停车线。
②在TLID中指定信号灯id。
③SignID中指定标识id。
④在LinkID中指定最近的LaneID。(如果有多个信号灯、标志的情况下，③和④选择最接近的那个)

##### ■ Zebrazone

![img](https://tools.tier4.jp/static/img/34.png)

①通过点击“add Zebrazone”按钮，添加一个斑马线。
②在LinkID指定最近的LaneID。
③通过点击“Add lane of Area”按钮，延长斑马线。
④重复③。

##### ■ Crosswalk

大多数情况下，人行横道由边界、条纹图案和自行车通行区组成。

![img](https://tools.tier4.jp/static/img/35.png)

①点击“Add Crosswalk”，添加一个人行横道(条纹模式和自行车道)。
②通过点击“Add line of Area”，扩大人行横道(条纹模式和自行车道)”。
③在Type里面选择人行横道样式。(1:条纹模式,2:自行车道)
④在LinkID中指定最近的LaneID。
⑤在BdID field中设置边界（Type指定为0）。
⑥重复①到⑤。
⑦ 在创建人行横道(条纹模式和自行车道)后，通过点击“ Add Crosswalk(①)”按钮添加人行横道(边界)
⑧)点击 Add line of Area(②)扩展人行横道(边界）。
⑨在Type中指定的边界(0:边)
⑩在LinkID中指定最近的车道ID (④)。
⑪边界的BdID中设定为“0”。

##### ■ RoadSurfaceMark

![img](https://tools.tier4.jp/static/img/36.png)

①单击“RoadSurfaceMark”按钮添加一个RoadSurfaceMark。
②通过点击“Add line of Area”，扩展RoadSurfaceMark。
③在Type中用字符形式说明RoadSurfaceMark的实际意义。
④在LinkID中指定最近的车道ID。
⑤重复②到④。

#### 3.2.4 Road Side

Road Side由护栏和人行道组成。需要确认在项目面板中选择路侧复选框以编辑Road Side数据，必须指定所有元素的LinkID。

##### ■ Guardrail（护栏）

![img](https://tools.tier4.jp/static/img/37.png)

①点击“Add Guardrail”按钮添加护栏。
②点击“Add line of Area”扩大护栏。
③在Type字段中设置wing 类型。(0-plate, 1-pipe)
④在LinkID中指定最近的车道id。

##### ■ Sidewalk（人行道）

![img](https://tools.tier4.jp/static/img/38.png)



①通过点击“Add Sidewalk”按钮添加人行道。
②通过点击“Add line of Area”，扩大人行道。
③在LinkID中指定最近的车道id。
④重复②。

#### 3.2.5 Structure

Structure的目的是利用杆子、电线杆、路牌、信号数据、路灯、Curb mirror、墙、栅栏、路道口等在地图上表达元素结构。您需要在项目面板中确认选择Strucuture复选框以编辑Structure数据。必须指定所有元素的Linked ID。

##### ■ Pole

有各种各样的杆单独存在，如标志杆、护栏杆和护栏杆。

![img](https://tools.tier4.jp/static/img/39.png)

①点击“Add Poledata”按钮添加Pole。
②在LinkID中指定最近的车道ID。
③在Length中设定杆的长度，在Dem中设定杆的粗细。

##### ■ Signal data

为了尽可能准确地指示信号灯的方向，需要从交通灯中心得到方向向量。

![img](https://tools.tier4.jp/static/img/40.png)
①通过点击“Add Signaldata”按钮，添加Signaldata。
②在PLID中设定信号灯的所在杆的ID。
③在Type中设定信号灯的类型。(1:红色,2:蓝色,3:黄色,4:行人信号红色,5:行人信号蓝色,9:其他)
④在LinkID 中设定最近的车道。
⑤通过Hang和Vang调整水平和垂直角度

##### ■ Road sign

从信号板的中心得到指示方向的向量，当需要一个杆时，确保设置它的ID。

![img](https://tools.tier4.jp/static/img/41.png)

①单击“Add Roadign”按钮添加Roadsign。
②在PLID 中指定杆的id。
③在LinkID中指定最近的车道LaneID。
④通过Hang和Vang调整水平和垂直角度

##### ■ Utility pole

在某些情况下，可能无法设置电线杆的高度。

![img](https://tools.tier4.jp/static/img/42.png)

①单击“Utilitypole”按钮添加电线杆。
②在LinkID中指定最近的车道ID。
③在Length中指定杆的长度，在Dem中指定的粗细。

##### ■ Street light

它是是夜间行驶的重要特征物。需要杆时，请务必设置其ID， 仅支持直杆。

![img](https://tools.tier4.jp/static/img/43.png)

① 点击 “Add Streetlight”添加街道路灯
②在 PLID 中 指定杆的id
③  在LinkID中指定最近的LaneID .

##### ■ Curve mirror

从mirror中心获取方向向量（方向是镜面中心向外指出）。当需要杆时，请务必设置其ID。

![img](https://tools.tier4.jp/static/img/44.png)

①单击“Curve mirror”按钮添加Curve mirror。
②在PLID中指定杆的id。
③在LinkID中指定最近的车道ID。
④在Length中指定杆的长度，在Dem中指定杆的粗细。

##### ■ Wall

![img](https://tools.tier4.jp/static/img/45.png)

①单击“Add Wall”按钮添加wall。
②单击“Add line of Area”按钮扩展wall。
③重复②。
④在LinkID中指定最近的LaneID。

##### ■ Fence

![img](https://tools.tier4.jp/static/img/46.png)
①单击“Add Fence”按钮添加栅栏。
②单击“Add line of Area”按钮展开栅栏。
③重复②。
④在LinkID中指定最近的LaneID。

##### ■ Railroad Crossing

![img](https://tools.tier4.jp/static/img/47.png)

①单击“add RailroadCrossing”按钮添加铁路交叉口。
②单击“Add line of Area”按钮扩展铁路交叉口。
③重复②。
④在LinkID中指定最近的LaneID。