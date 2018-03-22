---
id: 639
title: 'Sideproject: CryptoFolio'
date: 2017-12-10T14:24:38+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=639
permalink: /2017/12/sideproject-cryptofolio/
categories:
  - Xamarin
tags:
  - .NET
  - Android
  - App
  - Bitcoin
  - Cryptocurrency
  - CSharp
  - Development
  - Programming
  - ServiceStack
  - Sideproject
  - Xamarin
---
Today I would like to introduce my current side project. I&#8217;m currently developing [_CryptoFolio_](https://github.com/MarcelJurtz/CryptoFolio), an app that allows you to keep track of your investments in cryptocurrencies.

![Cryptocoin Logos](/assets/2017/cryptofolio.png)

## Functionality & MVP

The first release of the application will allow you to view the current rates of the top 100 cryptocurrencies with the highest market capitalization. You can then add investments, where input, output and the date of the transaction is recorded.

To address as many users as possible, I would like to support as many regular currencies as possible.

On the main page you will find an overview of all your assets, which are displayed in the form of a list for each existing currency with total in- and output. On the detail view of each currency, I would like to display a similar list, which shows the transaction history.

## Technologies and basic Thoughts

To implement the application I will work with Xamarin Forms. Basically, I prefer building native apps, but since I&#8217;m currently trying out Xamarin at work. Also, I&#8217;d like to gain new experiences, so I&#8217;ll try to realize the project using this technology.

However, I need to answer some technological questions before starting development. Android offers _SharedPreferences_ to store user settings. In this app, that functionality could be used to store the currency the user wants to use per default.

Fortunately, there is a [plugin from James Montemagno](https://github.com/jamesmontemagno/SettingsPlugin) that implements this functionality for Xamarin Forms. The plugin is available under MIT-License and can be installed easily via NuGet.

While this question clarifies a small part of the implementation, there are certainly more important problems: The data on the exchange rates of the cryptocurrencies must be obtained from somewhere. Also, I&#8217;d like to store the data locally in order to prevent unneccessary repeated loading.

The probably best known website for collecting current exchange rates, [coinmarketcap.com](https://coinmarketcap.com/), provides an [API](https://coinmarketcap.com/api/), which seems to suite my idea just fine. To consume this API, I&#8217;d like to use ServiceStack, a framework I&#8217;ve already discussed in [several articles](https://blog.mjurtz.com/2017/11/what-is-servicestack/).

To store data locally, I want to use LiteDB. LiteDB is an Open Source NoSQL database, which allows to save JSON documents. This way, I can theoretically store the whole response objects which I receive from the API. LiteDB is also available under MIT license from [GitHub](https://github.com/mbdavid/LiteDB) and can be installed via NuGet.

The last question I want to cover before development concerns usability and GUI design. I would like to use icons for the different currencies, but I am absolutely useless in the design of logos or icons. Fortunately, there are several icon packs available on GitHub. I have decided to use the [Christopher Downers icon pack](https://github.com/cjdowner/cryptocurrency-icons), which he provides under CC0 license. His icons are available in both color and black and white, making them suitable for a wide range of applications.

## Plans for future Updates

Besides the first functions I plan to include more features in future releases. I am thinking here of the presentation of more detailed data, such as the price trend in the form of a diagram. Also interesting would be a kind of ticker, so that the rate data is updated on the go.