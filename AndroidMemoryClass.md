Android devices have what is called a "memory class" which is a rough estimation of the heap size (in megabytes) available to your application. Android apps (particularly games) consume different kinds of memory (images consume non-heap memory), but the memory class gives you a rough idea of how powerful the device is and whether or not you're likely to run into limited memory issues.

In API level 11 (Android 3.0) Android added a notion of a "large" memory class, which is a mode apps can request to provide them with more heap. Games are an excellent example of the sort of apps that might need such a feature.

To find out the memory class of a device, put the following code in `YourGameActivity.java`:

```
ActivityManager am = (ActivityManager) getSystemService(ACTIVITY_SERVICE);
int memoryClass = am.getMemoryClass();
int largeMemoryClass = -1;
try {
    largeMemoryClass = am.getLargeMemoryClass();
} catch (Throwable t) {
    // this is not available on older SDKs; leave initialized to -1
}
```

Most devices running Android 2.3 or later will return a memory class of 24 or larger, which means that a game has roughly 24MB of heap to work with, before it starts to see `OutOfMemoryErrors`. However, as mentioned above, images consume non-heap memory which has different limits (which cannot be queried, as far as I know), so you may see `OutOfMemoryError` when loading images, even though your heap is ostensibly not exhausted.

An app requests "large memory" status by setting `android:largeHeap` to true in their `AndroidManifest.xml` file:

```
  <application ... android:largeHeap="true" ...>
```

If the `ActivityManager.getLargeMemoryClass` method works on the device on which your game is running, you should have that amount of heap, but if it does not work (i.e. it throws an exception most likely due to the method not existing), then you should expect that you are running on a pre-3.0 Android device and you only have the normal heap size.

Based on your memory class, you can make decisions at runtime, such as to whether to [down sample your images](AndroidDownSampling.md) or to make other memory saving measures.