# Android JAVA

> `R.java` file contains the link between XML and Java page.

---

## Table of contents

- [Activities](#activities)

- [Life Cycle](#life-cycle)
  - [Main life cycle](#main-life-cycle)
  - [Saving State](#saving-state)
  - [Restoring State](#restoring-state)
  
- [Intents](#intents)
  - [Explicit Intent](#explicit-intent)
  - [Implicit Intent](#implicit-intent)
    - [Intent action types](#intent-action-types)
    - [Receive implicit intent](#receive-implicit-intent)
  - [Broadcast Receiver](#broadcast-receiver)
  - [Sending Broadcast](#sending-broadcast)
  
- [Layout](#layout)
  - [`LinearLayout`](#--linearlayout)
  - [`RelativeLayout`](#--relativelayout)
  - [`TabLayout`](#--tablayout)
  
- [Designing stuff](#designing-stuff)
  - [`ScrollView`](#scrollview)
  - [`RecyclerView`](#recyclerview)
  - [EditText](#edittext)
  - [`TextView`](#textview)
  
- [Options menu](#options-menu)

- [Alerts](#alerts)

- [Notifications](#notifications)

  - [Creating a notification](#creating-a-notification)

  - [Handling a notification](#handling-a-notification)

- [Async tasks](#async-tasks)

- [Fragments](#fragments)
  
  - [Date Picker](#date-picker-fragment)

## Activities

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

*Note* that the `android:label` of the `MainActivity` gets synced as the name of the app, when it is specified as an `<application />` prop, it syncs brutally to the `MainActivity`.

```xml
<activity
    android:name=".ReadActivity"
    android:lable="You can read here"
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

## Intents

I use `intent` to communicate between activities, passing data and/or open a new activity.

### **Explicit intent**: 

*when I know the target.*

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

### **Implicit intent**: 

*I don't have the name of the target, but I have my orders.*

- *Open a **URL** in a web browser*

```java
String url = siteAddr.getText().toString();
if (!url.startsWith("http://") && !url.startsWith("https://")) 
    url = "http://" + url;
Uri webpage = Uri.parse(url);
// Add the actions in the next section
```

- *Open a location on a **map***

```java
String loc = mapAddr.getText().toString();
Uri addressUri = Uri.parse("geo:0,0?q=" + loc);
// Add the actions in the next section
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
// Skip the actions in the next section
```

- *[...]*

#### Intent action types:

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

#### Receive implicit intent

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

### Broadcast Receiver

When a system event occurs,  a *system broadcast* message is sent by the system.

- Add a new `BroadcastReceiver`:

`file > new > other > Broadcast Receiver`, select all tics.

- A `BroadcastReceiver` is either *static* or *dynamic*:
  - **static**:  Add `<receiver>` element in the `AndroidManifest.xml` file.
  - **dynamic**: Register the receiver with app *context* or activity *context*.

Inside the *MainActivity*:

```java
private MyReceiver myReceiver = new MyReceiver();
```

At the end on `onCreate()`, create an intent filter:

```java
IntentFilter filter = new IntentFilter();
```

Add action listeners:

```java
filter.addAction(Intent.ACTION_POWER_DISCONNECTED);
filter.addAction(Intent.ACTION_POWER_CONNECTED);
```

Register the receiver using the activity context:

```java
this.registerReceiver(myReceiver, filter);
```

Unregister the receiver in `onDestroy`:

```java
this.unregisterReceiver(myReceiver);
```

Implement `onReceive` in `MyReceiver.java`:

```java
@Override
public void onReceive(Context context, Intent intent) {
    String intentAction = intent.getAction();
    if (intentAction != null) {
        String toastMessage = "unknown intent action";
        switch (intentAction) {
            case Intent.ACTION_POWER_CONNECTED:
                toastMessage = "Power connected!";
                break;
            case Intent.ACTION_POWER_DISCONNECTED:
                toastMessage = "Power disconnected!";
                break;
        }
        Toast.makeText(context, toastMessage, Toast.LENGTH_SHORT).show();
    }
}
```

### Sending Broadcast

Broadcast action name should be like:
`BuildConfig.APPLICATION_ID + ".ACTION_MY_BROADCAST`.

- Normal broadcasts: asynchronous, receivers are run in undefined order.

Create a broadcast intent and pass it to `sendBroadcast(Intent)`:

- Local broadcasts: sent to receivers in the same app.

Create a broadcast intent and pass it to `LocalBroadcastManager.sendBroadcast()`.

```java
Intent intent = new Intent(ACTION_CUSTOM_BROAD);
LocalBroadcastManager.getInstance(this).sendBroadcast(intent);
```

- Ordered broadcasts: delivered to one receiver at a time, propagating results or canceling the broadcast.

Create a broadcast intent and pass it to `sendOrderedBroadcast(Intent, String)`.

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

### - `TabLayout`

- Add implementation of the library in `build.gradle (Module.app)`:

```java
implementation 'com.google.android.material:material:1.0.0'
```

- Add `TabLayout` and `ViewPager` in the *design xml*:

```xml
<com.google.android.material.tabs.TabLayout
        android:id="@+id/tab_layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        android:minHeight="?attr/actionBarSize"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>

<androidx.viewpager.widget.ViewPager
        android:id="@+id/pager"
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:layout_below="@id/tab_layout"/>
```

- Create as many `fragment (blank)` as the number of tabs you want to implement. Allowing `Create layout XML?`.
- For every `XML` use `RelativeLayout` and inside it it's the content of the current tab.
- Add a new *JAVA class* `PagerAdapter` inside the current package.
- Extend it from (calling `superclass`) `FragmentStatePagerAdapter`.

```java
public class PagerAdapter extends FragmentStatePagerAdapter {}
```

- Implement the default `constructor` with an additional parameter: `int numOfTabs`:

 ```java
public class PagerAdapter extends FragmentStatePagerAdapter {
	int numOfTabs;
    
    public PagerAdapter(FragmentManager fm, int NumOfTabs) {
        super(fm);
        this.numOfTabs = NumOfTabs;
    }
}
 ```

- Implement, overriding, the method `GetItem()` with a switch case:

```java
@Override
public Fragment getItem(int position) {
        switch (position) {
            case 0: return new TabFragment1();
            case 1: return new TabFragment2();
            case 2: return new TabFragment3();
            default: return null;
        }
}
```

- Implement, overriding, the method `GetCount()` to return the number of tabs:

```java
@Override
public int getCount() {
        return numOfTabs;
}
```

- Inside the `onCreate()` method of the *activity* implement the `TabLayout`:

```java
// Create an instance of the tab layout from the view.
TabLayout tabLayout = findViewById(R.id.tab_layout);
// Set the text for each tab.
tabLayout.addTab(tabLayout.newTab().setText("TAB TITLE 1"));
tabLayout.addTab(tabLayout.newTab().setText("TAB TITLE 2"));
tabLayout.addTab(tabLayout.newTab().setText("TAB TITLE 3"));
// Set the tabs to fill the entire layout.
tabLayout.setTabGravity(TabLayout.GRAVITY_FILL);
// Use PagerAdapter to manage page views in fragments.
// Each page is represented by its own fragment.
final ViewPager viewPager = findViewById(R.id.pager);
final PagerAdapter adapter = new PagerAdapter
                (getSupportFragmentManager(), tabLayout.getTabCount());
viewPager.setAdapter(adapter);
// Setting a listener for clicks.
viewPager.addOnPageChangeListener
        	(new TabLayout.TabLayoutOnPageChangeListener(tabLayout));

tabLayout.addOnTabSelectedListener
    (new TabLayout.OnTabSelectedListener() {
     @Override
        public void onTabSelected(TabLayout.Tab tab) {
         viewPager.setCurrentItem(tab.getPosition());
        }

        @Override
        public void onTabUnselected(TabLayout.Tab tab) {}

       @Override
        public void onTabReselected(TabLayout.Tab tab) {}
    });
}
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

### `RecyclerView`

Useful when the list of things to scroll is bigger, such as *contacts list* etc.

- Create the `RecyclerView` in the chosen layout:

```java
<androidx.recyclerview.widget.RecyclerView
        android:id="@+id/myRecycler"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

- Create the layout for a *single* line of the *recycler*:

Right-click the `app > res > layout` folder and choose `New > Layout resource file`.
Choose a name and a layout style, then open the generated file, add all the view you want:

```java
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
   	xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <androidx.appcompat.widget.AppCompatImageView
        android:id="@+id/sprite"
        android:layout_width="100dp"
        android:layout_height="100dp"/>

    <TextView
        android:id="@+id/name"
        android:layout_width="wrap_content"7
        android:layout_height="100dp"/>
</RelativeLayout>
```

Right-click each one of the views: `refactor > extract > style`, choose a title and select `Launch 'Use Style Where Possible'`. Now the layout file will be edited to use saved styles.

- Create an adapter:

Right-click the package name, select `new > Java Class`, name it `SomethingAdapter`.

```java
public class SomethingAdapter extends
    RecyclerView.Adapter<SomethingAdapter.SomethingViewHolder> {
}
```

-Implement methods `onCreateViewHolder`, `onBindViewHolder` and `getItemCount`.
-Add inner class `SomethingViewHolder`:

```java
public class SomethingAdapter extends
    RecyclerView.Adapter<SomethingAdapter.WordViewHolder> {
    // Implement as many view holders as the number of views 
    // you want to controll in the recycler view
    class FirstViewHolder extends RecyclerView.ViewHolder {
        public final TextView nameItemView;
		final SomethingAdapter mAdapter;
        
        public FirstViewHolder(View view, SomethingAdapter adapter) {
   			super(view);
   			nameItemView = view.findViewById(R.id.name);
   			this.mAdapter = adapter;
		}
        
        void bind(int position){
            String name = pokemonList.get(position);
            nameItemView.setText("something")
        }
    }
    class SecondViewHolder extends RecyclerView.ViewHolder {...}
}
```

- Get the list of things to display ready in `SomethingAdapter`:

```java
private LinkedList<String> pokemonList;
private LayoutInflater mInflater; //used to get the view
```

- Complete the getter method `getItemCount`:

```java
@Override
public int getItemCount() {
   return pokemonList.size();
}
```

- Implement the `SomethingAdapter`'s constructor:

```java
public SomethingAdapter(Context context, LinkedList<String> pkmList){
	mInflater = LayoutInflater.from(context);
    this.pokemonList = pkmList;
}
```

- Complete the `onCreateViewHolder` method:

```java
@Override
public SomethingViewHolder onCreateViewHolder(ViewGroup parent, 
                                                     int viewType) {
   View mItemView = mInflater.inflate(R.layout.wordlist_item, 
                                                     parent, false);
   if (viewType == 0){
       // First view holder
       return new FirstViewHolder(mItemView, this);
   } else { // Second view holder
       return new SecondViewHolder(mItemView, this)
   }
}
```

- Complete the `onBindViewHolder` method:

```java
@Override
public void onBindViewHolder(WordViewHolder holder, int position) {
   if (position%2!=0){ // odd indexes are names
       ((FirstViewHolder) holder).bind(position);
   } else { //even indexes are images
       ((SecondViewHolder) holder).bind(position);
   }
}
```

- Create the `RecyclerView` in my *Activity*:

```java
private RecyclerView mRecyclerView;
private SomethingAdapter mAdapter;

// inside oncreate
// Get a handle to the RecyclerView.
mRecyclerView = findViewById(R.id.myRecycler);
// Create an adapter and supply the data to be displayed.
mAdapter = new SomethingAdapter(this, pokemonList);
// Connect the adapter with the RecyclerView.
mRecyclerView.setAdapter(mAdapter);
// Give the RecyclerView a default layout manager.
GridLayoutManager layoutManager = 
    new GridLayoutManager(this.getContext(),10);
layoutManager.setSpanSizeLookup(new GridLayoutManager.SpanSizeLookup() {
	@Override
    public int getSpanSize(int position) {
        // Firstview.size = 3/10, Secondview.size = 7/10
    	return position%2==0 ? 3 : 7;
    }
});
mRecyclerView.setLayoutManager(layoutManager);
```

If the `recyclerView` goes inside a fragment:

```java
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, 												Bundle savedInstanceState) {
	// Inflate the layout for this fragment
    View view = inflater.inflate(R.layout.tab_fragment3, container, false);
    // Get a handle to the RecyclerView.
    mRecyclerView = view.findViewById(R.id.pkm_list_recycler);
    // Create an adapter and supply the data to be displayed.
    mAdapter = new PokemonListAdapter(this.getContext(), pokemonList);
    // Connect the adapter with the RecyclerView.
    mRecyclerView.setAdapter(mAdapter);
    // Give the RecyclerView a default layout manager.
    GridLayoutManager layoutManager = 
        					new GridLayoutManager(this.getContext(),10);
    layoutManager.setSpanSizeLookup(
        new GridLayoutManager.SpanSizeLookup() {
            @Override
            public int getSpanSize(int position) {
                return position%2==0 ? 3 : 7;
            }
        });
    mRecyclerView.setLayoutManager(layoutManager);

    return view;
}
```

This is because I have to inflate the layout to the container, and I have to use the `view` to find other `view`s.

- Make elements *clickable*:

```java
// Make all ViewHolders implement View.OnClickListener
class FirstViewHolder extends ... implements View.OnClickListener {}
class SecondViewHolder extends ... implements View.OnClickListener {}
```

This will make you implement the `onClick` handler for each one:

```java
@Override
public void onClick(View v) {
    // Get the position of the item that was clicked.
    int mPosition = getLayoutPosition()+1;
    // Use that to access the affected item in mWordList.
    String element = pokemonList.get(mPosition);
    Toast.makeText(v.getContext(), "Clicked: " + element, 															Toast.LENGTH_SHORT).show();
}
```

And don't forget to <u>bind the method</u> to the view inside `ViewHolder`'s constructor:

```java
itemView.setOnClickListener(this);
```

- To scroll to chosen place:

```java
mRecyclerView.smoothScrollToPosition(5);
```

### EditText

- `setInputType`

  ​	`(InputType.TYPE_CLASS_TEXT|InputType.TYPE_TEXT_FLAG_MULTI_LINE);`

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

### `TextView`

- Make text selectable: `android:textIsSelectable="true"`
- Automatically make links click-able: `android:autoLink="web"`
- `setTextColor(Color.RED)` sets the text color of the *TextView* as red.
- `setBackgroundColor(Color.RED)` sets the background color of the TV.

## Options menu

Create a folder as `res/menu/` and a file `menu.xml` inside it.

To show *some* of the menu items as icon at the `appbar` add the line `app:showAsAction="ifRoom"` as a prop.

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/mail"
        android:icon="@android:drawable/sym_action_email"
        android:title="Send a mail" />
    <item
        android:id="@+id/upload"
        android:icon="@android:drawable/ic_dialog_alert"
        android:title="Attention!" />
</menu>
```

To show the `appbar` in the designated `Activity` insert the method:

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
	MenuInflater menuInflater = getMenuInflater();
    menuInflater.inflate(R.menu.menu, menu);
    return true;
}
```

And to specify what every command does:

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    Toast.makeText(this, "something", Toast.LENGTH_SHORT).show();
    // Check menu item's id to determine the clicked item
    switch (item.getItemId()) {
        case R.id.mail:
            // do something
            return true;
        case R.id.upload:
            // do something
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
```

## Alerts

The main idea is:
create a builder -> set a title -> set a message -> set buttons.
The second argument of the button setter is the `onClickHandler` of the button.

```java
AlertDialog.Builder myAlertBuilder = new AlertDialog.Builder(MainActivity.this);
myAlertBuilder.setTitle("Are you sure you want to open this?");
myAlertBuilder.setMessage("Click OK to continue, or Cancel to stop:");

myAlertBuilder.setPositiveButton("OK",
	new DialogInterface.OnClickListener() {
    	public void onClick(DialogInterface dialog, int which) {
        	// User clicked OK button. Do something.
            
        }
    });

myAlertBuilder.setNegativeButton("Cancel",
	new DialogInterface.OnClickListener() {
    	public void onClick(DialogInterface dialog, int which) {
        	// User cancelled the dialog. Do something.
        }
});

myAlertBuilder.show();
```

## Notifications

### Creating a notification

- Create *notification channel* id, *notification manager* and id inside your activity:

```java
private static final String MY_ID = "primary_notification_channel";
private NotificationManager notificationManager;
private static final int NOTIFICATION_ID = 0;
```

- Initiate `NotificationManager` [inside `onCreate`]:

```java
notificationManager = (NotificationManager) 													getSystemService(NOTIFICATION_SERVICE); 
```

- Create and set a `NotificationChannel` [inside `onCreate`]:

```java
NotificationChanner notificationChanner = new NotificationChanner(MY_ID, 
                   	"Notification type",
                    NotificationManager.IMPORTANCE_HIGH);
notificationChannel.enableLights(true);
notificationChannel.setLightColor(Color.RED);
notificationChannel.enableVibration(true);
notificationChannel.setDescription("Notification from MEEE");
mNotifyManager.createNotificationChannel(notificationChannel);
```

- Choose a notification Icon:

`file > new > Image Asset` and choose *Notification Icons*.

- Create method `getNotificationBuilder()` for the *Activity*:

```java
private NotificationCompat.Builder getNotificationBuilder() {
    NotificationCompat.Builder notificationBuilder = new 					NotificationCompat.Builder(this, MY_ID)
        .setContentTitle("Hello I'm a notification!")
        .setContentText("And this is some senseless text!!!!")
        .setSmallIcon(R.drawable.ic_android);
    return notificationBuilder;
}
```

- Send the notification:

```java
NotificationCompat.Builder notifyBuilder = getNotificationBuilder();
mNotifyManager.notify(NOTIFICATION_ID, notifyBuilder.build());
```

### Handling a notification

Intents used for notifications are wrapped in `PendingIntent`.
To make the notification do something modify  `getNotificationBuilder`:

```java
Intent intent = new Intent(this, DatePickerActivity.class);
PendingIntent pendIntent = PendingIntent.getActivity(this, NOTIFICATION_ID, intent, PendingIntent.FLAG_UPDATE_CURRENT);
```

and add the following setters to the *Notification builder*:

```java
	.setContentIntent(notificationPenddIntent)
    .setAutoCancel(true);
```

#### Notification default options

```java
	.setDefaults(NotificationCompat.DEFAULT_ALL)
```

#### Notification priority

<table>
    <tr>
        <td>MIN</td>
        <td>LOW</td>
        <td>DEFAULT</td>
        <td>HIGH</td>
        <td>MAX</td>
    </tr>
    <tr>
    	<td>-2</td>
        <td>-1</td>
        <td>0</td>
        <td>1</td>
        <td>2</td>
    </tr>
</table>

```java
	.setPriority(NotificationCompat.PRIORITY_HIGH)
```

### Update or cancel notification

- Update

```java
public void updateNotification() {
    // Add a image from assets to the notification
    AssetManager assetManager = this.getAssets();
    InputStream inputStream;
    Bitmap bitmap = null;
    try {
        inputStream = assetManager.open("pokeball.png");
        bitmap = BitmapFactory.decodeStream(inputStream);
    } catch (IOException e) {
        e.printStackTrace();
    }
    NotificationCompat.Builder builder = getNotificationBuilder();
    notifyBuilder.setStyle(new NotificationCompat.BigPictureStyle()
    	.bigPicture(bitmap)
        .setBigContentTitle("Updated!!!!"));
    notificationManager.notify(NOTIFICATION_ID, builder.build());
}
```

- Cancel

```java
notificationManager.cancel(NOTIFICATION_ID);
```

## Async tasks

### **Use `AsyncTaskLoader` instead.**

`AsyncTask` is an abstract class that performs background task, such as *querying database*, *connecting to Internet* etc. I'll have to implement the following abstract methods:

- `onPreExecute()`: UI thread, sets up the task (*progress bar*).
- `doInBackground()`: main method in which goes the task.
- `onProgressUpdate()`: UI thread, when I have to update the status (*progress bar*).
- `onPostExecute()`: UI thread, when I have to update the result.

`AsyncTask` takes in 3 parameters:  *params*, *progress*, *result*.

```java
public class SimpleAsyncTask extends AsyncTask <Void, Void, String> {}
```

Add the reference to the `TextView` in which I'll show my progress/result:

```java
private WeakReference<TextView> mTextView;
```

Create a constructor based on this `TextView`:

```java
SimpleAsyncTask(TextView tv) {
       mTextView = new WeakReference<>(tv);
}
```

Implement the `doInBackground` method:

```java
@Override
protected String doInBackground(Void... voids) {
	// Do something interesting
   	return someString;
}
```

Implement `onPostExecute` method:

```java
// No override
protected void onPostExecute(String result) {
    // Access weakReference by using get()
    mTextView.get().setText(result);
}
```

Remember to <u>save the state</u>.

To implement `onProgressUpdate`:

Modify the second parameter of the *superclass* to the type you want to be using:

```java
public class SimpleAsyncTask extends AsyncTask<Void, String, String>
```

Implement the method using `@Override`: use values[0] if you use only one parameter.

```java
@Override
protected void onProgressUpdate(String... values) {
    super.onProgressUpdate(values[0]);
    showTheProgressSomewhere();
}
```

Inside `doInBackgroud` use `publishProgress` to update the progress:

```java
publishProgress("Progress String");
```

## Fragments

They are *mini-activities*, that hover on the activity.

### Date Picker Fragment

- Create a new blank fragment:

```java
public class DatePickerFragment extends DialogFragment 
   						implements DatePickerDialog.OnDateSetListener {
	// Dialog constructor with default values
    @NonNull
	@Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
    	// Use the current date as the default date in the picker.
    	final Calendar c = Calendar.getInstance();
    	int year = c.get(Calendar.YEAR);
    	int month = c.get(Calendar.MONTH);
    	int day = c.get(Calendar.DAY_OF_MONTH);
        // Create a new instance of DatePickerDialog and return it.
        return new DatePickerDialog(getActivity(), this, year, month, day);
    }
  
    // When a date is chosen, do the following
	@Override
    public void onDateSet
        		(DatePicker view, int year, int month, int dayOfMonth) {
        
        // Create an instance of the father activity
    	DatePickerActivity datePicker = (DatePickerActivity) getActivity();
        
        // Call the father's method
        datePicker.processDatePickerResult(year, month, dayOfMonth);
        
      }
  }
```

- Add methods in the father activity to *show the fragment* and *do something with it*:

```java
public void showDatePicker(View view) {
        DialogFragment newFragment = new DatePickerFragment();
        newFragment.show(getSupportFragmentManager(),"datePicker");
}

public void processDatePickerResult(int year, int month, int day) {
        String month_string = Integer.toString(month+1);
        String day_string = Integer.toString(day);
        String year_string = Integer.toString(year);
        String dateMessage = 
            (day_string + "/" + month_string + "/" + year_string);
    	// Show the date in a TextView
        TextView tw = findViewById(R.id.textView4);
        tw.setText(dateMessage);
    }
```

### Time Picker Fragment

Use the class `TimePickerDialog` and the method `TimePickerDialog.OnTimeSetListener`.

---

## Default methods

- `setTitle("New Title");` changes the current application title.

- `finish();` ends the activity life, when  `←` is pressed this activity won't be shown.


## Default classes

- `WeakReference`: Allows the object to be garbage collected.