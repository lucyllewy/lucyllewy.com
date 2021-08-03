---
title: "Kubernetes tutorials"
date: 2021-08-03T16:21:21+01:00
draft: false
---

## Motivation

Regular Twitch streamer, [Jeff Fritz (csharpfritz)](https://twitch.tv/csharpfritz) has been trying to configure a Kubernetes cluster using [K3s by Rancher Labs](https://k3s.io/) installation on six Raspberry Pis. They hit multiple roadblocks, such as trying to set up NFS-based Persistent Volumes in the cluster shared between the nodes, and accessing the dashboard. I took their frustration as a prompt to write up some tutorials that explain everything you need and why.

## Pre-requisites

First, we need our cluster to have Linux of some kind installed. I will assume that we've got a cluster of three Ubuntu Linux systems that have had no other changes.

## Caveats

You should replace my username and node names in the instructions on these pages. For reference, my username is `dllewellyn`, and my nodes are named as follows:

- k8s-1
- k8s-2
- k8s-3

## Chapters

{{< series "kubernetes" >}}
