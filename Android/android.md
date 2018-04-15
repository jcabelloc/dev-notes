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
