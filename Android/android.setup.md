# Android Setup Notes


### SDK Folder
* C:\Users\jcabelloc\AppData\Local\Android\Sdk

### Deactivate "Hiper-V"
* Deactivate "Hiper-V" on Windows Features(Control Panel)
* This deactivation causes docker fails

### Install Intel x86 Emulator Accelerator (HAXM installer) from Android Studio
```
Preparing "Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)".
Downloading https://dl.google.com/android/repository/extras/intel/haxm-windows_v7_5_1.zip
"Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)" ready.
Installing Intel x86 Emulator Accelerator (HAXM installer) in C:\Users\jcabelloc\AppData\Local\Android\Sdk\extras\intel\Hardware_Accelerated_Execution_Manager
"Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)" complete.
"Install Intel x86 Emulator Accelerator (HAXM installer) (revision: 7.5.1)" finished.
Parsing legacy package: C:\Users\jcabelloc\AppData\Local\Android\Sdk\.temp\PackageOperation01\unzip\src
Parsing legacy package: C:\Users\jcabelloc\AppData\Local\Android\Sdk\.temp\PackageOperation02\unzip\android-5.1.1
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\build-tools\28.0.3\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\emulator\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\extras\intel\Hardware_Accelerated_Execution_Manager\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\patcher\v4\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\platform-tools\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\platforms\android-28\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\sources\android-28\package.xml
Parsing C:\Users\jcabelloc\AppData\Local\Android\Sdk\tools\package.xml
Android SDK is up to date.
Running Intel® HAXM installer
Intel HAXM installed successfully!
Done
```