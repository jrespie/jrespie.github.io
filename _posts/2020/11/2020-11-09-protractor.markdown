# Resolving an issue with Protractor tests timing out
## Our tests started failing
You ever have that thing where your tests start failing for no obvious reason?
This is the story of how it happened to us, and how we figured out what had happened.

We have an Angular web app. Angular comes with Protractor, an end-to-end testing framework - so, that's what we use for our tests.

One of the key features of Protractor is that it knows to wait for Angular to finish processing before it tries to perform any actions.

So you don't have to mess about with waiting for the right element to appear, animations to complete, or browser.sleeps(). Protractor's waitForAngular() feature takes care of it for you.

But one day, waitForAngular() just stopped working. Every test started failing - they were timing out on basic functions.

We found a workaround - disable waitForAngular(), and fill the tests with more explicit waits. Which we did, and it worked, but it wasn't a great solution.


## Angular hasn't finished
Asserting that angular hasn't finished using .isStable()
## Figuring out _what_ hasn't finished
that angular zone thing from stackoverflow
## Unminifying the code
figuring out that it's mixpanel
## Locking the version
We figured out that there'd been an upgrade, and rolled the minor version back. Problem solved!