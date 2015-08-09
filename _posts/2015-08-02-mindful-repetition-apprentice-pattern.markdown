---
layout: post
title:  "Mindful Repetition: An Apprenticeship Pattern"
date:   2015-08-02
categories: apprenticesihp patterns
---

I've recently started [MakerSquare][ms], which is my first formal, in-person
technical instruction in programming or software engineering since I took a few
CS-related courses in 2011-2012 (Fundamentals, Discrete Math, Object-Oriented Programming, Data
Structures, Algorithms). 

In the intervening three-years, I've done a lot of self-teaching, through

- MOOCs (particularly, [EdX][edx])
- Gameified online tutorials (particularly, [CodeSchool][cs])
- Technical books (particuallry, [O'Reilly][or] and [The Pragmatic Bookshelf][pp] series)

This blog post is about a "pattern" I've landed upon to successfully negotiate a
common problem I've run into in my self-teaching: when not actively using some
recently learned technology, concept, or tool, I'll only retain an understandig
of the "gist" of what it is, or how it works, or how to use it, etc.  This will
happen even after several encounters with the technology, concept, or tool, so
long as those encounters are separated enough in time (on average, let's say, about 3 months). The rest of this blog post spells out why I think that's a problem and how I've gone about solving it.


The Goal: Progressing Towards Expertise
=======================================
I've grown attached to the ["craftsmanship analogy"][craft] for conceiving of the
stages of skill development in a programming domain.  I will probably go deeper into
that analogy in later posts, but for now I will just describe the desired end-state of
expertise in a domain:

**Expertise**

>The expert performer in a particular task environment
>has reached the final stage in the step-wise improvement of
>mental processing which we have been following. Up to this
>stage, the performer needed some sort of analytical principle
>(rule, guideline, maxim) to connect his grasp of the general
>situation to a specific action. Now his repertoire of
>experienced situations is so vast that normally each specific
>situation immediately dictates an intuitively appropriate
>action.

 Dreyfus, Stuart E.; Dreyfus, Hubert L. "A Five-Stage Model of
 the Mental Activities Involved in Directed Skill Acquisition" (February 1980). [PDF][dreyfus].

**Apprenticeship**

 If expertise is the goal, then the starting point, for any given problem domain
 or task environment, is novitiate or apprenticeship.
 What novices or apprentices can do to progress efficiently towards expertise is
 apply certain
 [apprentice patterns][apc], which, in the context of programming, David H. Hoover and Adewale Oshineye define in
 their book of the same name as:

 >Solutions to the dilemmas that are often faced by inexperienced software
 >developers. We’re not referring to technical dilemmas [...] Rather,
 >the dilemmas [..] are more personal, concerning [...] motivation
 >and morale. 

Hoover, David H. and Oshineye, Adewale (2010).  "Apprenticeship Patterns:
Guidance for the Aspiring Software Craftsman" [html][ap].

"Gisty" Understanding As Competence
====================================
The dilemma for apprentices that the pattern of mindful repetition can solve is associated with "gisty understanding".  When you have a gisty understanding of something, you generally know enough to be able to reason
your way through to a complete solution to a problem or where to search for the
information you need to develop a complete solution.  There is a
sense in which this is "good enough" to be get by as a competent developer.
With a gisty understanding, you can act autonomoulsy.  You
won't get stuck and be reduced to flinging guesses of what the correct code
might be at your compiler or
interpreter, and you won't need to ask questions of more experienced programmers before
landing upon correct code (which is not to say experts can't help you improve
upon your correct solution).  

If expertise is the goal, however, then understanding only the gist of something is short
of that goal.  Even though you can autonomously reason and research your way to
a correct solution, you have to break out of the optimal flow to get there,
where the optiomal flow means performing "intuitively appropriate actions" when
presented with problems or tasks.

Examples of gisty understanding:

- Vague undertanding of memoize ("store results, check if result is stored,
  store if not, return stored result") or reducing a collection ("need to set up an
accumulator, iterate through collection, and modify accumulator").
- Having to mentally simulate the general path to touch typing a special
  character.
- Ammending a commit ("I need to use the '--amend' option and do some other
stuff").
- All of the "what most people say" answers that MakerSquare's new head of
curriculum, Kyle Simpson, gives in [this interview][ksi], where he's talking
about good interview questions for JavaScript programmers and what good answers
would look like.

The Naive Approach to the Goal: Mindless Repetition
===================================================
Faced with the obstacle of gisty undertanding, you may attempt to simply
mindlessly repeat tutorials or reading that contains the concepts or practice
you need to get to complete understanding.  For my purposes: "mindlessly" means
a few things: 

1. The repetitions are scheduled haphazardly or ad hoc.
2. The repetitions are performed with no particular goal, outcome, or benchmark in mind other than vaguely "getting
better".
3. The repeater is not consciously focusing on maintaining attention on each
part of the repetition.
4. When repeating a tutorial, the repeater is merely "re-reading" the solutions
rather than implementing them first and re-reading to confirm that a particular implementation
is correct. 

The problem with this sort of mindless repetiion is that certain
psychological/cognitive biases around novelty start to rear their head.  When some problem is
completely new or you are doing some task for the first time, it's easier to be
attentive when presented with the solution to the problem or the method to
perform the task.  Once you've achieved "gisty understanding" of some
domain or task environment, however, your brain is more likely to go offline and
try to focus on other things.  In this way, gisty understanding becomes not only
a stage in moving towards expertise, but a stage that generates psychological
obstacles towards moving to expertise that can easily make it a
learning plateau.

A Better Approach: Mindful Repetition
=====================================
There are different conceptions of what mindfulness is, many of which have come out Buddhist
contemplative practices, but B. Allan Wallace provides a nicely minimal one:

>Attending continusously to a familiar object, without forgetfulness or
>distraction.

[The Attention Revolution][ar]. Wisdom Publications; 1st Wisdom Ed edition (April
13, 2006), p. 14. 

In the context of repeating certain programming tutorials or tasks to gain
expert understanding of them, there are a few things that can promote this
kind of mindfulness.

- Schedule your repetitions with a particular benchmark in mind. If you fail to
reach your benchmark after a couple of repetions, narrow your benchmark or the
scope of your repetiions. When you achieve your narrower goal, re-widen your
scope.
- Create a good environment: "Be at ease.  Be still.  Be vigilant."(Wallace,
17).  Probably best to be by yourself (i.e, not at MakerSquare).  Turn off your
phone and use social media blockers or other tools if you are prone to those
distractions.
- Keep your attention focused on discrete line-by-line "instructions". I.e., pretend
you are a JavaScript interpreter. Parse each statement rather than skimming it.  
- Re-read to confirm understanding rather than passively re-absorb content. 

Useful Applications of Mindful Repetition for Apprentices
=========================================================

- Tutorials or readings that can generally be done in 24 minute to 48 minute
blocks (vimtutor, git immersion, chrome tools debugging blog post, many of our
slide lectures, "code kata" at [codewars][cw]).
- Content that is very "recipe-link" but fundamental to productivity and
understanding (Git, Dev Tools, HTML5 semantics, SQL, HTTP request/response),
entirety of CSS,  RegEx, text-editor shortcuts/commands, CLI, inheritance
patterns/syntax, flash cards of "theory", especially unintuitive theory,  for a particular programming language).
- Canonical programming tasks (construct/sort/search/extract various data
structures out of JSON, tranform one data format to another, map/reduce/filter).

[ms]: http://www.makersquare.com
[cs]: https://www.codeschool.com
[dreyfus]: http://www.dtic.mil/cgi-bin/GetTRDoc?AD=ADA084551&Location=U2&doc=GetTRDoc.pdf
[craft]: http://www.amazon.com/Software-Craftsmanship-Imperative-Pete-McBreen/dp/0201733862/
[edx]: https://www.edx.org/
[or]: http://shop.oreilly.com/category/browse-subjects.do
[pp]: https://pragprog.com/categories
[ksi]: http://insights.dice.com/2015/07/06/interview-qs-javascript-developers/
[ar]: http://www.wisdompubs.org/sites/default/files/preview/Attention%20Revolution%20Book%20Preview.pdf
[cw]: http://www.codewars.com/
[apc]: http://chimera.labs.oreilly.com/books/1234000001813/pr02.html
[ap]: http://chimera.labs.oreilly.com/books/1234000001813/index.html
