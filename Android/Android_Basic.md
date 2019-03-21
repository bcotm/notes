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


### 应用资源(Resources)
#### 定义
与应用视觉呈现有关的内容，**通过 XML 文件定义用户界面的动画、菜单、样式、颜色和布局**   
> 怎么定位资源？   
> SDK构建工具给每项资源分配唯一整型 ID。保存在 **res/** 目录下。   

#### 默认资源
资源文件目录 **/res** 下不能直接保存资源文件，会导致编译失败。   

- **animator**: 属性动画
- **anim**: 渐变动画
- **color**: 颜色状态列表，有个statelist，first match
- **drawable**: 位图，可绘制对象
- **mipmap**: 启动器图标可绘制文件
- **layout**: 界面布局文件
- **menu**: 应用菜单
- **values**: 字符串、整形数、颜色等单值的xml文件
- **raw**: 原始形式保存，使用方式不同
- **xml**: 运行时可调用的XML文件

#### 备用资源
通过 **资源限定符** 支持适配。资源匹配算法-先淘汰冲突，再按限定符优先级从高往低适配。   
目录 **res\\<resources_name\>-<config_qualifier\>** (qualifier有严格顺序)，适配屏幕方向、语言、屏幕分辨率、键盘可用性等。   
   
**运行时配置变更**处理方法：   
1. 配置变更期间保留对象，将状态对象传给新Activity实例。   
> 可以保留Fragment来减轻重新初始化Activity的负担，保留有状态对象的引用，FragmentManager。   
2. 自行处理配置变更，阻止重启，接受变更回调来处理。   
> 在清单文件中声明configChanges，Activity会收到onConfigurationChanged   

资源别名不需要在所有目录下都拷贝同一份资源，创建一个xml指向默认资源即可。   
布局别名 <merge> <include layout="\@layout/main_ltr"/> </merge> 


### 清单文件(AndroidManifest.xml)
应用向Android系统提供必要信息，应用项目根目录。
**作用**

- **描述应用组件**，包括Activity、服务、广播接收器、内容提供程序。
- **确定托管应用组件的进程**。
- **声明应用权限**，访问受保护的API以及与其他应用交互需要的权限。
- **声明最底Android API级别**。
- **列出必须链接的库**。

```xml
<?xml version="1.0" encoding="utf-8"?>

<manifest>

    <uses-permission />
    <permission />
    <permission-tree />
    <permission-group />
    <instrumentation />
    <uses-sdk />
    <uses-configuration />  
    <uses-feature />  
    <supports-screens />  
    <compatible-screens />  
    <supports-gl-texture />  

    <application>

        <activity>
            <intent-filter>
                <action />
                <category />
                <data />
            </intent-filter>
            <meta-data />
        </activity>

        <activity-alias>
            <intent-filter> . . . </intent-filter>
            <meta-data />
        </activity-alias>

        <service>
            <intent-filter> . . . </intent-filter>
            <meta-data/>
        </service>

        <receiver>
            <intent-filter> . . . </intent-filter>
            <meta-data />
        </receiver>

        <provider>
            <grant-uri-permission />
            <meta-data />
            <path-permission />
        </provider>

        <uses-library />

    </application>

</manifest>
```