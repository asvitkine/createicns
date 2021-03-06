# createicns

This is a generator for the Apple Icon Image Format (.icns) which reads PNGs
without changing them.

It's run like this: `createicons x.iconset` and outputs a file `x.icns`.

The input is a .iconset directory with files conforming to the naming scheme
for .iconset directories. It reads a 'complete' set of PNG icons as described
here:
<https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html>
Not all icons in the list have to be present, but only icons with names from
the list are processed by `createicns`.

To generate a .iconset directory from an existing x.icns file, use
`readicns x.icns`

`createicns` is similar to running `iconutil -c icns x.iconset`, except it
doesn't change the PNG images in any way.

## Installation

`createicns` and `readicns` have only been tested on macOS. Use
`make createicns` and `make readicns` to compile them.

## Optimizing an icon set

Say you have a 'complete' set of PNG icons, to make sure your icons look
beautiful in all environments, on retina and non-retina. But the PNG files
are much bigger than you'd like them to be, and your set is taking up
more than 500KB. Luckily their are various (lossy) ways to optimize PNG
files, including dithering to reduce the color space. Unfortunately the
Apple tool `iconutil` will happily convert your optimized PNGs back to
32-bit when you run it.

Here's an example of optimizing an iconset using `pngquant`, which you
can find at <https://pngquant.org> (lines starting with '$' are commands
to enter):

* Our existing icon set is stored in `icons.icns`. Let's extract it.
  * `$ readicns icons.icns`
  * You should now have a folder `icons.iconset` whose contents look like this:
```
$ ls -l icons.iconset
total 1224
-rw-r--r--  1 user01  staff   15903 Jan 17 09:21 icon_128x128.png
-rw-r--r--  1 user01  staff   41182 Jan 17 09:21 icon_128x128@2x.png
-rw-r--r--  1 user01  staff    2462 Jan 17 09:21 icon_16x16@2x.png
-rw-r--r--  1 user01  staff   41182 Jan 17 09:21 icon_256x256.png
-rw-r--r--  1 user01  staff   99938 Jan 17 09:21 icon_256x256@2x.png
-rw-r--r--  1 user01  staff    6070 Jan 17 09:21 icon_32x32@2x.png
-rw-r--r--  1 user01  staff   99938 Jan 17 09:21 icon_512x512.png
-rw-r--r--  1 user01  staff  292329 Jan 17 09:21 icon_512x512@2x.png
-rw-r--r--  1 user01  staff    1826 Jan 17 14:11 icon_data_il32
-rw-r--r--  1 user01  staff     579 Jan 17 14:11 icon_data_is32
-rw-r--r--  1 user01  staff    1024 Jan 17 14:11 icon_data_l8mk
-rw-r--r--  1 user01  staff     256 Jan 17 14:11 icon_data_s8mk
```
* Optimize the PNGs using `pngquant`
  * `$ cd icons.iconset`
  * `$ pngquant --ext .png --force *`
  * The contents of the folder should now have smaller files:
```
$ ls -l icons.iconset
total 408
-rw-r--r--  1 user01  staff   6355 Jan 17 09:25 icon_128x128.png
-rw-r--r--  1 user01  staff  13901 Jan 17 09:25 icon_128x128@2x.png
-rw-r--r--  1 user01  staff   1782 Jan 17 09:25 icon_16x16@2x.png
-rw-r--r--  1 user01  staff  13901 Jan 17 09:25 icon_256x256.png
-rw-r--r--  1 user01  staff  31002 Jan 17 09:25 icon_256x256@2x.png
-rw-r--r--  1 user01  staff   3030 Jan 17 09:25 icon_32x32@2x.png
-rw-r--r--  1 user01  staff  31002 Jan 17 09:25 icon_512x512.png
-rw-r--r--  1 user01  staff  82188 Jan 17 09:25 icon_512x512@2x.png
-rw-r--r--  1 user01  staff   1826 Jan 17 14:11 icon_data_il32
-rw-r--r--  1 user01  staff    579 Jan 17 14:11 icon_data_is32
-rw-r--r--  1 user01  staff   1024 Jan 17 14:11 icon_data_l8mk
-rw-r--r--  1 user01  staff    256 Jan 17 14:11 icon_data_s8mk
```
* Run `createicns` to get them back into `icons.icns`
   * `$ createicns icons.iconset`
   * Open `icons.icns` using `Preview.app` to make sure the icons are
     there and look OK.
