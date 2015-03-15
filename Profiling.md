# Profiling Your Game #

It's very likely that you'll eventually want to profile your game to make it faster. There are a number of resources available for profiling PlayN games, which are useful in different circumstances.

## Java Backend ##

Though you probably aren't deploying your game to players using the Java backend, it can still be useful to profile your game running in the Java backend, as it can uncover costly or inefficient things going on in your code that will slow it down on any platform. Here are some options:

### `YourKit` Java Profiler ###

`YourKit` provides an excellent [Java profiler](http://www.yourkit.com/java/profiler/index.jsp) that works right out of the box with a PlayN game running on the Java backend. Simply start your game and then start up the `YourKit` profiler and the JVM instance running your game will show up in the list of processes available for profiling.

`YourKit` is also kindly providing licenses for their profiler to developers working on the PlayN project, so that we can ensure that the PlayN code itself is as memory and CPU efficient as possible.

### HProf ###

Java contains a built-in heap profiling mechanism that can be useful for basic CPU and memory debugging. You can read the [Oracle page describing it](http://docs.oracle.com/javase/7/docs/technotes/samples/hprof.html).

## iOS Backend ##

A PlayN game compiled for the iOS backend is essentially a MonoTouch application, and the same procedure for profiling MonoTouch applications can be used for PlayN games compiled for running on iOS. Xamarin provides a guide describing  [how to use Apple's Instruments profiler on a MonoTouch application](http://docs.xamarin.com/ios/Guides/Deployment%252c_Testing%252c_and_Metrics/Using_Instruments_to_Detect_Native_Leaks_Using_MarkHeap).

## Android Backend ##

Android provides a profiling tool called [DDMS](http://developer.android.com/tools/debugging/ddms.html) which can be used on PlayN games compiled for the Android backend.

## HTML Backend ##

Profiling a game compiled for the HTML backend can be accomplished with a few different tools:

  * [Speed Tracer](https://chrome.google.com/webstore/detail/speed-tracer-by-google/ognampngfcbddbfemdapefohjiobgbdl) for Chrome
  * [FireBug](http://getfirebug.com/javascript) for Firefox