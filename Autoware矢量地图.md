

# autoware Vector Map

## 1、基本操作

### 注册Tier.iv账号

Accessing Vector Map Builder

使用VectorMap及相关服务需要注册T4账号
注册地址：<https://account.tier4.jp/>
![img](https://tools.tier4.jp/static/img/2.png)



进入标注地址 ( <http://maps.tier4.jp/>).

![img](https://tools.tier4.jp/static/img/1.png)

### 基本操作

#### 界面结构

1. Header menu
2. Toolbox ( add / edit tools for various objects )
3. Change the camera button
4. Project panel ( Data being read list panel )
5. Edit panel ( Detailed and Edit panel of the selected object )
6. VectorMap visualization area

#### 1. Header menu

在此菜单的帮助下，您可以加载、导出和创建新项目。

#### ◆ Setting function

FPS设置，增加每秒帧数(FPS)将使屏幕刷新更频繁，将消耗您的计算机资源，默认值是20。如果你遇到一些问题，或者你的电脑没有专门的GPU，试着降低这个值来改善你的体验。允许的最大值是60。这将提供一个平滑的可视化。

![img](https://tools.tier4.jp/static/img/5.png)

![img](https://tools.tier4.jp/static/img/6.png)



#### ◆ User Guide function

单击Help / UserGuide将显示当前手册。

![img](https://tools.tier4.jp/static/img/7.png)

#### 2. Toolbox ( Add / Edit tools for various objects )

Inside this toolbox you will find the button required to add objects to the VectorMap. Several categories can be find switching while clicking in the tabs.

在这个工具箱中，您将找到向VectorMap添加对象所需的按钮。在选项卡中单击时可以找到几个类别的切换。

#### 3. Camera POV

 vector map可以在二维和三维两种模式下进行可视化。在3D模式下，可以在单击和拖动可视化区域的同时移动相机。相机的高度可以通过改变垂直移动(3D)参数来调整。

#### 4. Project panel ( Data being read list panel )

The data previously loaded can be displayed or hidden with the help of this panel (ADAS Map, Waypoints, PointCloud and data lists)
When loading several ADAS Maps in the same project, only one can be shown at a time. To select which one to display, do it so by clicking on the option button next to the name of the map.
You can also select which sub region of the map to display using the check buttons indicated in ①

在此面板(ADAS Map、Waypoints、PointCloud和数据列表)的帮助下，可以显示或隐藏以前加载的数据。在同一项目中加载多个ADAS Map时，一次只能显示一个。要选择要显示哪一个，单击地图名称旁边的option按钮。你还可以使用414/5000中所示的复选按钮选择要显示地图的哪个子区域

![img](https://tools.tier4.jp/static/img/8.png)

1. Create / Edit switch ( checkbox ) 
- -允许在版本和创建模式之间切换。

2. Show / hide ( eye icon)
- 用于隐藏显示在左侧的对象实例

3. Setting function ( gear icon)
- Delete / Focus function
- Delete: delete map data
- Focus: camera focus

4. Select - deselect function
- Select / deselect all child objects.

5. Show - hide function
- Show / Hide all child objects.

#### 5. Edit panel ( Details and Edit panel of the selected object )

显示所选对象的详细信息。它还允许更改object的固有参数。

#### 6. Main page

#### ◆ Focus function

此函数允许将坐标轴移动到所需位置。这可以通过单击action菜单上的focus按钮，然后将其拖动到新的选定位置来实现。

![img](https://tools.tier4.jp/static/img/9.png)

![img](https://tools.tier4.jp/static/img/10.png)

#### ◆ Shortcuts key

按下Delete键可以将Object从可视化区域中删除。

向量和极点objects可以使用复制/粘贴组合进行复制。

当窗口中显示“复制对象”时，复制的对象将在相同的位置生成。

![img](https://tools.tier4.jp/static/img/11.png)

如果出现“此对象不支持复制”消息，则无法使用此方法复制所选对象。

![img](https://tools.tier4.jp/static/img/12.png)

#### Arranging objects

![img](https://tools.tier4.jp/static/img/13.png)

可以移动的Objects将显示如图所示的箭头。

若要沿着所需轴移动对象，请单击并拖动箭头到所需位置。

当拖动显示在对象中心的小矩形时，对象也可以沿平面移动。

## 2、创建ADASMap

从顶部标题菜单的Create选项卡中单击Create ADASMap

![img](https://tools.tier4.jp/static/img/14.png)

为新Map编写一个名称，选择一个平面参考编号(见下图)，然后单击Create按钮。

Write a name, choose a Plane reference number (see below) for the new map, and click the Create button.

![img](https://tools.tier4.jp/static/img/15.png)

Reference number refers to the position according to this link.

参考编号是指根据这个链接的位置。

["わかりやすい平面直角座標系｜国土地理院" - http://www.gsi.go.jp/sokuchikijun/jpc.html](http://www.gsi.go.jp/sokuchikijun/jpc.html)

![img](https://tools.tier4.jp/static/img/16.png)

通过点击顶部栏头菜单中的“导入PCD…”按钮加载PCD文件(点云数据)

![img](https://tools.tier4.jp/static/img/17.png)

Select PCD folders/files using the Browse… button.

使用Browse…按钮选择PCD文件夹/文件。

![img](https://tools.tier4.jp/static/img/18.png)

Click the import button after selecting the checkbox of reading the file and setting Max Intensity.
And the initial setting value of Max Intensity is 10.

选择“读取文件并设置最大强度”复选框后，单击“导入”按钮。

最大强度初始设定值为10。

![img](https://tools.tier4.jp/static/img/19.png)

## 3、ADASMap编辑

## Import

\1. Click “Import ADASMap…” button from the header menu at the top, and select ADAS Map folder you want to read. 
\2. Select the folder containing the list of CSV files to be read.
\3. Import the map files using the Import button.

1. 从顶部的标题菜单中点击“导入ADASMap…”按钮，选择要读取的ADASMap文件夹。

2. 选择包含要读取的CSV文件列表的文件夹。

3.使用Import按钮导入map文件。

## Edit ( ex. Road Network ))

1. From the upper right project panel, select the desired data to edit ( ex. Select the Road region when you edit the Road network )
   2. Select the node and lane desired to edit
      3. Once selected, the parameters can be changed in the upper right edit panel. 
         4. New nodes and lanes can be added from the toolbox.

 1从右上角的项目面板中，选择要编辑的数据(例如，在编辑路网时选择道路区域)

2选择要编辑的节点和lane

3一旦选中，可以在右上角的编辑面板中更改参数。

4可以从工具箱中添加新的节点和通道。  

## Export

\1. Select the desired ADAS map to export, then click “Export ADASMap...” button from the header menu at the top.
\2. Download the zip file to the local place.

## Objects in the ADASMap specification

### Road

・Node、Lane
・Way area
・Dtlane：not supported in current release

### Road Shape

・Curb
・Roadedge
・Gutter
・Intersection

### 道路表面（Road Surface）

・Whiteline
・Stopline
・Zebrazone
・Crosswalk
・RoadSurfaceMark

### Roadside

・Guardrail
・Sidewalk

### Structure

・Pole
・Utilitypole
・Roadsign
・Signaldata
・Streetlight
・Curvemirror
・Wall
・Fence
・RailroadCrossing

### Road

The purpose of Road is to express the road line, and network information using Node, Lane, Dtlane, and Way area.(Dtlane not supported in current release).Be sure to select the Road network using the checkbox in the project panel to edit the corresponding Road data.

#### ■ Node, Lane

Lane is used to define the road where vehicles drive.
It is made up of two nodes. The default length is 1m on general roads,5m on highways. 
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/20.png)

![img](https://tools.tier4.jp/static/img/21.png)

① Add a node by clicking Add Node button. 
② Add a Lane by clicking “Add Node with Lane” while selecting the ending node.
Working with lanes
③ Input Dtlane ID in the DID field.（Dtlane not supported in current release.）
④ Specify the current lane branching/merging pattern in JCT.
　 normal (0), branch to the left (1) branch to the right (2), merge to the left (3) merge to the right (4) ）
⑤ Excluding BLID, specify the lane ID merged to this lane in the BLID2-4 field. 
　( Choose 0 ( if None ) when no lane is merged. )
⑥ Excluding FLID, specify the lane ID branch to this lane in the FLID2-4 field. 
　( Choose 0 ( if None ) when no lane is branched. )
⑦ Crossing ID. (Use only on intersections, input the Cross ID, otherwise value is 0 ）
⑧ Number of lanes in the LCnt field. （ Inner intersection is 0 ）
⑨ Number of lanes in the Lno field. 
　（ Set it to 1 from the left lane. Inner intersection is 0 ） 
⑩ Maximum speed in the LimitVel field.
⑪ Target speed ( estimated speed during driving ) in the RefVel field.
⑫ Lane Type.
　（ 0: Center lane, 1: Left lane, 2: Right lane ）
⑬ Input Road section in the RoadSecID field.
　（ Inside Intersection is 0, Adjacent lanes usually have the same RoadSecID. ）
⑭ Select whether lanes can be changed or not in the LaneChgFG field.
　（ 0: Permitted, 1: Not permitted ( Inside intersection is 0 ) ）
⑮ Way area ID in the LinkWAID field.
⑯ Repeat ② to ⑮ . 

#### ■ Lanes

There are two main functions of the Lane object

##### ◯ Reverse Direction

This function allows to change the lane direction by replacing BNID and FNID.

〔 Method of operation 〕
After selecting the lane desired to change its direction, click “ Change direction ” button in Edit panel.

![img](https://tools.tier4.jp/static/img/22.png)

　![img](https://tools.tier4.jp/static/img/23.png)　↓　　



##### ◯ Lane Split

This function allows to split a lane larger than 1m. 
（ Display [Split Lane] button in Action when select a lane which is over 1m ）

〔 Method of operation 〕
After selecting the lane to split, click “Split Lane” button in Action.



　　![img](https://tools.tier4.jp/static/img/24.png)↓　　

![img](https://tools.tier4.jp/static/img/25.png)

#### ■ Wayarea

Wayarea defines the area of the road that vehicles can drive onto.

〔 Method of operation 〕

![img](https://tools.tier4.jp/static/img/25.png)

① Add Wayarea by clicking “Add Wayarea” button.
② Extend Wayarea by clicking “Add line of Area” button.
（※ Dot lane means the connect from start point to end point. ) ③ Repeat ②.

#### ■ Dtlane

Dtlane contains a Linear element and longitudinal transverse gradient added.
Not supported in this version.

### Road Shape

The purpose of Road shape is to define the road shape by using Curve, Road Edge, Gutter and Intersection. You need to confirm selecting Road Shape checkbox in project panel to edit Road Shape data. Finally, specify Linked ID for each element.

#### ■ Curb

〔 How to create 〕



![img](https://tools.tier4.jp/static/img/25.png)

![img](https://tools.tier4.jp/static/img/25.png)

① Add Curb by clicking “Add Curb” button.
② Set the height of curb at Height.
③ Define the width of curb using the Width parameter.
　（ the direction of width is set using the Dir param, explained below ）
④ Set the direction of the recently created curb using Dir parameter ( right : 1, left : 0 ）
⑤ Input the closest lane ID in LinkID after selecting Lane.
⑥ Expand Curb by clicking “Add Curb” button after selecting the end node.
⑦ Repeat ② to ⑥ .

#### ■ RoadEdge

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/28.png)

① Add a RoadEdge element by clicking “Add RoadEdge” button. 
② Set the nearest lane ID in LinkedID after selecting Lane.
③ Expand the RoadEdge by clicking “Add RoadEdge” button after selecting the end node.
④ Repeat ② and ③ .

#### ■ Gutter

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/29.png)

① Add Gutter by clicking “Add Gutter” button. 
② Set the closest lane ID in LinkedID after selecting Lane.
③ Indicate the type of Gutter using the Type parameter.
（ 0 : No lid, 1 : With lid, 2 : Grating ）
④ Expand the Gutter by clicking “Add line of Area” button. 
⑤ Repeat ④.

#### ■ Intersection

The area of ideal intersection until stopline.Circumference is usually the same line as road edge. 
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/30.png)

① Add an intersection by clicking “Add intersection”.
② Set the closest lane ID using LinkID parameter.
③ Expand the intersection by clicking “Add line of Area” button.
④ Repeat ③ .

### Road Surface

The purpose of Road surface is to express the line and mark etc. on the road by using White line、Stop Line、Zebrazone、Crosswalk、 RoadSurfaceMark. You need to confirm selecting Road Surface checkbox in project panel to edit Road Surface data.And you have to specify Linked ID to all element.

#### ■ Whiteline

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/31.png)

![img](https://tools.tier4.jp/static/img/32.png)

① Add a white line by clicking “Add White line” button.
② Set the width of the line using the Width parameter.
③ Select the color of line with the Color parameter.
（ White：W（White）, Yellow：Y（Yellow））
④ Select the type of line.
（ 0 : Solid line, 1 : Dotted line, 2 : Blank of dotted line）
⑤ Set the closest lane ID using LinkID.
⑥ Extend white line by clicking Add White line after selecting “Add Whiteline”.
⑦ Repeat ② to ⑥ .

#### ■ Stopline

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/33.png)

① Add a Stopline by clicking “Add Stopline” button.
② Set the associated signal lamp id in the TLID parameter.
③ Define the associated sign id in the SignID parameter.
④ Set the closest Lane ID using the LinkID parameter.
（※ In ③ and ④, reflect the closer one if there are multiple signal lamps and signs ）

#### ■ Zebrazone

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/34.png)

① Add a Zebrazone by clicking “Add Zebrazone” button. 
② Set the closest lane ID in the LinkID parameter.
③ Extend the zebrazone by clicking “Add lane of Area” button.
④ Repeat ③.

#### ■ Crosswalk

Crosswalk is made of borders, striped pattern and bicycle passage zone (in most of the cases).
〔 How to create 〕



① Add a Crosswalk ( striped pattern and bicycle passage zone ) by clicking “Add Crosswalk” .![img](https://tools.tier4.jp/static/img/35.png)
② Expand Crosswalk ( striped pattern and bicycle passage zone ) by clicking “Add line of Area”.
③ Select the crosswalk pattern using Type.
（ 1 : striped pattern, 2 : bicycle passage zone ）
④ Set the closest Lane ID in the LinkID field. 
⑤ Set the border id on the BdID field.
⑥ Repeat ① to ⑤ .
⑦ Add Crosswalk ( border ) by clicking Add Crosswalk(①) after creating Crosswalk ( striped pattern and bicycle passage zone ) by clicking “Add Crosswalk(①)” button.
⑧ Extend Crosswalk ( border ) by clicking Add line of Area (②).
⑨ Select the border in the Type field(③)（ 0 : border ）
⑩ Define the nearest Lane ID in the LinkID parameter (④). 
⑪ Set “0” in the BdID field. 

#### ■ RoadSurfaceMark

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/36.png)

① Add a RoadSurfaceMark by clicking “RoadSurfaceMark” button.
② Extend RoadSurfaceMark by clicking “Add line of Area”.
③ Set the meaning of RoadSurfaceMark at Type by character.
④ Input the nearest Lane ID in the LinkID field. 
⑤ Repeat ②.

### Road Side

Road Side is made of Guardrail and Sidewalk.You need to confirm selecting Road Side’s checkbox in project panel to edit Road Side’s data. And you have to specify Linked ID to all element.

#### ■ Guardrail

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/37.png)

① Add Guardrail by clicking “Add Guardrail” button. 
② Expand Guardrail by clicking “Add line of Area”.
③ Select wing type in the Type field. ( 0 : Feathers , 1: Pipe )
④ Input the near lane id on the LinkID parameter. 

#### ■ Sidewalk

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/38.png)

① Add Sidewalk by clicking “Add Sidewalk” button.
② Expand Sidewalk by clicking “Add line of Area”. 
③ Input the near lane id in the LinkID field.
④ Repeat ②.

### Structure

The purpose of Structure is to express the structure on the map by using Pole, Utility pole、Road sign, Signal data, Streetlight, Curb mirror, Wall, Fence, Railroad Crossing.You need to confirm selecting Strucuture checkbox in project panel to edit Structure data.And you have to specify Linked ID to all element.

#### ■ Pole

There are various pole such as sign poles, guardrail poles and guard poles that exist alone.
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/39.png)

① Add Pole by clicking “Add Poledata” button.
② Write the nearest Lane ID in the LinkID field.
③ Input the length of pole in Length and the dimension of pole in the Dem fields.

#### ■ Signal data

To be as accurate as possible and indicate the direction of the signal lamp. Obtain the direction vector from the center of the traffic signal lamp.
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/40.png)

① Add Signaldata by clicking “Add Signaldata” button.
② Input the pole of signal lamp in the PLID field to set the parent pole.
③ Select the type of lamp using the Type field.
（ 1 : Red, 2 : Blue, 3 : Yellow, 4 : Pedestrian signal red, 5 : Pedestrian signal blue, 9 : other ）
④ Input the nearest Lane ID using the LinkID field. 
⑤ Input the rotation angles at Hang, Vang to adjust the direction of the vector.

#### ■ Road sign

Obtain the vector indicating the direction from the center of the signal plate.When a pole is required, make sure to set its ID.
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/41.png)

① Add Roadsign by clicking “Add Roadign” button. 
② Input the id of the follow pole using the PLID field.
③ Input the nearest lane ID in the LinkID field.
④ Input the rotation angles in the Hang, and Vang fields to adjust the direction of the vector.

#### ■ Utility pole

It might no be possible to set the height of the utility pole in certain cases.
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/42.png)

① Add Utility pole by clicking “Utilitypole” button. 
② Input the nearest lane id using the LinkID field.
③ Input the length of the pole in the Length field and the dimension of pole in Dem.

#### ■ Street light

It is assumed as a recognizable feature from a vehicle during night.
When a pole is required, make sure to set its ID. Only straight poles are supported.
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/43.png)

① Add street light by clicking “Add Streetlight”.
② Input the id of the following pole in the PLID field.
③ Input the nearest lane id in the LinkID field.

#### ■ Curve mirror

Get the direction vector from the center of the mirror.When a pole is required, make sure to set its ID.
〔 How to create 〕

![img](https://tools.tier4.jp/static/img/44.png)

① Add Curve mirror by clicking “Curve mirror” button.
② Input the id of the follow pole in the PLID field.
③ Input the nearest lane id in the LinkID field.
④ Input the length of the pole in the Length field and its dimension in Dem.

#### ■ Wall

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/45.png)

① Add a Wall by clicking the “Add Wall” button.
② Expand a Wall by clicking “Add line of Area” button.
③ Repeat ②.
④ Input the nearest lane id in the LinkID field.

#### ■ Fence

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/46.png)

① Add a Fence by clicking the “Add Fence” button.
② Expand a Fence by clicking the “Add line of Area” button.
③ Repeat ②.
④ Input the nearest lane id in the LinkID field.

#### ■ Railroad Crossing

〔 How to create 〕

![img](https://tools.tier4.jp/static/img/47.png)

① Add a Railroad Crossing by clicking “Add RailroadCrossing” button.
② Expand a Railroad Crossing by clicking “Add line of Area” button.
③ Repeat ②.
④ Input the nearest lane ID in the LinkID field.