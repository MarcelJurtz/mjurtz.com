---
id: 504
title: Touch Controls for Unity Game Development
date: 2017-11-15T18:00:46+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=504
permalink: /2017/11/touch-controls-unity-game-development/
categories:
  - android
  - game-development
tags:
  - dotnet
  - android
  - app
  - csharp
  - development
  - gamedev
  - programming
  - unity
---
Some time ago, I created [this game](https://github.com/MarcelJurtz/Escape) ([online playable version](https://marceljurtz.github.io/Escape-Web/)) in Unity, and I wanted to be able to play it on my Android phone. Since the input options on the smartphone are slightly different from those on the pc, a certain amount of adjustments were necessary. I won&#8217;t go over the specific details on the game mechanics, you can check out these on GitHub, they are described in the README file. However, the basic concept of the game is  the movement of a cube to one of the directions up, down, left or right. The information about which direction to use is displayed on the screen.

On a PC, you would use your arrow keys to move the cube. Using Android, you don&#8217;t have any keys available. Instead, there is a touchscreen. So you need a way to receive information about user interaction on the touchscreen. Therefore, I&#8217;ve created the script _GestureRecognizer_, which can be seen in the listing below.

The script fetches user input at the first contact, which is saved in the _isTouching_ boolean. The script checks every frame, if the user is touching the screen. If so, it also checks if the user has moved the position on the screen. I use _Input.touches_ for this, since it is an array which contains information about the movement on the screen. All that is left to do now is to check, in which direction the user moved his finger.

However, the immediate evaluation is difficult, as the player will often carry out a minimal movement in a different direction than the intended one (just try it out). For this reason, I have defined a minimum distance that must be exceeded before the script actually communicates the movement. The current movements distance is saved in _SwipeDelta_, the minimal distance is saved in _SWIPE_TOLERANCE_, which is assigned 200 units.

{% highlight c# %}
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GestureRecognizer : MonoBehaviour {

    private bool tap, swipeLeft, swipeRight, swipeUp, swipeDown;
    private bool isTouching = false;
    private Vector2 swipeStart, swipeDelta;
    private const int SWIPE_TOLERANCE = 200;

    private void resetSwipePosition()
    {
        swipeStart = Vector2.zero;
        swipeDelta = Vector2.zero;
        isTouching = false;
    }

    public Vector2 SwipeDelta { get { return swipeDelta; } }
    public bool SwipeLeft { get { return swipeLeft; } }
    public bool SwipeRight { get { return swipeRight; } }
    public bool SwipeUp { get { return swipeUp; } }
    public bool SwipeDown { get { return swipeDown; } }
    public bool Tap { get { return tap; } }

    private void Update()
    {
        tap = false;
        swipeLeft = false;
        swipeRight = false;
        swipeUp = false;
        swipeDown = false;

        // Mobile Inputs
        if(Input.touches.Length > 0)
        {
            if(Input.touches[0].phase == TouchPhase.Began)
            {
                tap = true;
                isTouching = true;
                swipeStart = Input.touches[0].position;
            }
            else if(Input.touches[0].phase == TouchPhase.Ended || Input.touches[0].phase == TouchPhase.Ended)
            {
                isTouching = false;
                resetSwipePosition();
            }
        }

        // Distance Calculation
        swipeDelta = Vector2.zero;
        if(isTouching && Input.touches.Length > 0)
        {
            swipeDelta = Input.touches[0].position - swipeStart;         
        }

        // Swipe Recognition
        if(swipeDelta.magnitude > SWIPE_TOLERANCE)
        {
            float x = swipeDelta.x;
            float y = swipeDelta.y;

            if(Mathf.Abs(x) > Mathf.Abs(y))
            {
                // Left or Right
                if(x < 0)
                {
                    swipeLeft = true;
                }
                else
                {
                    swipeRight = true;
                }
            }
            else
            {
                // Up or Down
                if (y < 0)
                {
                    swipeDown = true;
                }
                else
                {
                    swipeUp = true;
                }
            }

            resetSwipePosition();
        }
    }
}
{% endhighlight %}

To be able to move the cube with this script, it needs to be added to the Update() method of the cubes controller script. To support both touchscreen and keyboards, I simply add an _or_ operator to the if statement:

{% highlight c# %}
if (Input.GetKeyDown(KeyCode.UpArrow) || swipeControls.SwipeUp)
{
    // ...
}
{% endhighlight %}

SwipeControls is simply an instance of the above created _GestureRecognizer_ class.

And that&#8217;s basically it! To release a game for android, you have to select the _Android_ entry under _File_ &#8211; _Build Settings_ and then click _Build_. If you have an emulator running or your phone (with USB-debugging enabled) connected to your PC, you can directly run the game on the device. Note, that for building apps for Android, you&#8217;ll need to have the Android SDK installed, which can be downloaded directly in Unity.
