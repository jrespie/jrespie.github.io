---
title: Unit testing in React
description: 30 days of Testability, day twenty-one
image: /assets/images/2020/10/react-logo.png
---
# {{page.title}}
## {{page.description}}

#### Unit testing

Today's challenge:
> Unit tests provide insight into an applications testability. Pair with a developer to explore some unit tests.

My friend Akshay is a full-stack developer, with a background in testing. He's working in React these days, so I took the chance to pair up with him on an exercise looking at unit tests in React.

Apologies in advance for some of the video quality, you can't really read what's in the terminal windows. Recommend watching this in full screen if you can.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Eex047KKgJ0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Two different kinds of unit test

What was interesting about this was learning that there are different ways to unit test.

The first test Akshay demonstrated was using [React Testing Library](https://github.com/testing-library/react-testing-library), for making assertions on items in a component.
The second test we did used [React Test Renderer](https://reactjs.org/docs/test-renderer.html), which compared the 'before' and 'after' version of a component and made sure they were identical.

I also learned something about some of the syntax quirks - GetByTestId worked for asserting an element existed, but was going to error if I tried to assert it _didn't_ exist.
You can read more about that in the [React Testing Library Documentation](https://testing-library.com/docs/dom-testing-library/api-queries).

#### Fun times!
This was a fun little exercise. I would strongly recommend pairing up with your favourite developer and getting them to show you how your unit tests work!
I'm super grateful to Akshay for helping me out. You can follow him on Twitter as [@akshay_sud](https://twitter.com/akshay_sud).

As always, I hope you enjoyed reading this, and I look forward to hearing from anyone else who gives this paired exercise a go!