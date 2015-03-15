PlayN supports the use of custom fonts, but such fonts need to be integrated specially for each of
the different backends that you wish to use in your game. TrueType (.ttf) fonts are supported by
all backends. OpenType (.otf) fonts are supported by some backends (iOS, HTML5, Android).

## Assets ##

You must place your custom font(s) in the `assets` submodule in your game. For our example here, we
will assume you are using a single custom font which you have placed in:

`assets/src/main/resources/assets/fonts/copperplate.ttf`

## Core ##

When you register your custom font with each of the backends, you will supply a name by which the
font can be accessed, except on iOS, which will read the name from the font file itself. If you
plan to use the iOS backend, then you must find out the name of the font embedded in the font file
and use that name on all platforms in order for your game to work properly.

Once your font is registered, you can use it in your cross-platform code by simply supplying the
name to `graphics().createFont()`:

```
  Font font = graphics().createFont("Copperplate", Font.Style.PLAIN, 18);
  // use font as normal
```

## Java ##

To use a custom font in the Java backend (which you will use for development and testing), add code
like the following to `MyGameJava.java` in your `java` submodule:

```
  public static void main(String[] args) {
    JavaPlatform platform = JavaPlatform.register();
    platform.graphics().registerFont("Copperplate", "fonts/copperplate.ttf");
    PlayN.run(new MyGame());
  }
```

## Android ##

To use a custom font in the Android backend, add code like the following to `MyGameActivity.java`
in your 'android' submodule:

```
  public void main() {
    platform().graphics().registerFont("fonts/copperplate.ttf, "Copperplate", Font.Style.PLAIN);
    PlayN.run(new MyGame());
  }
```

Versions of Android (prior to API level 19) contain a bug when handling ligatures (for example,
combining fi into a single glyph which combines the f and i), resulting in incorrect line breaking
and measurement. PlayN allows you to work around this bug by supplying a list of known ligature
strings when registering a font. For example:

```
  platform().graphics().registerFont("fonts/hobostd.otf", "HoboStd", Font.Style.PLAIN, "fi");
```

## iOS ##

To use a custom font in the iOS backend, you must add the fonts to the `Info.plist` file in the
`ios` submodule:

```
	<key>UIAppFonts</key>
	<array>
		<string>assets/fonts/copperplate.ttf</string>
	</array>
```

Note that the name of the font cannot be specified. It will be extracted from the font itself. You
can right click on a TTF or OTF file on a Mac and choose "Get Info" and in the info window will be
a "Full Name" entry which displays the name that you must use when referring to this font.

If you are making use of the iOS backend, then you should use this canonical name when you register
the font in all backends so that your cross-platform code can use a consistent name.

## HTML ##

To use a custom font in the HTML backend, you must add the fonts to your `MyGame.html` launcher,
which is in `html/src/main/webapp/MyGame.html`.

The fonts must be added to a stylesheet in the `<head>` block:

```
  <head>
    <title>My Game</title>
    <style>
      @font-face {
        font-family: "Copperplate";
        src: url(mygame/fonts/copperplate.ttf);
      }
    </style>
  </head>
```

and you may also want to cause the fonts to be pre-loaded, otherwise you may encounter issues the
first time you use the font in your game. This can be done like so:

```
  <body bgcolor="black">
    <!-- the contents of this div will be cleared when PlayN initializes -->
    <!-- the contents will be replaced with the PlayN <canvas> element -->
    <div id="playn-root">
      <!-- cause the browser to preload our custom fonts -->
      <span style="font-family: 'Copperplate'">.</span>
    </div>
    <script src="mygame/mygame.nocache.js"></script>
  </body>
```

Font metrics in HTML5 browsers are very limited. As a result, the heuristics that PlayN uses to
obtain font metrics may not work well for your custom fonts. PlayN provides a way to manually
specify the line height for your fonts:

```
  public class MyGameHtml extends HtmlGame {
    @Override
    public void start () {
      // force line height for ErinGoBragh plain 30pt to 27 pixels
      platform.graphics().registerFontMetrics("ErinGoBragh", Font.Style.PLAIN, 30, 27);
      // force line height for HoboStd plain 18pt to 20 pixels
      platform.graphics().registerFontMetrics("HoboStd", Font.Style.PLAIN, 18, 20);
      PlayN.run(new MyGame());
    }
  }
```

## Variants (Bold, Italic, etc.) ##

When using a custom font, you must register font variants manually and treat them all as
`Font.Style.PLAIN` from PlayN's perspective. Though Java can automatically attempt to generate bold
or italic versions of a font from a plain file, the results are ugly and would make a font designer
roll over in their grave. Other platforms don't even attempt to automatically generate variants.

Instead, register your variants with separate names and tell PlayN to use the `PLAIN` version of
the font. For example, the following code registers plain, bold and italic variants on the Java
backend:

```
    platform.graphics().registerFont("Copperplate", "fonts/copperplate.ttf");
    platform.graphics().registerFont("Copperplate Bold", "fonts/copperplate-bold.ttf");
    platform.graphics().registerFont("Copperplate Italic", "fonts/copperplate-ital.ttf");
```

and then in your cross-platform game code, you would use the fonts like so:

```
   Font cpPlain = graphics().createFont("Copperplate", Font.Style.PLAIN, 18);
   Font cpBold = graphics().createFont("Copperplate Bold", Font.Style.PLAIN, 18);
   Font cpItal = graphics().createFont("Copperplate Italic", Font.Style.PLAIN, 18);
```