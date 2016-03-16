# ReactNative安卓首屏白屏优化

#问题描述
公司现有app中部分模块使用reactnative开发，在实施的过程中，rn良好的兼容性，极佳的加载、动画性能，提升了我们的开发、测试效率，提升了用户体验。

但是，在android中，当点击某个rn模块的入口按钮，弹出rn的activity到rn的页面展现出来的过程中，会有很明显的白屏现象，不同的机型不同（cpu好的白屏时间短），大概1s到2s的时间。

注意，只有在真机上才会有此现象，在模拟器上没有此现象完全是秒开。ios上也是秒开，测试的最低版本是ios7，iphone4s。

reactnative版本0.20.0。

jsbundle文件大小717kb。

#优化效果
经过了大量的源码阅读，和网上资料查找，最终完美的解决了这个问题，无论什么机型，都可以达到秒开，如图（虽然下图是模拟器的截图，但是真机效果基本一样）：

![1](media/1.gif)

#优化过程

##时间分布
一般优化速度问题，首先就是要找到时间分布，然后根据二八原则，先优化耗时最长的部分。

android集成rn都会继承官方提供的ReactActivity

```
public class MainActivity extends ReactActivity {
```
然后只在自己的activity中覆盖一些配置项。

在官方的ReactActivity中的onCreate方法中

```
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    if (getUseDeveloperSupport() && Build.VERSION.SDK_INT >= 23) {
      // Get permission to show redbox in dev builds.
      if (!Settings.canDrawOverlays(this)) {
        Intent serviceIntent = new Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION);
        startActivity(serviceIntent);
        FLog.w(ReactConstants.TAG, REDBOX_PERMISSION_MESSAGE);
        Toast.makeText(this, REDBOX_PERMISSION_MESSAGE, Toast.LENGTH_LONG).show();
      }
    }

    mReactInstanceManager = createReactInstanceManager();
    ReactRootView mReactRootView = createRootView();
    mReactRootView.startReactApplication(mReactInstanceManager, getMainComponentName(), getLaunchOptions());
    setContentView(mReactRootView);
  }
```
最慢的就是这两行代码，占了90%以上的时间。

```
ReactRootView mReactRootView = createRootView();
mReactRootView.startReactApplication(mReactInstanceManager, getMainComponentName(), getLaunchOptions());
```

这两行代码就是把jsbundle文件读入到内存中，并进行执行，然后初始化各个对象。

##优化思路---内存换时间

在app启动时候，就将mReactRootView初始化出来，并缓存起来，在用的时候直接setContentView(mReactRootView)，达到秒开。

###步骤1 缓存rootview管理器

缓存rootview管理器主要用于初始化和缓存rootview对象。

```
import android.app.Activity;
import android.os.Bundle;
import android.view.ViewParent;

import com.facebook.react.LifecycleState;
import com.facebook.react.ReactInstanceManager;
import com.facebook.react.ReactPackage;
import com.facebook.react.ReactRootView;

import java.lang.reflect.Field;

/**
 * 缓存view管理
 */
public class RNCacheViewManager {

    private static ReactRootView mRootView = null;
    private static ReactInstanceManager mManager = null;
    private static AbsRnInfo mRnInfo = null;

    //初始化
    public static void init(Activity act, AbsRnInfo rnInfo) {
        init(act, rnInfo, null);
    }

    public static void init(Activity act, AbsRnInfo rnInfo, Bundle launchOptions) {
        if (mManager == null) {
            updateCache(act, rnInfo, launchOptions);
        }
    }

    public static void updateCache(Activity act, AbsRnInfo rnInfo) {
        updateCache(act, rnInfo, null);
    }

    //更新cache，适合于版本升级时候更新cache
    public static void updateCache(Activity act, AbsRnInfo rnInfo, Bundle launchOptions) {
        mRnInfo = rnInfo;
        mManager = createReactInstanceManager(act);
        mRootView = new ReactRootView(act);
        mRootView.startReactApplication(mManager, rnInfo.getMainComponentName(), launchOptions);
    }

//设置模块名称，因为是private，只能通过反射赋值
    public static void setModuleName(String moduleName) {
        try {
            Field field = ReactRootView.class.getDeclaredField("mJSModuleName");
            field.setAccessible(true);
            field.set(getReactRootView(), moduleName);
        } catch (Throwable e) {
            throw new RuntimeException(e);
        }
    }
    
//设置启动参数，因为是private，只能通过反射赋值
    public static void setLaunchOptions(Bundle launchOptions) {
        try {
            Field field = ReactRootView.class.getDeclaredField("mLaunchOptions");
            field.setAccessible(true);
            field.set(getReactRootView(), launchOptions);
        } catch (Throwable e) {
            throw new RuntimeException(e);
        }
    }

    public static ReactRootView getReactRootView() {
        if(mRootView==null){
            throw new RuntimeException("缓存view管理器尚未初始化！");
        }
        return mRootView;
    }

    public static ReactInstanceManager getReactInstanceManager() {
        if(mManager==null){
            throw new RuntimeException("缓存view管理器尚未初始化！");
        }
        return mManager;
    }

    public static AbsRnInfo getRnInfo() {
        if(mRnInfo==null){
            throw  new RuntimeException("缓存view管理器尚未初始化！");
        }
        return mRnInfo;
    }

    public static void onDestroy() {
        try {
            ViewParent parent = getReactRootView().getParent();
            if (parent != null)
                ((android.view.ViewGroup) parent).removeView(getReactRootView());
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }

    public static void clear() {
        try {
            if (mManager != null) {
                mManager.onDestroy();
                mManager = null;
            }
            if (mRootView != null) {
                onDestroy();
                mRootView = null;
            }
            mRnInfo = null;
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }

    private static ReactInstanceManager createReactInstanceManager(Activity act) {

        ReactInstanceManager.Builder builder = ReactInstanceManager.builder()
                .setApplication(act.getApplication())
                .setJSMainModuleName(getRnInfo().getJSMainModuleName())
                .setUseDeveloperSupport(getRnInfo().getUseDeveloperSupport())
                .setInitialLifecycleState(LifecycleState.BEFORE_RESUME);

        for (ReactPackage reactPackage : getRnInfo().getPackages()) {
            builder.addPackage(reactPackage);
        }

        String jsBundleFile = getRnInfo().getJSBundleFile();

        if (jsBundleFile != null) {
            builder.setJSBundleFile(jsBundleFile);
        } else {
            builder.setBundleAssetName(getRnInfo().getBundleAssetName());
        }
        return builder.build();
    }
}

```

###步骤2 重写ReactActivity
将官方的ReactActivity粘出来，重写2个方法，onCreate和onDestroy，其余代码不动。

onCreate方法中使用缓存rootview管理器来获得rootview对象，而不是重新创建。

这里曾尝试继承ReactActivity，而不是重写这个类，但是子类覆盖onCreate方法时候，必须要调用super.onCreate，否则编译会报错，但是super.onCreate方法会重新创建rootview，所以实在是绕不过去了。

```
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        if (RNCacheViewManager.getRnInfo().getUseDeveloperSupport() && Build.VERSION.SDK_INT >= 23) {
            // Get permission to show redbox in dev builds.
            if (!Settings.canDrawOverlays(this)) {
                Intent serviceIntent = new Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION);
                startActivity(serviceIntent);
                FLog.w(ReactConstants.TAG, REDBOX_PERMISSION_MESSAGE);
                Toast.makeText(this, REDBOX_PERMISSION_MESSAGE, Toast.LENGTH_LONG).show();
            }
        }

        mReactInstanceManager = RNCacheViewManager.getReactInstanceManager();
        ReactRootView mReactRootView = RNCacheViewManager.getReactRootView();
        setContentView(mReactRootView);
    }
```


onDestroy方法中，不能再调用原有的mReactInstanceManager.destroy()方法了，否则rn初始化出来的对象会被销毁，下次就用不了了。同时，要卸载掉rootview的parent对象，否则下次再setContentView时候回报错。

```
protected void onDestroy() {
        RNCacheViewManager.onDestroy();
        super.onDestroy();
    }
```    
    
RNCacheViewManager.onDestroy的方法：

```
public static void onDestroy() {
        try {
            ViewParent parent = getReactRootView().getParent();
            if (parent != null)
                ((android.view.ViewGroup) parent).removeView(getReactRootView());
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }
```


###步骤3 在app启动时候初始化缓存rootview管理器

```
RNCacheViewManager.init((Activity) context, new RnInfo(moduleName, launchOptions));
```

其中RnInfo如下：

```
public class RnInfo extends AbsRnInfo {

    private String mModuleName;
    private Bundle mLaunchOptions;

    public RnInfo(String moduleName) {
        this.mModuleName = moduleName;
    }

    public RnInfo(String moduleName, Bundle launchOptions) {
        this.mModuleName = moduleName;
        this.mLaunchOptions = launchOptions;
    }

    @Nullable
    @Override
    public Bundle getLaunchOptions() {
        return mLaunchOptions;
    }

    @Override
    public String getMainComponentName() {
        return mModuleName;
    }

    @Override
    public String getJSMainModuleName() {
        return RNKeys.Default.DEf_JS_MAIN_MODULE_NAME;
    }

    @Nullable
    @Override
    public String getJSBundleFile() {
        return RNManager.getJsBundlePath();
    }

    @Override
    public boolean getUseDeveloperSupport() {
        return true;
    }

    @Override
    public List<ReactPackage> getPackages() {
        return Arrays.asList(
                new MainReactPackage(),
                new BBReactPackage()
        );
    }
}
```

#结语
希望本篇文档能帮助遇到类似问题的小伙伴们。

reactnative虽然不是银弹，但是在目前移动端浏览器兼容性弱爆了的情况下，还是能极大的提升开发测试效率的，性能也是极佳的，看好rn的未来。


#参考文章
http://zhuanlan.zhihu.com/magilu/20587485

http://zhuanlan.zhihu.com/magilu/20259704

https://yq.aliyun.com/articles/3208?spm=5176.100239.yqblog1.39.t2g49u&utm_source=tuicool&utm_medium=referral


