# Running your PlayN game on Android #

**Note:**  This article assumes that you already have a working PlayN game, with the game and PlayN loaded in projects in Eclipse.  It also assumes that you have a basic familiarity with Android development and have already installed the Android SDK and API 11 into Eclipse.   If you are unfamiliar with these topics, please see the documentation on the official [Android Developers site](http://developer.android.com/sdk/index.html) and the other articles in this Wiki.

## Maven Note ##
For users familiar with Maven, it is preferable to use Maven to deploy your game to a device using the "mvn android:deploy" command from the android directory of your PlayN project. However, this tutorial should still be a useful how-to for users attempting to build their game using Eclipse.

Please see [MavenAndroidBuild](MavenAndroidBuild.md) for instructions on how to configure Maven with the location of your Android SDK.

## Project Set-Up ##

Begin by creating a new Android project in Eclipse.  Name it what you like, although a descriptive name such as _mygame-android_ is best.  Make sure that you are targeting API level 11, and allow Eclipse to create a new Activity for you.

If you haven't already, you will now need to import the _playn-android_ project into Eclipse.  Go to **File -> Import -> General -> Existing Projects** into Workspace.  The root directory is in _.../playn/android_, where _playn/_ is the root folder of your existing PlayN project.  Select the _playn-android_ project and hit **Finish**.

Open your _mygame-android_ project's **Properties**.  If you set your project to target the wrong API, you can fix this now via the Android selection in the left navigation bar.  If not, click on the **Java Build Path** selection.  Under the **Sources** tab, you should already have two sources: _mygame-src_ and _mygame-res_.  In addition to this, you will need to add a Linked Source to the _playn-android_ source.  Hit the Linked Source... button and browse to the _.../playn/android/src_ folder for the Linked Folder Location.  Call the Folder Name _playn-android\_src_ or something simliar.

Now open the **Projects** tab.  Add the existing _playn_ and _mygame_ projects.  Hit **OK** to save your changes.


## Configure AndroidManifest.xml ##

Open up the _AndroidManifest.xml_ file in your Android game project.  Add the following lines within the `<manifest>`  tag:

```
<uses-sdk android:minSdkVersion="6" android:targetSdkVersion="11" />
 <uses-permission android:name="android.permission.WAKE_LOCK" /> 
```

and add these within the `<activity>` tag:

```
android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
android:configChanges="keyboardHidden|orientation">
```

## Writing the Activity ##

Now open the _MyGameActivity.java_ file that Eclipse should have generated for you. The extended _OnCreate()_ method is the entry point into your game, and will serve the same purpose as the _main()_ function in your Java game.

First, set up your imports by adding the following lines, where _MyGame_ is your existing main class that extends _playn.core.Game_.

```
import playn.android.GameActivity;
import playn.core.PlayN;
import com.mygames.MyGame;
```

Edit your Activity declaration so that your Activity extends _GameActivity_.  Then write your _!main()_ method so that it resembles this:

```
  @Override
  public void main() {
    platform().assetManager().setPathPrefix("com/mygames/resources");
    PlayN.run(new MyGame());
  }
```

Make sure that the path prefix points to the base package for your resource files.  It should look the same as the path prefix in the Java version of your game, but without the _src/_ at the start of the path.

Congratulations!  Your should now be able to run this new project as an Android application and see your game run on the emulator or a connected Android device.  If it runs fine, you're done!  If it crashes, read on for some tips on some possible reasons why.



## Tips for an Easy Port ##

When developing your PlayN game, it's easy to begin thinking that it will always be run in a Java or web desktop environment.  If you plan on running your game on Android devices, however, there are several key things to keep in mind that will make your life much easier when the time comes:

  * Test your game on Android throughout development.
    * This is the key to an easy porting process.  Developing and testing the Android version of your game in parallel with the desktop and web versions will help you catch any issues early, before they get too embedded in your code to easily locate and debug.
  * Avoid references to device-specific (or GWT-only) classes in your game code.
    * This one can be a killer.  When developing and building your code, Eclipse will not throw up an error  if you use these classes in game code that will run on every platform.  However, this can cause InvocationExceptions and other errors at runtime.
    * Common mistakes include catching _JavaScriptExceptions_ instead of generic _RuntimeExceptions_, which will cause your game to crash on Android even if the exception is not thrown, or referencing _Window.alert()_ instead of the platform-agnostic _PlayN.log().error()_.
  * Make sure you plan your controls around different input schemes.
    * This is both a design concern and a coding concern.  Try to design your game so that it will work using different input schemes, including mouse, pointer events, multitouch, and desktop and mobile keyboards.
    * Make sure that your make _Game_-extending class implements multiple control type Listeners, especially for the various mouse-like input controls (Mouse, Touch, and Pointer).  You can also check if _PlayN.mouse()_, _PlayN.touch()_, and _PlayN.pointer()_ are null in order to establish which Listeners your game should set itself at at runtime.


## Preparing for Release ##

An Android APK must be signed and zipaligned before it can be submitted to the Google Play store. You can accomplish this by placing the following into your `android/pom.xml` file:

```
  <!-- run 'mvn package -Pandroid -Psign' to sign and align -->
  <profiles>
    <profile>
      <id>sign</id>
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jarsigner-plugin</artifactId>
            <version>1.2</version>
            <executions>
              <execution>
                <id>signing</id>
                <goals><goal>sign</goal></goals>
                <phase>package</phase>
                <inherited>true</inherited>
                <configuration>
                  <archiveDirectory></archiveDirectory>
                  <includes>
                    <include>target/*.apk</include>
                  </includes>
                  <keystore>KEYSTOREPATH</keystore>
                  <storepass>STOREPASS</storepass>
                  <keypass>KEYPASS</keypass>
                  <alias>KEYALIAS</alias>
                  <arguments>
                    <argument>-sigalg</argument><argument>MD5withRSA</argument>
                    <argument>-digestalg</argument><argument>SHA1</argument>
                  </arguments>
                </configuration>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <groupId>com.jayway.maven.plugins.android.generation2</groupId>
            <artifactId>android-maven-plugin</artifactId>
            <inherited>true</inherited>
            <configuration>
              <zipalign><skip>false</skip></zipalign>
              <sign><debug>false</debug></sign>
            </configuration>
            <executions>
              <execution>
                <id>alignApk</id>
                <phase>package</phase>
                <goals><goal>zipalign</goal></goals>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
```

KEYSTOREPATH must be changed to the path to your keystore. STOREPASS and KEYPASS can either be set to your passwords, or you can change them to `${sign.pass}` and from the command line, when you build your signed and aligned APK you will supply the password like so:

```
% mvn package -Pandroid -Psign -Dsign.pass=PASSWORD
```

Be sure to also read the [Android Guidelines for preparing a release](http://developer.android.com/distribute/googleplay/publish/preparing.html). That will provide details on how to create your signing keystore and a bunch of other useful things relating to the Google Play store.