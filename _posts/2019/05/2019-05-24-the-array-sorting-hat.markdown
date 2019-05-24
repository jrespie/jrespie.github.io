---
title: The array sorting hat
description: Making a Chrome extension, part two
---
# The array sorting hat
## Making a Chrome extension, part two
### Incase you lost your memory...
<img src="/assets/images/2019/05/2019-05-24-obliviate.gif" class="center" width="400px">
##### What we did last time
We're building a Chrome extension that tells us how many custom emoji each team member has created.

Like this:
<img src="/assets/images/2019/05/2019-05-24-01.png" class="center" width="600px">
In [part one]({% post_url /2019/05/2019-05-20-making-fetch-happen %}), we used a JavaScript `fetch()` to get an array of emoji objects.

Now, we need to do something with that information!
<hr/>
### Step 1 - Double trouble
<img src="/assets/images/2019/05/2019-05-24-twins.gif" class="center" width="400px">

We're going to be working with *two* arrays - we'll call them the emoji array, and the creators array.

##### The emoji array
This is the information we got from our `fetch()` command in part one.

Each object in the emoji array includes a `user_display_name`, which is the bit we're interested in.


##### The creators array
An empty array - we'll fill it with each creator's name, and the count of how many emoji they've created.

Each object in the array will look like this:
```javascript
{ name: "Harry Potter", countOfEmojiCreated: 200}
```
##### Getting the arrays ready
We can start by getting both these arrays ready for use.
```javascript
//...follows on from the fetch() in part 1
rawJSONResult = await response.json();
emojiData = rawJSONResult.emoji;

creatorData = []
```
This creates the emoji array - _emojiData_ - and populates it with the emoji information we've got.

Then it initializes a new empty array - _creatorData_ for the creator information.
<hr/>
### Step 2 - Array magic
<img src="/assets/images/2019/05/2019-05-24-magic.gif" class="center" width="400px">

Not _really_ magic. Just a `forEach` loop!
We're going to loop through every item in the _emojiData_, to populate _creatorData_.

There are a couple of functions that will help with this:

##### array.findIndex()

We can use `array.findIndex()` to see if the creator already exists in the _creatorData_ array.
This is useful for two reasons:
* if the creator doesn't exist, it will tell us by returning a -1 value.
* if the creator does exist, it will tell us their index in the _creatorData_ array. This will make it easy to update.

##### array.push()

`array.push()` will pop a new object onto the end of the array.

##### Let's put them together

We can use these functions in a `forEach` loop like this:

```javascript
emojiData.forEach(function(emoji){
	creatorIndex=creatorData.findIndex(creator=>creator.name===emoji.user_display_name)
	if(creatorIndex===-1){
		creatorData.push({name: emoji.user_display_name, countOfEmojiCreated: 1})
	}
	else{
		creatorData[creatorIndex].countOfEmojiCreated++;
	}
})
```
So, for each emoji in emojiData, we will
* Look for the creator's name in the creatorData array using `findIndex()`
	* If it _can't_ be found, `push()` a new record to the creatorData array with a count of 1
	* If it _can_ be found, increment the count on the record by 1
<hr/>
### Step 3 - The array sorting hat
<img src="/assets/images/2019/05/2019-05-24-sorting-hat.gif" class="center" width="400px">

There's a nice JavaScript function that will help us sort the creatorData array.

##### array.sort()
array.sort() accepts a 'compare' fuction as an argument.
The compare function takes two objects, and compares them according to the parameters we give it.

In our code, we're going to be comparing the `count` of two different creators.
```javascript
creatorData.sort((creator1,creator2) =>
	(creator1.countOfEmojiCreated > creator2.countOfEmojiCreated) ? -1 :
	((creator2.countOfEmojiCreated > creator1.countOfEmojiCreated) ? 1 : 0));
```
A negative result shifts the first compared item to the left in the array.
A positive result shifts the second compared item to the left. A result of zero means - no change.

Once we've run this across our creatorData array, the data will be sorted in descending order by countOfEmojiCreated.
<hr/>
### Put it all together
It looks like this!

```javascript
//...follows on from the fetch() in part 1
rawJSONResult = await response.json();
emojiData = rawJSONResult.emoji;

creatorData = []

emojiData.forEach(function(emoji){
	creatorIndex=creatorData.findIndex(creator=>creator.name===emoji.user_display_name)
	if(creatorIndex===-1){
		creatorData.push({name: emoji.user_display_name, countOfEmojiCreated: 1})
	}
	else{
		creatorData[creatorIndex].countOfEmojiCreated++;
	}
})

creatorData.sort((creator1,creator2) => (creator1.countOfEmojiCreated > creator2.countOfEmojiCreated) ? -1 : ((creator2.countOfEmojiCreated > creator1.countOfEmojiCreated) ? 1 : 0));
```

And if we run it in the console, it gives us this!
<img src="/assets/images/2019/05/2019-05-24-02.png" class="center" width="600px">
<hr/>
### The end?
<img src="/assets/images/2019/05/2019-05-24-adults.gif" class="center" width="400px">
Now, we've got all the information we're after.
We could end it here, really.
We've got the answer we want, we know who the top emoji creators are.

But, it takes a bit of knowledge of running JavaScript in the console to make this happen.

It'd be great if this was more accessible - so in the next post, we'll make it into a Chrome extension.
<hr/>
### Final note - a bug!
<img src="/assets/images/2019/05/2019-05-24-snitch.gif" class="center" width="400px">

Finally, there is (at least) one pretty clear bug in the way I've implemented this.
Can you find it?