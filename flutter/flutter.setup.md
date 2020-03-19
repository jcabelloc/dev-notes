# Flutter Setup Notes
* Reference: https://flutter.dev/docs/get-started/install


## Flutter SDK

### Get the flutter SDK
* Get: flutter_windows_v1.12.13+hotfix.5-stable.zip
* Extract the zip file and place the contained flutter in the desired installation: C:\jcabelloc\programs\flutter

### Update your path (system env variables)
* Append the full path to flutter\bin

### Test installation
```cmd
flutter --version
flutter doctor
```


## Android setup

### Download and install Android Studio.
* Installation location: C:\Program Files\Android\Android Studio
* "Do not import settings"
```
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\sources\android-29\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\tools\package.xml
Android SDK is up to date.
Running Intel® HAXM installer
Failed to install Intel HAXM. For details, please check the installation log: "C:\Users\jcabelloc\AppData\Local\Temp\haxm_log2.txt"
HAXM installation failed. To install HAXM follow the instructions found at: https://software.intel.com/android/articles/installation-instructions-for-intel-hardware-accelerated-execution-manager-windows
Installer log is located at C:\Users\jcabelloc\AppData\Local\Temp\haxm_log2.txt
Installer log contents:
=== Logging started: 1/17/2020  19:06:19 ===
This computer does not support Intel Virtualization Technology (VT-x) or it is being exclusively used by Hyper-V. HAXM cannot be installed. 
Please ensure Hyper-V is disabled in Windows Features, or refer to the Intel HAXM documentation for more information.

=== Logging stopped: 1/17/2020  19:06:19 ===

```
### Install the Flutter and Dart plugins
To install these:

* Start Android Studio.
* Open plugin preferences (Preferences > Plugins on macOS, File > Settings > Plugins on Windows & Linux).
* Select Marketplace, select the Flutter plugin and click Install.
* Click Yes when prompted to install the Dart plugin.
* Click Restart when prompted.

* See now the option: "Start a new flutter project"

### JCC: Additional step

* https://developer.android.com/studio/run/emulator-acceleration#accel-vm

To install WHPX on Windows, follow these steps:

* From the Windows desktop, right-click the Windows icon and select Apps and features.
* Under Related settings, click Programs and Features.
* Click Turns Windows Features on or off.
* Select Windows Hypervisor Platform
* Click OK.

* Aun no verificamos el efecto de este paso, pues se reinstalo Android luego de este cambio pero igual el error de instalacion de Intel HAXM

### Set up your Android device
* Enable Developer options and USB debugging on your device
* the terminal, run the flutter devices command to verify that Flutter recognizes your connected Android device
```cmd
flutter devices
```

### Set up the Android emulator
* Enable VM acceleration on your machine.
* Launch Android Studio > Tools > Android > AVD Manager and select Create Virtual Device. (The Android submenu is only present when inside an Android project.)
* Choose a device definition and select Next.
* Select one or more system images for the Android versions you want to emulate, and select Next. An x86 or x86_64 image is recommended.
* Under Emulated Performance, select Hardware - GLES 2.0 to enable hardware acceleration.
* Verify the AVD configuration is correct, and select Finish.