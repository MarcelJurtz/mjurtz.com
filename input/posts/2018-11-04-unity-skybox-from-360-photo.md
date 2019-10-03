---
title: 'Create Unity Skyboxes From 360° Images'
date: 2018-11-04T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/11/unity-skybox-from-360-photo
categories:
  - unity
tags:
  - game
  - vr
  - unity
  - programming
---

Today I want to show you how to use a 360° panorama picture as a skybox for Unity. As you may know, skyboxes are usually created from 6 images (cubemaps), with each image representing one of the sides. However, 360° photos usually use a so-called "Equirectangular" format, which looks something like this:
 
<a href="/assets/2018/equirectangular.png" data-lightbox="Equirectangular 360° Photo"><img src="/assets/2018/equirectangular.png"/></a>

So in order to be able to use such an image in Unity, the idea of creating a cubemap from a 360° photo is obvious. But there is an easier way, which I would like to show you now: Cubemap: I start with my image, which I import into Unity. Then I select it and change the *Texture Shape* property from 2D to Cube. I then save the changes by hitting Apply and then create a new material. I change the shader from default to Skybox - Cubemap and specify the image I want to use.

That's almost it, now you only have to set the Skybox via *Window - Lighting Settings* and already you have your image and can integrate it into your projects.