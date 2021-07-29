---
title: "Awesomify anything or anyone with OpenFaaS!"
date: 2019-01-17 17:54:23
published: true
categories: [development]
tags: [openfaas, awesome, serverless]
wpid: 10447
---

While I've been lurking about the [OpenFaaS](https://www.openfaas.com/) Community I haven't really had the wherewithal to actually get myself knuckled-down to build something that might be classed as useful or fun. To remedy that I finally came up with a new idea for a function that I can publish into the FaaS Store. I'm calling this function "Awesomify". It will take any text you throw at it and make it awesome!

![A photo of a sign pointing to 'awesome' on the right, and 'less awesome' on the left](jon-tyson-unknown.jpg)

Photo by [Jon Tyson](https://unsplash.com/@jontyson) on [Unsplash](https://unsplash.com)

First thing I need is a [Kubernetes](https://kubernetes.io/) cluster. After a quick trip down to [Marks and Sparks](https://www.marksandspencer.com/) to fill my trolly with juicy servers, I remembered that this is supposed to be serverless computing, plus M&S is a clothing store so I wouldn't get servers there anyway. Putting the servers back on the shelves I instead fired up [Google Cloud](https://cloud.google.com/)'s dashboard and ordered some Kubernetes with a side order of Chips (Fries for folk over the pond).

Following the OpenFaaS documentation and my serverless system is now fully deployed, and it was easy (mostly, I had a couple of hiccups from following the docs too closely which I've filed bugs about so they should get resolved soon).

The function
------------

Now to my function. Which language or framework should I use to build it? [NodeJS](https://nodejs.org/)? Pass. [Go](https://golang.org/)? Hmm, interesting idea, but nah. [BASH](https://tiswww.case.edu/php/chet/bash/bashtop.html)? Now you're talking; you can't get more esoteric than that! Let's write an entire function using BASH...

![Photo of a laptop showing a text editor with computer source code](luca-bravo-XJXWbfSo2f0-unsplash.jpg)

Photo by [Luca Bravo](https://unsplash.com/@lucabravo) on [Unsplash](https://unsplash.com)

For my function to work, I need some sound clips. So pulling out my scissors I found a cassette of Tegan and Sara's greatest hits and started hacking away. After 36 hours of toil, I finally had a series of clips that are usable and a large pile of wasted audio tape.

The only thing missing now is a voice generator. While I could use a cloudy serverless service such as [Amazon's Polly](https://aws.amazon.com/polly/) or [Google's Cloud TTS](https://cloud.google.com/text-to-speech/) I decided that open source was the way forward. There's a little-known project called [Mycroft](https://mycroft.ai/) which creates a completely open source Intelligent Assistant like Amazon Echo, Google Assistant, or Apple Siri. They have released a piece of code they named "[Mimic](https://mimic.mycroft.ai/)" to perform text to speech duties.

> As an aside, my good friend [Alan Pope](https://popey.com/) from [Canonical](https://canonical.com/) lent his voice to the Mimic TTS engine and the Mycroft assistant so this choice of engine was even more fun for me.

With the function taking shape I felt ready to publish a trial run onto my test cluster on Google Cloud. Two hours after publishing it and tweeting a few times the function has been hit 396 times and nobody has complained since I increased the timeouts and minimum instance count from their defaults.

![Photo of an athlete getting ready to run in a sprint race](william-stitt-unknown.jpg)

Photo by [William Stitt](https://unsplash.com/@willpower) on [Unsplash](https://unsplash.com)

And we're off to the races!
---------------------------

A Pull Request is now into the OpenFaaS Store GitHub repository to add the function for anybody to use with a simple clickity-doodle. If you don't have or want your own instance of the function you may use the test that is still operational. You just need to point your web browser to <https://openfaas.bowlhat.net/function/awesomify?q=OpenFaaS>. By changing the text after the "?" you can tailor what is awesome to your own needs. (Make sure you replace spaces in the text with a + symbol because addresses need to be "URL Encoded")

A few examples I've tried are:

- <https://openfaas.bowlhat.net/function/awesomify?q=The+Ubuntu+Podcast>
- <https://openfaas.bowlhat.net/function/awesomify?q=You+are>
- and of course, I had to add one for my friend whose voice I used <https://openfaas.bowlhat.net/function/awesomify?q=Alan+Pope>

- - - - - -

[My source code is published at GitHub](https://github.com/diddledani/awesomify/).