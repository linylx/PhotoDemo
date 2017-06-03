# Fragment生命周期
主要流程如下：
* 看Fragment生命周期图
* 跑测试代码
## 一.Fragment生命周期图

## 二.测试相关代码
### 1.Navigation 底部导航
```java
public class Navigation extends Activity implements View.OnClickListener{

    //记录是否有首次按键
    private static boolean mBackKeyPressed = false;
    //用于展示学生信息的Fragment
    private MainActivity contactsFragment;
    //用于展示发布签到的Fragment
    private SendSign newsFragment;
    //用于展示设置的Fragment
    private Setting settingFragment;
    //学生信息界面布局
    private View contactsLayout;
    //发布签到界面布局
    private View newsLayout;
    //设置界面布局
    private View settingLayout;
    //在Tab布局上显示学生信息图标的控件
    private ImageView contactsImage;
    //在Tab布局上显示发布签到图标的控件
    private ImageView newsImage;
    //在Tab布局上显示设置图标的控件
    private ImageView settingImage;
    //在Tab布局上显示学生信息标题的控件
    private TextView contactsText;
    //在Tab布局上显示发布签到标题的控件
    private TextView newsText;
    //在Tab布局上显示设置标题的控件
    private TextView settingText;
    //标题栏
    private TextView topText;
    //用于对Fragment进行管理
    private FragmentManager fragmentManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        this.requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.navigation);
        initViews();
        fragmentManager=getFragmentManager();
        // 第一次启动时选中第0个tab
        setTabSelection(0);
    }
    /**
     -    Function:    initViews()
     -    Description:    获取控件
     **/
    private void initViews() {
        contactsLayout = findViewById(R.id.contacts_layout);
        newsLayout = findViewById(R.id.news_layout);
        settingLayout = findViewById(R.id.setting_layout);
        contactsImage = (ImageView) findViewById(R.id.contacts_image);
        newsImage = (ImageView) findViewById(R.id.news_image);
        settingImage = (ImageView) findViewById(R.id.setting_image);
        contactsText = (TextView) findViewById(R.id.contacts_text);
        newsText = (TextView) findViewById(R.id.news_text);
        settingText = (TextView) findViewById(R.id.setting_text);
        //topText=(TextView)findViewById(R.id.txt);
        contactsLayout.setOnClickListener(this);
        newsLayout.setOnClickListener(this);
        settingLayout.setOnClickListener(this);
    }
    /**
     -    Function:    onClick(View v)
     -    Description:    点击事件
     **/
    public void onClick(View v){
        switch (v.getId()) {
            case R.id.contacts_layout:
                // 当点击了学生信息tab时，选中第1个tab
                setTabSelection(0);
                break;
            case R.id.news_layout:
                // 当点击了发布签到tab时，选中第2个tab
                setTabSelection(1);
                break;
            case R.id.setting_layout:
                // 当点击了个人设置tab时，选中第3个tab
                setTabSelection(2);
                break;
            default:
                break;
        }
    }

    /**
     -    Function:    setTabSelection(int index)
     -    Description:    根据传入的index参数来设置选中的tab页
     **/
    private void setTabSelection(int index) {
        // 每次选中之前先清楚掉上次的选中状态
        clearSelection();
        // 开启一个Fragment事务
        FragmentTransaction transaction = fragmentManager.beginTransaction();
        // 先隐藏掉所有的Fragment，以防止有多个Fragment显示在界面上的情况
        hideFragments(transaction);
        switch (index) {

            case 0:
                // 当点击了主页tab时，改变控件的图片和文字颜色
                contactsImage.setImageResource(R.drawable.home1);
                contactsText.setTextColor(Color.parseColor("#1E90FF"));
                //contactsLayout.setBackgroundColor(Color.parseColor("#1e90ff"));
                //topText.setText("首页");
                if (contactsFragment == null) {
                    // 如果ContactsFragment为空，则创建一个并添加到界面上
                    contactsFragment = new MainActivity();
                    transaction.add(R.id.content,contactsFragment);
                } else {
                    // 如果ContactsFragment不为空，则直接将它显示出来
                    transaction.show(contactsFragment);
                }
                break;
            case 1:
                // 当点击了发布tab时，改变控件的图片和文字颜色
                newsImage.setImageResource(R.drawable.send1);
                newsText.setTextColor(Color.parseColor("#1E90FF"));
                if (newsFragment == null) {
                    // 如果NewsFragment为空，则创建一个并添加到界面上
                    newsFragment = new SendSign();
                    transaction.add(R.id.content, newsFragment);
                } else {
                    // 如果NewsFragment不为空，则直接将它显示出来
                    transaction.show(newsFragment);
                }
                break;
            case 2:
            default:
                // 当点击了设置tab时，改变控件的图片和文字颜色
                settingImage.setImageResource(R.drawable.set1);
                settingText.setTextColor(Color.parseColor("#1E90FF"));
                if (settingFragment == null) {
                    // 如果SettingFragment为空，则创建一个并添加到界面上
                    settingFragment = new Setting();
                    transaction.add(R.id.content, settingFragment);
                } else {
                    // 如果SettingFragment不为空，则直接将它显示出来
                    transaction.show(settingFragment);
                }
                break;
        }
        transaction.commit();
    }

    /**
     -    Function:    setTabSelection()
     -    Description:    清除掉所有的选中状态
     **/
    private void clearSelection() {
        contactsImage.setImageResource(R.drawable.home);
        contactsText.setTextColor(Color.parseColor("#82858b"));
        newsImage.setImageResource(R.drawable.signin);
        newsText.setTextColor(Color.parseColor("#82858b"));
        settingImage.setImageResource(R.drawable.setting);
        settingText.setTextColor(Color.parseColor("#82858b"));
        newsLayout.setBackgroundColor(Color.parseColor("#ffffff"));
        settingLayout.setBackgroundColor(Color.parseColor("#ffffff"));
        contactsLayout.setBackgroundColor(Color.parseColor("#ffffff"));
    }
    /**
     -    Function:    hideFragments(FragmentTransaction transaction)
     -    Description:    将所有的Fragment都置为隐藏状态。
     **/
    private void hideFragments(FragmentTransaction transaction) {

        if (contactsFragment != null) {
            transaction.hide(contactsFragment);
        }
        if (newsFragment != null) {
            transaction.hide(newsFragment);
        }
        if (settingFragment != null) {
            transaction.hide(settingFragment);
        }
    }
    /**
     -    Function:    onKeyDown(int keyCode, KeyEvent event)
     -    Description:    后退按钮监听
     **/
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if ((keyCode == KeyEvent.KEYCODE_BACK)) {
            if(!mBackKeyPressed){
                Toast.makeText(this, "再按一次退出程序", Toast.LENGTH_SHORT).show();
                mBackKeyPressed = true;
                new Timer().schedule(new TimerTask() {//延时两秒，如果超出则擦错第一次按键记录
                @Override
                public void run() {
                    mBackKeyPressed = false;
                }
                }, 2000);
                }
            else{//退出程序
                System.exit(0);
            }
            return false;
        }else {
            return super.onKeyDown(keyCode, event);
        }

    }

}
```
### 2.侧重放一个测试的MainActivity
```java
public class MainActivity extends Fragment {
    public void onAttach(Context context) {
        super.onAttach(context);
        Log.e(TAG,"MainActivity----onAttach----");
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.e(TAG,"MainActivity----onCreate----");
    }
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        Log.e(TAG,"MainActivity----onCreateView----");
        View contactsLayout = inflater.inflate(R.layout.main,
                container, false);
        return contactsLayout;
    }
    public void onActivityCreated(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.e(TAG,"MainActivity----onActivityCreated----");
        Button btn_second = (Button) getActivity().findViewById(R.id.second);
        btn_second.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                Intent intent = new Intent(getActivity(), Second.class);
                startActivity(intent);
            }
        });
    }

    @Override
    public void onStart() {
        super.onStart();
        Log.e(TAG,"MainActivity----onStart----");
    }

    @Override
    public void onResume() {
        super.onResume();
        Log.e(TAG,"MainActivity----onResume----");
    }

    @Override
    public void onPause() {
        super.onPause();
        Log.e(TAG,"MainActivity----onPause----");
    }

    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
    }

    @Override
    public void onStop() {
        super.onStop();
        Log.e(TAG,"MainActivity----onStop----");
    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        Log.e(TAG,"MainActivity----onDestroyView----");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.e(TAG,"MainActivity----onDestroy----");
    }

    @Override
    public void onDetach() {
        super.onDetach();
        Log.e(TAG,"MainActivity----onDetach----");
    }
```

### 运行一下代码如图： 
### 首先我们看一下打印的log：
### 和开始我们贴的那张Fragment生命周期图符合
### 现在我们再来仔细看看这张图
### 在Fragment生命周期中，如下方法会被系统回调
    *onAttach()：当该Fragment添加到Activity时，被回调一次
    *onCreate()：创建Fragment时被回调一次
    *onCreateView()：每次创建绘制Fragment的View时都会回调
    *onActivityCreated()：当Fragment所在的Activity启动时回调
    *onStart()：启动Fragment时回调
    *onResume()：恢复Fragment时回调 ps：在onStart()后一定会回调onResume()
    *onPause()：暂停Fragment时回调
    *onStop()：停止Fragment时回调
    *onDestroyView()：销毁Fragment的View组件时回调
    *onDestroy()：销毁Fragment时回调
    *onDetach()：将Fragment从Activity中删除，替换完成时回调一次
