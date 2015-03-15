Android supports a wide range of devices vary widely in the available memory and processing power which are available to your game. Invariably, there will be a device out there on which your game runs poorly, and this article describes one technique that can help.

To determine whether or not you _need_ to downsample images, you should read [AndroidMemoryClass](AndroidMemoryClass.md).

## Down-sampling image assets ##

Android provides a mechanism for automatically down-sampling images when they are loaded, which will make them look less good, but also make them take up 1/4th or less memory. PlayN provides a way to trigger this downsampling, and also allows you to perform downsampling only on "large" images or images which meet whatever criteria you wish to enforce.

It is also possible to "downsample" the color range available to an image by using two bytes per pixel, in `RGB_565` configuration, rather than a full byte for each of the red, green, blue and alpha channels (aka `ARGB_8888`).

The following sample code demonstrates both of these mechanisms. This code goes into `YourGameActivity.java`:

```
    @Override public void main () {
        // ...
        final boolean shouldDownsample = ...;
        platform().assets().setBitmapOptionsAdjuster(new AndroidAssets.BitmapOptionsAdjuster() {
            @Override public void adjustOptions (String path, AndroidAssets.BitmapOptions options) {
                // use a 16-bit per pixel format for JPGs; looks decent, saves memory
                if (path.endsWith(".jpg")) {
                    options.inPreferredConfig = Bitmap.Config.RGB_565;
                }
                if (shouldDownsample) {
                    options.inSampleSize = 2;
                    options.scale = Scale.ONE;
                }
            }
        });
        // ...
    }
```

By setting `inSampleSize` to 2, that indicates that every 2x2 pixel block in the source image should be represented in memory by a 1x1 pixel block. This reduces memory usage by 75%, but can make the images look very grainy. Thus a game may wish to indicate in the filename of the image (available via the `path` argument) whether the image is a candidate for downsampling on low-memory platforms, and only downsample those images.

The setting of `options.scale` to `Scale.ONE` is necessary if you are using [HiDPISupport](HiDPISupport.md) and are loading an `@2x` image which would normally have scale 2, but will now have scale 1 because it is being downsampled.

## Reducing Memory Use of `Canvas` images ##

If you are using [HiDPISupport](HiDPISupport.md), your `CanvasImage` images are automatically created at HiDPI resolutions. Some Android devices support high-resolution screens, but don't provide enough memory for games to comfortably use those high-resolution screens at full resolution. In such cases, one can reduce the scale of the `CanvasImage` bitmaps created in the game.

While one could manually create images of a smaller size and manually apply scale factors, PlayN provides a way to have this done automatically, so your code is written as if nothing is down-sampled and everything operates at the same scale, but for canvas images that meet certain criteria (like being very large), the down-sampling is automatically performed.

The following code (which goes into `YourGameActivity.java`) demonstrates this feature:

```
    @Override public void main () {
        // ...
        // the dimensions of our view in logical/resolution-independent pixels
        float logicalWidth = 480, logicalHeight = 800;
        // we assume this code is running on a HiDPI device, and thus the "native" graphics scale
        // (passed in as gfxScale) is higher than 1, most likely 2
        platform().graphics().setCanvasScaleFunc(new AndroidGraphics.ScaleFunc() {
            public Scale computeScale (float width, float height, Scale gfxScale) {
                // if the image is 1/2 the logical screen or larger, downsample
                return (width*height > logicalWidth*logicalHeight/2) ? Scale.ONE : gfxScale;
            }
        });
        // ...
    }
```

The above code might be running on a device with a 960x1600 HiDPI screen, which maps to 480x800 logical pixels, at a scale factor of 2. When creating a large canvas image, the above code will downsample the underlying bitmap to scale 1, meaning a 480x800 canvas image will only actually use 480x800 pixels rather than 960\*1600 pixels, and the image will be scaled up (in hardware) when actually drawn to the screen.