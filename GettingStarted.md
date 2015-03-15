<h1>TL;DR</h1>

You can run the PlayN showcase sample in three easy steps:
```
1. git clone https://code.google.com/p/playn-samples
2. cd playn-samples/showcase
3. mvn test -Pjava # Alternatively: ant run-java
```

That's it!



# Troubleshooting #

If the instructions on this page do not work for you, please...
  1. Try searching for your problem using http://google.com/ :)
  1. Check on Stack Overflow: http://stackoverflow.com/questions/tagged/playn



&lt;hr&gt;



<h1>Table of contents</h1>

This guide contains sections on:





&lt;hr&gt;



# Checking out the sample projects #

The sample projects are hosted in the Git repository of the `playn-samples` project.

First you must [install Git](http://book.git-scm.com/2_installing_git.html).

Next you can check out the samples code to your machine like so:
```
git clone https://code.google.com/p/playn-samples
```

This will create a directory named `playn-samples` which contains the sample project source code. We will refer to this directory in the instructions below.

# Running the sample projects #

There are three main sample projects:

  * `hello` - demonstrates the barebones skeleton needed to get a project working on all of the supported PlayN platforms
  * `showcase` - demonstrates various capabilities of the PlayN framework
  * `cute` - demonstrates a more complete game (note: it is woefully incomplete at the moment)

The sample projects may be run via Maven or Eclipse. Presently, building and running a sample for a given platform (HTML5, JVM, Android) may or may not be supported by a particular build tool. The documentation below describes the combinations of build tool and platforms that are supported.

The instructions below describe how to run the `showcase` sample, but they may be used on the other samples by substituting the given sample everywhere `showcase` appears in the instructions.

## Running via Maven ##

First, **make sure that you are using Maven version 3.0.3 or newer**. Run `mvn -v` to see what version of Maven you are using.

**Java**: To run on the Java version (which is recommended for rapid edit-refresh style development), do the following:
```
cd playn-samples/showcase
mvn test -Pjava
```

**HTML5**: To run the HTML5 version, do the following:
```
cd playn-samples/showcase
mvn -Phtml integration-test
```

Then browse to http://localhost:8080/ once you see the console reports:
```
[INFO] Started Jetty Server
```

**Android**: To deploy the Android version to an attached device or a running emulator instance (be sure your emulator image [supports OpenGL](http://android-developers.blogspot.com/2012/04/faster-emulator-with-better-hardware.html)), you will first need to [configure the location of the Android SDK](http://code.google.com/p/playn/wiki/MavenAndroidBuild). Then do the following:
```
cd playn-samples/showcase
mvn -Pandroid install
```

**iOS**: To run the iOS version in the iOS simulator please see the separate [iOS build instructions](iOSBuild.md).

## Running via Eclipse ##

### One time Eclipse setup ###
Prior to importing the project, you must be sure that the following prerequisites are met:

  * Use either:
    * [Eclipse IDE for Java Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/indigor) Indigo (which has m2eclipse installed by default)
    * [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/indigor), which requires separate installation of **Maven Integration for Eclipse** known as m2e:
      * Select **Help → Install New Software...**
      * Select **All Available Sites**
      * Search for '**maven**'
      * Check the box for **Maven Integration for Eclipse**
      * Click **Finish** to complete the installation

If you wish to use Eclipse to build and deploy to Android:

  * Install [Android Plugin for Eclipse](http://developer.android.com/sdk/eclipse-adt.html)
  * In **Preferences → Maven → Discovery** click **Open Catalog** check **Android Connector** and click **Finish**
  * In **Preferences → Maven → Installations** click **Add...** and add a Maven installation that is at least version 3.0.3 or newer (unfortunately the Maven bundled with Eclipse is version 3.0.2 which is incompatible with the Android Maven plugins).

If you choose not to install the Android plugins, you should avoid importing any `foo-android` sub-projects or they will report having errors.

Finally:

  * Restart Eclipse at least once after these installation steps

### Import projects ###

In order to build and run the PlayN samples via Eclipse, you must import the project into your Eclipse workspace.

  1. Select **File → Import** from the menu
  1. Select **Maven → Existing Maven Projects** and click **Next**.
  1. Enter the directory into which you checked out `playn-samples` in the **Root Directory** box and press enter.
  1. The various sample projects will appear in the **Projects** listing. Click **Finish** to import the projects.

### Running the imported samples ###

To run the Java version (which is recommended for rapid edit-refresh style development), do the following:
  1. Click on _playn-showcase-java_ in the **Package Explorer**
  1. Press Command-Shift-F11 or right click and select **Run as → Maven test**

To run the compiled HTML version (which is what your users will use), do the following:
  1. Right click on _playn-showcase-html_ in the **Package Explorer**
  1. Select **Run as → Maven build...**
  1. Enter "integration-test" into the **Goals** text field and press **Enter** or click **Run**
  1. Point your web browser at http://127.0.0.1:8080

To install the Android version to a connected device or simulator:
  1. Right click on _playn-showcase-android_ in the **Package Explorer**
  1. Select **Run as → Maven install**

# Creating a new project #

A project that uses PlayN must accommodate some complexity due to the fact that the project must be built and deployed in different ways for each of the supported platforms (Java, HTML5, etc.). We have endeavored to hide as much of this complexity as possible for developers who use our default project structure, which is based on Maven.

It is possible to create a skeleton project using the PlayN Maven Archetype. Instructions for doing so are provided later in this document.


## PlayN Game Project Structure ##

A PlayN game project consists of multiple subprojects. These subprojects serve to isolate the platform-specific code and dependencies for each version into its own subproject. A basic PlayN game project looks like so:
```
mygame/pom.xml - main project Maven build file
      /core/pom.xml - core project build file
      /core/src/main/java/com/mygame/core/... - main game source code
      /core/src/main/java/com/mygame/resources/... - main game media (images, etc.)
      /core/src/main/java/com/mygame/java/MyGameJava.java - JVM entry point
      /html/pom.xml - HTML version build file
      /html/src/main/java/com/mygame/MyGame.gwt.xml - HTML version GWT module
      /html/src/main/java/com/mygame/html/MyGameHtml.java - HTML version entry point
      /html/src/main/webapp/MyGame.html - HTML version main web page
      /html/src/main/webapp/WEB-INF/... - HTML version war data
      /android/pom.xml - Android version build file
      /android/AndroidManifest.xml - Android app configuration
      /android/res - Android-specific resources (icons)
      /android/src/main/java/com/mygame/android/MyGameActivity.java - Android entry point
      /ios/pom.xml - iOS version build file
      /ios/Info.plist - iOS app configuration
      /ios/src/Main.cs - iOS entry point
      /ios/mygame.sln - Xamarin iOS/MonoTouch build configuration
      /ios/mygame.csproj - Xamarin iOS/MonoTouch build configuration
```

Fortunately, all of this boilerplate can be automatically generated for you, and you can mostly concern yourself only with the contents of `mygame/core/src/main/java`.


## Generating a skeleton project with Maven ##

A new PlayN project can be generated from the command line or via Eclipse. To do so from the command line, enter:
```
mvn archetype:generate -DarchetypeGroupId=com.googlecode.playn -DarchetypeArtifactId=playn-archetype -DarchetypeVersion=1.8
```

_Note, after a new PlayN release is pushed to Maven Central, there is a [significant delay](https://docs.sonatype.org/display/Repository/Central+Repository+FAQ) (potentially up to one week) before the archetype becomes available. Until that time you will receive an error message like this:
> `[ERROR] Failed to execute goal` ... `The desired archetype does not exist (com.googlecode.playn:playn-archetype `...`)`_

This will ask you to provide certain metadata for your project. These are explained in turn:
```
Define value for property 'groupId': :
```

This should be a reverse domain that identifies your project. Examples include: `com.googlecode.myproject`, `com.github.myid.myproject`, `com.mydomain.myproject`.

```
Define value for property 'artifactId': :
```

This should be an all lowercase identifier that identifies your game. Examples include: `monkeybattle`, `upsetavians`, `gameamazing`.

```
Define value for property 'version':  1.0-SNAPSHOT: :
```

This defines the version used when naming your jar files. You can press enter to use the default: `1.0-SNAPSHOT`, or if you don't plan on using Maven to deploy snapshot versions of your game to a Maven repository, you can specify whatever version you prefer (e.g. `1.0`, `0.1`, `r55-alpha`).

```
Define value for property 'package':  com.mydomain.myproject: :
```

This defines the Java package in which your game files will be placed. The default will be the value you provided for `groupId`, which is often also a good package name. However, you can use whatever package you like.

```
Define value for property 'JavaGameClassName': :
```

This defines the name used for your game when naming various Java classes. As such it should follow Java class naming conventions and start with an upper-case letter. Examples include: `MonkeyBattle`, `UpsetAvians`, `GameAmazing`.

Once you have provided these properties, you will be asked to confirm that they are correct.
```
Confirm properties configuration:
groupId: ....
artifactId: ...
version: ...
package: ...
JavaGameClassName: ...
 Y: : 
```

If they are, enter `y` and press return. Your game skeleton will be generated in a directory with the same name that you provided for `artifactId`.

At this point you could import the newly created game directory into Eclipse, using the same instructions you used to import the PlayN project above.

You can also build and run your new sample game using maven.

To run and test your new game:
```
cd <your-new-game-directory>
mvn test -Pjava
```

To compile the HTML version and run it locally:
```
cd <your-new-game-directory>
mvn -Phtml integration-test
```


## Generating a skeleton project with Eclipse ##

  1. Select **File → New → Other...**
  1. Select **Maven → Maven Project** in the dialog that pops up
  1. Click **Next**
  1. Click **Next** again unless you wish to specify a custom workspace location.
  1. Specify `com.googlecode.playn` in the **Filter** field
  1. Select `playn-archetype` version `1.8`
  1. Configure the _Group Id_, _Artifact Id_, _Version_, _Package_, and _JavaGameClassName_ properties per the instructions provided above in the _Generating a skeleton project with Maven_ section.
  1. Click **Finish** when you have configured all of the required properties.

Your projects should now appear in the **Package Explorer**. You can use the same instructions provided in the sections above on building and running the samples from Eclipse to build and run your project.


## Running your new game on App Engine ##

To run your game on [Google App Engine](http://appengine.google.com/), we can use the kindleit.net [maven-gae-plugin](http://www.kindleit.net/maven_gae_plugin/).
  1. Verify that the `maven-gae-plugin` has access to a locally extracted App Engine SDK. There are two ways to make this happen:
    1. Use `mvn gae:unpack` to automatically download and extract the contents of `com.google.appengine:appengine-java-sdk:zip` into your local maven repository. (Make sure that `gae.home` is not set in your `~/.m2/settings.xml`, since that will [prevent](https://github.com/maven-gae-plugin/maven-gae-plugin/issues/8) `gae:unpack` from working correctly.)
    1. Manually download and extract the [App Engine SDK](http://code.google.com/appengine/downloads.html) and set the `gae.home` property in your `~/.m2/settings.xml` to the location of the extracted SDK.
  1. Run the game using the local dev\_appserver:
```
mvn -f html/pom.xml gae:run
```
> > The game is now accessible at http://localhost:8080/, the dev\_appserver admin console is available at http://localhost:8080/_ah/admin.
  1. Change the `<application></application>` section of `src/main/webapp/WEB-INF/appengine-web.xml` to specify the app id you wish to deploy to.
  1. Deploy the game to App Engine:
```
mvn -f html/pom.xml gae:deploy
```


## Optional enhanced HTML5 logging ##

To enable enhanced HTML5 logging, provided by [gwt-log](http://code.google.com/p/gwt-log/):
  1. Modify your project's `*.gwt.xml` file to include the following line
```
<inherits name="playn.logging.Enhanced" />
```
  1. Read [Enhanced.gwt.xml](https://github.com/threerings/playn/blob/master/html/src/playn/logging/Enhanced.gwt.xml) for additional `*.gwt.xml` enhanced logging configuration options, including stack trace deobfuscation, and enabling a remote logger, to forward client-side log messages to the server for log aggregation and analysis.