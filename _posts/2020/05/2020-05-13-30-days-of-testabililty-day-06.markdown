---
title: Improving testability by using mixpanel for analytics
description: 30 days of Testability, day six
image: https://jpie.nz/assets/images/2020/05/2020-05-13-mixpanel.png
---
# {{page.title}}
## {{page.description}}

#### Analytics

Today's challenge is:
> Explore the output of your applications analytics, how can they help guide your testing?

[Mixpanel](https://mixpanel.com/) is our analytics tool of choice - it tells us a bunch about user behaviour.

I don't want to go and reveal all the analytics we have in Mixpanel. But, I thought I'd go ahead and learn how it's implemented - the same way I did for Prometheus in the last video.

So, I did just that! I've paired up with one of our developers, Bhumika Mishra, to go through some steps of a basic Mixpanel setup.

Here's the vid!

<hr/>

#### Setting up Mixpanel

<iframe width="560" height="315" src="https://www.youtube.com/embed/u4b6OF67ARw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

So, that's how you set up Mixpanel for server side and client side metrics. Easy! I find this useful from two angles:
* Knowing how Mixpanel analytics are implemented makes it easier to understand reading them, especially if something unusual is happening
* At some point, I'm going to have to test our Mixpanel analytics are giving the information we expect. If I know _how_ they work, it makes it less of a 'black box' when testing.

<hr/>

#### Mixpanel and testability

How does this feed into the subject of testability?

It feeds into testability by revealing user behaviour. So that when we are testing, we can attempt behave in the same manner.

The easiest example is that Mixpanel might reveal the most common devices users are on. So when testing, we can prioritise those devices.

Of course, the analytics can get much more complex. Which means they way they can influence testability can get much more complex too!

<hr/>

#### Thanks
As always, thanks for watching, and feedback always welcome!