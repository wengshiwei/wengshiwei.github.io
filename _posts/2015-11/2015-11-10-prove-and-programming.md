---
published: false

layout: post
title: 证明、编程、复杂度
comments: true
category:
- programming
- chinese
---

本文和Computer-assisted proof无关。

# 1. 问题由来

没错，我考虑的就是编程的essential complexity。数学证明可以看不懂，但是证明的流程是清晰的。编程就不一样。编程需要debug。debug需要run。


[An analogy between mathematics and programming by Dijkstra](http://www.dijkstrascry.com/node/75)

> By way of introduction, I should like to draw attention to the not unknown fact that it is impossible to prove a mathematical theorem completely, because when one thinks that one has done so, one still has the duty to prove that the first proof was flawless, and so on, ad infinitum. So much for human fallibility. One can never guarantee that a proof is correct, the best one can say is "I have not discovered any mistakes." We sometimes flatter ourselves with the idea of giving watertight proofs, but in fact we do nothing but make the correctness of our conclusions plausible. And let us be honest: even extremely plausible. We achieve this high degree of plausibility by a means specially designed for this purpose, viz. theorems. On the one hand, so many people have, each in their own way, derived these theorems, that there is a non-negligible probability that they do indeed follow from the axioms, on the other hand, the pretended conclusions are subject to conditions so orderly that the user's task of showing that he has applied the theorem correctly is not too cumbersome.

> The programmer is in exactly the same position, since it is not possible for him to prove the correctness of his programs. And yet the correctness of the programs is of vital importance: everybody working with an automatic computer knows from sad experience that it is very easy to produce an awful lot of numbers, but he also knows that they are worthless if their correctness is subject to doubt. Instead of only starting with envy at the fabulously convincing power of the proofs in pure mathematics, it seems more fruitful to me to inquire whether we can learn from the way the pure mathematician works. He has theorems, we have subroutines. A theorem, however, is (see above) only useful if we can apply it under a minimum number of clear conditions. In the same way the usefulness of a subroutine (or, in a language, a grammatical instruction) increases as the chance decreases, that it will be used incorrectly. From this point of view we should aim at a programming language consisting of a small number of concepts, the more general the better, the more systematic the better, in short: the more elegant the better.

> For purposes of clarification let us consider ordinary English as language, in the use of which, however, certain additional rules must be obeyed. The simplest of these may be of the following nature: words of more than 15 letters are forbidden; the total number of letters of three consecutive words may not be greater than 40; sentences of more than 60 words are not allowed; in one and the same sentence the same word may not be used twice as a subject; furthermore a list of, say 2000, words is given, that are so rarely used that they have been forbidden for the sake of convenience, etc, etc...


[An analogy between computer programs and mathematical proofs](http://www.dijkstrascry.com/node/80)

[一种新的操作系统设计](http://www.yinwang.org/blog-cn/2013/04/14/os-design/)



# -. [程序的本质复杂性和元语言抽象](http://coolshell.cn/articles/10652.html)

> 每种组件形式都代表了特定的抽象维度，组件复用只能在其维度上进行抽象层次的提升

> 程序的本质复杂性就是逻辑，非本质复杂性就是控制

> 绝大多数程序不够简洁优雅的根本原因：逻辑与控制耦合

> 通过元语言抽象让逻辑和控制彻底解耦

证明不需要控制。控制需要时间。

# -. [类型的本质和函数式实现](http://coolshell.cn/articles/10169.html)

> 类型的本质是由操作(Operation)和操作间的关系或不变式(Invariant)所定义的，我们称之为类型规范(Type Specification)

# 4. 关于2和3

# 5. 间接与抽象


