# Android Firebase Notes

### First Steps
* Create a Firebase Project
* Register the package name in the Firebase Project
* Download the google-services.json file
* Copy that file to the project/app directory
* Modify the gradle files according to google instructions


### Writing to Database

* Saving a List
```java 
    // ...
    FirebaseDatabase database = FirebaseDatabase.getInstance();
    DatabaseReference myRef = database.getReference("friends");
    List<String> friends = new ArrayList<>();
    friends.add("Jose");
    friends.add("Pedro");
    friends.add("Mateo");
    myRef.setValue(friends);
    // ...
```

* Saving a Map
```java
    // ...
    FirebaseDatabase database = FirebaseDatabase.getInstance();
    DatabaseReference myRef = database.getReference("profile");
    Map<String, String> values = new HashMap<>();
    values.put("name", "Juan");
    values.put("email", "jcabelloc@gmail.com");
    values.put("major", "engineer");
    myRef.setValue(values)
    // ...
```

* Saving an Object
```java
    // ...
    FirebaseDatabase database = FirebaseDatabase.getInstance();
    DatabaseReference myRef = database.getReference();
    User user = new User("Juan", "jcabelloc@gmail.com");
    myRef.child("users").child("jcabelloc").setValue(user);
    // ...
```

* Saving with callback
```java
    //...
    myRef.setValue(values, new DatabaseReference.CompletionListener() {
        @Override
        public void onComplete(DatabaseError databaseError, DatabaseReference databaseReference) {
            if (databaseError == null) {
                Log.i("Info", "Save Successful");
            } else {
                Log.i("info", "Save failed");
            }
        }
    });
    //...
```

### Reading from Database
* Reading a Map Object
```java
    //...
    FirebaseDatabase database = FirebaseDatabase.getInstance();
    DatabaseReference myRef = database.getReference("profile");
    myRef.addValueEventListener(new ValueEventListener() {
        @Override
        public void onDataChange(DataSnapshot dataSnapshot) {
            // This method is called once with the initial value and again
            // whenever data at this location is updated.
            Map<String, String> value = (Map<String, String>)dataSnapshot.getValue();
            Log.d("Firebase", "Value is: " + value);
        }

        @Override
        public void onCancelled(DatabaseError error) {
            // Failed to read value
            Log.d("Firebase", "Failed to read value.", error.toException());
        }
    });
    //...
```