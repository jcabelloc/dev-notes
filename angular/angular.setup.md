# Angular SetUp Notes

## Set up tools
* Install NodeJS (it includes npm)
* Install Visual Studio Code
* Install Git Bash (https://git-scm.com)
* Integrate terminal into Visual Studio Code (https://code.visualstudio.com/docs/editor/integrated-terminal)
* Install Angular CLI:
```bash
npm install -g @angular/cli@latest
```
* Check Version Tools
```bash
node -v
npm -v
ng -v
```
* npm modules are located at: C:\Users\jcabelloc\AppData\Roaming\npm\node_modules


### First Steps
* Create a new app
```bash
ng new demoapp
```
* Serve the app
```bash
ng serve --open
```
* Go to http://localhost:4200/

* Generate a component
```bash
ng generate component components/dashboard
```
* Generate a Service (By default a service provider is added to the root application injector)
```bash
ng generate service services/usuario
```

### Install packages
```bash
npm install --save bootstrap popper.js jquery font-awesome
```

### Update the "package.json" file (remove "^")
```
    "bootstrap": "4.1.1",
    "font-awesome": "4.7.0",
    "jquery": "3.3.1",
    "popper.js": "1.14.3",
```


### Modify the "angular.json" file
```
    "styles": [
        "src/styles.css",
        "node_modules/font-awesome/css/font-awesome.css",
        "node_modules/bootstrap/dist/css/bootstrap.css"
    ],
    "scripts": [
        "node_modules/jquery/dist/jquery.js",
        "node_modules/popper.js/dist/umd/popper.js",
        "node_modules/bootstrap/dist/js/bootstrap.js"
    ]
```