# shadow_plugin_test
宿主加载插件找不到so问题demo

代码说明：

       宿主代码：host-project      
       插件代码：plugin-project     
       manager代码：manager-project

       打包后的插件文件：plugin-debug.zip
       打包后的manager apk: sample-manager-debug.apk

demo所用shadow版本：
       当前GitHub上该项目的master分支。

背景：

       公司项目网络请求需要接入微信网关相关sdk，该sdk都是so文件。插件项目接入该so，用宿主去加载插件，会出现加载so失败的报错。

现象：

     1，用集成老shadow版本的宿主，manager也是老的（2.1.0版本），加载集成微信网关so的插件，加载启动插件是正常的。         
     2，用集成新shadow版本的宿主,  manager是当前最新的（当前GitHub上该项目的master分支），加载集成微信网关so的插件，加载启动插件会报错。
     3，插件代码 plugin-project  编译成独立运行的apk，运行加载so没问题。 打包成插件，然后通过宿主APP去加载启动，加载so会有问题，报错如下：



2023-03-01 11:14:13.650 18888-18888/com.tencent.shadow.sample.host E/AndroidRuntime: FATAL EXCEPTION: main
    Process: com.tencent.shadow.sample.host:plugin, PID: 18888
    java.lang.UnsatisfiedLinkError: com.tencent.shadow.core.loader.classloaders.PluginClassLoader[DexPathList[[zip file "/data/user/0/com.tencent.shadow.sample.host/files/ShadowPluginManager/UnpackedPlugin/sample-manager/30debf5002f47c8e419059c3f86e210e/plugin-debug.zip/plugin-app-plugin-iwirst-debug.apk"],nativeLibraryDirectories=[/system/lib64]]] couldn't find "libtquic.so"
        at java.lang.Runtime.loadLibrary0(Runtime.java:1012)
        at java.lang.System.loadLibrary(System.java:1669)
        at com.tencent.protocol.PrivateLink.<clinit>(PrivateLink.java:15)
        at com.tencent.protocol.PrivateLink.getPrivateLink(PrivateLink.java:70)
        at com.tencent.shadow.sample.plugin.MainActivity.onCreate(MainActivity.java:30)
        at com.tencent.shadow.core.loader.delegates.ShadowActivityDelegate.onCreate(ShadowActivityDelegate.kt:156)
        at com.tencent.shadow.core.runtime.container.PluginContainerActivity.onCreate(PluginContainerActivity.java:84)
        at android.app.Activity.performCreate(Activity.java:7144)
        at android.app.Activity.performCreate(Activity.java:7135)
        at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1271)
        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2928)
        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:3083)
        at android.app.servertransaction.LaunchActivityItem.execute(LaunchActivityItem.java:78)
        at android.app.servertransaction.TransactionExecutor.executeCallbacks(TransactionExecutor.java:108)
        at android.app.servertransaction.TransactionExecutor.execute(TransactionExecutor.java:68)
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1840)
        at android.os.Handler.dispatchMessage(Handler.java:106)
        at android.os.Looper.loop(Looper.java:193)
        at android.app.ActivityThread.main(ActivityThread.java:6713)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:493)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:911)


  demo中就是插件项目基础微信网关sdk，然后宿主去启动插件，会出现找不到so问题。麻烦帮忙看下什么原因。谢谢！


