# mLab Notes

### Initial Steps

* Sign up at https://mlab.com/
* Create Database (Choose sandbox)
* Choose platform (i.e. Google Platform)
* Enter mongoDB name (i.e. yelpdatabase)
* Confirm Settings
* Then the DB is created
* Add a DB user


### Connect from nodejs app to mLab DB (app.js)
```javascript
//...
mongoose.connect("mongodb://user:password@ds113179.mlab.com:13179/yelpdatabase");
//...

```
