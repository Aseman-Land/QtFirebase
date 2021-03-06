# Setup
How to set up QtFirebase for inclusion and to utilize Firebase modules in your project.

For a working and up-to-date example that can be compiled in Qt Creator please follow the **Quick start** section found in the [QtFirebaseExample](https://github.com/Larpon/QtFirebaseExample) README. (The example app links back here in some places - but not all). It's strongly adviced to see how the example app is setup in-order to avoid any missing entries.

## Prerequisities
QtFirebase rely on a relativly large software stack. Many common errors people encounter with QtFirebase is due to little or no knownledge of the complete stack of technology used. It's therefore stongly adviced that developers, as a minimum, know or familiarize themselves with the following software in general before diving into the project.

In general
[Firebase](https://firebase.google.com/docs), [Firebase C++ SDK](https://firebase.google.com/docs/cpp/setup), [Qt](https://qt.io), [QML](https://doc.qt.io/qt-5/qmlapplications.html), [QMake](https://doc.qt.io/qt-5/qmake-manual.html)

Android specific
[Gradle](https://gradle.org/), [Android SDK](https://developer.android.com/studio/releases/sdk-tools), [Android NDK](https://developer.android.com/ndk/), [Java](https://www.oracle.com/technetwork/java/index.html)

iOS specific
[XCode](https://developer.apple.com/xcode/), [Firebase iOS SDK](https://firebase.google.com/download/ios)

## Base setup

### Clone the [QtFirebase](https://github.com/Larpon/QtFirebase) project

Clone into the "extensions" or "vendor" folder or into another folder of your choice.
Here we use the [QtFirebaseExample](https://github.com/Larpon/QtFirebaseExample) structure.

```
cd /path/to/projects/QtFirebaseExample/extensions
git clone git@github.com:Larpon/QtFirebase.git
```

### Download and extract the latest version of [Google's Firebase C++ SDK](https://firebase.google.com/docs/cpp/setup).

```
cd /path/to/download
curl --location https://firebase.google.com/download/cpp --output "firebase_cpp_sdk.zip"
unzip firebase_cpp_sdk.zip -d /path/to/sdk
rm firebase_cpp_sdk.zip
```

### Add symlink OR set `QTFIREBASE_SDK_PATH` variable

If you have multiple projects using QtFirebase it's a space-saver to have the Firebase C++ SDK (~832 MB) in one place.

You can either symlink the Firebase C++ SDK to the default search path OR set the `QTFIREBASE_SDK_PATH` variable to the absolute path of the SDK in your project's `.pro` file for the app build.

#### Symlink method
```
ln -s /path/to/sdk/firebase_cpp_sdk /path/to/projects/QtFirebase/firebase_cpp_sdk
```

#### `QTFIREBASE_SDK_PATH` variable method

Open your project with Qt Creator.
Navigate to the `.pro` (sub)project file in the Project pane.
```
QtFirebaseExample
|_...
|_App
    |_App.pro
```

Locate the lines:
```
# NOTE QTFIREBASE_SDK_PATH can be symlinked to match $$PWD/firebase_cpp_sdk
...
```

Change it to match your path(s) - or leave it to use default `$$PWD/firebase_cpp_sdk`
```
QTFIREBASE_SDK_PATH = /path/to/sdk/firebase_cpp_sdk
```
([Example](https://github.com/Larpon/QtFirebaseExample/blob/cc190b/App/App.pro#L86-L87))

#### Choose Firebase modules
In your project's `.pro` (sub)project file (before including `qtfirebase.pri`) - choose the modules to include:
```
QTFIREBASE_CONFIG += analytics messaging admob remote_config
```
([Example](https://github.com/Larpon/QtFirebaseExample/blob/cc190b/App/App.pro#L88))

#### include QtFirebase
Finally include `qtfirebase.pri` in your project's `.pro` file.
```
include(/path/to/QtFirebase/qtfirebase.pri)
```
([Example](https://github.com/Larpon/QtFirebaseExample/blob/cc190b/App/App.pro#L93-L94))

## Note on building
If you're planing on compiling QtFirebase as part of a library in your project, please be aware that QtFirebase in this case will **omit** registering of it's QML types. This is due to how [Q_COREAPP_STARTUP_FUNCTION](https://doc.qt.io/qt-5/qcoreapplication.html#Q_COREAPP_STARTUP_FUNCTION) works. So for auto registering of QML to work - QtFirebase need to be [built as an app](https://github.com/Larpon/QtFirebaseExample/blob/ada77a8b0cde17f8da8df89015bf111ae2c1f328/App/App.pro#L1). Otherwise you must [register the types manually](https://github.com/Larpon/QtFirebase/blob/2ba1c1e4d7cf174aad2108bcdf19c29acdcf6610/qtfirebase_register.h#L79-L113).

## Desktop specific setup
Currently we haven't tested the parts of Firebase C++ SDK that have desktop support.

QtFirebase provides stub implementations ("empty shells" or "placeholders") for desktop builds and ***no*** firebase libraries are linked to the application - *this may change* depending on what parts of the SDK Google make available for desktop builds in the future.

## Android specific setup
When building QtFirebase for Android targets a few extra steps is needed.
Please see [SETUP_ANDROID](https://github.com/Larpon/QtFirebase/blob/master/docs/SETUP_ANDROID.md).

## iOS specific setup
When building QtFirebase for iOS targets a few extra steps is needed.
Please see [SETUP_IOS](https://github.com/Larpon/QtFirebase/blob/master/docs/SETUP_IOS.md).

## What to do next

Congratiolations! You fought through the setup - pad yourself on the back!

This is kind of where we leave you off as a developer. Why? Because your entering Google's domain - and your project's specific setup.
Firebase and AdMob are (mostly) closed platforms. Any problems with Ad fill-rates and backend messaging are out of scope for this project.
The QtFirebase project only provide Qt wrapper classes - that's it.

If you want to know more about how things really work please refer to the official documentation of the respective projects:

Documentation:
* https://firebase.google.com/docs/cpp/setup
* https://developers.google.com/admob/

Troubleshooting
* https://stackoverflow.com/questions/tagged/firebase%20c%2b%2b

Community
* https://github.com/firebase/firebase-cpp-sdk
* https://groups.google.com/forum/#!forum/google-admob-ads-sdk
