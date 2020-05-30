---
title: Testability and application logs
description: 30 days of Testability, day two
image: http://jpie.nz/assets/images/2020/05/2020-05-04-cloudwatch.png
---
# {{page.title}}
## {{page.description}}

#### UneeQ

I'm not sure if this has come up on the blog yet - but I work for [UneeQ](https://digitalhumans.com/), we make digital humans.

I thought a good experiment for day two would be to make one of our digital humans do 'something', then try and find it in the logs.

To do this, I paired up with one of our developers, Darragh McCarthy, and recorded the resulting conversation.

<hr/>

#### Security, privacy and that

This was a great conversation, but - we witnessed a lot of stuff that is kinda sensitive to the company, or shouldn't be shared.

So - I've had to edit or censor a lot of this conversation. That's why it's taken so long to publish.

But, I still think it's interesting, and you might find it valuable. So - apologies for the quality and the chopping and cutting - but enjoy anyway!

<iframe width="560" height="315" src="https://www.youtube.com/embed/qFYRWLxquP4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<hr/>

#### Learnings

I learned about using our cloudwatch logs a bit better, and how to read through a 'conversation' happening in our logs.
It also led to some really good conversation about what is / isn't disclosable to the public.

It probably wasn't clear in the video, but, it raises an interesting point about when privacy clashes with testability too.
For example, it would help **testability** if we could see all the transcripts logged there in Cloudwatch.

However, this presents a privacy problem - we can't control what someone says to the avatar, and if that conversation is saved and logged somewhere - we could end up holding on to information we don't want.

So - in a situation like that, privacy has to win out!

Thanks, see you shortly for day three!