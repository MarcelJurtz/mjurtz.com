Blockchains and Cryptocurrency have been around for a while now and a lot of people are familiar with the basic concepts of them. However, how would you start implementing the concept for yourself?
I thought about that for a while and decided to share my (minimalistic) approach. Of course, current technologies like Ethereum have way more complex features, but this should be sufficient to get a grasp of the basics.

## Introduction

This post is not supposed to go into the details on blockchains in general. I'll cover some of the basics while implementing them, but you might look the topics up for yourself to read into further details.

From a minimalist point of view, a blockchain is - as the name suggests - a chain of blocks. A block is an object that contains a timestamp, transaction data and a hash. It also contains the previous blocks hash in order to indicate the order of blocks in the chain.
These basics are the points I'm going to implement walking through this article.

I'm using C# / .NET 6 to implement the code samples as a console application, but feel free to code along in any language you prefer.

## Conclusion

Did I forgot about something or do you think I've made a mistake in some point? Please let me know, either message me directly or feel free to create an issue on the GitHub-Repo!
