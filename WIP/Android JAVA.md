# Android JAVA

> `R.java` file contains the link between XML and Java page.

## Activity

File: `AndroidManifest.xml`

- Show `back` arrow to child activity

```xml
<activity
	android:name=".ReadActivity"
    android:parentActivityName=".MainActivity"
    />
```

- Hide application name bar

```xml
<activity
	android:name=".ReadActivity"
    android:theme="@style/Theme.AppCompat.NoActionBar"
    />
```

- Change application name

*Note* that if `android:lable` is present as `<application />`'s prop, then this won't work. [ERROR]
*Note* also that the `android:label` of the `MainActivity` gets synced as the name of the app.

```xml
<activity
    android:name=".ReadActivity"
    android:lable="This header name"
    />
```

## Life Cycle

The methods are:

`onCreate` `onStart` `onPause` `onRestart` `onResume` `onStop` `onDestroy`

I can implement each of them by `ovveriding` them. (*code -> Override Methods*)

### Main life cycles

When I launch the app:

- `MainActivity:`
  - `onCreate -> onStart -> onResume`

When I open (with a `button`) a second activity:

- `MainActivity.onPause()`
- `SecondActivity`:
  - `onCreate -> onStart -> onResume`
- `MainActivity.onStop()`

When I click the `←` button on the `SecondActivity` to go back to `MainActivity`:

- `SecondActivity.onPause()`
- `MainActivity`:
  -  `(onDestroy -> onCreate)/ onRestart -> onStart -> onResume `
- `SecondActivity`:
  - `onStop -> onDestroy`

When I rotate the device, the current activity is recreated.

- `CurrentActivity:`
  - `onPause -> onStop -> onDestroy -> `
  - `-> onCreate -> onStart -> onResume `

When I go press multitasking little square:

- `CurrentActivity:`
  - `onPause -> onStop`

When I close the app:

- `AllActivities.onDestroy()`

When I re-open the app from background (not closed):

- `CurrentActivity`:
  - `onRestart -> onStart -> onResume`

### Saving state

`onSaveInstanceState` method is called *after* `onPause` and *before* `onDestroy`.

```java
@Override
public void onSaveInstanceState(Bundle outState) {
	super.onSaveInstanceState(outState);
   	outState.putString("item_key", "What I want to be remembered");
    outState.putBoolean("another_item_key", false);
}
```

*Note* that the system saves the state of some View automatically, such as `EditText`.

### Restoring state

This action can be implemented inside the `onCreate` method or in the `onRestoreInstanceState` callback method.
Best practice: `onCreate`.

```java
if (savedInstanceState != null) {
    anotherItem = savedInstanceState.getBoolean("another_item_key");
    remembered = savedInstanceState.getString("item_key");
}
```

## Intent

I use `intent` to communicate between activities, passing data and/or open a new activity.

- **Explicit intent**:  when I know the target.

```java
/* Creating and sending intent */
Intent intent = new Intent(this, ActivityToOpen.class);
String message = "ciao";
intent.putExtra("extra_key", message);
startActivity(intent); //open the target activity
```

```java
/* Receiving intent */
Intent intent = getIntent();
String message = intent.getStringExtra("extra_key");
// Do something with it
```

- **Implicit intent**: I don't have the name of the target, but I have my orders.

  - *Open a **URL** in a web browser*

  ```java
  String url = siteAddr.getText().toString();
  if (!url.startsWith("http://") && !url.startsWith("https://")) 
      url = "http://" + url;
  Uri webpage = Uri.parse(url);
  ```

  - *Open a location on a **map***

  ```java
  String loc = mapAddr.getText().toString();
  Uri addressUri = Uri.parse("geo:0,0?q=" + loc);
  ```

  - *Share **text***

  ```java
  String txt = textToshare.getText().toString();
  String mimeType = "text/plain";
  ShareCompat.IntentBuilder
  	.from(this)
      .setType(mimeType)
      .setChooserTitle("Share this text with")
      .setText(txt)
      .startChooser();
  // Skip actions below
  ```

  - *[...]*

Intent action types:

`Intent.ACTION_VIEW` 						`Intent.ACTION_EDIT` 					`Intent.ACTION_DIAL`

```java
/* Creating and sending intent */
Intent intent = new Intent(Intent.ACTION_VIEW, dataToTransmit);
if (intent.resolveActivity(getPackageManager()) != null) {
    /* At least one app is able to handle my intent */
    startActivity(intent);
} else {
    /* No app is able to handle my intent */
   Log.d("ImplicitIntents", "Can't handle this!");
} 
```

### Receive implicit intent

I'll have to define an `Intent` filter in `AndroidManifest.xml` to define the type of implicit `Intent` I want to handle.

```xml
<activity android:name=".ImplicitIntent">
	<intent-filter>
     	<!-- Action type VIEW -->
   		<action android:name="android.intent.action.VIEW" />
        <!-- Accept implicit intent [IMPORTANT] -->
        <category android:name="android.intent.category.DEFAULT" />
        <!-- Accept browsable category intent -->
        <category android:name="android.intent.category.BROWSABLE" />
        <!-- Take in only url with http scheme and google.it host -->
        <data android:scheme="http" android:host="google.it" />
    </intent-filter>
</activity>
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

## Designing stuff

### `ScrollView`

```xml
<ScrollView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">
    <TextView
        android:id="@+id/textView3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:lineSpacingExtra="5sp"
        android:padding="@dimen/padding_regular"
        android:text="@string/sample_text" />
</ScrollView>
```

In this example, TextView is scrollable.

### `autoLink`

Automatically make links click-able.

`android:autoLink="web"` goes inside `TextView` as a property.

### EditText

I can set multiple `inputType`s by appending `|newType`.
Find all input types [in the documentation](https://developer.android.com/reference/android/widget/TextView.html#attr_android:inputType).

```xml
<EditText
	android:id="@+id/editTextid"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:inputType="textCapSentences|textMultiLine"
    android:hint="Enter a website address"/>
```

---

## Default methods

- `setTitle("New Title");` changes the current application title.

- `finish();` ends the activity life, when  `←` is pressed this activity won't be shown.

- `TextView.setTextColor(Color.RED)` sets the text color of the *TextView* as red.

- `TextView.setBackgroundColor(Color.RED)` sets the background color of the TV.

- `EditText.setInputType(`

  ​	`InputType.TYPE_CLASS_TEXT|InputType.TYPE_TEXT_FLAG_MULTI_LINE);`

- 

