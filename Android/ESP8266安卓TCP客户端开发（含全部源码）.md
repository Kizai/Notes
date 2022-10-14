## ESP8266安卓TCP客户端开发
最近在玩8266模块，想让它在局域网控制下开关，所以就搞开发一个安卓客户端调试的工具
直接上步骤：
### 第一步新建一个空白的Activity
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702130122671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
工程名字自己可以改
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702130158997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
### 第二先在AndroidManifest.xml加入网络权限

```javascript
<uses-permission android:name="android.permission.INTERNET"/>
```
### 第三修改界面布局（注意这个布局必须要支持约束布局才能正常使用不然会报错，可以使用AS3.6以上版本）：

```javascript
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/btn_send"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="@color/colorPrimary"
        android:text="@string/c"
        android:textColor="#F4F2F2"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline2"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline2"
        app:layout_constraintVertical_bias="0.52" />

    <EditText
        android:id="@+id/et_port"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:autofillHints=""
        android:ems="10"
        android:hint="@string/s"
        android:inputType="number"
        app:layout_constraintBottom_toTopOf="@+id/guideline3"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.81"
        app:layout_constraintStart_toStartOf="@+id/guideline4"
        app:layout_constraintTop_toTopOf="@+id/guideline3"
        app:layout_constraintVertical_bias="0.555" />

    <EditText
        android:id="@+id/et_ip"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:autofillHints=""
        android:ems="10"
        android:hint="@string/sip"
        android:inputType="date"
        app:layout_constraintBottom_toTopOf="@+id/guideline"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.81"
        app:layout_constraintStart_toStartOf="@+id/guideline4"
        app:layout_constraintTop_toTopOf="@+id/guideline"
        app:layout_constraintVertical_bias="0.555" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="@string/ssip"
        android:textAlignment="textEnd"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline"
        app:layout_constraintEnd_toStartOf="@+id/guideline4"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="@string/sd"
        android:textAlignment="textEnd"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline3"
        app:layout_constraintEnd_toStartOf="@+id/guideline4"
        app:layout_constraintHorizontal_bias="0.51"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline3"
        app:layout_constraintVertical_bias="0.526" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.3" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.6" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.9" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.2" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.75" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.38" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline7"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.5" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline8"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.45" />

    <Button
        android:id="@+id/btn_link"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="#16E6CE"
        android:text="@string/contenct"
        android:textColor="#F4EFEF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline8"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline8" />

    <EditText
        android:id="@+id/et_data"
        android:layout_width="255dp"
        android:layout_height="48dp"
        android:autofillHints=""
        android:ems="10"
        android:hint="@string/plesdata"
        android:inputType="date|text"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline5"
        app:layout_constraintVertical_bias="0.446"
        tools:ignore="LabelFor" />

    <TextView
        android:id="@+id/tv_sendata"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="@string/senddata"
        android:textAlignment="textEnd"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toStartOf="@+id/guideline4"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline5" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/tcp"
        app:layout_constraintBottom_toTopOf="@+id/guideline6"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline6" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702130708821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
### 第四新建SendAsyncTask.java文件

```java
package com.kizai.projects.esp8266;

import android.os.AsyncTask;

import java.io.IOException;
import java.io.PrintStream;
import java.net.Socket;

//这个要根据自己的文件来导包，需要改这里
import static com.kizai.projects.esp8266.MainActivity.ip;//记得导包
import static com.kizai.projects.esp8266.MainActivity.port;

public class SendAsyncTask extends AsyncTask<String, Void, Void> {

    //这里是连接ESP8266的IP和端口,连接手机可以看到IP地址
    private static final String IP = ip;//可以直接调用全局变量
    private static final int PORT = port;

    @Override
    protected Void doInBackground(String... params) {
        String str = params[0];
        try {
            Socket client = new Socket(IP, PORT);
            client.setSoTimeout(5000);
            //获取Socket的输出流，用来发送数据给服务端
            PrintStream out = new PrintStream(client.getOutputStream());
            out.print(str);
            out.flush();
            out.close();
            client.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }
}

```
### 第五回到主活动代码MainActivity.java：

```java
package com.kizai.projects.esp8266;

import androidx.appcompat.app.AppCompatActivity;

import android.annotation.SuppressLint;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import java.net.Socket;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    public static String ip, pport, sendData;//调用全局数据变量，方便调用
    public static int port;
    private EditText et_ip, et_port, et_data;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();//初始化控件
    }

    //初始化控件
    private void initView() {
        Button btn_send = findViewById(R.id.btn_send);
        Button btn_link = findViewById(R.id.btn_link);
        et_ip = findViewById(R.id.et_ip);
        et_port = findViewById(R.id.et_port);
        et_data = findViewById(R.id.et_data);

        btn_send.setOnClickListener(this);
        btn_link.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            //连接
            case R.id.btn_link:
                if (et_port.length() != 0 && et_ip.length() != 0) {
                    ip = et_ip.getText().toString().trim();//获取输入的ip地址
                    pport = et_port.getText().toString().trim();//由于EditText的输入类型是String类型，所以先把输入的内容获取到，再通过下面的代码进行Int类型转换，如果自己有更好的方法也可以自行修改
                    port = Integer.parseInt(pport);//把端口的String类型的数据转换成int类型
                    String str = "o\n";//这是连接8266时发送的测试数据，要在8266那边做对应的判断才会有反应，如果不是连接8266的可以把以下两行行代码注释掉
                    new SendAsyncTask().execute(str);
                    Toast.makeText(getApplicationContext(), "如果看到ESP8266的灯闪烁两次就代表连接成功！", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(getApplicationContext(), "请输入正确的ip和端口号！", Toast.LENGTH_SHORT).show();
                }
                break;
                //发送数据
            case R.id.btn_send:
                sendData = et_data.getText().toString().trim();//输入的数据
                if (!sendData.isEmpty()) {
                    new SendAsyncTask().execute(sendData);
                } else {
                    Toast.makeText(getApplicationContext(), "请输入数据！", Toast.LENGTH_SHORT).show();
                }
                break;
        }
    }
}
```
### 第六ESP8266 TCPServer服务端代码：

```c
#include <ESP8266WiFi.h>
 
//定义最多多少个client可以连接本server(一般不要超过4个)
#define MAX_SRV_CLIENTS 2
//以下三个定义为调试定义
#define DebugBegin(baud_rate)    Serial.begin(baud_rate)
#define DebugPrintln(message)    Serial.println(message)
#define DebugPrint(message)    Serial.print(message)
 
const char* ssid = "KIZAI-HOME";//自己路由器的wifi名
const char* password = "700800900";//自己路由器的密码
 
//创建server 端口号是23
WiFiServer server(23);
//管理clients
WiFiClient serverClients[MAX_SRV_CLIENTS];
 
void setup() {
  DebugBegin(115200);
  //定义输出引脚
  pinMode(LED_BUILTIN, OUTPUT);
  //定义高低电平
  digitalWrite(LED_BUILTIN, HIGH); 
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  DebugPrint("\nConnecting to "); 
  DebugPrintln(ssid);
  uint8_t i = 0;
  while (WiFi.status() != WL_CONNECTED && i++ < 20) {
    delay(500);
  }
  if (i == 21) {
    DebugPrint("Could not connect to"); 
    DebugPrintln(ssid);
    while (1) {
      delay(500);
    }
  }
  //启动server
  server.begin();
  //关闭小包合并包功能，不会延时发送数据
  server.setNoDelay(true);
 
  DebugPrint("Ready! Use 'telnet ");
  DebugPrint(WiFi.localIP());
  DebugPrintln(" 23' to connect");
}
 
void loop() {
  uint8_t i;
  //检测是否有新的client请求进来
  if (server.hasClient()) {
    for (i = 0; i < MAX_SRV_CLIENTS; i++) {
      //释放旧无效或者断开的client
      if (!serverClients[i] || !serverClients[i].connected()) {
        if (serverClients[i]) {
          serverClients[i].stop();
        }
        //分配最新的client
        serverClients[i] = server.available();
        DebugPrint("New client: "); 
        DebugPrint(i);
        break;
      }
    }
    //当达到最大连接数 无法释放无效的client，需要拒绝连接
    if (i == MAX_SRV_CLIENTS) {
      WiFiClient serverClient = server.available();
      serverClient.stop();
      DebugPrintln("Connection rejected ");
    }
  }
  //检测client发过来的数据
  for (i = 0; i < MAX_SRV_CLIENTS; i++) {
    if (serverClients[i] && serverClients[i].connected()) {
      if (serverClients[i].available()) {
        //get data from the telnet client and push it to the UART
        while (serverClients[i].available()) {
          //发送到串口调试器
//          Serial.write(serverClients[i].read());
          char data = serverClients[i].read();//接收到的数据可以做出不同的反应
          Serial.print(data);
          switch(data){
            case 'o':
            //闪烁两次
              digitalWrite(LED_BUILTIN, LOW); // 点亮LED
              delay(100);
              digitalWrite(LED_BUILTIN, HIGH); // 熄灭LED
              delay(100);
              digitalWrite(LED_BUILTIN, LOW); // 点亮LED
              delay(100);
              digitalWrite(LED_BUILTIN, HIGH); // 熄灭LED
              delay(100);
              digitalWrite(LED_BUILTIN, LOW); // 点亮LED
              break;
          }
        }
      }
    }
  }
  if (Serial.available()) {
    //把串口调试器发过来的数据 发送给client
    size_t len = Serial.available();
    uint8_t sbuf[len];
    Serial.readBytes(sbuf, len);
    //push UART data to all connected telnet clients
    for (i = 0; i < MAX_SRV_CLIENTS; i++) {
      if (serverClients[i] && serverClients[i].connected()) {
        serverClients[i].write(sbuf, len);//向所有客户端发送数据
        delay(1);
      }
    }
  }
}
//连接情况 用led灯的状态显示
void blink()
{
    static long previousMillis = 0;
    static int currstate = 0;

    if (millis() - previousMillis > 200)  //200ms
    {
        previousMillis = millis();
        currstate = 1 - currstate;
//        digitalWrite(LED_BUILTIN, LOW); // 点亮LED
        digitalWrite(LED_BUILTIN, currstate);
    }
}
```
8266端口监视器：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702133906604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
### 第七现在运行软件（真机和虚拟机都可以实现连接：前提是连接在统一个局域网里面）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702134104569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
也可以看到串口监视器会有反应：New client:0o
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702134133928.png)
测试发送数据：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702134235689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200702134312593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)
看到串口监视器都是可以接收到数据的
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020070213432565.png)
