---
layout: post
title: Recap Software Complexity (2) PCSD
comments: true
category:
- software-engineering
- software-complexity
- recap
- english
---

# 1. Prologue

The first material that I will recap is Chapter 1 of Principles of Computer System Design: An Introduction(PCSD)_ by J. H. Saltzer and M. F. Kaashoek, 1st Edition, 2009. It's a textbook for [MIT 6.033 Computer System Engineering](http://web.mit.edu/6.033/www/general.shtml) and I took a Copy-to-China version at Fudan University. When I took it in junior year, I was ignorant of many topics covered in this book. Every time I re-visit it, I always got some new inspirations.

_Chapter 1 Systems_ is the underlie concepts and guideline of the whole book. It talks about what're systems and complexity, the source of complexity, and how to cope with complexity in high-level. Authors integrate many design principles in sections, which I will review them at last. The following chapters of the book usually focus on one specific apsect or property.

# 2. Recap

## 2.1 Common Problems in Systems

The chapter starts with dividing common problems one encounters in many kinds of systems into four categories: emergent properties, propagation of effects, incommensurate scaling, and trade-offs.

**emergent properties** _are properties that are not evident in the individual components of a system, but show up when combining those components, so they might also be called surprises._ 

**propagation of effects** describe small changes in some components will affect many other components in the system. _Folk wisdom characterizes propagation of effects as: “There are no small changes in a large system”._

**incommensurate scaling** means: _as a system increases in size or speed, not all parts of it follow the same scaling rules, so things stop working. It is usually the factor that limits the size or speed range that a single system design can handle._

**trade-offs**. In gaining some goodness, the system will lose goodness elsewhere in the system. It mentions two common forms of trade-offs: _waterbed effect(pushing down on a problem at one point causes another problem to pop up somewhere else) and binary classifiation_ (false positive and false negative increasing at the same time)

## 2.2 From Systems to Complexity

In this book, **a system** is defined as **a set of interconnected components that has an expected behavior observed at the interface with its environment.**. _The underlying idea is to divide the world into two groups: those under discussion as part of the system, those not under discussion as part of the environment. There is interaction between a system and its environment; these interaction are the interface between the system and the environment. Identifying the components of a system depending on the point of view, with different _purpose_ and _granularity_.

Components of systems can be systems from a difference point of view. To avoid recursion in their writings, authors and designers have come up with a list of synonyms trying to capture the same concept: _systems, subsystems, components, elements, constituents, objects, modules, submodules, assemblies, subassemblies, and so on._

PCSD lists five signs of complexity:

- Large number of components. 
- Large number of interconnections. 
- Many irregularities. 
- A long description.
- A team of designers, implementers, or maintainers.

Then, it discusses the sources of complexity, in which _two merit special mention: 1. number of requirements and 2. one particular requirement - maintaining high utilization._

## 2.3 Complexity

## 2.3.1 Sources of Complexity - Number of Requirements

_A primary source of complexity is just the list of requirements for a system. Each requirement, viewed by itself, may seem straightforward. Any particular requirement may even appear to add only easily tolerable complexity to an existing list of requirements. The problem is that the accumulation of many requirements adds not only their individual complexities but also complexities from their interactions. This interaction complexity arises from pressure for generality and exceptions that add complications, and it is made worse by change in individual requirements over time._

_Meeting many requirements with a single design is sometimes expressed as a need for **generality**. Generality may be loosely defined as “applying to a variety of circumstances.” Unfortunately, generality contributes to complexity, so it comes with a tradeoff, and the designer must use good judgment to decide how much of the generality is actually wanted._

_Finally, a major source of complexity is that requirements change._

## 2.3.2 Sources of Complexity - Maintaining High Utilization

Let's recall [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law), which states the theoretical speedup of a system whose resources are improved. To put it simply, the scarce resource matters. To improve the utilization of that resource, the system may become more complex, which in return need more effort the next improvement will require.

## 2.3.3 Coping with Complexity

Some techniques tackling complexity can be loosely divided into four general categories: _modularity, abstraction, layering, and hierarchy_.

**Modularity** is the divide-and-conquer technique: analyze or design the system as a collection of interacting subsystems, called _modules_. The power of this technique is to divide the interactions into those inside a module and those outside a module. One feature is to replace an inferior module with an improved one, without completely rebuilding the whole.

**Abstraction** comes from although there are lots of ways of dividing a system up into modules, some of these ways will prove to be better than others. Thus the best divisions usually follow natural or effective boundaries. They are characterized by fewer interactions among modules and by less propagation of effects from one module to another. More generally, they are characterized by the ability of any module to treat all the others entirely on the basis of their external specifications, without need for knowledge about what goes on inside. This additional requirement on modularity is called abstraction.

The goal of minimizing interconnections among modules may be defeated if unintentional or accidental interconnections occur as a result of implementation errors or even well-meaning design attempts to sneak past modular boundaries in order to improve performance or meet some other requirement. Software is particularly subject to this problem because the modular boundaries provided by separately compiled subprograms are somewhat soft and easily penetrated by errors in using pointers, filling buffers, or calculating array indices. For this reason, system designers prefer techniques that enforce modularity by interposing impenetrable walls between modules. These techniques ensure that there can be no unintentional or hidden interconnections, e.g. Clients-and-Services and Virtualizations.

Closely related to abstraction is the robustness, or _be tolerant of input and strict on output._ One robust practice is to providing two modes of operations: development and production. In development mode, every input is checked carefully and refuse to accept anything out of specification, thus allowing immediate discovery of problems near their source. In production mode, modules accept any input that they can reasonably interpret.

**Layering** is a method of module organization to reduce interconnections. In designing with layers, one builds on a set of mechanisms that is already complete (a lower layer) and uses them to create a different complete set of mechanisms (an upper layer). A layer may itself be implemented as several modules, but as a general rule, a module of a given layer interacts only with its peers in the same layer and with the modules of the next higher and next lower layers. That restriction can significantly reduce the number of potential intermodule interactions in a big system.

**Hierarchy** is the result by recursively assembling subsystems to produce a large subsystem, until the final one. The advantage of a hierarchy is that the module designer can focus just on interactions with the interfaces of other members of its immediate subsystem.

These four techniques are a major help, but they aren't enough to keep the resulting complexity under control. The reason is that all these techniques assume that the designer understands the system being designed, where in the real, fast-changing world of computer science, it's hard to choose the right one (any of the four).

# 2.3.4 Indirection with Names

_Names_ are what one module refers the other modules. Making the choice of combining a specific implementation of a module interface is called _binding_. A name helps to delay binding. By the time the feature is actually invoked, the name must be bound to a real implementation. Using a name to delay or allow changing a binding is called _indirection_. After all, _any problem in a computer system can be solved by adding a layer of indirection_(by David Wheeler).

# 3. Summary

Modularity is the concept connecting systems and complexity. Modularity also plays a core role in the whole book, as authors say

> Modularity appears in each engineering topic either as one of the goals of that topic or as one of its design cornerstones. Words from chapter titles suggest this theme. Abstractions and layering are particular ways to build on modularity. Naming is a fundamental mechanism for interconnecting and replacing modules. Clients and services and virtualization are two ways of enforcing modularity. Networks are built on a foundation of modularity. In fault tolerance, the module is the unit that limits the extent of failure. Atomicity is an exceptionally robust form of modularity that the designer can exploit to obtain consistency. Finally, protection of information involves further strengthening of modular walls.

The chapter starts with common problems in systems. Then they reaches definitions of systems and other related concepts like environments, etc. Complexity is the natural property in systems with many interconnections, irregularies, and lots of people in separate roles.

# Asides

## A.1 Design Principles

These are principles appearing in this chapter.

**Principle of escalating complexity**
Adding a requirement increases complexity out of proportion.

**Avoid excessive generality**
If it’s good for everything, it’s good for nothing.

**The law of diminishing returns**
The more one improves some measure of goodness, the more effort the next improvement will require.

**The unyielding foundations rule**
It is easier to change a module than to change the modularity.

**The robustness principle**
Be tolerant of inputs and strict on outputs.

**The safety margin principle**
Keep track of the distance to the cliff, or you may fall over the edge.

**Decouple modules with indirection**
Indirection supports replaceability.

**The incommensurate scaling rule**
Changing any system parameter by a factor of 10 usually requires a new design.

**Design for iteration**
You won’t get it right the frst time, so make it easy to change.

**Keep digging**
Complex systems fail for complex reasons.

**Adopt sweeping simplifcations**
So you can see what you are doing.

## A.2 One Quote

It's just the quote that I put at the end of every page of this blog. This quote comes from Poe's novel _The Murders in the Rue Morgue_(1841). Detective Dupin comments on _the sight if the matter as a whole_ comparing to the _vision by holding the object too close_. He believes the more important knowledge is invariably superficial. **The depth lies in the valleys where we seek her, and not upon the mountain-tops where she is found.** . Using the metaphor of looking at star light, he thinks **by undue profundity we perplex and enfeeble thought**.

Authors of PCSD choose the latter sentence in hold. I appreciate the former one and put it at the end of every page of this blog.