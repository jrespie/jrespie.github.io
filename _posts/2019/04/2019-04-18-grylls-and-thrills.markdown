
# Grylls and Thrills
## Using systematic and speculative tracking to solve software problems

> Note: this blog post contains minor spoilers for the Netflix series **You vs wild (s1e3)**. Maybe don't read on if you care about that sort of thing.

##### You vs wild
Bear Grylls' has a new interactive TV series on Netflix called *You vs Wild*.

It's interactive! At points during the show, the viewer is called upon to make decisions about Bear's next action.

<img src="/assets/images/2019/04/2019-04-18-01.jpg" class="left" width="280px">

For example, he's only got room in his pack for either a slingshot or a grappling hook. You decide which one to take, and that shapes the rest of the episode. This can determine whether Bear succeeds in his mission or not.

It's great fun! A bit like a [Choose Your Own Adventure](https://en.wikipedia.org/wiki/Choose_Your_Own_Adventure) book you can do with the whole family.

(You *can't* kill Bear Grylls at any point, as far as I can tell. We've tried.)

##### A lost dog
In one episode, Bear's mission is to find a missing rescue dog in the Swiss Alps.

At a certain point in the story, Bear needs to decide how he's going to try and track the dog.

The options are:
- Try to look around for clues!
A snagged piece of fur, pawprints, or some poo poo. Anything that could indicate the direction the dog travelled in. This is  **systematic tracking**.
- Try to think like a dog!
Consider what you would do if you were a dog. Try and find water? Head for somewhere warm? This is  **speculative tracking**.

These are techniques we apply in software all the time!

They come in useful when trying to diagnose issues from a production environment. For example, when a customer reports a problem that you can't reproduce. Or, if an unexpected alert fires from your monitoring tools.
##### Systematic Tracking
<img src="/assets/images/2019/04/2019-04-18-02.jpg" class="right" width="280px">

Systematic tracking means looking at the clues to find out what happened.
For example, you might look at:
* Audit logs - is anything logged about what the user might have been doing?
* Monitoring tools - was there anything out of the ordinary happening on the system at  the time? E.g. a spike in load on the system?
* Analytics - do you use a system like Google Analytics that can tell you what page the user was on at the time? What button they clicked?
* Database tools - does the database reveal anything unusual about the user? What about other database records?

Using these sorts of clues, it's often possible to piece together a picture of what the user did to cause an error.
It also demonstrates the importance of having good monitoring and logging in place!

##### Speculative Tracking
<img src="/assets/images/2019/04/2019-04-18-03.jpg" class="left" width="280px">

Speculative tracking means trying to think like a user. Asking yourself, if I was using this product, what might I be doing to get myself in this situation?

This can mean getting creative. Some things you could consider:
* Why do users use this product? Ask yourself what goal the user is trying to achieve. Then try and achieve that goal yourself!
* What _else_ might the user be trying to do? Consider using the product for something other than its intended purpose. This can uncover strange behaviour!
* Is there anything external to the product that might be having an effect? For example, are there any Chrome plugins your user is likely to have? Would they be using browser features like multiple tabs, or the back button?

##### Why not both?
<img src="/assets/images/2019/04/2019-04-18-04.jpg" class="right" width="280px">

The great thing is, unlike in *You vs Wild*, we don't have to pick one method or the other.
Often when my own team is confronted by a problem, we tag-team it.
I start tracking down the problem using speculative techniques. Meanwhile, one of my teammates tries to tackle it using a systematic approach. (or, vice versa).
This way, we can collaborate to figure out the root cause of our problem.

In my experience, it works really well!