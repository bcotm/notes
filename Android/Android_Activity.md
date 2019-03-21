## Activity
Activity是APP用户的入口。   
一个Activity提供一个window。**用户界面单一屏幕**。   
Android系统通过调用特定时期的回调来启动Activity实例中的代码。比如startActivity。   

### 注册Activity(AndroidManifest.xml)

### Activity生命周期
Activity的5个状态：

*根据状态转换图重新描述一下先后顺序*
- **Destroyed**, Activity销毁, `onDestroy()`之前
- **Initialized**, Activity构建, `onCreate()`之前
- **Created**, Activity创建完成, `onStop()`之前, 或者`onCreate()`之后
- **Started**, Activity可见, `onPause()`之前, 或者`onStart()`之后
- **Resumed**, Activity可交互, `onResume()`之后


Activity在各个状态间转换时，会触发相应的回调，**开发人员在各个回调中处理相应的工作，让状态的转换更加合适**，数据完整，用户体验良好

1. 回调时发生了什么
2. 开发人员实现什么

Started和Paused都是短暂停留，不久就会切换

**Lifecycle-Aware Components**
**UI保存更新问题，Saving UI States**
`ViewModel`, `onSaveInstanceState()`

Lifecycle-Aware组件(`android.arch.lifecycle`)可以响应activity，fragment生命周期状态变化。  
生命周期 **Event** ，**State**

- **onCreate()**：系统创建Activity时触发回调，初始化必要的组件，比如创建views，绑定data等，定义布局**`setContentView`** ，整个生命周期 **触发一次** 。可能收到`savedInstanceState`
```java
@Override
public void onCreate(Bundle savedInstanceState){
    super.onCreate(savedInstanceState);
    if(savedInstanceState != null){
        savedInstanceState.getString(XX_KEY);
    }
    setContentView(R.layout.main_activity);
}
@Override
public void onRestoreInstanceState(Bundle savedInstanceState){
    textView.setText(savedInstanceState.getString(YY_KEY));
}
@Override
public void onSaveInstanceState(Bundle outState){
    outState.putString(XX_KEY, "Wow");
    super.onSaveInstanceState(outState);
}
```
- **onStart()**：Activity进入前台的最后准备
- **onResume()**：Activity在系统Activity栈顶
- **onPause()**：暂时离开该Activity，可以持续更新UI
- **onStop()**：
- **onRestart()**：
- **onDestroy()**：Activity被摧毁之前调用，主要用于释放资源

### Activities间切换，多窗口切换
`startActivity()`
`startActivityForResult()`