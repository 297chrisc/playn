# Running your PlayN game on Android #

**Note:** This article assumes that you already have a PlayN game working with the Java backend. See [GettingStarted](GettingStarted.md) if you have not yet cleared that hurdle.

## Setting Up ##

To compile and deploy your game to Android, you'll need to set a few things up:

  * If you use Maven, you'll need to [set up the Maven Android plugin](MavenAndroidBuild.md).
  * If you use Eclipse, then you'll need to install the [Eclipse Android plugin](http://developer.android.com/tools/sdk/eclipse-adt.html).

It is recommended to build your APK and deploy it to devices using Maven from the command line. This is the best tested and most reliable way to get your game onto a device. You can still use Eclipse or any other IDE to develop and test your game using the Java backend, but when you want to install it onto an Android device or the Android emulator, using Maven on the command line is the least error prone approach.

Testing your game on a real Android device is highly recommended, but it is possible to test your game on the Android emulator. To do so, you will need to [set up the emulator](http://developer.android.com/tools/devices/emulator.html) and create an AVD that supports OpenGL. The best setup is one that uses [Intel's HAXM accelerator](http://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager) in conjunction with an [Intel Atom CPU configuration](http://software.intel.com/en-us/articles/installing-the-intel-atom-x86-system-image-for-android-emulator-add-on-from-the-android-sdk) for the AVD. This allows the emulator to run directly on your desktop's x86 CPU, which is orders of magnitude faster than if it must emulate an ARM CPU using your desktop's x86 CPU.

## Brief Anatomy of an Android project ##

Assuming you generated your project using the PlayN Maven archetype, you will have an `android` directory, which will contain all of the necessary metadata to build and deploy your game on Android. Here we will briefly describe the files involved so that you know where to look when you want to customize things further.

  * `AndroidManifest.xml` - this contains all of the metadata Android needs to know about your game. It includes things like whether it supports portrait or landscape orientation, what permissions your game needs (network access for example), and a number of other things. You can learn more about this file [via the Android documentation](http://developer.android.com/guide/topics/manifest/manifest-intro.html).
  * `res` - this directory contains Android "resources" which are things like your app icon, and any translation strings that are used in your `AndroidManifest.xml`. PlayN games generally do not put additional resources in this directory because it must use an approach that works across many different platforms, not just Android.
  * `assets` - this is a symlink generated to the `assets/src/main/resources/assets` directory in your `assets` project submodule. Android treats the files in this directory specially (it bundles them in your APK without further compressing them). Files loaded via the `playn.core.Assets` class come from this directory.
  * `default.properties` - This is used by the Android SDK tools. If you change the `targetSdkVersion` in your `AndroidManifest.xml` you should update this file as well.
  * `src/main/java/yourpackage/YourGameActivity.java` - this is the main entry point of your game on Android. All Android apps are structured around "activities", and a PlayN game is one big `Activity` as far as Android is concerned. If you integrate platform-specific SDKs into your game, that integration code will be initialized and configured in this class.

## Building and Deploying to Emulator or Device ##

Building your game for Android is accomplished by invoking the following Maven command:

```
mvn clean package -Pandroid
```

The `-Pandroid` tells Maven to activate the "android" profile, which causes it to build the `android` submodule in your project. Normally that submodule is ignored while you build and test using the Java backend.

After invoking the above command, an `android/target/yourgame-android-1.0-SNAPSHOT.apk` file will have been created. APK is the app packaging format used by Android. It's just a zip file with special contents, so you can open it up and look at it with any zip tool.

You can use the standard Android tools to install this APK to your device (or send it to friends who want to help test your game), but the Maven Android plugin will automatically invoke the installation tools if, instead of `package`, you use `install`, like so:

```
mvn clean install -Pandroid
```

This same `install` command will deploy your game to the Android emulator if an emulator is running.

Also note that the addition of the `clean` target is not necessary, but it is wise to include `clean` before deploying to a device if you've been doing a lot of testing of the Java backend, especially with Eclipse or another IDE, which may do incremental recompilation of your code.

## Various Android-specific Things ##

PlayN provides a few Android-specific hooks and APIs that allow your game to be a better "Android citizen" and to work better across a wider range of devices. These are described in separate articles below:

  * [Supporting HiDPI devices](HiDPISupport.md)
  * [Determining how much memory you have](AndroidMemoryClass.md)
  * [Downsampling images for low-memory devices](AndroidDownSampling.md)
  * [Properly handling the Android back button](AndroidBackButton.md)

## Publishing your Game to Google Play Store ##

There are a variety of things you should look over before publishing your game to the Google Play Store (or any other Android store), and Google conveniently provides a [publishing checklist](http://developer.android.com/distribute/googleplay/publish/preparing.html) with details.

The technical requirements for publishing are described on [this page](http://developer.android.com/tools/publishing/preparing.html). It describes the requirements that [published APKs must be signed](http://developer.android.com/tools/publishing/app-signing.html). Assuming you follow the instructions in the previous link for creating a keystore, you can make use of the PlayN Android Maven configuration to automatically sign and zipalign your APK in preparation for publishing.

You will need to edit your `android/pom.xml` and configure the following two properies:

```
  <properties>
    <sign.keystore>game.keystore</sign.keystore>
    <keystore.alias>game</keystore.alias>
  </properties>
```

`sign.keystore` should specify the path to your `.keystore` file and `keystore.alias` should be configured with the alias of the key you created for signing your APK. You can either provide the keystore (and key) password on the command line when building your signed APK, or you can put those in your `android/pom.xml` as well, depending on your level of concern that your source code and keystore might fall into the wrong hands:

```
  <properties>
    <keystore.password>PASSWORD</keystore.password>
    <key.password>PASSWORD</key.password>
  </properties>
```

To generate a signed and zipaligned APK, run the following Maven command:

```
mvn clean package -Pandroid -Psign -Dkeystore.password=PASSWORD
```

You can omit `-Dkeystore.password=PASSWORD` if you configured the password in your POM. If you configured a password for your private key, in addition to your keystore, you will need to also supply `-Dkey.password=PASSWORD`.

Once this command has run successfully, it will generate your signed, aligned APK in a separate file:

```
android/target/yourgame-android-1.0-SNAPSHOT-aligned.apk
```

This is the file you should upload to the Google Play App Console when publishing your game.