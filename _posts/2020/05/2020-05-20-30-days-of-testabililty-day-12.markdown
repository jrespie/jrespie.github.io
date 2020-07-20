---
title: Testing in production using circuit breakers
description: 30 days of Testability, day twelve
image: https://jpie.nz/assets/images/2020/05/2020-05-20-quiz.png
---
# {{page.title}}
## {{page.description}}

#### Circuit breakers

Today's exercise:
> Decomposability is an important part of testability. Complete our circuit breakers testing exercise over on The Club!

The 'circuit breakers' exercise is to take a piece of software, and see how you might apply a 'circuit breaker' to it.

Circuit breakers are a concept borrowed from electronics. If there is a fault detected in a circuit (like an overload), interrupt what the circuit is doing. This minimises any damage caused.

We can do the same thing in software. Run a function, but if a fault is detected, stop running that function (and do something else).

This is really useful for testing - it means you can test something in production, and have a failsafe if it goes awry!

I won't go into too much more detail, I've written [another article on it over on Ministry of Testing](https://www.ministryoftesting.com/dojo/lessons/testing-in-production-the-mad-science-way).

#### Quiz time!

To do the exercise, I got together with my friend Sonia. We tried to see if we could apply the circuit breaker model to the [Stuff Quiz](https://www.stuff.co.nz/national/quizzes).

It's a bit of an over-the-top example, but, it was a fun excuse to do the quiz too :P

Watch us try and do the quiz, and talk about circuit breakers!

<iframe width="560" height="315" src="https://www.youtube.com/embed/GJxxUUhR0JE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


#### Thanks

As always, thanks for watching! These posts are getting pretty delayed now, so, thanks for your patience too! We'll make it to day thirty!
And thanks [Sonia](https://twitter.com/sonialobo139) for joining me!