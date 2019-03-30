# Android UI组件

## 一. 实验内容

1.利用SimpleAdapter实现如下界面效果

![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/1y.png)

2.创建自定义布局的AlertDialog

创建如图所示的自定义对话框

请创建一个如图所示的布局，调用 AlertDialog.Builder对象上的setView() 将布局添加到AlertDialog。

![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/2y.png)

3.使用XML定义菜单

（1）字体大小（有小，中，大这3个选项；分别对应10号字，16号字和20号字），点击之后设置测试文本的字体

（2）普通菜单项，点击之后弹出Toast提示

（3）字体颜色（有红色和黑色这2个选项），点击之后设置测试文本的字体

![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/3y.png)

4.创建上下文操作模式(ActionMode)的上下文菜单

（1）使用ListView或者ListActivity创建List

（2）为List Item创建ActionMode形式的上下文菜单

（3）参考文献：https://developer.android.google.cn/guide/topics/ui/menus.html

![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/4y.png)

5.自学内容

（1）学习Android RecyclerView

指南：https://developer.android.google.cn/guide/topics/ui/layout/recyclerview.html

Sample：https://github.com/googlesamples/androidRecyclerView/

（2）Card-Based Layout

指南：https://developer.android.google.cn/guide/topics/ui/layout/cardview.html#AddDependency

## 二. 关键代码

  1.利用SimpleAdapter实现界面效果
  
   （1）activity_list_view.xml
  
    <?xml version="1.0" encoding="utf-8"?>
    <android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".ListViewActivity">

        <ListView
            android:id="@+id/ListView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />
    </android.support.design.widget.CoordinatorLayout>
    
   （2）items.xml（作为listview的每个item）
    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:id="@+id/title"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="3"
            android:layout_gravity="center"
            android:textSize="25dp"/>

        <ImageView
            android:id="@+id/image"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:textColor="@color/colorAccent"
            android:adjustViewBounds="true"/>

    </LinearLayout>
    
  （3）ListViewActivity.java

    package com.example.uicomponents;

    import android.os.Bundle;
    import android.support.design.widget.FloatingActionButton;
    import android.support.design.widget.Snackbar;
    import android.support.v7.app.AppCompatActivity;
    import android.support.v7.widget.Toolbar;
    import android.view.View;

    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;

    import android.graphics.Color;
    import android.widget.ListView;
    import android.widget.SimpleAdapter;

    import android.widget.AdapterView;
    import android.widget.AdapterView.OnItemClickListener;
    import android.widget.LinearLayout;
    import android.widget.TextView;
    import android.widget.Toast;
    public class ListViewActivity extends AppCompatActivity {
        private ListView listView;
        private SimpleAdapter simpleAdapter;
        private List<Map<String, Object>> listItems;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_list_view);
            listView = (ListView) findViewById(R.id.ListView);
            //填充数据
            putData();
            simpleAdapter = new SimpleAdapter(this, listItems, R.layout.items, new String[]{"title", "image"}, new int[]{R.id.title, R.id.image});
            listView.setAdapter(simpleAdapter);
        }

        private void putData() {

            int[] imageId = new int[]{R.drawable.cat,
                    R.drawable.dog,
                    R.drawable.elephant,
                    R.drawable.lion,
                    R.drawable.monkey,
                    R.drawable.tiger};
            final String[] title = new String[]{"Cat", "Dog", "Elephant", "Lion", "Monkey", "Tiger"};

            listItems = new ArrayList<Map<String, Object>>();
            //通过for循环将图片id和列表项文字放到Map中，并添加到list集合中
            for (int i = 0; i < imageId.length; i++) {
                //实例化Map对象
                Map<String, Object> map = new HashMap<String, Object>();
                map.put("image", imageId[i]);
                map.put("title", title[i]);
                //将map对象添加到List集合
                listItems.add(map);
            }
            listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
                @Override
                public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                    view.setBackgroundColor(Color.TRANSPARENT);
                    Toast.makeText(ListViewActivity.this,title[position],Toast.LENGTH_SHORT).show();
                }
            });

        }

    }

    
  2.创建自定义布局的AlertDialog
  
  （1）activity_alert_dialog.xml

    <?xml version="1.0" encoding="utf-8"?>
    <android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".AlertDialogActivity">

        <android.support.design.widget.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:theme="@style/AppTheme.AppBarOverlay">

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="?attr/colorPrimary"
                app:popupTheme="@style/AppTheme.PopupOverlay" />

        </android.support.design.widget.AppBarLayout>

        <android.support.design.widget.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="@dimen/fab_margin"
            app:srcCompat="@android:drawable/ic_dialog_email" />

    </android.support.design.widget.CoordinatorLayout>
    
   （2）login.xml（作为弹出框）
    
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:adjustViewBounds="true"
            android:cropToPadding="true"
            app:srcCompat="@drawable/header_logo" />

        <EditText
            android:id="@+id/username"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Username"
            android:inputType="text" />

        <EditText
            android:id="@+id/password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="Password"
            android:inputType="textPassword" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <Button
                android:id="@+id/btnCancel"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:text="@string/text_btnCancel"
                android:textAllCaps="false"
                android:textSize="20sp" />

            <Button
                android:id="@+id/btnSignin"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:adjustViewBounds="true"
                android:text="@string/text_btnSignin"
                android:textAllCaps="false"
                android:textSize="20sp" />
        </LinearLayout>


    </LinearLayout>
  
  （3）AlertDialogActivity.java
  
    package com.example.uicomponents;

    import android.content.DialogInterface;
    import android.os.Bundle;
    import android.support.design.widget.FloatingActionButton;
    import android.support.design.widget.Snackbar;
    import android.support.v7.app.AlertDialog;
    import android.support.v7.app.AppCompatActivity;
    import android.support.v7.widget.Toolbar;
    import android.view.View;
    import android.widget.Button;
    import android.widget.EditText;
    import android.widget.Toast;

    public class AlertDialogActivity extends AppCompatActivity {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_alert_dialog);

            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            //获取布局
            View view = View.inflate(AlertDialogActivity.this, R.layout.login, null);

            builder.setView(view);

            //创建对话框
            builder.create().show();
        }

    }
    
  3.使用XML定义菜单
  
  （1）activity_menu.xml
  
    <?xml version="1.0" encoding="utf-8"?>
    <android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MenuActivity">


        <TextView
            android:id="@+id/tv_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:layout_marginTop="16dp"
            android:text="@string/test_text"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />
    </android.support.constraint.ConstraintLayout>
    
  （2）menu.xml（res/menu/menu.xml，作为菜单）
  
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto">
        <item
            android:id="@+id/file_menu"
            android:title="@string/xml_menu"
            app:showAsAction="always">
            <menu>
                <item
                    android:id="@+id/font_size_menu_item"
                    android:title="@string/font_size"
                    app:showAsAction="ifRoom|withText">
                    <menu>
                        <item
                            android:id="@+id/font_size_big_menu_item"
                            android:title="@string/font_size_big"
                            app:showAsAction="ifRoom|withText"></item>
                        <item
                            android:id="@+id/font_size_middle_menu_item"
                            android:title="@string/font_size_middle"
                            app:showAsAction="ifRoom|withText"></item>
                        <item
                            android:id="@+id/font_size_small_menu_item"
                            android:title="@string/font_size_small"
                            app:showAsAction="ifRoom|withText"></item>
                    </menu>
                </item>

                <item
                    android:id="@+id/common_menu_menu_item"
                    android:title="@string/common_menu"
                    app:showAsAction="ifRoom|withText"></item>
                <item
                    android:id="@+id/font_color_menu_item"
                    android:title="@string/font_color"
                    app:showAsAction="ifRoom|withText">


                    <menu>
                        <item
                            android:id="@+id/font_color_red_menu_item"
                            android:title="@string/font_color_red"
                            app:showAsAction="ifRoom|withText"></item>
                        <item
                            android:id="@+id/font_color_black_menu_item"
                            android:title="@string/font_color_black"
                            app:showAsAction="ifRoom|withText"></item>
                    </menu>


                </item>

                <item
                    android:id="@+id/exit_menu_item"
                    android:title="@string/exit_app"
                    app:showAsAction="ifRoom|withText"></item>

            </menu>
        </item>
    </menu>
    
  （3）MenuActivity.java
  
    package com.example.uicomponents;

    import android.graphics.Color;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;

    import android.app.Activity;
    import android.os.Bundle;
    import android.view.Menu;
    import android.view.MenuItem;
    import android.widget.TextView;
    import android.widget.Toast;

    public class MenuActivity extends AppCompatActivity {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_menu);
        }

        /**
         * 利用菜单资源文件创建选项菜单
         *
         * @param menu
         * @return
         */
        @Override
        public boolean onCreateOptionsMenu(Menu menu) {
            // 利用菜单填充器将菜单资源文件映射成菜单
            getMenuInflater().inflate(R.menu.menu, menu);
            return true;
        }

        /**
         * 菜单项单击事件处理方法
         *
         * @param item
         * @return
         */
        @Override
        public boolean onOptionsItemSelected(MenuItem item) {
            TextView txt = (TextView)findViewById(R.id.tv_text);;
            switch (item.getItemId()) {

                case R.id.font_size_big_menu_item:
                    txt.setTextSize(20);
                    break;
                case R.id.font_size_middle_menu_item:
                    txt.setTextSize(16);
                    break;
                case R.id.font_size_small_menu_item:
                    txt.setTextSize(10);
                    break;

                case R.id.common_menu_menu_item:
                    Toast.makeText(this, "你单击了【普通菜单项】", Toast.LENGTH_SHORT).show();
                    break;

                case R.id.font_color_red_menu_item:
                    txt.setTextColor(android.graphics.Color.RED);
                    break;
                case R.id.font_color_black_menu_item:
                    txt.setTextColor(android.graphics.Color.BLACK);
                    break;
                case R.id.exit_menu_item:
                    finish();
                    break;
            }
            return true;
        }
    }
  
  4.创建上下文操作模式(ActionMode)的上下文菜单

  ## 三. 实验结果及截图
  
  1.利用SimpleAdapter实现界面效果
  
  ![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/1r.png)
  
  2.创建自定义布局的AlertDialog
  
  ![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/2r.png)
  
  3.使用XML定义菜单
  
  ![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/3r.png)
  
  4.创建上下文操作模式(ActionMode)的上下文菜单
  
  ![Image text](https://github.com/1045896802/UIcomponents/blob/master/img/4r.png)


