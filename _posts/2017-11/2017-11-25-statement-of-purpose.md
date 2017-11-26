---
layout: post
title: Statement of Purpose for PhD Programs
comments: true
category:
- programming-languages
- english
---

# Before

This is the prototype of my Statement of Purpose (SoP) for PhD programs. I will shrink it and adding some university and professor related content for different programs. I read some requirements and guides on SoP. Some emphasize objective (like a proof) and some emphasize subjective (like a letter). I am a little confused.

I appreciate any feedback and suggestions. Tell me in replies, Facebook comments, emails(wengshiwei@gmail.com), or any IMs.

My first silly question: is `PL` a valid abbreviation for admissions committee like `PhD`, or shall I use Programming Langauges?

# Main

I, Shiwei Weng, will finish CS master program in Johns Hopkins University this winter and apply for a **CS PhD program**. After some related courses, **one independent study on package management system**, **lab research work on static analysis from this summer**, **three semesters of PL seminar (attend or talk)**, **one PL summer school**, **a whole volunteering work for SPLASH 2017**, and what's more, observation and cooperation with my advisor, Dr. Scott Smith, and other smart persons in my group, I am eager to become a PhD student. Rigorous academic training is my best choice to become an expert, who can solve theoretical problems and impact the industry.

I asked for doing **independent study** with my advisor, Dr Scott Smith, even before enrollment. After emails and talking, Scott and I chose the topic of **language package management system**, from my first six ideas, including language translation, IDE extension for user interaction patterns, better language keywords, better distinguish between copy-by-value and copy-by-reference. That time I was totally new to PL research. All these came from previous practice and thoughts on how to make programming better.

As the study going, I found this topic is not widely studied in academic and not every language well separates a package manager for that language and a package manager for project in that language. I kept a weekly reporting and discussion with Scott. I gradually realized it's a theoretical problem dealing with dependency and versions, a practical problem since many languages invent package managers after prevailing, and a software engineering problem about enhancing scalability of legacy code.

The result of this study is **a technique report with details of concepting, survey results and discussion on its design**, and **a talk in our PL seminar**. OOPSLA 2017 has a paper titled Déjà Vu, studying code duplicates on GitHub. I think the discussion on lockfiles and version control systems in my report can explain some phenomenon in that paper. I also talked this topic with many friends working in industry, and conclude that the design of package managers is affected by the preference of best practice in that language community.

From this summer, I worked on **Demand-Driven Program Analysis (DDPA) project** in my lab. It's the first demand-driven analysis for high-order languages. My work is **adding contracts to the language for analysis** and **research how the contracts and the analysis affect each other**. It's challenging at that time I was ignorant of static analysis and abstract interpretation. The soundness proof seemed mystical to me. For the code part, the language for analysis is an ANF language, the analysis tool is in OCaml, and the benchmark is in Scheme. It took me about a month, to read papers in my group and enough understanding of some reference papers. As Scott suggested, I analyzed all ANF program samples in papers by hand correctly, then demonstrated to group members. I strategically postponed some parts in paper for example a push-down system for optimization. By re-read these papers recently did I gain a much better understanding. For the code, I figured out the main steps of the analysis framework. I studied implementation of contracts and blaming in (Typed) Racket. Though I manually wrote ANF program in the summer, but that I worked on a sugar-ed language and a translator based on Nanopass. I need to thank group members on their patient to explain OCaml, Racket and background knowledge when I got stuck.

I will finish my master program this winter, but I will **continue this project as a research assistant next spring**, waiting for a possible PhD offer. My work is an extension of existing analysis tool but after gaining more insight, I was also thinking about improving its core part. To develop my independent study report into a paper is also a potential work.

**The First Programming Language Implementation Summer School (PLISS 2017)** and **SPLASH 2017** play a significant role for me. I met young and prestige researchers in PL community. Summer school had introduction and in-depth discussion on many aspects in PL. I got travel fund from a mentoring workshop in SPLASH and I applied for and worked as a volunteer to participate the whole conference. I almost asked every PhD student I met on his or her research and daily life, and I asked many professors on how they co-work with their students. For me, to de-mystery is to ensure. I have known the panorama and close-ups of academic world. I want to join them.

These community events greatly open my eyes. I wrote blogs, commenting every talk I heard. One section on gradual typing is inspiring for its quality and presenting. Since these papers is already on our PL seminar’s schedule, I gave a talk on this. I also consulted other state-of-the-art materials. I did some tests and find both Flow, a static checker by Facebook and our DDPA can find errors statically while they mainly emphaze run-time optimization.

I got A+ in my advisor's PL course. I started programming in Pascal at 13 for Olympiad in Informatics. In undergrad, I won National Scholarship (for GPA top one) and other scholarship. Now I took courses challenging or which I was unfamiliar but think to be important e.g. distributed systems, natural language processing, theory of computation and cryptography. I am not the most expert among many areas among friends but I am always the first person they would to ask: Shiwei, what's PL? What's NP problem? How does TCP work? Depending on their background, it takes several minutes or hours to explain them, in high level and with enough details. I am willing to share. I believe without thorough understanding one can never explain to others. I worked as two course assistants for Graphics and Machine Learning and I will work as a teaching assistant of my favorite PL course next semester.

I appreciate your reading and hope we can work together.

# Reference
- [http://matt.might.net/articles/how-to-apply-and-get-in-to-graduate-school-in-science-mathematics-engineering-or-computer-science/](http://matt.might.net/articles/how-to-apply-and-get-in-to-graduate-school-in-science-mathematics-engineering-or-computer-science/)
- [http://www.cs.umd.edu/grad/writing-statement-of-pupose](http://www.cs.umd.edu/grad/writing-statement-of-pupose)
- [http://www.cs.cmu.edu/~harchol/gradschooltalk.pdf](http://www.cs.cmu.edu/~harchol/gradschooltalk.pdf)
- [http://www.cs.cmu.edu/~pavlo/blog/2015/10/how-to-write-a-bad-statement-for-a-computer-science-phd-admissions-application.html](http://www.cs.cmu.edu/~pavlo/blog/2015/10/how-to-write-a-bad-statement-for-a-computer-science-phd-admissions-application.html)
- [http://web.mit.edu/msrp/myMSRP/docs/Statement%20of%20purpose%20guidelines.pdf](http://web.mit.edu/msrp/myMSRP/docs/Statement%20of%20purpose%20guidelines.pdf)
- [https://grad.ucla.edu/asis/agep/advsopstem.pdf](https://grad.ucla.edu/asis/agep/advsopstem.pdf)
- [https://career.ucsd.edu/_files/statmentofintent.pdf](https://career.ucsd.edu/_files/statmentofintent.pdf)