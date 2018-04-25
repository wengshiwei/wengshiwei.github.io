---
layout: post
title: Recap Software Complexity (1) Overall
comments: true
category:
- software-engineering
- software-complexity
- recap
- english
---

In this series, I plan to recap some materials on software complexity. These materials come from

- Principles of Computer System Design: An Introduction(PCSD)_ by J. H. Saltzer and M. F. Kaashoek, 1st Edition, 2009
- [`Interaction-Oriented Programming (IOP)`](https://pdfs.semanticscholar.org/413b/1e75cfb4db05e3e57d4e4c1ffc9f3422b121.pdf), PhD thesis by [Yu David Liu](http://www.cs.binghamton.edu/~davidl/), 2007
- Code Complete 2nd Edition, by Steve McConnell, 2004
- The Mythical Man-Month 2nd Edition, by Frederick P. Brooks Jr., 1995

For the recap parts, I will re-iterate or just copy the content from the original source. It's a warm-up for either readers or even myself. Although I suggestion to read them all, not my second-hand knowledge, I try to make them succinct and consistent. I cannot precisely mark the content which is identical to source, and which is not. You can imagine you are talking with a friend, who are telling you some interesting knowledge and then making some comments.

# 2. Motivation

The story comes from I read about `Ownership Types`. I first learned related concepts from Liu's thesis, then I found a collection of survey papers on `Ownership Types`. Either this thesis or `Ownership Types` deserve another post on it.

The complexity in this series isn't computational complexity or other complexity talked in Theoretical Computer Science(TCS). A better name will be software complexity or system complexity. As a PL guy, I especially concern more about software, and less about hardware or physical world.