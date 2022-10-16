# 安卓android-BMI体质指数测试项目完整版

### 一、 界面布局：

（1）、 主界面：

![主界面](https://img-blog.csdnimg.cn/20200425151336260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)


**Activity_main.xml源代码：**

```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/pp"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="219dp"
        android:layout_height="45dp"
        android:background="@drawable/shape"
        android:fontFamily="sans-serif-black"
        android:text="@string/button"
        android:textAlignment="center"
        android:textColor="@color/colorPrimary"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/guideline6"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline6"
        app:layout_constraintVertical_bias="0.437" />

    <Button
        android:id="@+id/btn_kizai"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@drawable/shape"
        android:fontFamily="sans-serif-black"
        android:text="@string/button_aboutme"
        android:textColor="@color/colorPrimary"
        app:layout_constraintBottom_toTopOf="@+id/guideline9"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline9" />

    <Button
        android:id="@+id/btn_BMI"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@drawable/shape"
        android:fontFamily="sans-serif-black"
        android:text="@string/adoutBMI"
        android:textAlignment="center"
        android:textColor="@color/colorPrimary"
        app:layout_constraintBottom_toTopOf="@+id/guideline7"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline7" />

    <EditText
        android:id="@+id/editText"
        android:layout_width="125dp"
        android:layout_height="52dp"
        android:autofillHints=""
        android:digits=".0123456789"
        android:ems="10"
        android:fontFamily="sans-serif-black"
        android:hint="@string/height_hint"
        android:inputType="number"
        android:textAlignment="textStart"
        android:textColor="#FFFFFF"
        android:textColorHint="#ffffff"
        app:layout_constraintBottom_toTopOf="@+id/guideline4"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.557"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline4"
        app:layout_constraintVertical_bias="0.511" />

    <EditText
        android:id="@+id/editText2"
        android:layout_width="125dp"
        android:layout_height="52dp"
        android:autofillHints=""
        android:digits=".0123456789."
        android:ems="10"
        android:fontFamily="sans-serif-black"
        android:hint="@string/weight_hint"
        android:inputType="number"
        android:textAlignment="textStart"
        android:textColor="#FFFFFF"
        android:textColorHint="#ffffff"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.552"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline5"
        app:layout_constraintVertical_bias="0.557" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="29dp"
        android:layout_height="29dp"
        android:layout_marginTop="16dp"
        android:layout_marginEnd="16dp"
        android:contentDescription="@string/image_back"
        app:layout_constraintBottom_toTopOf="@+id/guideline2"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="@+id/guideline"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.0"
        app:srcCompat="@drawable/ic_close_black_24dp" />

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="75dp"
        android:layout_height="75dp"
        android:contentDescription="@string/Sex_show"
        app:layout_constraintBottom_toTopOf="@+id/guideline6"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="@+id/guideline"
        app:layout_constraintTop_toTopOf="@+id/guideline3"
        app:layout_constraintVertical_bias="0.499"
        app:srcCompat="@drawable/men" />

    <RadioGroup
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toTopOf="@+id/guideline6"
        app:layout_constraintEnd_toStartOf="@+id/guideline"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline3">

        <RadioButton
            android:id="@+id/radioButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:checked="true"
            android:fontFamily="sans-serif-black"
            android:text="@string/radiobutton"
            android:textColor="#ffffff"
            android:textSize="18sp" />

        <RadioButton
            android:id="@+id/radioButton2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:fontFamily="sans-serif-black"
            android:text="@string/radiobutton2"
            android:textColor="#ffffff"
            android:textSize="18sp" />
    </RadioGroup>

    <TextView
        android:id="@+id/TextView1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="@font/archivo_black"
        android:text="@string/TextView1"
        android:textColor="#FFFFFF"
        android:textSize="30sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/guideline2"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline2"
        app:layout_constraintVertical_bias="0.147" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:fontFamily="sans-serif-black"
        android:text="@string/textview3"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline4"
        app:layout_constraintEnd_toStartOf="@+id/editText"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline4"
        app:layout_constraintVertical_bias="0.958" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:fontFamily="sans-serif-black"
        android:text="@string/textview4"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toStartOf="@+id/editText2"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline5"
        app:layout_constraintVertical_bias="0.541" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.5" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.1" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.25" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.35" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.4" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.6" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline9"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.95" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline7"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.85" />

    <TextView
        android:id="@+id/textView9"
        android:layout_width="71dp"
        android:layout_height="25dp"
        android:fontFamily="sans-serif-black"
        android:text="@string/textview9"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline4"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toEndOf="@+id/editText"
        app:layout_constraintTop_toTopOf="@+id/guideline4"
        app:layout_constraintVertical_bias="0.4" />

    <TextView
        android:id="@+id/textView12"
        android:layout_width="52dp"
        android:layout_height="24dp"
        android:fontFamily="sans-serif-black"
        android:text="@string/textview12"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline5"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.026"
        app:layout_constraintStart_toEndOf="@+id/editText2"
        app:layout_constraintTop_toTopOf="@+id/guideline5" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
（2）、测试结果界面：

![测试结果界面](https://img-blog.csdnimg.cn/20200425151619128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

**Activity_bmiresult.xml源代码：**

```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".BmiresultActivity">

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline10"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.5" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline11"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.15" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline12"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.5" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline25"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.5" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline26"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.6" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline27"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.8" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline13"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.9" />

    <!--<ImageView
        android:id="@+id/image_show"
        android:layout_width="400dp"
        android:layout_height="400dp"
        android:contentDescription="@string/imageShow"
        app:layout_constraintBottom_toTopOf="@+id/guideline12"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline11"
        android:src="@drawable/ps"/>-->

    <pl.droidsonroids.gif.GifImageView
        android:id="@+id/gifimage_show"
        android:layout_width="246dp"
        android:layout_height="243dp"
        android:src="@drawable/fp"
        app:layout_constraintBottom_toTopOf="@+id/guideline12"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.496"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline11"
        app:layout_constraintVertical_bias="0.625" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/result"
        android:textAlignment="center"
        android:textColor="#0F9365"
        android:textSize="18sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline26"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline12"
        app:layout_constraintVertical_bias="0.478" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/textview2"
        android:textAlignment="center"
        android:textColor="@color/colorPrimary"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/guideline11"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button2"
        android:layout_width="190dp"
        android:layout_height="52dp"
        android:layout_marginBottom="16dp"
        android:background="@drawable/shape"
        android:fontFamily="sans-serif-black"
        android:text="@string/button2"
        android:textColor="@color/colorPrimary"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline13" />

    <TextView
        android:id="@+id/tv_sport"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:fontFamily="sans-serif-black"
        android:text="@string/tv_sport"
        android:textAlignment="center"
        android:textColor="#FF008C"
        android:textSize="12sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline27"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline26" />

    <Button
        android:id="@+id/btn_sport"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="12dp"
        android:background="@drawable/shape"
        android:fontFamily="sans-serif-black"
        android:text="@string/button_sport"
        android:textColor="#DA0379"
        app:layout_constraintBottom_toTopOf="@+id/guideline13"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline27"
        app:layout_constraintVertical_bias="0.0" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

（3）、按钮自定义形状

**Shape.xml源代码：**

```bash
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <stroke
        android:width="1dp"
        android:color="#0000ff"
        android:dashWidth="4dp"
        android:dashGap="4dp" />

    <corners android:radius="15dp"/><!--设置圆角，圆角半径为15dp-->
    <solid android:color="#FBEEFF"/><!--设置填充色-->
    <!--<stroke android:width="1dp" android:color="#0000ff" />-->
</shape>
```

 
(4) 、自定义Dialog样式

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020042515191165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70#pic_center)

**Aboutme.xml源代码：**

```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".BmiresultActivity">


    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline8"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.4" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline19"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.3" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline23"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.35" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline22"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.15" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline14"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.55" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline15"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.6" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline16"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.65" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline24"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.7" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline17"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.8" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline18"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.85" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline20"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.95" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline21"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="1.0" />

    <ImageView
        android:id="@+id/imageView3"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:contentDescription="@string/todo1"
        app:layout_constraintBottom_toTopOf="@+id/guideline14"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline8"
        app:srcCompat="@drawable/kizai" />

    <TextView
        android:id="@+id/tv_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/name"
        android:textAlignment="center"
        android:textSize="14sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline15"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline14"
        app:layout_constraintVertical_bias="0.545" />

    <TextView
        android:id="@+id/tv_phone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/phone"
        android:textAlignment="center"
        android:textSize="14sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline16"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline15" />

    <RatingBar
        android:id="@+id/ratingBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toTopOf="@+id/guideline20"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline18" />

    <TextView
        android:id="@+id/textView8"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/pj"
        android:textSize="14sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline18"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.491"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline17"
        app:layout_constraintVertical_bias="0.6" />

    <ImageView
        android:id="@+id/imageView4"
        android:layout_width="100dp"
        android:layout_height="100dp"
        app:layout_constraintBottom_toTopOf="@+id/guideline19"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.491"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline22"
        app:srcCompat="@drawable/bmi"
        android:contentDescription="@string/todo3" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="@font/krona_one"
        android:text="@string/appname"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@+id/guideline23"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline19" />

    <TextView
        android:id="@+id/textView6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="@font/krona_one"
        android:text="@string/version"
        app:layout_constraintBottom_toTopOf="@+id/guideline8"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline23" />

    <TextView
        android:id="@+id/textView7"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/mail"
        app:layout_constraintBottom_toTopOf="@+id/guideline24"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline16" />

    <Button
        android:id="@+id/btn_updata"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:background="#FFFFFF"
        android:fontFamily="sans-serif-black"
        android:text="@string/button_updata"
        android:textAlignment="center"
        app:layout_constraintBottom_toTopOf="@+id/guideline17"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline24" />

    <TextView
        android:id="@+id/textView10"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="@font/archivo_black"
        android:text="@string/cc3"
        android:textColor="#A8A5A5"
        android:textSize="8sp"
        app:layout_constraintBottom_toTopOf="@+id/guideline21"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.517"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline20"
        tools:text="@string/cc" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
（5）、自定义Toast

![toast](https://img-blog.csdnimg.cn/20200425152029781.png#pic_center)

**Diy_toast.xml源代码：**

```bash
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline8"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.5" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="385dp"
        android:layout_height="73dp"
        android:contentDescription="@string/todo1"
        app:layout_constraintBottom_toTopOf="@+id/guideline8"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="1.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideline8"
        app:layout_constraintVertical_bias="0.505"
        app:srcCompat="@drawable/kizaitoast1" />

    <TextView
        android:id="@+id/tv_toast"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-black"
        android:text="@string/diy_display"
        android:textColor="@color/colorPrimary"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />


</androidx.constraintlayout.widget.ConstraintLayout>
```
 
### 二、第三方框架：

#### （1）、Gifimages动态图片显示框架
1、首先在build.grade(Project)中添加：

```bash
buildscript {
    repositories {
        mavenCentral()
    }
}
allprojects {
    repositories {
        mavenCentral()
    }
}
```


2、然后在build.grade(Modules)中添加：

```bash
dependencies {
    implementation 'pl.droidsonroids.gif:android-gif-drawable:1.2.19'
}
```
3、使用直接用gifimageView.setImageResource(地址)；

```bash
gifImageView.setImageResource(R.drawable.fp);
```
（2）、Toast第三方样式
1、首先在build.grade(Project)中添加：

```bash
allprojects {
    repositories {

        maven { url 'https://jitpack.io' }
    }
}
```
2、然后在build.grade(Modules)中添加：

```bash
dependencies {
    implementation 'com.github.Zzzia:ToastEx:1.0.1'
}
```
3、使用方法：ToastFx.error(content,”内容”).show();
例如：

```bash
ToastEx.error(MainActivity.this,"体重不能为0").show();
```
### 第一界面
一、逻辑代码：

**MainActivity.java源代码：**

```bash
package com.example.bmitest;

import androidx.appcompat.app.AppCompatActivity;

import android.annotation.SuppressLint;
import android.app.AlertDialog;
import android.content.Context;
import android.content.DialogInterface;
import android.content.Intent;
import android.graphics.Color;
import android.os.Bundle;
import android.text.SpannableString;
import android.text.Spanned;
import android.text.style.ForegroundColorSpan;
import android.view.Gravity;
import android.view.KeyEvent;
import android.view.LayoutInflater;
import android.view.MotionEvent;
import android.view.View;
import android.view.inputmethod.EditorInfo;
import android.view.inputmethod.InputMethodManager;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.RadioButton;
import android.widget.RatingBar;
import android.widget.TextView;
import android.widget.Toast;

import com.zia.toastex.ToastEx;
import com.zia.toastex.anim.ToastImage;
import com.zia.toastex.anim.ToastText;

public class MainActivity extends AppCompatActivity {
    private EditText et_height, et_weight;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //绑定控件
        et_height = findViewById(R.id.editText);
        et_weight = findViewById(R.id.editText2);
        Button btn_submit = findViewById(R.id.button);
        ImageView back = findViewById(R.id.imageView);
        final ImageView sex_show = findViewById(R.id.imageView2);
        RadioButton rtn_m = findViewById(R.id.radioButton);
        final RadioButton rtn_w = findViewById(R.id.radioButton2);

        //内部匿名类
        //开始计算
        btn_submit.setOnClickListener(new View.OnClickListener() {
            @SuppressLint("SetTextI18n")
            public void onClick(View v) {
                //让它输入只能是数字键盘，但是测试无效
/*                et_height.setInputType(EditorInfo.TYPE_CLASS_NUMBER);
                et_weight.setInputType(EditorInfo.TYPE_CLASS_NUMBER);*/
                //获取身高、体重文本
                String height = et_height.getText().toString().trim();
                String weight = et_weight.getText().toString().trim();
                //初始化身高、体重、结果
                double res = 0, heightNum = 0, weightNum = 0;

                //定义性别为男
                String bmisex = getString(R.string.callman);

                //判断性别女按钮点击事件
                //如果点击赋值为美铝
                if (rtn_w.isChecked()) {
                    bmisex = getString(R.string.callwoman);
                    sex_show.setImageResource(R.drawable.lady);
                } else {
                    bmisex = getString(R.string.callman);
                    sex_show.setImageResource(R.drawable.men);
                }

                //判断输入身高、体重不为空
                if (!height.isEmpty() && !weight.isEmpty()) {
                    //转换为小数
                    heightNum = Double.parseDouble(height);
                    weightNum = Double.parseDouble(weight);
                    //判断身高输入是否为0
                    if (heightNum == 0) {
                        //定义一个带图片的Toast,一定要改成getApplicationContext()在
                        @SuppressLint("ShowToast") Toast toastH = Toast.makeText(getApplicationContext(), R.string.toast_zero, Toast.LENGTH_SHORT);//身高为除数不能为0
                        toastH.setGravity(Gravity.CENTER, 0, 0);
                        LinearLayout linearLayout = (LinearLayout) toastH.getView();
                        ImageView imageView = new ImageView(getApplicationContext());
                        imageView.setImageResource(R.drawable.ic_close_black_24dp);
                        linearLayout.addView(imageView, 0);
                        toastH.show();
                    } else {
                        //判断体重输入是否为0
                        if (weightNum == 0) {
                            //默认
//                            Toast.makeText(getApplicationContext(), R.string.toast_zero1, Toast.LENGTH_SHORT).show();//体重不能为
                            ToastEx.error(MainActivity.this,"体重不能为0").show();//使用Toast插件实现
                        } else {
                            res = (weightNum * 10000) / (heightNum * heightNum);//BMI计算公式：体重/身高^2，因为原单位是M，单位改成cm之后放大10000倍
                            res = (double) Math.round(res * 10) / 10;//保留一位小数
                            Intent intent = new Intent(MainActivity.this, BmiresultActivity.class);
                            intent.putExtra("weiNum", weightNum);//传输身高
                            intent.putExtra("heiNum", heightNum);//传输体重
                            intent.putExtra("bmiValue", res);//传输BMI结果
                            intent.putExtra("bmisex", bmisex);//传输性别
                            startActivity(intent);
                        }
                    }
                } else
                    mToast();//用自己自定义的Toast警告
//                    Toast.makeText(MainActivity.this, R.string.toast_warming, Toast.LENGTH_SHORT).show();//警告提示
            }
        });
        //关于BMI指数
        Button btn_BMI = findViewById(R.id.btn_BMI);
        btn_BMI.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setMessageDiaolog();
            }
        });
        //关于作者
        Button btn_kizai = findViewById(R.id.btn_kizai);
        btn_kizai.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setView();
            }
        });
        //退出程序的按钮
        back.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                finish();
            }
        });
    }

    //创建dialog提示框
    private void setMessageDiaolog() {
        //创建构建器
        android.app.AlertDialog.Builder builder = new AlertDialog.Builder(this);
        //设置参数
        builder.setTitle("什么是BMI").setIcon(R.drawable.bmi)
                .setMessage(R.string.adout_bmi)
                .setPositiveButton("关闭", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        Toast toast = Toast.makeText(getApplicationContext(), "感谢查看", Toast.LENGTH_SHORT);
                        toast.setGravity(Gravity.BOTTOM, 0, 0);
                        LinearLayout linearLayout = (LinearLayout) toast.getView();
                        ImageView imageView = new ImageView(getApplicationContext());
                        imageView.setImageResource(R.drawable.ic_sentiment_very_satisfied_black_24dp);
                        linearLayout.addView(imageView, 0);
                        toast.show();
                    }
                });
        builder.create().show();
    }

    //创建setView()设置对话框为自定义View
    private void setView() {
        //创建对话框构建器
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        //获取布局
        View view = View.inflate(MainActivity.this, R.layout.aboutme, null);
        //获取布局中的控件一定要加上view（view.findViewById），不然会闪退
        //更新按钮
        final Button btn_updata = view.findViewById(R.id.btn_updata);
        btn_updata.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                 ToastEx.warning(MainActivity.this,"已经是最新版本").show();
//                Toast.makeText(getApplicationContext(), R.string.updata, Toast.LENGTH_SHORT).show();
            }
        });
        //打分控件
        final RatingBar ratingBar = view.findViewById(R.id.ratingBar);
        ratingBar.setOnRatingBarChangeListener(new RatingBar.OnRatingBarChangeListener() {
            @Override
            public void onRatingChanged(RatingBar ratingBar, float v, boolean b) {
                //改变Toast位置
                Toast toast = Toast.makeText(getApplicationContext(), String.valueOf(v) + "星评价", Toast.LENGTH_SHORT);
                toast.setGravity(Gravity.TOP, 0, 200);
                toast.show();

            }
        });
        //设置参数
        builder.setTitle(R.string.adoutBMI).setView(view)
                .setPositiveButton("关闭", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        //定义一个带图片的Toast
                        Toast toast = Toast.makeText(MainActivity.this, "感谢查看与评价", Toast.LENGTH_SHORT);
                        toast.setGravity(Gravity.BOTTOM, 0, 0);
                        LinearLayout linearLayout = (LinearLayout) toast.getView();
                        ImageView imageView = new ImageView(getApplicationContext());
                        imageView.setImageResource(R.drawable.ic_mood_black_24dp);
                        linearLayout.addView(imageView, 0);
                        toast.show();
                    }
                });
        //创建对话框
        builder.create().show();//不能少这句
    }

    //定义一个完全自定义的mToast
    private void mToast() {
        //创建构建器
        LayoutInflater mlif = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE);
        //绑定自定义的布局界面
        assert mlif != null;
        @SuppressLint("InflateParams") View view = mlif.inflate(R.layout.diy_toast, null);
        //绑定控件
        TextView tv_toast = view.findViewById(R.id.tv_toast);
        SpannableString ss = new SpannableString("非法输入！请重新输入！");
        //改变字体颜色
        ss.setSpan(new ForegroundColorSpan(Color.BLUE), 0, 4, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        ss.setSpan(new ForegroundColorSpan(Color.RED), 5, 11, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        //显示文字
        tv_toast.setText(ss);
        Toast myToast = new Toast(this);
        myToast.setView(view);
        myToast.setGravity(Gravity.CENTER, 0, 0);
        myToast.show();//不能少这个
    }

}
```
### 第二界面源代码

**BmiresultActivity.java源代码：**

```bash
package com.example.bmitest;

import androidx.appcompat.app.AppCompatActivity;
import pl.droidsonroids.gif.GifImageView;

import android.annotation.SuppressLint;
import android.app.AlertDialog;
import android.content.DialogInterface;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import com.zia.toastex.ToastEx;

public class BmiresultActivity extends AppCompatActivity {

    @SuppressLint("SetTextI18n")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_bmiresult);
        Button button = findViewById(R.id.button2);
        TextView textView = findViewById(R.id.textView);
        GifImageView gifImageView = findViewById(R.id.gifimage_show);
        Intent intent = getIntent();
        TextView textView1 = findViewById(R.id.textView2);
        //运动建议
        Button btn_sport = findViewById(R.id.btn_sport);
        btn_sport.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sportDialog();
            }
        });
        //不服，再测一下
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent i = new Intent(BmiresultActivity.this, MainActivity.class);
                startActivity(i);
            }
        });
        //接收身高体重
        double hei = intent.getDoubleExtra("heiNum", 0.0);//身高
        double wei = intent.getDoubleExtra("weiNum", 0.0);//体重
        //传递BMI的数值进行判断
        double Bmi = intent.getDoubleExtra("bmiValue", 0.0);//接收bmi值
        String sex = intent.getStringExtra("bmisex");//接收性别
        String advice = "";
        //进行比较得出结论
        if (Bmi <= 18.4) {
            advice = getString(R.string.thin);
            gifImageView.setImageResource(R.drawable.ps);
        } else if (Bmi >= 18.5 && Bmi <= 23.9) {
            advice = getString(R.string.slim);
            gifImageView.setImageResource(R.drawable.zc);
        } else if (Bmi >= 24 && Bmi <= 27.9) {
            advice = getString(R.string.fat);
            gifImageView.setImageResource(R.drawable.sr);
        } else if (Bmi >= 28 && Bmi <= 39) {
            advice = getString(R.string.fater);
            gifImageView.setImageResource(R.drawable.fp);
        } else if (Bmi >= 40) {
            advice = getString(R.string.fatest);
            gifImageView.setImageResource(R.drawable.gz);
        }
        textView.setText(sex + ",您的BMI值是：" + Bmi + "," + advice);
        textView1.setText("您的身高：" + hei + "cm" + "\n您的体重：" + wei + "kg" + "\n测试结果如下");

        setTitle("测试结果");
    }

    //运动建议的dialog
    private void sportDialog() {
        final TextView tv_sport = (TextView) findViewById(R.id.tv_sport);
        final String[] sports = new String[]{"篮球", "跑步", "爬山", "游泳", "不想运动"};
        //创建构建器
        final AlertDialog.Builder builder = new AlertDialog.Builder(BmiresultActivity.this);
        //设置参数
        builder.setIcon(R.drawable.ic_motorcycle_black_24dp).setTitle("请选择你喜欢的运动")
                .setSingleChoiceItems(sports, 0, new DialogInterface.OnClickListener() {
                    @SuppressLint("SetTextI18n")
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        //选择什么显示什么
                        ToastEx.success(BmiresultActivity.this, sports[which]).show();
                        switch (sports[which]) {
                            case "篮球":
                                tv_sport.setText("你选择的是:" + sports[which] + "\n打篮球的好处："+"\n打篮球可以增强体内营养物质的消耗，使整个肌体的代谢能力增强，进而提高食欲。另外，还会促进胃肠蠕动和消化液分泌，改善肝脏和胰腺的功能，" +
                                        "从而使整个消化系统的功能得到提高，为人的健康和长寿提供良好的物质d保证。");
                                break;
                            case "跑步":
                                tv_sport.setText("你选择的是:" + sports[which] + "\n跑步的好处：" + "\n经常跑步可以增强人体的柔韧度，保护心脏，通过跑步增强了人体各部位的抗损伤能力，同时提高供血量，对心脏大脑的健康都有好处，" +
                                        "可以缓解疲劳、消除紧张感、消除焦虑，抑郁的情绪，通过锻炼，人的精神可以得到振奋，心情会更加的开朗，对学习，工作都有一定好处。");
                                break;
                            case "爬山":
                                tv_sport.setText("你选择的是:" + sports[which] + "\n爬山的好处：" + "\n减肥最基本的道理是消耗的热量大于吸收的热量，运动是人体最主要的能量消耗渠道，但并不是说参加什么运动都可以取得良好减肥作用。理想的减肥运动方式是强度较低的运动，" +
                                        "由于供氧充分，持续时间长，总的能量消耗多。爬山正是这样一种运动强度适宜，持续时间较长的运动方式，因而具有独到的减肥效果。");
                                break;
                            case "游泳":
                                tv_sport.setText("你选择的是:" + sports[which] + "\n游泳的好处：" + "\n1、健身的作用。游泳是一项比较消耗体力的活动，它能够提高身体的体能，长期游泳可以使自己保持很好的体力。2、有减肥的作用。游泳，它的体力消耗量比较大，" +
                                        "可以消耗我们体内的脂肪，能够通过游泳来进行塑身、减肥。");
                                break;
                            case "不想运动":
                                tv_sport.setText("你选择的是:" + sports[which] + "\n不想运动的后果：" + "\n世界卫生组织最新统计数字表明，每年全球有两百万人死于“身体缺乏活动”，所以，为了自己，也为爱你的人和你爱的人，赶快运动起来，哪怕是最简单的步行!不要为了便捷而放弃健康，放弃生命｡");
                                break;
                        }
                    }
                }).setPositiveButton("确定", new DialogInterface.OnClickListener() {
            @SuppressLint("SetTextI18n")
            @Override
            public void onClick(DialogInterface dialog, int which) {
                /*switch (sports[which]) {
                    case "篮球":
                        tv_sport.setText("你选择的是:" + sports[which] + "\n打篮球的好处：\n"+R.string.bask);
                        break;
                    case "跑步":
                        tv_sport.setText("你选择的是:" + sports[which] + "\n跑步的好处：\n" + R.string.run);
                        break;
                    case "爬山":
                        tv_sport.setText("你选择的是:" + sports[which] + "\n爬山的好处：\n" + R.string.shan);
                        break;
                    case "游泳":
                        tv_sport.setText("你选择的是:" + sports[which] + "\n游泳的好处：\n" + R.string.swing);
                        break;
                    case "不想运动":
                        tv_sport.setText("你选择的是:" + sports[which] + "\n不想运动的后果：\n" + R.string.nosport);
                        break;
                }*/
            }
        }).setNegativeButton("取消", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                builder.create().dismiss();//关闭
            }
        });
        builder.create().show();
    }
}
```
### 演示效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425152814235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425152814211.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425152814178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425152813867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200425152813870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDAxMTYz,size_16,color_FFFFFF,t_70)

> 本项目已经开源：
有能力的可以自己升级一下。
```bash
https://github.com/Kizai/BMItest.git
```


