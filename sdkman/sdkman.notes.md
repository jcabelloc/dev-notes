# SDKman Notes
* Reference: https://sdkman.io/install


###  Install Gitbash if you do not have

### Install MinGW 
* Go to: http://www.mingw.org/
* Then: Download (http://www.mingw.org/) and install
* Add all packages
* Add to the PATH env variable: C:\jcabelloc\Programs\MinGW\bin

### Install 7-Zip / Set up
* Install 7-zip
* Then add to the PATH env variable: C:\Program Files\7-Zip
* Clone file 7-zip.exe to zip.exe

### Install SDK Man
```bash
curl -s "https://get.sdkman.io" | bash

source "$HOME/.sdkman/bin/sdkman-init.sh"

sdk version

```

### Install JDK
```bash
sdk list java

sdk install java 11.0.4-zulu
sdk install java 8.0.222.hs-adpt

```

### Set a default java version
```bash
sdk use java 8.0.222.hs-adpt
sdk default java 11.0.4-zulu
echo $JAVA_HOME
java -version
```