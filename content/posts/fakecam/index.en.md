---
title: "Fakecam Open Source Virtual Background"
excerpt: "Benjamin Elder wrote a post about their Open Source Virtual Background. I expanded on the app by creating a nice UI to allow easy configuration and use. However, performance was bad. Very bad. Let's go over what I did to improve it"
date: 2020-06-11 01:58:42
published: true
categories: [development, ubuntu-snapcraft]
tags: [ai, Tensorflow, webcam]
cover_image: /wp-content/uploads/2020/06/fakecam-facebook-img.jpg
featuredImage: /wp-content/uploads/2020/06/fakecam-facebook-img.jpg
wpid: 10749
---

In a post by Benjamin Elder there is a really nice proof-of-concept app. This app is based on the Bodypix.js AI/ML library. Ben's article about their [Open Source Virtual Background](https://elder.dev/posts/open-source-virtual-background/) explains the inner workings. Using Ben's code, I created a [Snap Package called Fakecam](https://snapstats.org/snaps/fakecam). I also include a nice User Interface, which allows easy configuration of the options.

![Screenshot of Fakecam user interface](/wp-content/uploads/2020/06/image.png)

Performance issues
------------------

Next, I convinced several members of the [Ubuntu Podcast](https://ubuntupodcast.org/) Telegram channel to give Fakecam a test run. Among those members is [Stuart Langridge](https://kryogenix.org/). When testing, Stuart finds that the speed on their machine is woefully bad. It turns out that using [TensorflowJS](https://www.tensorflow.org/js) in [NodeJS](https://nodejs.org) is slow. This is made worse by having to copy the image between Python and NodeJS. This copy goes over an HTTP link, slowing everything down. The result is a very slow pictures.

First attempt to fix
--------------------

TensorflowJS is sped up if you have an nVidia graphics card with the CUDA API. However, I feel that if I rely on this requirement it would severely limit the scope of the app. After trawling through the [TensorflowJS GitHub](https://github.com/tensorflow/tfjs) repository I see a somewhat interesting area. There is code to use a WebGL-based API. This, when running inside a ReactNative app.

ReactNative is made to run on mobile devices, which often don't use nVidia graphics. So using nVidia CUDA just is not good enough. The code I found in [`platform_react_native.ts`](https://github.com/tensorflow/tfjs/blob/master/tfjs-react-native/src/platform_react_native.ts) uses OpenGLES. It extends TensorflowJS' already working WebGL code. With this idea, can I also use a native OpenGL stack with NodeJS? That means I can use WebGL on a desktop or laptop?

With a lot of effort, and lots of Cola and Coffee, I work through several evenings until morning. Now, the NodeJS-based TensorflowJS talks to my AMD graphics device via OpenGL. The speed is faster on my rather powerful desktop PC by a large margin. To do this, I copy the ReactNative Platform abstraction for TensorflowJS. This gives me a [TensorflowJS platform-library for WebGL in NodeJS is in my Fakecam GitHub repository](https://github.com/diddlesnaps/fakecam/tree/e4cf2629baee9d5978cbc0f9580c1cbc025f4e9a/bodypix/node-webgl).

Fakecam works well now?
-----------------------

Well not quite. I gave the new version of Fakecam back to Stuart who ran it on their laptop again. While the speed is not as bad as it was, Stuart still finds this version is slow. The image is just too slow to be usable. Oh dear, back to the drawing board.

Second attempt...
---------------

The Python side of this app is using [OpenCV](https://opencv.org/), which also supports running AI models. At the moment we're using [Bodypix.js](https://github.com/tensorflow/tfjs-models/tree/master/body-pix) with TensorflowJS. So, the next logical step, then, is remove the NodeJS side entirely. That way the images stay in the Python app. This removes the copying between Node and Python via HTTP.

Hold your horses, it's not that easy.

I converted the Bodypix.js model to a more standard [Tensorflow](https://www.tensorflow.org) model. But, giving this to OpenCV it complains that it can't work the model. OpenCV's support for part of the AI model is not complete in the current release. I am a good community member so I wrote [a Bug on the OpenCV GitHub page](https://github.com/opencv/opencv/issues/17243). After a few days the authors have [a fix added to the OpenCV code](https://github.com/opencv/opencv/commit/ba3cf4760069b53b2388155e15b095e0897d9659) that makes the model work.

SUCCESS
-------

Finally, with the new OpenCV code from the Bleeding Edge, I can load the Bodypix AI model. Continuing, I re-write the last javascript code as Python. But, the output from OpenCV, and Tensorflow, have different shapes. So, this takes a lot of guessing to find out.

So, now, I load the images into any available hardware. And, with the images in the hardware, I keep them there until the end. Then, I pull the images back out and put them into the [Video4Linux](https://lwn.net/Articles/203924/) loop-back device. Now, apps such as [Zoom](https://snapstats.org/snaps/zoom-client) or [Skype](https://snapstats.org/snaps/skype) show the new video as an option.

Fakecam version 2.0!
--------------------

Hooray! This new version is even faster than the interim version. It works *much* faster than the original version. And there is another side-effect. The Python-only version cuts-out the background more accurately! Win-win!

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/fakecam)
