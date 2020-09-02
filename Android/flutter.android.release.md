
### References
* [Build and release an Android app](https://flutter.dev/docs/deployment/android)
* [Upload your app to the Play Console](https://developer.android.com/studio/publish/upload-bundle)


## Signing the app

### Create a keystore
```
~ ❯ pwd                                                                                           4s 22:07:02
/Users/jcabelloc
~ ❯ keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key      22:07:27

Enter keystore password: a.....d
Re-enter new password:
What is your first and last name?
  [Unknown]:  Juan Cabello
What is the name of your organizational unit?
  [Unknown]:  iTana
What is the name of your organization?
  [Unknown]:  iTana
What is the name of your City or Locality?
  [Unknown]:  Lima
What is the name of your State or Province?
  [Unknown]:  Lima
What is the two-letter country code for this unit?
  [Unknown]:  PE
Is CN=Juan Cabello, OU=iTana, O=iTana, L=Lima, ST=Lima, C=PE correct?
  [no]: yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA256withRSA) with a validity of 10,000 days
	for: CN=Juan Cabello, OU=iTana, O=iTana, L=San Borja, ST=Lima, C=PE
[Storing /Users/jcabelloc/key.jks]
```

### Reference the keystore from the app
* Create a file named <app dir>/android/key.properties that contains a reference to your keystore:
```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, such as /Users/<user name>/key.jks>
```

### Configure signing in gradle
* 1era Parte
```
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
      ...
}
```

* 2da Parte
```
signingConfigs {
    release {
        keyAlias keystoreProperties['keyAlias']
        keyPassword keystoreProperties['keyPassword']
        storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
        storePassword keystoreProperties['storePassword']
    }
}
buildTypes {
    release {
        signingConfig signingConfigs.release
    }
}
```
* This prevents cached builds from affecting the signing process.
```
flutter clean
```


## Reviewing the app manifest
* application: Edit the android:label
* uses-permission. Add the android.permission.INTERNET permission (Pendiente)

## Reviewing the build configuration
* applicationId
* versionCode & versionName. You can do this by setting the version property in the pubspec.yaml file
* minSdkVersion & targetSdkVersion

## Building the app for release

### Build an app bundle
* Desde el directorio del proyecto
```
flutter build appbundle
```
* Verificar el bundle generado
```
ll build/app/outputs/bundle/release  
```

### Test the app bundle

* Online using Google Play
