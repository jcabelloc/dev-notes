# Ionic Framework SetUp Notes

## Installation
* Notes from: https://ionicframework.com/docs/installation/cli
* Before proceeding, make sure the latest version of Node.js and npm are installed. 

### Install the Ionic CLI
```bash
npm install -g ionic
```
### Start an App
```cmd
ionic start myApp
```
* Select "blank"
* Appflow: NO

### Run the app
```cmd
cd myApp
ionic serve
```

### Inside Visual Studio Code
Add Extensions: View / Essentials: 
* angular essentials (From John Papa)
* Material Icon Theme (From Philipp Kief)


## First Steps

### Adding a page
```bash
ionic generate
-> page
-> "NAME_OF_THE_PAGE"
```

```bash
ionic generate page recipes
```

### Adding a service
```bash
ionic generate service recipes/recipes
```

### Adding a component
```bash
ionic generate component recipes/recipe-item
```

## Running on Android

### Install and set up Android Studio
* Reference: https://capacitor.ionicframework.com/docs/android
* Reference: https://ionicframework.com/docs/building/android

### Project Setup
```bash
ng build
ionic capacitor add android
```

### Set the Package ID.
* For Capacitor, open the capacitor.config.json file and modify the appId property


### Running with capacitor
```bash
ng build
ionic capacitor copy android
```

### Other way of running. Build / Copy / Launch Android Studio
```bash
ionic capacitor run android
```

### Live Reload on Emulator (Native App)
```bash
ionic capacitor run android -l
```

### Useful Links
* Running the App on iOS: https://ionicframework.com/docs/building/ios
* Running the App on Android: https://ionicframework.com/docs/building/android
* Official Capacitor Docs: https://capacitor.ionicframework.com/


### Activate Debugging in Visual Studio Code
* https://code.visualstudio.com/docs/nodejs/angular-tutorial#_debugging-angular

### Debugging Android Apps
* chrome://inspect/, you can see the chrome dev env from the native android app, running the ionic app on the web view. 
