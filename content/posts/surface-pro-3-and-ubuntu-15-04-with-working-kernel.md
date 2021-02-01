+++
date = "2015-06-22T00:02:51-05:00"
title = "Surface Pro 3 and Ubuntu 15.04 with working kernel"
draft = false

+++

![surfacelinux](https://photos.marioharvey.com/file/marioharvey/media/photos/surface-linux.jpg)

Having Ubuntu 15.04 running on a Surface Pro 3 is pretty awesome. However, for it to work you need to compile your own patched kernel. That being said, I did the annoying work for you and have a provided a link to a patched kernel below.

**What works**

* battery
* power button
* volume buttons
* front/rear cam
* all known type covers

**What’s broke**

* multitouch/gestures
* finnicky touch screen
* finnicky pen

Here is the link to the kernel files for download:

[SurfacePro3-Ubuntu15.04-3.19.0-23-kernel](https://docs.google.com/uc?id=0B1U4Djb5-G7XTXAtdzJ6M2E0dlU&export=download)

To install just extract the tarball and run:

`sudo dpkg -i linux-headers* linux-image*`

If you want to build your own kernel you can follow a great guide here:

https://github.com/neoreeps/surface-pro-3

One thing to note is that you will also want to use a patch I created to make sure your keyboard works:

[Type Cover 3 Patch](https://github.com/badmadrad/surface-pro-3/blob/c2b7d8d05dde630c41b278c7997608501715c340/typecover.patch)

The reason is that there are 5 different Type Covers with different hard-coded product ids. The current 15.04 kernel only supports 1 out of the 5 known type covers. This patch will make sure that your type cover will work no matter what region or model.

Good luck and let me know if you have questions.
