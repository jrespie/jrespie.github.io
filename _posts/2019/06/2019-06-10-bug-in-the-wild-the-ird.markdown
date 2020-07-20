---
title: "A bug in the wild"
description: My amazing tax refund
image: https://jpie.nz/assets/images/2019/06/2019-06-10-logo.png
---
# {{page.title}}
## {{page.description}}


#### Make it rain
Imagine my unparalleled joy when I got this in my email:

<img src="/assets/images/2019/06/2019-06-10-Refund.png" class="center" width="600px">

**I'm getting money from the government!**
<img src="/assets/images/2019/06/2019-06-10-01.jpg" class="left" width="280px">

But, apparently they don't have my bank account details. No probs, I'll just follow the instructions...
<hr/>

#### Managing bank accounts

This is what the Inland Revenue website looks like for me:
<img src="/assets/images/2019/06/2019-06-10-Accounts.png" class="center" width="600px">
It's confusing!
<img src="/assets/images/2019/06/2019-06-10-02.jpg" class="right" width="280px">

Firstly, I can see my bank account number.
So - they already have my bank account number?

Then there's two other rows, that have an 'account type', and some IRD reference numbers, but I can't click on them. What are they? It's all a bit weird.
<hr/>

#### Adding a new bank account

I *can* add a new bank account. That looks like this.
<img src="/assets/images/2019/06/2019-06-10-AddNewAccount.png" class="center" width="600px">
There's a clue here that starts to help piece things together.
In this screen, next to the Account Type of 'Income tax', it tells me 'No refund bank account'.

So, I fill in the form, and check the box in the 'Income tax' row.
<hr/>

#### That made it worse?

After saving, my list of accounts now looks like this:
<img src="/assets/images/2019/06/2019-06-10-NewAccounts.png" class="center" width="600px">
It's added a _new_ row, with a bank account number but no other details.

My guess is, it's _supposed_ to look more like this, but something's gone awry in the way they're building the table.
<img src="/assets/images/2019/06/2019-06-10-Fixed.png" class="center" width="600px">
I can't be sure about that of course.
<hr/>

#### Is it a bug?
Based on the information I have, I'd say there's a bug in the way the table is being built.
It is possible however, that it's behaving as designed. Maybe it's meant to be this way, and I'm not smart enough to understand it.

If that's the case, *this is still a bug*. Bugs don't just come from code. If the layout of a page is difficult to understand, that's a flaw in the way it's been designed.

Bugs can appear in design, in plans, in documentation, and many other places.
<hr/>

#### Why is it bad?
It might be easy to dismiss this bug as 'not that bad'. It still works.

But it's not obvious what's going on.
Here are some possible negative outcomes of this page the way it is:
* My bank account number is already there, so I might just ignore the advice to add it, and miss out on my $1.57.
* It raises a _bunch_ of uncertainty about how this page actually works. For example, what happens to the _other_ account - the Donation tax credit? Could I overwrite it? Or, what if I add a third account - which row will it apply to?
* Ultimately, I can't work out what it all means. This could mean a call to IR's support team - a waste of my time and theirs.
<hr/>

#### $1.57
<img src="/assets/images/2019/06/2019-06-10-03.jpg" class="left" width="280px">Anyway, since I started writing this post, my $1.57 refund has come through. So clearly, despite the problems with the page, I managed to make it work.
I hope those with more significant refunds have the same level of success.

Excuse me while I go treat myself to a Creme Egg (and I'll have seven cents left over!)
