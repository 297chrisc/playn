There are three main concepts in PlayN's graphics architecture: `Layer`,
`Surface`, and `Canvas`. All drawing is performed in one of these three
objects.


## Layers ##

The outermost part of the system is the `Layer`. The graphics system contains a
hierarchy of layers that is rendered automatically by the system. Take the
following example:

```
public void init() {
  Image bg = assetManager().getImage("images/bg.png");
  ImageLayer bgLayer = graphics().createImageLayer(bg);
  graphics().rootLayer().add(bgLayer);

  Image pea = assetManager().getImage("images/pea.png");
  ImageLayer peaLayer = graphics().createImageLayer(pea);
  peaLayer.rotate((float) (Math.PI / 4));
  graphics().rootLayer().add(peaLayer);
}
```

This creates a simple two-layer structure, with a background image, and a
smaller image hovering above it (and rotated 45 degrees). The resulting layer
hierarchy is as follows:

  * RootLayer
    * ImageLayer (bg)
    * ImageLayer (pea)

Each layer has an affine transform and several other associated properties,
which can be manipulated directly (changes take effect automatically on the
next rendered frame). In addition to `ImageLayer`, there is also a `GroupLayer`
for binding other layers together into a group (this is particularly useful
when you want a set of layers to move together, share the same alpha value, and
so forth), along with `ImmediateLayer`, and `SurfaceLayer`.


## ImmediateLayer and SurfaceLayer ##

While layers can be convenient, it is often preferable to be able to render
part of a scene directly, using an imperative API. `ImmediateLayer` and
`SurfaceLayer` allow you to do this easily.

The primary difference between `ImmediateLayer` and `SurfaceLayer` is that
`ImmediateLayer` renders directly into the framebuffer, whereas `SurfaceLayer`
creates an off-screen buffer (in GPU memory) and renders into that buffer. The
off-screen buffer is then copied into the frame buffer on every frame.

Thus, if you will be clearing and recreating your layer every frame, you should
use the `ImmediateLayer` and avoid the extra memory usage and extra copy. But
if you will only be updating your layer periodically (every 10 frames, for
example), then you may find the `SurfaceLayer` to be more performant.

Because the `ImmediateLayer` renders directly into the frame buffer, your game
does not control when the rendering is performed. So you must provide a
callback when creating your `ImmediateLayer` that will perform the rendering.
The PlayN rendering engine will then invoke your callback at the appropriate
time so that your `ImmediateLayer` is rendered with the proper transform and at
the proper depth.

Here is an example of an `ImmediateLayer` in use:

```
public void init() {
  final Image pea = assetManager().getImage("images/pea.png");
  ImmediateLayer imm = graphics().createImmediateLayer(WIDTH, HEIGHT, new ImmediateLayer.Renderer() {
    public void render(Surface surf) {
      surf.clear(0);
      for (int i = 0; i < 100; ++i) {
        int x = (int)(random() * WIDTH);
        int y = (int)(random() * HEIGHT);
        surf.drawImage(pea, x, y);
      }
    }
  });
  graphics().rootLayer().add(imm);
}
```

This creates an `ImmediateLayer` that has 100 peas drawn on it in random
locations every frame. Note that this layer can coexist peacefully with other
layers, and has the same properties as other layers (transform, alpha, and so
forth).


## Images and Canvases ##

The above examples use `Image` in a fairly straightforward way. In general, an
`Image` can be used as a source of bitmap data to be drawn into a `Surface` or
`Canvas`.

A `Canvas` is an interface that allows you to draw directly into an `Image`,
which itself will be used as a source for drawing calls.

The most common use of a `CanvasImage` is to render complex graphics ahead of
time into an image and then display that image in your scene graph using
`ImageLayer`. Here's a simple example:

```
public void init() {
  CanvasImage image = graphics().createImage(50, 50);
  Canvas canvas = image.canvas();
  canvas.setStrokeWidth(2);
  canvas.setStrokeColor(0xffff0000);
  canvas.strokeRect(1, 1, 46, 46);

  ImageLayer layer = graphics().createImageLayer(image);
  layer.setTranslation(25, 25);
  graphics().rootLayer().add(layer);
}
```

It is also possible to create a `CanvasImage` ahead of time and then draw it
into a `SurfaceLayer` or `ImmediateLayer`:

```
Image pea;
CanvasImage borderedPea;
SurfaceLayer surf;

public void init() {
  pea = assetManager().getImage("images/pea.png");
  borderedPea = graphics().createImage(48, 48);

  Canvas canvas = borderedPea.canvas();
  canvas.setStrokeWidth(2);
  canvas.setStrokeColor(0xffff0000);
  canvas.strokeRect(1, 1, 46, 46);
  canvas.drawImage(pea, 12, 12);

  surf = graphics().createSurfaceLayer(WIDTH, HEIGHT);
  graphics().rootLayer().add(surf);
}

public void paint() {
  surf.clear(0);
  surf.drawImage(borderedPea, 0, 0);
}
```

## The differences among `Layer`, `Canvas` and `Surface` ##

You may be asking yourself "How do I choose among `Layer`, `Canvas` and
`Surface`?" If your game structure fits naturally into layers, then it almost
always makes sense to use the `Layer` hierarchy directly. Layers will be
rendered in the most efficient way possible on each platform.

For imperative rendering (i.e., when you want to draw part of your scene
back-to-front by hand, as in the [Cute](Cute.md) sample), you can choose between
`Canvas` and `Surface`. The distinction is actually fairly simple. `Canvas` is
a full-featured 2D rendering API, with text, paths, curves, gradients, and so
forth. `Surface` is a much simpler API. The difference between the two is
performance -- `Canvas` makes no guarantees, and is indeed often quite slow;
`Surface` is explicitly designed to be easy to map to OpenGL calls, and can
thus be much faster.

A typical use-case for a `Canvas` is when you have more complex 2D rendering to
perform, but once it's done you can cache the results, using the `Canvas`
either directly within an `ImageLayer` or as a parameter to
`Surface.drawImage()`. This gives you the flexibility you need to render almost
anything, but still allows you to retain control over the speed of rendering.