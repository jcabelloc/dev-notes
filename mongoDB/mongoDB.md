# Mongo DB Notes

### First Steps

Reference:
https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/

After install MongoDB
* Install Path: C:\Program Files\MongoDB
* Create the directories in the MongoDB Root: \data\db


Start MongoDB
```bash
C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe
```
Connect to MongoDB
```bash
C:\Program Files\MongoDB\Server\3.4\bin\mongo.exe
```

### First Basic Commands

```
show dbs;
use college;
db.student.insert({name:"Jhon", major:"CS" });
db.student.insert({name:"Mary", major:"MIS" });
db.student.find();
db.student.find({name:"Mary"});
db.student.insert({name:"Jack", major:"MIS" });
db.student.find({major:"MIS"});
db.student.update({name:"Jack"}, {$set:{name:"Phil", resident: true}});
db.student.remove({resident:true});

db.campgrounds.drop();
```

