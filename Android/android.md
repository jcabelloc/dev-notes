# Android Notes


### Animations

```java
    ImageView guineaPig = findViewById(R.id.imageView);
    // fading in
    guineaPig.animate().alpha(1f).setDuration(2000);
    // fading out
    guineaPig.animate().alpha(0f).setDuration(2000);
    // Translation to the left 
    guineaPig.setTranslationX(-1000f);
    // Horizontal relative Translation
    guineaPig.animate().translationXBy(1000f).setDuration(2000);
    // Rotation
    guineaPig.animate().rotation(360f).setDuration(2000);
    // Scaling
    guineaPig.animate().scaleX(0.5f).scaleY(0.5f).setDuration(2000);
```

### Refering Resources using Ids

```java
    // Method created for Click Event
    public void buttonTapped(View view) {
        int viewId = view.getId();
        String ourId = "";
        
        // id defined by user, in this case it refers to name of audio files
        ourId = view.getResources().getResourceEntryName(viewId);

        // Get a resource id, in this case for an audio file
        int resourceId = getResources().getIdentifier(ourId, "raw", "com.tamu.jcabelloc.myapp");

        // Reproduce that audio file
        MediaPlayer mediaPlayer = MediaPlayer.create(this, resourceId);
        mediaPlayer.start();

    }

```

### List Views
```java
    //...

    ListView myListView = (ListView)findViewById(R.id.myListView);

    final List<String> myFamily = new ArrayList<String>();
    myFamily.add("Ana");
    myFamily.add("Julio");
    myFamily.add("Tatiana");
    myFamily.add("Orlando");
    myFamily.add("Juan");

    // Create de the array adapter
    ArrayAdapter<String> arrayAdapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, myFamily);
    
    // Set that adapter for the List View
    myListView.setAdapter(arrayAdapter);

    // Set Click Listener on the List View
    myListView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
        @Override
        public void onItemClick(AdapterView<?> adapterView, View view, int position, long id) {
            Toast.makeText(MainActivity.this, myFamily.get(position), Toast.LENGTH_LONG).show();
        }
    });
    //...
```

### Seek Bars
```java
    //...
    final SeekBar timesTableSeekBar = (SeekBar) findViewById(R.id.timesTableSeekBar);
    timesTableSeekBar.setMax(20);
    // Set initial position
    timesTableSeekBar.setProgress(10);

    // Set Listener based on Bar Change
    timesTableSeekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
        @Override
        public void onProgressChanged(SeekBar seekBar, int progress, boolean b) {
            int min = 1;
            int timesTable;
            // In case we want the seek bar works starting at 1
            if (progress < 1) {
                timesTable = min;
                timesTableSeekBar.setProgress(min);
            } else {
                timesTable = progress;
            }
            doSomething(timesTable);
        }

        @Override
        public void onStartTrackingTouch(SeekBar seekBar) {

        }

        @Override
        public void onStopTrackingTouch(SeekBar seekBar) {

        }
    });
    //...
```

### Handler as a Timer

```java
    //...
    final Handler handler = new Handler();
    Runnable run = new Runnable() {
        @Override
        public void run() {
            // Insert code to be run every second
            Log.i("Runnable has run!", "A second must have happened");
            handler.postDelayed(this, 1000);
        }
    };
    handler.post(run);
    //...
```

### Count Down Timer

```java
    //...
    new CountDownTimer(10000, 1000) {
        public void onTick(long milliSecondsUntilDown) {
            // Countdown is counting down (every second)
            Log.i("Second Left: ", String.valueOf(milliSecondsUntilDown/1000));
        }
        public void onFinish () {
            // Coounter is finished! (After 10 seconds)
            Log.i("Done!: ", "Countdown timer Finished");
        }
    }.start();
    //...
```

### Set Visibility
```java
    public void start(View view) {
        startButton.setVisibility(View.INVISIBLE);
        gameRelativeLayout.setVisibility(RelativeLayout.VISIBLE);
        //...
    }
```

### Get Tag (i.e a view is click)
```java
    //...
    public void evaluateAnswer(View view){
        if (view.getTag().toString().equals(String.valueOf(locationCorrectAnswer))) {
            score++;
            resultTextView.setText("Correct!");

        } else {
            resultTextView.setText("Wrong!");
        }
        //...
    }
```

### Downloading Web Content
```java
    //...
    public class DownloadTask extends AsyncTask<String, Void, String>{
        @Override
        protected String doInBackground(String... urls) {
            String result = "";
            URL url;
            HttpURLConnection urlConnection = null;
            try {
                url = new URL(urls[0]);
                urlConnection = (HttpURLConnection) url.openConnection();
                InputStream in = urlConnection.getInputStream();
                InputStreamReader reader = new InputStreamReader(in);
                int data = reader.read();
                while ( data!= -1) {
                    char current = (char)data;
                    result += current;
                    data = reader.read();
                }
                return result;
            }
            catch (Exception e) {
                e.printStackTrace();
                return "Failed";
            }
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DownloadTask task = new DownloadTask();
        String result = null;

        try {
            result = task.execute("https://www.google.com").get();
            Log.i("Attention:", "immediately");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
        Log.i("Contents of URL: ", result);
    }
    //...
```
Grant permission in the AndroidManifest.xml
```xml
    <uses-permission android:name="android.permission.INTERNET"/>
```

### Downloading Images
```java
    //...
    ImageView downloadedImg;
    // ClickEvent on the Button triggers this Method to download Image
    public void downloadImage(View view) {
        ImageDownloader task = new ImageDownloader();
        Bitmap myImage;
        try {
            myImage = task.execute("https://upload.wikimedia.org/wikipedia/en/a/aa/Bart_Simpson_200px.png").get();
            downloadedImg.setImageBitmap(myImage);
        } catch (Exception e) {
            e.printStackTrace();
        }
        Log.i("Interaction: ", "Botton Tapped");
    }

    public class ImageDownloader extends AsyncTask<String, Void, Bitmap> {
        @Override
        protected Bitmap doInBackground(String... urls) {
            try {
                URL url = new URL(urls[0]);
                HttpURLConnection connection = (HttpURLConnection) url.openConnection();
                connection.connect();
                InputStream inputStream = connection.getInputStream();
                Bitmap myBitmap = BitmapFactory.decodeStream(inputStream);
                return myBitmap;
            } catch (MalformedURLException e | IOException e) {
                e.printStackTrace();
            }
            return null;
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        downloadedImg = (ImageView) findViewById(R.id.imageView);
    }
    //...
```


### Processing JSON Data
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        DownloadTask task = new DownloadTask();
        task.execute("http://api.openweathermap.org/data/2.5/weather?q=tokyo,jp&appid=THE_API_ID"+ );
    }
    public class DownloadTask extends AsyncTask<String, Void, String> {
        @Override
        protected String doInBackground(String... urls) {
            String result = "";
            URL url;
            HttpURLConnection urlConnection = null;
            try {
                url = new URL(urls[0]);
                urlConnection = (HttpURLConnection) url.openConnection();
                InputStream inputStream = urlConnection.getInputStream();
                InputStreamReader reader = new InputStreamReader(inputStream);
                int data = reader.read();
                while (data!=-1) {
                    char current = (char)data;
                    result += current;
                    data = reader.read();
                }
                return result;

            } catch (MalformedURLException e | IOException e) {
                e.printStackTrace();
            }
            return null;
        }
        /* Result Example
            {
                "coord": {
                    "lon": 139.76,
                    "lat": 35.68
                },
                "weather": [
                    {
                        "id": 501,
                        "main": "Rain",
                        "description": "moderate rain",
                        "icon": "10n"
                    },
                    {
                        "id": 701,
                        "main": "Mist",
                        "description": "mist",
                        "icon": "50n"
                    },
                    {
                        "id": 741,
                        "main": "Fog",
                        "description": "fog",
                        "icon": "50n"
                    }
                ]
            }
        */
        @Override
        protected void onPostExecute(String result) {
            super.onPostExecute(result);
            try {
                JSONObject jsonObject = new JSONObject(result);
                String weatherInfo = jsonObject.getString("weather");
                Log.i("Weather Content: ", weatherInfo);
                JSONArray arr = new JSONArray(weatherInfo);
                for (int i=0; i< arr.length(); i++){
                    JSONObject jsonPart = arr.getJSONObject(i);
                    Log.i("main", jsonPart.getString("main"));
                    Log.i("description", jsonPart.getString("description"));
                }
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    }
}
```
### Encode String to URL format (In case names contain spaces i.e "San Francisco")
``` java
        String encodedCityName = URLEncoder.encode(cityNameEditText.getText().toString(), "UTF-8");
        DownloadTask task = new DownloadTask();
        String result = task.execute("http://api.openweathermap.org/data/2.5/weather?q=" + encodedCityName + "&appid=THE_API_ID").get();

```

### Hide Keyboard (After an event)
```java
    InputMethodManager mgr = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
    mgr.hideSoftInputFromWindow(cityNameEditText.getWindowToken(), 0);
```



### Local Notifications

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Intent intent = new Intent(getApplicationContext(), MainActivity.class);
        PendingIntent pendingIntent = PendingIntent.getActivity(getApplicationContext(), 1, intent, 0);

        Notification notification = new Notification.Builder(getApplicationContext())
                .setContentTitle("Class Starting")
                .setContentText("Today is an exam day, be prepared...")
                .setContentIntent(pendingIntent)
                .addAction(android.R.drawable.sym_action_chat, "Chat", pendingIntent)
                .setSmallIcon(android.R.drawable.sym_def_app_icon)
                .build();

        NotificationManager notificationManager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);

        notificationManager.notify(1, notification);

    }
}
```


### Multi Screen Mode

* Create a project with API Support >= 24 version

* Modify the AndroidManifest.xml like this
```xml
    <application
        android:resizeableActivity="true" 
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity"
            android:supportsPictureInPicture="true">
            <layout
                android:defaultHeight="500dp"
                android:gravity="top"
                android:minWidth="300dp"
                />
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
```

* Code in the ManinActivity.java
```java
public class MainActivity extends AppCompatActivity {
    @Override
    public void onMultiWindowModeChanged(boolean isInMultiWindowMode, Configuration newConfig) {
        super.onMultiWindowModeChanged(isInMultiWindowMode, newConfig);
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        if (isInMultiWindowMode()){

        }

        if (isInPictureInPictureMode()){

        }
    }
}
```

