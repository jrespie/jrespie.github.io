---
title: Good Artists Steal
description: Making a Chrome extension, part three
image: http://jpie.nz/assets/images/2019/06/2019-06-28-cage-avatar.jpg
---
# {{page.title}}
## {{page.description}}

#### Let's go!
<img src="/assets/images/2019/06/2019-06-28-cage-getting-there.gif" class="right" width="200px">
This is the third post in a series about creating a chrome extension.


Read [part one]({% post_url /2019/05/2019-05-20-making-fetch-happen %}) and [part two]({% post_url /2019/05/2019-05-24-the-array-sorting-hat %}) for context!

As a reminder, this is the end goal:
<img src="/assets/images/2019/06/2019-06-28-01.png" class="center" width="600px">

So far, we've got all the data we need, appearing nicely in the dev console.

Now we need to:
- make it appear on the page
- make it look presentable
- make it work for more than one Slack team (currently it's hard-coded)

So let's go do that!
<hr/>
#### Knock me for a 'for' loop

<img src="/assets/images/2019/06/2019-06-28-cage-for-loop.gif" width="200px" class="left">The first thing to do is to put our raw data into an HTML readable format.

A plain old HTML table will suffice!

We can use a simple `for` loop to do this.
First, create a table element. Then, for each item in the `creatorData` array, add a new row containing the creators name, and how many emoji they've created.

Something like:
```javascript
const HTMLTableOfCreatorStats = document.createElement('table');
creatorData.forEach(function(creator){
	HTMLTableOfCreatorStats.innerHTML+=`
	<tr>
		<td>${creator.name}</td>
		<td>${creator.countOfEmojiCreated}</td>
	</tr>`
});
```
Great!

Now, we need to get the `HTMLTableOfCreatorStats` to actually appear on our page.
<hr/>
#### Good artists steal
<img src="/assets/images/2019/06/2019-06-28-cage-steal.gif" width="200px" class="right">
Here's where we start stealing.

Every dev I talk to says that part of development is leveraging solutions other people have already solved.
So when I say 'stealing', what I really mean is, finding someone else that has solved our problem, and seeing how they've done it.

I'm looking at a different chrome extension called [Neutral Face Emoji Tools](https://github.com/Fauntleroy/neutral-face-emoji-tools).
It's an extension for bulk uploading emoji, and it's really great - you should check it out.

Their extension adds an element to the Customize Emoji page.
Their code is open source, so we can copy what they've done and alter it to fit.

They use a function called ChildNode.before(), to insert an element before another element in the page.

We can do the same thing!

We want our table to appear before the main table, which is wrapped by an element of class `.p-customize_emoji_wrapper`.
So, something like:
```javascript
const customizeEmojiWrapperElement = document.querySelector('.p-customize_emoji_wrapper');
customizeEmojiWrapperElement.before(HTMLTableOfCreatorStats);
```
Should put the table on the page for us, in the position we want.

Like this!
<img src="/assets/images/2019/06/2019-06-28-02.png" class="center" width="600px">

<hr/>
#### Make me look good
<img src="/assets/images/2019/06/2019-06-28-cage-good-looking.gif" width="200px" class="left">
So, that's cool, but it doesn't look great.
It also takes up a *huge* chunk of the page.

It'd be nice to make it look similar in style to the main emoji table.

We can easily do that by stealing all the styling options from that table. Yep, stealing again.

First, the table itself.

Slack aren't using an HTML table, they're using `div`s with custom classes.
So we can rewrite the loop from above to match what they're doing:
```javascript
creatorData.forEach(function(creator,index){
	HTMLTableOfCreatorStats.innerHTML+=`
	<div class="c-virtual_list__item" role="presentation" style="top: ${index*65}px;" tabindex="-1">
		<div role="presentation" class="c-table_view_row_container">
			<div role="row" class="c-table_view_flexbox c-table_view_row p-customize_emoji_list__row">
				<div role='gridcell' class='c-table_view_row_item'>${creator.name}
				</div>
				<div role='gridcell' class='c-table_view_row_item'>${creator.countOfEmojiCreated}
				</div>
			</div>
		</div>
	</div>`
});
```
Note the `${index*65}` sets the start position of each row.

Once we've got that sorted, we can insert the `HTMLTableOfCreatorStats` into a larger block of HTML. This code is just a copy-paste job from further down in the Customize Emoji page.

Please excuse the size of this, there's a lot of stuff in there.
I've added a couple of headings to suit my own needs, and a few changes to make the sizing bearable.

```javascript
<h4 class="large_bottom_margin inline_block black">${creatorData.length} people have added custom emoji</h4>
	<div class="c-table_view_container">
		<div role="grid" class="c-table_view_keyboard_navigable_container">
			<div role="rowgroup" class="c-table_view_flexbox_container">
				<div role="row" class="c-table_view_flexbox c-table_view_row_header">
					<div role="columnheader" tabindex="-1" class="c-table_view_header_item c-table_view_hover_disabled p-customize_emoji_list__header">
						<span class="c-table_view_header_item_value">Emoji Creator</span>
					</div>
					<div role="columnheader" tabindex="-1" class="c-table_view_header_item c-table_view_hover_disabled p-customize_emoji_list__header">
						<span class="c-table_view_header_item_value">Emojis created</span>
					</div>
				</div>
				<div role="presentation" class="c-table_view_all_rows_container" style="height: 200px;">
					<div style="overflow: visible; height: 0px; width: 0px;" role="presentation">
						<div role="presentation">
							<div tabindex="0" style="position: absolute; width: 1px; height: 1px; outline: none; box-shadow: none;">
							</div>
						<div role="presentation" class="c-virtual_list c-virtual_list--scrollbar c-scrollbar" style="width: 894px; height: 200px;">
							<div role="presentation" class="c-scrollbar__hider">
								<div role="presentation" class="c-scrollbar__child" style="width: 894px;">
									<div class="c-virtual_list__scroll_container" role="presentation" style="position: relative; height: ${creatorData.length*65}px;">
										${HTMLTableOfCreatorStats}
									</div>
								</div>
							</div>
							<div role="presentation" class="c-scrollbar__track">
								<div role="presentation" class="c-scrollbar__bar" tabindex="-1" style="height: 50px; transform: translateY(0px);">
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
<br/>`;
```
Et viola! Now our table looks a whole lot nicer!
<img src="/assets/images/2019/06/2019-06-28-03.png" width="600px" class="center">
<hr/>
#### Okay! Nearly there!
<img src="/assets/images/2019/06/2019-06-28-cage-okay.gif" width="200px" class="right">
There's one more important thing to do.
As it stands, this is only going to work for _my_ Slack team. If I want to share it with others, it needs to work on whatever Slack team a user is logged in to.

Remember the API call from part one? This API call contains a token.(You _could_ see it here, but I've redacted it for security reasons.)
```javascript
...
	"body":"page=1&count=100&token=redacted",
...
```
This token determines the Slack team you are getting the data for. For this to work on other Slack teams, it needs to be dynamic.

Once again, I know that the creators of the bulk emoji uploader must already have solved this problem.
They've done it by scanning the scripts on the page, and using a regular expression to pull it out, like so:
```javascript
scripts.forEach((script) => {

		const apiTokenResult = /["]?api_token["]?\:\s*\"(.+?)\"/g.exec(script.innerText);
		if (apiTokenResult) {
			apiToken = apiTokenResult[1];
		}
	});
```
We can use this! We can take the apiToken, and use that instead of our hardcoded token.
```javascript
...
	`body":"page=1&count=100&token=${apiToken}`,
...
```

If I try it on some different slack teams that I belong to:
<img src="/assets/images/2019/06/2019-06-28-04.png" class="center" width="800px">

Yay! Looks great!
<hr/>
#### Wow, what's next?
<img src="/assets/images/2019/06/2019-06-28-cage-wow.gif" width="400px" class="center">
So, that's all the code we need for our Chrome extension.
The next step is to actually turn it into an extension, and submit it to the Chrome store!

I haven't actually done this yet, so we'll see what happens!
