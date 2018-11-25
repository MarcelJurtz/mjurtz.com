---
title: 'Announcing MTG LifeCounter V2'
date: 2018-11-25T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2018/11/announcing-lifecounter-v2
categories:
  - android
tags:
  - android
  - app
  - development
  - programming
  - sideproject
---

With my LifeCounter app for Magic The Gathering, I released my first Android app on the Google PlayStore over two years ago. Besides my first release, this was also another milestone for me: My first real app. Over two years later, I am proud to announce a completely revamped version of the app. Several complete revisions have taken place in the meantime and I'm already planning the next one, as I admittedly tried to learn the MVP pattern with the app and now, a few months later, I'm completely dissatisfied and want to switch to MVVM. But that doesn't really matter at the moment.

Today I announce the release of version 2.0, providing major improvements and enhancements. If the player only had the possibility to manage health points in a 2-player game so far, now up to four players can be served in parallel. In addition there are other features like a dice function for a 20-sided dice, a counter manager to organize counters on individual cards and the adjustment of various values such as the standard colors of the game areas.

I have decided to offer a paid Pro version in addition to the standard version. Quite simply for the reason that in the meantime a lot of work went into the creation of the app and I would like to compensate at least parts of my expenses with it again. V2 will be released in the PlayStore in the course of the next week.

## 4 Player Mode

Probably the biggest innovation of the app is the 4-player mode. The mode can be started via the menu which can be dragged sideways into the app. The view adapts to the screen size. By default, for example, the color selection is displayed above the life points. If the screen of the device is too small, the points are hidden during the customization, so that a certain clarity remains ensured.

Game scores are saved in both game modes, so opening and closing the app, as well as switching between 2 and 4 player modes, will not result in any loss of scores. The 4-player mode is only available in the Pro version.

## Counter Manager

Another major innovation is the Counter Manager. You are probably familiar with the problem that tokens to change the attack and defense values of your cards are documented in a complicated manner using a tally sheet. This new feature allows you to manage the values of your cards in a simple and intuitive way. These values will also be saved when you exit the application, so you won't accidentally delete any entries. This feature is also only available in the pro version.

## Dice

A rather small innovation is the dice function. Touching the screen displays a random number between one and 20, simulating a 20-sided dice.

## Energy Saving Mode

Already implemented in earlier app versions, the power-saving mode provides a way for devices with AMOLED technology to darken the large areas during the game. This means the device consumes less power because black pixels do not consume energy.

## Introductory Screen

Finally, I'd like to briefly discuss another feature that I can provide thanks to Paolo Rotolo's AppIntro library. The first time you start the application, you will see an introduction that demonstrates how to use the app and introduces you to the various features. Optionally, this introduction can be recalled from the Preferences pane.

## Questions? Answers!

You have found suggestions for improvements, ideas, or perhaps an error in the application? I'm always happy to hear from you, preferably by sending an email to feedback@mjurtz.com or via Twitter: @MarcelJurtz. And of course I'm always happy about every review in the PlayStore!