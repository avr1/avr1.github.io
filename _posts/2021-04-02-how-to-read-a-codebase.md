---
layout: post
title: How to Read a Codebase
subtitle: It's an art, not a science, but here's a way to "grok" it quicker.
comments: true
---

## How to Read a Codebase

 Ever tried to get involved with an open source project, but run into difficulties understanding the codebase?

We've all struggled with understanding a foreign codebase in an efficient manner. But, there are some pointers for being able to understand the codebase quickly, and at least develop a working knowledge about where things are situated.

First of all, it is important to gain familiarity with the tools the codebase gives you in order to grok it. Whether it's a hacking guide, a book written to understand the functionality, or a mailing list of frequently asked questions, most codebases will have existing ways and means to onboard new developers.

Then, we get into far more interesting means. After reading these, you most likely will have a superficial idea of how the codebase is structured, and how to being contributing. Now, depending on how far you wish to learn before contributing, you may be ready here. But, for most people who wish for a long-term understanding, the next step is to get to the point of view of the user in terms of the feature set, while simultaneously gaining a deeper understanding of the inner workings of the code.

So, play around a bit with the code. Learn how possible use cases work, and what are some common workflows using the tool. But, to learn more about the codebase, the best place to start looking is the tests. There, provided that the project focuses on test-driven development, you'll see rigorous tests for every point of contact the program has externally, and hopefully several testing core, internal methods. So, skim through these tests for a couple reasons: one, so that you can evaluate the quality of the codebase you are about to work on, and two, so that you can further understand, generally, where things are located. At this stage, it's best to think of the codebase as a library: your job is currently just to skim, not really to finish every book. But, if you can understand where a book's location is generally, and if you can narrow it down to a few shelves, you're achieving the goal here.

So you've skimmed the tests, and now want to look further at the code. Good idea! Start by looking at those key features you've seen earlier, and try followign one of your average use cases through the code. That way, in addition to gaining familiarity with the codebase, you also can test practical knowledge, and also find any reduncies in the normal case. After that, look at the heirarchy of source code. Is it neatly organized, and packaged into self-contained, individual chunks. Or, is it one big monolith, with code only tangentially related lumped together. In general, it's probably also a good idea to compare the source file heirarchy with the test directory's, since they should be analagous.
