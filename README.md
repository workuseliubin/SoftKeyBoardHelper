### 简单的几行代码解决键盘遮挡登录或注册按钮

-  在项目开发中，这种情况是十分常见的，尤其是在登陆界面，登陆按钮经常被键盘挡住，导致用户输入完账号之后需要关闭键盘，然后再输入密码，然后再关掉键盘，点击登陆。十分繁琐，一旦用户输入错误，就要重复上述步骤。

-  今天的这个小案例，就是一次性解决键盘遮挡问题，动态的计算，登陆按钮显示需要的高度，进而滚动布局，使之每次输入，键盘都无法遮挡输入框以及按钮。

### 先来一张效果图：


<iframe height=800 width=500 src="http://ww3.sinaimg.cn/mw690/005O1u7Gjw1f9sm4k991pg30eb0l77ch.gif">


### 进入代码实战：

-  极为简单的布局：

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/main"
    tools:context="com.example.liubin1.softkeyboardhelper.MainActivity">

    <EditText
        android:id="@+id/name"
        android:hint="请输入用户名："
        android:layout_centerInParent="true"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        />
    <EditText
        android:id="@+id/pas"
        android:layout_below="@id/name"
        android:hint="请输入密　码："
        android:layout_centerInParent="true"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        />
    <EditText
        android:id="@+id/rpas"
        android:layout_below="@id/pas"
        android:hint="请再输入密码："
        android:layout_centerInParent="true"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        />
    <Button
        android:id="@+id/res"
        android:layout_below="@id/rpas"
        android:layout_centerHorizontal="true"
        android:text="注册"
        android:layout_width="180dp"
        android:layout_height="50dp" />
</RelativeLayout>

```
简单的三个输入框和注册按钮。你只需要关注的是根布局main和注册按钮res

*  然后我们来看Activity

```
public class MainActivity extends AppCompatActivity {

    private EditText name;
    private EditText pas;
    private EditText rpas;
    private Button res;
    private RelativeLayout main;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initialize();
        addLayoutListener(main,res);
    }

    private void initialize() {
        name = (EditText) findViewById(R.id.name);
        pas = (EditText) findViewById(R.id.pas);
        rpas = (EditText) findViewById(R.id.rpas);
        res = (Button) findViewById(R.id.res);
        main = (RelativeLayout) findViewById(R.id.main);
    }

    /**
     *  1、获取main在窗体的可视区域
     *  2、获取main在窗体的不可视区域高度
     *  3、判断不可视区域高度
     *      1、大于100：键盘显示  获取Scroll的窗体坐标
     *                           算出main需要滚动的高度，使scroll显示。
     *      2、小于100：键盘隐藏
     *
     * @param main 根布局
     * @param scroll 需要显示的最下方View
     */
    public void addLayoutListener(final View main, final View scroll) {
        main.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                Rect rect = new Rect();
                main.getWindowVisibleDisplayFrame(rect);
                int mainInvisibleHeight = main.getRootView().getHeight() - rect.bottom;
                if (mainInvisibleHeight > 100) {
                    int[] location = new int[2];
                    scroll.getLocationInWindow(location);
                    int srollHeight = (location[1] + scroll.getHeight()) - rect.bottom;
                    main.scrollTo(0, srollHeight);
                } else {
                    main.scrollTo(0, 0);
                }
            }
        });
    }
}
```

-  OK ，里面的注释已经写的很清楚了。如果还有不明白的可以在下方给我留言。

### 重要的事情说三遍： 
1、不需要在AndroidManifest.xml中配置键盘属性。
2、不需要在AndroidManifest.xml中配置键盘属性。
3、不需要在AndroidManifest.xml中配置键盘属性。

欢迎fork star
