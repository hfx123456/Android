# 实验四 **Intent**

**本实验通过自定义 WebViewWebView WebView 加载 URL 来验证隐式 Intent Intent 的使用。**
**实验包含两个应用：**
 **第一个应用：获取 URL 地址并启动隐式 Intent 的调用。**
**第二个应用：自定义 WebViewWebView WebView 来加载 URL**

## 新建一个工程获取URL并启动intent



activity_layout.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal">
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/input"
        android:hint="@string/web_url"
        android:textSize="30sp"

        android:autofillHints=""
        tools:ignore="TextFields" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/go"
        android:background="@color/colorAccent"
        android:layout_gravity="center"
        android:textSize="30sp"

        android:gravity="center" />


</LinearLayout>
```

mainactivity.java

```
package com.example.webview;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import org.w3c.dom.Text;

public class MainActivity extends AppCompatActivity {
    EditText editText;
    Button button;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button =(Button) findViewById(R.id.button);
        editText  =(EditText)findViewById(R.id.input);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                String uri="";
                intent.setAction(Intent.ACTION_VIEW);
                uri=button.getText().toString();
                intent.setData(Uri.parse(uri));
                startActivity(intent);
            }
        });

    }


}
```

![1589071653210](../image/aaa.png)

![1589071948177](../image/bbb.png)

## 个工程使用webview加载url

activity.mxl

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_horizontal">
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/urlinput"
        android:hint="@string/web_url"
        android:textSize="30sp"
        android:autofillHints="" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="GO"
        android:textSize="30sp"
        android:background="@color/colorAccent"
        android:layout_gravity="center"
        android:gravity="center" />
    <WebView
        android:layout_width="wrap_content"
        android:id="@+id/web"
        android:layout_height="wrap_content">
    </WebView>

</LinearLayout>
```

mainactivity.java

```
package com.example.myweb;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Button;
import android.widget.EditText;

import java.net.URL;

public class MainActivity extends AppCompatActivity {
    EditText editText;
    Button button;
    @Override
    protected  void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button=findViewById(R.id.button);
        editText  =(EditText)findViewById(R.id.urlinput);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                String uri="";
                uri=editText.getText().toString();
                intent.setData(Uri.parse(uri));
                intent.setAction(Intent.ACTION_VIEW);
                startActivity(intent);
            }
        });

        Intent intent=getIntent();
        Uri data = intent.getData();
        URL url=null;
        try{
            url=new URL(data.getScheme(),data.getHost(),data.getPath());
            startBroswer(url);
        }
        catch (Exception e){
            e.printStackTrace();
        }

    }
    private void startBroswer(URL url){
        WebView web=findViewById(R.id.web);
        web.loadUrl(url.toString());
        web.setWebViewClient(new WebViewClient(){
            @Override
            public boolean shouldOverrideUrlLoading(WebView v,String url){
                v.loadUrl(url);
                return true;
            }
        });
    }
}
```

![1589074800092](../image/fff.png)

![1589074813618](../image/ggg.png)

## 遇到的问题

就是网页不允许明文的HTTP，所以显示不出来，同学的方法和百度的方法都试了也没显示出来。。一添加后开起来就会闪退

还遇到的就是安卓版本更新，有些包更新了

import android.support.v7.app.AppCompatActivity;

报错 

经百度，发现更新为import androidx.appcompat.app.AppCompatActivity;

