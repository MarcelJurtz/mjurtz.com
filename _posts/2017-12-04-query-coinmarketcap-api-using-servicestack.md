---
id: 615
title: Query Coinmarketcap API using ServiceStack
date: 2017-12-04T21:45:36+00:00
author: Marcel Jurtz
layout: post
guid: https://blog.mjurtz.com/?p=615
permalink: /2017/12/query-coinmarketcap-api-using-servicestack/
categories:
  - CSharp
tags:
  - .NET
  - Bitcoin
  - Cryptocurrency
  - CSharp
  - Development
  - Programming
  - REST
  - ServiceStack
---
In the [first article of my series on using ServiceStack](https://blog.mjurtz.com/2017/11/what-is-servicestack/) I mentioned the feature of the framework that the individual components can be used independently of each other.

This short article is intended as an example of how the C# client can be used separately from a custom service to communicate with third-party APIs. If you would like to know more about the client specifically, you can read [my article on this feature](https://blog.mjurtz.com/2017/11/servicestack-using-the-c-client/).

## Idea &#8230;

I would like to use [coinmarketcap.com](https://coinmarketcap.com/)&#8216;s API to access statistics on different crypto currencies. The API documentation is available on the [projects website](https://coinmarketcap.com/api/). Within the scope of this article, there are two things I want to achieve: Querying all available currency data, which contains the 100 strongest crypto currencies in terms of turnover, as well as the targeted query of a single currency.

We receive all the necessary information from the docs. These are the paths of the service as well as the structure of the objects we&#8217;d like to receive.

I have decided to use Bitcoin as an example, which leads to the following two paths for use in the project:

  * [api.coinmarketcap.com/v1/ticker/bitcoin/](https://api.coinmarketcap.com/v1/ticker/bitcoin/)
  * [api.coinmarketcap.com/v1/ticker/](https://api.coinmarketcap.com/v1/ticker/)

## &#8230; And Execution

Since we have no influence on the objects we receive from api, we have to adapt a class accordingly. In order to be able to design the properties according to my own ideas, I use the attributes [DataContract] and [DataMember], so that the properties do not have to be named identically to those of the API.

{% highlight c# %}[DataContract]
public class CoinDTO
{
    [DataMember(Name = "id")]
    public String Id { get; set; }

    [DataMember(Name = "name")]
    public String Name { get; set; }

    [DataMember(Name = "symbol")]
    public String Symbol { get; set; }

    [DataMember(Name = "rank")]
    public String Rank { get; set; }

    [DataMember(Name = "price_usd")]
    public String PriceUsd { get; set; }

    [DataMember(Name = "price_btc")]
    public String PriceBtc { get; set; }

    [DataMember(Name = "24h_volume_usd")]
    public String VolumeUsd24h { get; set; }

    [DataMember(Name = "market_cap_usd")]
    public String MarketCapUsd { get; set; }

    [DataMember(Name = "available_supply")]
    public String AvailableSupply { get; set; }

    [DataMember(Name = "total_supply")]
    public String TotalSupply { get; set; }

    [DataMember(Name = "percent_change_1h")]
    public String PercentChange1h { get; set; }

    [DataMember(Name = "percent_change_24h")]
    public String PercentChange24h { get; set; }

    [DataMember(Name = "percent_change_7d")]
    public String PercentChange7d { get; set; }

    [DataMember(Name = "last_updated")]
    public String LastUpdated { get; set; }
}{% endhighlight %}

And that was actually the most complicated part. To obtain the data through the API, I directly specify the corresponding paths from the documentation, since we do not use request DTOs.

{% highlight c# %}
public class APIClient
{
    private const String SERVICE_URL = "https://api.coinmarketcap.com/v1/ticker/";

    public List<CoinDTO> GetAllCurrencies()
    {
        List<CoinDTO> response;
        using (var client = new JsonServiceClient(SERVICE_URL))
        {
            response = client.Get<List<CoinDTO>>("/");
        }
        return response;
    }

    public CoinDTO GetBitcoin()
    {
        List<CoinDTO> response;
        using (var client = new JsonServiceClient(SERVICE_URL))
        {
            response = client.Get<List<CoinDTO>>("/bitcoin");
        }
        return response[0];
    }
}{% endhighlight %}

Since the API always returns an array, I simply return the first element from the Bitcoin queries response. As promised, the implementation of an external service with the ServiceStack Client is quick and easy as usual. The concluding image shows the result of the query.

<img src="https://i1.wp.com/blog.mjurtz.com/wp-content/uploads/2017/12/API_BreakPoint.png?resize=497%2C409&#038;ssl=1" alt="ServiceStack Client - CoinMarketCap Response" width="497" height="409" class="aligncenter size-full wp-image-617" data-recalc-dims="1" />