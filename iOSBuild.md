# Building your PlayN game on iOS #

Note: This article describes the process for building and running the PlayN Showcase [sample project](https://code.google.com/p/playn-samples) on iOS. The same instructions will work for your own PlayN game.

## Requirements ##

  1. You must use Maven to build and package your game. You can invoke the necessary Maven commands from within Eclipse or IDEA or another IDE, but there is no custom IDE plugin.
  1. You must be developing on a Mac. It is not possible to build iOS applications on any other platform.
  1. Install [MonoTouch](http://xamarin.com/monotouch), following their [installation instructions](http://docs.xamarin.com/ios/getting_started/installation).
  1. Download [ikvm-monotouch](https://github.com/samskivert/ikvm-monotouch) using [this pre-built zip file](http://dl.dropbox.com/u/404021/ikvm-monotouch.zip). Unpack it somewhere and note the directory in which it is unpacked.

## Configuration ##

You must provide the path to your ikvm-monotouch directory to the Maven plugin that will prepare your game for iOS. Edit your `~/.m2/settings.xml` file and add (or incorporate) the following:

```
<settings>
  <profiles>
    <profile>
      <id>ikvm-monotouch</id>
      <properties>
        <ikvm.path>${user.home}/ikvm-monotouch</ikvm.path>
      </properties>
    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>ikvm-monotouch</activeProfile>
  </activeProfiles>
</settings>
```

Be sure to change the path to ikvm-monotouch if you did not install it in your home directory.

## Building and Running in Simulator ##

  1. Build and package your game code via Maven. You can do so on the command line thusly:
```
  cd playn-samples/showcase
  mvn -Pios package
```
  1. Then open the ios MonoDevelop project like so:
```
  open ios/showcase.sln
```
  1. Click the "Start Debugging" button in the toolbar, or press Command-Enter. This will launch the game in the iOS simulator.

## Building and Running on a Device ##

To deploy your game to an iOS device, you will need to follow the standard instructions for becoming a registered Apple developer and configuring one or more iOS devices for development: [Configuring Development and Distribution Assets](https://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/10-Configuring_Development_and_Distribution_Assets/identities_and_devices.html).

You will also need a [licensed copy of MonoTouch](https://store.xamarin.com/). The evaulation copy cannot be used to deploy to devices.

Once you have followed those instructions and obtained and installed the necessary development certificate and provisioning profile, you can follow the instructions in the "Deploying your app to your device" section of [this blog post](http://lostechies.com/marcusbratton/2010/08/13/getting-started-with-monotouch/).

## Additional Notes ##

### Assets symlink ###

If you create a new PlayN project from scratch, using the Maven archetype, you need to create a symlink in the `ios` directory named `assets` that points to `../core/src/main/java/yourpackage/yourgame/resources`. For example:
```
cd mygame/ios
ln -s ../core/src/main/java/mypackage/mygame/resources assets
```

This will allow your `yourgame-ios.csproj` file to reference your assets as if they were inside the `ios` directory which will make MonoDevelop much happier.

We would create this symlink automatically, but it is unfortunately not possible to create symlinks using the Maven archetype system.

Note: **do not** name this symlink `resources` as that is a reserved directory name in the Visual Studio project file format used by MonoDevelop.

### Automatically Add New Resources ###

Every resource in your game must be enumerated in `yourgame-ios.csproj` which can be quite cumbersome. If you have set up your `assets` symlink as described above, you can configure MonoDevelop to automatically detect new assets and add them to your `.csproj` file.

Select the _Project -> yourgame options_ menu and on the _Main Settings_ page, check _Search for new files on load_. Once that is checked, MonoDevelop will prompt you whenever you open your `yourgame-ios.sln` file and it discovers new asset files.