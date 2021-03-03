---
title: Jumping in to a Protractor timeout problem
description: What to do when Protractor tests start failing, and Angular becomes unstable.
image: /assets/images/2021/03/Angular-Protractor-avatar.jpg
---
# {{page.title}}
<hr/>
## {{page.description}}

Hey!

This is my second [bloggers club](https://club.ministryoftesting.com/t/bloggers-club-february-2021-your-struggles-successes-with-automation/46900) post, and ~~again, I'm just scraping through on time~~ nope I'm late.

The topic this time is:

> Your struggles/successes with automation

I'm going to tell you what I've learned recently about Protractor and Angular stability.
Firstly, I'll introduce Protractor, and how it works. Then I'll tell you about a problem we had, and how we solved it.

Let's begin!

## Angular and Protractor

The frontend of our web app is written in [Angular](https://angular.io/). For E2E testing, we use [Protractor](http://www.protractortest.org/#/).

Protractor is a testing framework specifically designed to work *with* Angular. It's really cool. Protractor can be used on non-Angular apps too, but partnering it with Angular is where its power lies.

## The Testability class and "waitForAngular"
<img src="/assets/images/2021/03/Angular-Protractor-Comic_000a.jpg" class="center" width="500px" alt="On the left, two people labelled Angular and Protractor. They are bungee jumping. Protractor is waiting for Angular to be ready. On the right, a similar pair labelled Other Test Framework and Other App. Other Test Framework pushes Other App off the bridge before they are ready to jump.">

Every Angular app has a *Testability* class. The Testability class provides some information about the apps 'readiness' for testing.

Basically, the app knows when it is 'stable' - when it has loaded all elements to the page, finished processing, that sort of thing.

Using these functions, Protractor can be aware of when an Angular app is ready to be tested. Behind the scenes, it has a boolean property called `waitForAngular`, that defaults to true.

Because Protractor waits for Angular to be stable before running, **you don't have to explicitly wait for elements to appear**, or play about with timeout settings for slow loading pages.

You can turn this feature off by using `waitForAngularEnabled(false)`. It's not recommended, but there might be reasons you want to - for example, if you have an app that has some pages using Angular, and some that don't.

Protractor is very reliable in this way, and we've had very few issues with 'flaky' tests.

## The "isStable()" function
Angular apps have this cool in built function that will tell you if the app is 'stable' or not.
Unsurprisingly, the function is called `isStable()`, and it returns true or false.
Depending on the app, you can even run it from the browser console in Chrome.

Why not try it? JetBlue.com use Angular. If you head to their site, open the dev console and run:
```
window.getAngularTestability(document.querySelector('body > jb-app')).isStable();
```
<img src="/assets/images/2021/03/angular-protractor02.png" class="center" width="500px" alt="A screenshot of the browser console running the isStable() function, and returrning true">

It should return `true`!

Neat huh!

## The day Angular became unstable

<img src="/assets/images/2021/03/Angular-Protractor-Comic_002.jpg" class="left" width="300px" alt="Angular and Protractor standing on a bungee platform. It is night time. Protractor asks Angular if they are ready yet, and Angular says no. Protractor is exhausted at waiting so long.">So - why does all this matter?
Well, recently, our tests just started to fail in our staging environment.

The failure wasn't very clear - Protractor was timing out trying to find an element on the page. The element was clearly present though, and the functionality, when examined, didn't seem to be broken.

This was a real puzzle. What could be up?

Well, we had a hunch that *maybe* Angular was still doing something. If Angular gets stuck in a loop, it will never tell Protractor that it's 'stable' and ready for the tests to run.

Sure enough - when we opened the dev console and ran our `isStable()` command, **it consistently returned `false`.**

## The workaround

Now that we knew this, we had a workaround.

We could set `WaitForAngularEnabled(false)`, to disable the functionality that tells Protractor to wait for Angular.
Then, we could rewrite our tests to wait for each element we were testing.

It means though, that a test that looked like this:

```
it('should open the login screen', async () => {
        await browser.get(browser.baseUrl);
        loginPage = new LoginPage();
        expect(await loginPage.loginButton.isDisplayed());
});

```

Now looks like this:

```
waitForAngularEnabled(false);
.
.
.
        it('should open the login screen', async () => {
                await browser.get(browser.baseUrl);
                loginPage = new LoginPage();
                const until = protractor.ExpectedConditions;
                browser.wait(until.presenceOf(loginPage.loginButton), 15000, 'Element taking too long to appear in the DOM');
                expect(await loginPage.loginButton.isDisplayed());
        });
```

It's a bit messier, and we open up this test to become 'flaky', because now we're dependent on the element appearing within 15 seconds.

In the short term, this fixed the tests. But what we really want to know is - why did this start happening?

## Diagnosing the problem

To figure out why Angular had become unstable, we turned to everybody's favourite solution, [StackOverflow](https://stackoverflow.com/questions/48627074/how-to-track-which-async-tasks-protractor-is-waiting-on)! (So thanks [magnattic](https://stackoverflow.com/users/554340/magnattic) for asking the question, and [JeB](https://stackoverflow.com/users/1544364/jeb) for answering it!).

Here's the [relevant section of that post](https://stackoverflow.com/questions/48627074/how-to-track-which-async-tasks-protractor-is-waiting-on):

> 1. Go to `node_modules/zone.js/dist/zone.js`.
>
> 2. Look for Zone's constructor function: `function Zone(parent, zoneSpec)`
>
> 3. Add the following inside: `this._tasks = {}`
>
> 4. Add counter variable in *global scope*: `let counter = 0`
>
> 5. Look for `Zone.prototype.scheduleTask` and add the following right before the call to `this._updateTaskCount(task,1)`:
>
>    ```js
>    task.id = counter++;
>    this._tasks[task.id] = task;
>    ```
>
> 6. Look for `Zone.prototype.scheduleTask` and add the following right before the call to `this._updateTaskCount(task,-1)`:
>
>    ```js
>    delete this._tasks[task.id]
>    ```
>
> 7. Recompile the application and run it
>
> Assuming you did the above tweaks you can always access the pending tasks like this:
>
> ```js
> tasks = Object.values(window.getAllAngularTestabilities()[0]._ngZone._inner._tasks)
> ```
>
> To get all the pending macro tasks that affect the testability state:
>
> ```js
> tasks.filter(t => t.type === 'macroTask' && t._state === 'scheduled')
> ```
>
> After that similarly to the previous solution check out the `.callback.[[FunctionLocation]]` and fix by running outside of Angular zone.

So - it's starting to get complex.

Basically, we had to make a change to a part of an Angular dependency (zone.js), to get it to log to the browser console the name of the task Angular is waiting on.

> Note: the post specifies zone.js, but in our case, we needed to change a file called zone-evergreen.js. I'm not sure what the difference is here.

Anyway, by making that change, and redeploying (locally), we can now look at the browser console to see what is causing it to become unstable.

And... there it is!!
<img src="/assets/images/2021/03/angular-protractor01.png" class="center" width="500px" alt="A screenshot of the browser console with the offending function highlighted.">

It's a javascript file from one of our **third party dependencies!**

## The fix

It turned out, that this particular dependency had a new version released about the same time this test started failing.

The way our app was configured, it automatically updated to the new version of that dependency.

At this point we don't know if it's a bug in the dependency itself, or the way we're implementing it.

One solution could be to try running it [outside of Angular](https://angular.io/api/core/NgZone#runOutsideAngular), but that's beyond the scope of this post.

For now, we can **lock that dependency to a known working version** (we should have been doing this anyway).

That means changing our `package.json` to read:

`"m**************r": "~2.38.0"`

instead of

`"m**************r": "2.38.0"`

This instructs it to always use version **2.38.0** (instead of 2.38.0 or higher).

Sure enough, when we lock it to that version, and I run `isStable()` in the console, **it starts returning true again!**

This meant we could remove the workaround code, and start using Protractor to it's full potential again. **Yay!**

## Summary
<img src="/assets/images/2021/03/Angular-Protractor-Comic_003.jpg" class="right" width="300px" alt="Angular and Protractor on a bungee platform. Angular is jumping off and Protractor is cheering them on. They are both happy.">
So, there's a lot there. Here's the tl;dr:

* Protractor waits until Angular is 'stable' before it runs tests.
* By using the `isStable()` function, we could tell that something was preventing Angular from ever becoming 'stable'.
* We could work around this by setting `waitForAngularEnabled(false)` in our tests, so that they would keep running.
* By hacking the `zone.js` file, we could send information to the console that would tell us what function was keeping Angular from becoming 'stable'.

* Once we knew the culprit (a third party dependency), we could roll that dependency back to a known working version.
* This solved the problem, and we could return to `waitForAngularEnabled(true)` in our test code.

I hope that was useful! I wrote this because I thought it was a tricky and interesting problem to solve, and it might serve as helpful for anyone else that encounters something like this.

I learned quite a bit about Angular and dependencies in the process. **If you found it useful, please let me know!**