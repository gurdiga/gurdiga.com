---
layout: post
title: An easy TDD exercise and some more
date: 2021-08-22 12:57 +0300
---

A couple of months ago I’ve set out to write a lil [RSS-checking program][0] that I thought would be useful for a friend. As a small green-field project, I thought it’s going to be perfect to—once again—try that hippy TDD method that everyone was talking about 10 years ago. In retrospect, a few things came in really handy while writing this program, and wanted to share them.

[0]: https://github.com/gurdiga/rss-email-subscription

So, in no particular order, here they come:

## Programming by intention

It’s a concept I’ve seen recently [applied by James Shore][1] and the TLDR version of it is this: I write some pseudo-code that reflects how would I like my code to look like given my current understanding. The good thing about it is that it guides me towards putting together the API of the code, and get some understanding of the pieces involved, with the smallest effort investment possible — it’s just a comment.

[1]: https://www.youtube.com/playlist?list=PLD-LK0HSm0Hpp-OspFpZ32uY766YntGVQ

Here is a quick pseudo-example:

```ts
function main() {
  /*
  const dataDir = getDataDirFromFirstCliArg();
  const feedUrl = getFeedUrl(dataDir);
  const lastItemTimestamp = getLastItemTimestamp(dataDir);
  const rssItems = getRssItems(feedUrl);
  const newItems = getNewItems(rssItems, lastItemTimestamp);
  storeNewItems(newItems);
  */
}
```

I can mess with it until I like how it reads, and then, when I have some clarity about what’s supposed to do, I can take each of those pieces and TDD it.

I think this approach has something in common with [Readme Driven Development][2].

[2]: https://tom.preston-werner.com/2010/08/23/readme-driven-development.html

## “Logic sandwich”

This is a concept I’ve learned—again—from [James Shore][3]. Since this RSS-checking program would involve a significant portion of IO, I found that using the “logic sandwich” approach helped me keep the IO and business logic unentangled, which in turn, made more of the code TDD-able.

[3]: https://www.jamesshore.com/v2/blog/2018/testing-without-mocks#logic-sandwich

Conceptually, it means that the code is structured in code chunks that do either IO or business logic, but not both. Something like this:

```ts
function main() {
  const input = infrastructure.readData();
  const output = logic.processInput(input);

  infrastructure.writeData(output);
}
```

## Microtest TDD

It’s a concept I’ve heard a few months ago [from GeePaw Hill][4], and it’s a particular style of TDD where the emphasis is on a couple of things:

[4]: https://www.geepawhill.org/2020/06/12/microtest-tdd-more-definition/

- unit test size leans on the smaller side, the smaller the better: as in “micro.” Yeah, I know: that’s just Unit Testing 101, right? 😉
- unit tests are “gray-box” tests (as opposed to “black-box” tests) — they know some of the things that happen in the code under test, which helps in keeping them more practical.

## Functional Core, Imperative Shell

It’s a concept I’ve heard years ago [from Gary Bernhardt][5] and means that the program “tree” is structured in a way that the “leaf” parts are written in a functional style, and the parts closer to the “trunk” are written in an imperative style.

In this particular program, the imperative part is the `main` function that acts like a controller that wires all the more functional pieces together. I found that the code written in a more functional style is easier to TDD.

[5]: https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell

## Result type

This is a concept I saw used in typed functional languages like Haskell, Elm, and F#. FWIW, it even has [its own Wikipedia page][6]. 🙂

[6]: https://en.wikipedia.org/wiki/Result_type

I find it brings clarity to the program flow by clearly delineating the failure paths, and, in a well-typed language like TypeScript, this makes for pretty readable and resilient code.

## Try easy!

Last but not least, it’s a meta-idea that I got from the [“Accidental Genius” book][7]: it’s called “Try easy!”. Its essence—seemingly counterintuitive at first—is that you can actually achieve more when you’re _not_ trying to give 105% of yourself to an activity, but on the contrary, to relax a bit, maybe to 90%.

[7]: https://www.goodreads.com/book/show/8360431-accidental-genius

That way you’re more flexible and keep the ability to work around the obstacles that inevitable come up with every project. Yes, this is all metaphorical and meta, but I still found it valuable. On this project specifically, I actually started twice: once with a straight face, saying to myself “I’m going to TDD this to death!” and then gave up on this idea after a week of struggling because this attitude of perfectionism was making me feel crippled.

Then, after few days of sadness and disappointment, I’ve changed my posture to “try easy” and said something to the effect of “Let’s see if I can do just this one little thingy over here.” Then, another “little thingy,” and then another.

Happy coding!
