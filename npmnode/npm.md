# NPM notes

Package Manager for JavaScript

### Install and use a package
```bash
# install, for instance the "cat-me" package
npm install cat-me
```

```javascript
// In the app.js file, call the package
var cat = require("cat-me");
console.log(cat());
```

```bash
# run the app.js file
npm app.js
```

### Create a package.json for the App
Use the npm init command to create a package.json file for your application. 
```bash
npm init
```
### Install express and save in the package.json file
```bash
npm install express --save
```

