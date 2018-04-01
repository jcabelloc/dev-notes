# Heroku Notes for NodeJs

### Steps to deploy a Node.js app to Heroku
* Based on Heroku Getting Started
* Create a heroku account
* Install Heroku CLI
* Check you have installed node, npm and git
* Prepare de node.js app
* Initialize Git and commit all changes
* Include in the package.json file, in the scripts section ("start": "node app.js")
* Set up the port in the app.js
```javascript
//...
const PORT = process.env.PORT || 3000
app.listen(PORT, function(){
    console.log("Server Started");
});
//...
```
* Deploy the app
```bash
# Create the web deployment space
$ heroku create
# Deploy our app
$ git push heroku master
# See logs
$ heroku logs
```

### Useful commands
```bash
# Run a command on the heroku side
$ heroku run ls
$ heroku run cat app.js
# Add an environment/config variable
$ heroku config:set DATABASEURL=mongodb://use:password@ds113179.mlab.com:13179/yelpdataba 

```