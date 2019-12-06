# Android JAVA

## Activity hierarchy

File: `AndroidManifest.xml`

- Show `back` arrow to child activity

```xml
<activity
	android:name=".ReadActivity"
    android:parentActivityName=".MainActivity" <!--set parent-->
    />
```

- Hide application name bar

```xml
<activity
	android:name=".ReadActivity"
    android:theme="@style/Theme.AppCompat.NoActionBar"
    />
```

## Layout

### - `LinearLayout`

`android:layout_weight`: if it's used only on one element, lets it take *all* the free space. If used by multiple elements, they divide the space according to the weight given.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".SomeActivity">
    
    <Button
        android:id="@+id/button3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
    <TextView
        android:id="@+id/countNumber"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        />
</LinearLayout>
         
```

### - `RelativeLayout`

`android:layout_below`: show *this* element <u>under</u> given element.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SomeActivity">
    
    <Button
        android:id="@+id/button3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
    <TextView
        android:id="@+id/countNumber"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/button3"
        android:layout_alignParentLeft="true"
		android:layout_alignParentStart="true"
        android:layout_centerHorizontal="true"
        />
</RelativeLayout>
```

