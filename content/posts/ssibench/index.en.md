---
title: "SSIBench"
date: 2015-10-27 21:09:23
published: true
categories: [sysadmin]
wpid: 506
---

It's been a while since I had this available online, and I've just found my backup copy.

SSIBench is a parallel processing benchmark test tool and framework. It allows you to create your own benchmark routines using a simple structure. Written in perl, the tool by default runs through ten threads of 5 million calculations each. The whole point of the process is that it is multithreaded so that newer dual and quad core processors (CPUs) can be tested for their performance.

Originally I developed it to test beowulf-style clusters, but the technique can easily be applied to any SMP (symmetric multi-processing) system such as modern servers with more than one processor and desktops with multi-core processors.

I wrote the tool to be fully extendible, and have only included a very basic test to begin with. It it my hope that other more capable developers will take the code and design their own calculations. As it's written in perl, plugin modules should be able to hook into C libraries and programs for complete customisability. e.g. ssibench -> plugin -> c program -> plugin -> ssibench.

My intel core i7 processor takes 10 threads with loop values of 1000 & 1000 and completes everything in 1.644089 seconds. Deadpan110's core2quad does the same in 2.162209 seconds.

[ssibench-1.0](/wp-content/uploads/2009/05/ssibench-10.zip)