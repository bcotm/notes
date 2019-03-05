
## Android
### 基础知识
**APK** 代码+资源+数据文件  ----> Android SDK 编译 ----> APK存档文件。   
**Android APP**  对应一个 **Linux User ID**，这样就可以使用Android的权限控制，文件访问权限等。   
**Android APP Process**  <--> VM，每个进程有一个独立的虚拟机。   

这样实现 **最小权限** 控制   
可以两个APP共用一个ID，证书要相同

### Android应用组件
#### Activity
**用户界面单一屏幕**，相互独立存在。
#### Service
**后台运行组件**，可以与Activity绑定。
#### Content Provider
**共享的应用数据**，鉴权供其他应用使用。存储在文件系统、SQLite数据库、网络等。
#### Broadcast Receiver
**用于响应系统范围的广播通知的组件**。屏幕关闭、电量不足等系统广播，其他应用数据下载完成等应用广播。状态栏通知。

任何应用可启动其他应用的组件，由系统中转Intent消息(Activity、服务、广播接收器)。   
系统启动应用组件时，启动应用进程、实例化该组件所需的类。因此，android没有main函数。   

### 清单文件
AndroidManifest.xml（应用项目根目录）  
**作用**：   
1. 告知android系统应用组件信息。包括组件功能(Intent-filter等)   
2. 说明应用需要的权限(联系人、互联网等)、最低API级别、软硬件功能(相机等)、API库等   
<activity> <service> <receiver>（可动态创建） <provider>


### 应用资源(Resources)
与应用视觉呈现有关的内容，通过 XML 文件定义用户界面的动画、菜单、样式、颜色和布局  
SDK构建工具给每项资源分配唯一整形 ID。保存在 **res/** 目录下。   
通过限定符类支持适配。   
提供默认资源res/，备用资源<resources_name>-<config_qualifier>(qualifier有严格顺序)，屏幕方向、语言、屏幕分辨率、键盘可用性等。   
系统资源匹配算法-先淘汰冲突，再按限定符优先级从高往低   
animator, anim,    
color, 有个statelist，first match    
drawable, 可绘制对象   
mipmap,    
layout, 布局文件   
menu, raw,    
values, 字符串、整形数、颜色等单值的xml文件   
xml，
资源别名不需要在所有目录下都拷贝同一份资源，创建一个xml指向默认资源即可。   
布局别名 <merge> <include layout="\@layout/main_ltr"/> </merge>

**运行时配置变更**处理方法：   
1. 配置变更期间保留对象，将状态对象传给新Activity实例。   
> 可以保留Fragment来减轻重新初始化Activity的负担，保留有状态对象的引用，FragmentManager。   
2. 自行处理配置变更，阻止重启，接受变更回调来处理。   
> 在清单文件中声明configChanges

Activity
entry point for an app's interaction with the user
one screen in an app
在AndroidManifest.xml中声明，<application><activity><intent-filter>
Intent-filter    显示、隐式声明
<activity android:name=".ExampleActivity" android:icon="@drawable/app_icon">
<intent-filter>
<action android:name="android.intent.action.SEND" />
<category android:name="android.intent.category.DEFAULT" />
<data android:mimeType="text/plain" />
</intent-filter>
</activity>

Lifecycle
 

长久状态
Resumed, Paused, Stopped
onSaveInstanceState Activity移除栈顶时保留UI

