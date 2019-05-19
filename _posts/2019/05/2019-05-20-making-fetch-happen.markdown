---
title: Making fetch happen
description: Making a Chrome extension, part one
---
# Making fetch happen
## Making a Chrome extension, part one

### I <font color="#ff69b4">love</font> making emoji
<div class="rowofimages">
<img src="/assets/images/2019/05/2019-05-20-mean-girls-burn-book-emoji.jpg" class="left" width="64px"><img src="/assets/images/2019/05/2019-05-20-mean-girls-mouse-emoji.png" class="right" width="64px"><img src="/assets/images/2019/05/2019-05-20-mean-girls-on-wednesdays-emoji.jpg" class="left" width="64px" height="64px"><img src="/assets/images/2019/05/2019-05-20-mean-girls-regina-emoji.jpg" class="right" width="64px"></div>

I _love_ Slack, and one of the features I love about Slack is being able to add <strong><font color="#ff69b4">custom emoji</font></strong>.

Like these Mean Girls emoji that I added today, for some reason.

Any team member with the right permissions can add custom emoji by visiting:
[https://[your-slack-team-name].slack.com/customize/emoji](https://[your-slack-team-name].slack.com/customize/emoji)

What you _can't_ do is see how many emoji you (or others) have created. I don't know if Slack has any native way to surface this information, but it's a fun and interesting thing to know.

So, <strong><font color="#ff69b4">I made a Chrome extension to do it.</font></strong> It looks like this:
<img src="/assets/images/2019/05/2019-05-20-01.png" class="center" width="600px">

This post will be the first in a series of the steps I took to make it, and the learnings along the way.
I hope it's useful, or at least interesting!
<hr/>

### Step 1 - <font color="#ff69b4">Use your ESPN</font> to track down HTTP requests
<img src="/assets/images/2019/05/2019-05-20-fifth-sense.gif" class="center" width="400px">
The first question is:

Is there any programmable way to get information on custom emoji?

My instincts tell me that there must be some kind of HTTP request when loading the _Customize Emoji_ page.

So, let's take a look at <strong><font color="#ff69b4">Chrome Developer Tools</font></strong> and see what's happening.
<img src="/assets/images/2019/05/2019-05-20-02.png" class="center" width="600px">

When the page loads, we can see in the _network_ tab, a <strong><font color="#ff69b4">POST</font></strong> request to:
```
https://[your-slack-team-name].slack.com/api/emoji.adminList
```

That sounds promising, and sure enough, it returns an array of emoji. Nice.

Let's play with it some more!
<hr/>

### Step 2 - <font color="#ff69b4">Fetch</font> the information
<img src="/assets/images/2019/05/2019-05-20-so-fetch.gif" class="center" width="400px">
Chrome dev tools has a feature where you can copy a request as a JavaScript <strong><font color="#ff69b4">fetch</font></strong> command.
<img src="/assets/images/2019/05/2019-05-20-03.png" class="center" width="400px">

So, let's <strong><font color="#ff69b4">copy as fetch</font></strong> the request from above, and see if it tells us what we need.
To do this, we can go to the console tab, paste the command, and run it:
```javascript
> response = await fetch("https://[your-slack-team-name].slack.com/api/emoji.adminList", {
	"credentials":"include",
	"headers":{
		"accept":"*/*",
		"accept-language":"en-US,en;q=0.9",
		"content-type":"multipart/form-data; boundary=----WebKitFormBoundarya"
	},
	"referrerPolicy":"no-referrer",
	"body":"------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"query\"\r\n\r\n\r\n
	------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"page\"\r\n\r\n1\r\n
	------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"count\"\r\n\r\n100\r\n
	------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"sort_by\"\r\n\r\nname\r\n
	------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"sort_dir\"\r\n\r\nasc\r\n
	------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"token\"\r\n\r\nredacted\r\n
	------WebKitFormBoundarya\r\nContent-Disposition: form-data; name=\"_x_mode\"\r\n\r\nonline\r\n
	------WebKitFormBoundarya--\r\n",
	"method":"POST",
	"mode":"cors"
});

```
>
`Await` makes the command wait until it's complete before returning a result. We have to do this because fetch must run asynchronously.

Once that's complete, we can then run:
```javascript
> await response.json()
```
This converts the response to a JSON object:

<img src="/assets/images/2019/05/2019-05-20-04.png" class="center" width="600px">

Alright! This gives us an <strong><font color="#ff69b4">array of 100 emoji</font></strong>, and it includes information on who created it.

It also tells us there are <strong><font color="#ff69b4">3681</font></strong> emoji altogether that's going to be useful later.
<hr/>

### Step 3 - Making it <font color="#ff69b4">readable</font> (and useful)
<img src="/assets/images/2019/05/2019-05-20-burn-book.gif" class="center" width="400px">

We don't just want the first 100 emoji though, we want <strong><font color="#ff69b4">all</font></strong> of them.

The first thing to do is tidy up the query, because it's pretty bulky and hard to read.

I've stripped out all the unnecessary stuff, and changed the Content-Type from `multipart/form-data` to `application/x-www-form-urlencoded`.
Now, this is the query I'm using:
```javascript
fetch("https://[your-slack-team-name].slack.com/api/emoji.adminList", {
	"credentials":"include",
	"headers":{
		"accept":"*/*",
		"accept-language":"en-US,en;q=0.9",
		"content-type":"application/x-www-form-urlencoded"
	},
	"referrerPolicy":"no-referrer",
	"body":"page=1&count=100&token=redacted",
	"method":"POST",
	"mode":"cors"
});
```

This makes the fetch command a lot clearer. It's pretty easy to see where the count of 100 is coming from.

We know from earlier that we have 3681 emoji in total - so, let's try <strong><font color="#ff69b4">changing the count to 3681</font></strong>.
This is the response:
<img src="/assets/images/2019/05/2019-05-20-05.png" class="center" width="600px">

It returns all our emoji, in one nice array! <strong><font color="#ff69b4">Woo hoo!</font></strong>
<hr/>

### So, <font color="#ff69b4">what now?</font>
<img src="/assets/images/2019/05/2019-05-20-so.gif" class="center" width="400px">

So now, we know we can programmatically get information on all our custom emoji, including who created it.

The next step will be to do some number crunching, and work out how many emoji each person has created.
<strong><font color="#ff69b4">Stay tuned for part two.</font></strong>
<hr/>

### <font color="#ff69b4">Breast cancer awareness</font>
The gratuitous use of <strong><font color="#ff69b4">pink</font></strong> in this post happens to coincide with our office's <strong><font color="#ff69b4">pink ribbon breakfast</font></strong>.
Pink ribbon breakfast is a fundraiser for the [Breast Cancer Foundation of New Zealand](https://pinkribbonbreakfast.co.nz/page/pushpaypinkribbonmorningtea).
If you've found this post interesting, please consider making a donation via the link above.
<img src="/assets/images/2019/05/2019-05-20-love-ya-bye.gif" class="center" width="400px">