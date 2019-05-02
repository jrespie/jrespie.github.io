# A bug in the wild
## Paying a zero dollar bill

##### I want an e-scooter!
I love finding bugs out in the real world. Here's one that cropped up last week.

One of our utility providers is running a promotion. Pay your bill using the mobile app, and you could win an e-scooter!

I *really* want an e-scooter. So, I downloaded the app and tried to pay my bill!

Thing is, my current balance is **$0.00**.

But I *really* want an e-scooter, so, I tried to pay my zero dollar bill. Hopefully this gif is clear enough to show you what happened!

<img src="/assets/images/2019/05/2019-05-02-paymentbug.gif" class="center" width="280px">

It crashes if you try and pay zero dollars! Oh no!

##### Nobody's going to do that anyway
<img src="/assets/images/2019/05/2019-05-02-01.jpg" class="left" width="280px">
I don't know anything about the development of this app.
I hope someone just forgot to check this possibility. That happens, we've all done it.

But, it's possible too, that someone said "don't bother checking that, because nobody would do that anyway". I've heard it said many a time - and am probably even guilty of doing it myself.

*"Nobody would do that anyway"* is a dangerous saying. Not everybody thinks the same way - what seems silly to one person might seem sensible to another!

In this case, I found a (somewhat) legitimate reason to try and pay nothing.

##### Where is the bug?
Where is the bug here?
The obvious answer is that it should fail elegantly instead of crashing. Maybe an error message, or some field validation.

But there might be some design problems too. If I owe no money, why do I get a 'pay now' button and payment options?

It's *possible* there's a good reason for that.

But, it might be more elegant to disable payment options when my balance is zero.
If I was testing this app, I'd certainly be asking about that!

##### So what, who cares?
So - the app crashes. So what? I was trying to do something a bit dumb anyway.

Does a bug like this have any measurable impact?

I think it highlights a few things.
For one, if this exception hasn't been handled - what *else* hasn't been handled?
An error like this is a 'code smell' to me. If there's a problem here, there's a good chance there are other problems in this same area.

It also erodes trust in the product. If they've missed something like this - what else have they missed?
Do I still want to use it?
Should I trust them with my credit card information?
<img src="/assets/images/2019/05/2019-05-02-02.jpg" class="right" width="280px">

Finally, it's possible to contrive a situation where someone accidentally clears the field as they go to pay.
It's a bit far fetched, but possible.
In that case, the user ends up in a situation where they don't know if their bill has been paid or not.
It could end up as a bad day for a support person - something that could have been easily avoided.

##### Anyway, I still don't have an e-scooter
But I have a bill due before the promotion ends. I'll try the app again then. Wish me luck!

##### Footnotes
* The good folk over in the [MoT Auckland Slack group](https://wetestauckland.slack.com/join/shared_invite/enQtNDY3OTQyMTg0MTgyLWRmMGY0Mzk0OTZkNTU0NjM4MDNjNTg2ZDMyODMzZTc0ZjJkMzQ4OTNkNzY2ODUxMjUzODE3MmIyZThhNDRlMjY) helped me flesh out my thoughts on this. They're good people, you should join in the fun.
* It's probably easy to work out whose app it is in the gif above. If you work it out, please don't be too harsh or go telling the whole world. We all have bugs.