---
title: "A bug in the wild"
description: A postcode panic
image: http://jpie.nz/assets/images/2020/02/2020-02-23-logo.png
---
# {{page.title}}
## {{page.description}}


#### My wife's birthday
<img src="/assets/images/2020/02/2020-02-23-01.png" class="right" width="200px">
It was my wife's birthday recently, and so I bought her an item she wanted from a popular online store.

I'm getting the item delivered to our home in Papatoetoe, so I put in the address details:

Here's a screenshot:

(It's worth noting it's got one of those auto-completey address pickers. I have mixed feelings about them.)
<img src="/assets/images/2020/02/2020-02-23-bug-01.png" class="center" width="460px">



Anyway, the shopping cart experience was pretty normal, I clicked the 'buy' button, and waited.

#### An alarming email
A few minutes later, I got an 'order being processed' email.
The address seemed a bit funky:

<img src="/assets/images/2020/02/2020-02-23-bug-02.png" class="center" width="460px">

<img src="/assets/images/2020/02/2020-02-23-02.png" class="right hideonmobile" width="280px">
Wait a minute...**the suburb is missing from the email!**


It still has the postcode, which means it will _probably_ get to the right place.

But:
* It's a gift for my wife, so I _really_ want it to arrive in a timely fashion. Address shenanigans could jeopardise this.
* If it _does_ get lost, well - have you ever tried to get NZPost to compensate you for lost mail? It's not straight forward.

So, **<font color="red">engage panic mode</font>**.
#### Help me, Marco

I reached out to their support team. Support is via live chat - again, mixed feelings.
Twelve minutes later, the support agent (Marco) confirmed:

> Marco: I have looked and can see in the system that the address is what you have provided, though you may not see the suburb but it is in the system, it will still be delivered as addressed.

Whew. So - the address in the system is correct - it's just wrong in the email.
That's that, right?

#### Another email

Later that evening, another email, this time 'order shipped'.
<img src="/assets/images/2020/02/2020-02-23-bug-03.png" class="center" width="460px">

The address is wrong again! This time, the suburb is correct, but the city is missing!

At this point though, I'm not overly worried. Marco assured me the address was correct - but also, it's shipped now. There's nothing I can do. Here's hoping.

And, sure enough, the package arrived the next day - addressed _pretty much_ correctly.
<img src="/assets/images/2020/02/2020-02-23-bug-04.jpg" class="center" width="460px">

#### It still turned up... so... 

<img src="/assets/images/2020/02/2020-02-23-03.png" class="right" width="280px">
The package arrived in time. So - it's not a problem?

I dunno. It demonstrates that there's a **problem with the way this site is presenting address data**. Possibly even they way they're storing it.
It's not consistent across the product - that smells. Maybe there's some technical debt to be paid down?

But also. It was **twelve minutes of a support persons time**.
If five customers hit this issue, and reach out to support, that's a whole hour gone. Forty people, that's a whole day.
Given this particular site makes over one thousand sales a day - that's entirely possible. 
_Someone_ is having their time wasted, and this will ultimately be a cost to the business.

I guess it's an example of how a minor bug could (potentially) be a major cost!

Thanks for reading!