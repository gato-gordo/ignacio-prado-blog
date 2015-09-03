---
layout: post
title:  "Flash Memory: Creating a Minimum Viable Product in Two Days"
date:   2015-09-02
categories: project management, product design
---

[MakerSquare][ms] structures program around "sprints" that have different
learning objectives.  We recently completed a sprint where we were tasked with
creating a minimum viable in 36 clock hours (in my case, it was just about 22 hours of
work). I built a basic [flashcard app][fm] and want to reflect here on my basic
goals, approach, and process throughout the sprint. I can't make the full
repository public until the end of the program in sevens, but I'll include
samples of code from different stages of process here.

The Task: Make a Working Prototype in 36 Hours (and learn the MEAN stack)
=========================================================================
The task we were given was to create a minimum viable product in 36 hours that
could be demo'd to the rest of our MakerSquare cohort and staff.  The
pedagogical goal was to get us to manage scope so that we could have a working
prototype to show, jettisoning whatever features couldn't be implemented in
time.

My personal technical goal was to create an app that could do basic [CRUD][crud] on one
resource, utilizing the full [MEAN stack][mest] (Angular for the client, Node/Express
for the server, Mongo for persistence).

The Product Idea: Basic Flashcard App
=====================================
I love flashcards as a way of learning technical theory or recipes, but they are
not the most portable items once they start to accumulate, which they usually do
quite rapidly.  I've often thought a web app that you could use to
make, store, and access flashcards would be useful to overcome these portability
and scaleability limits.  As with ebooks or digital music, you would be trading the benefits of having a
tangible physical object for that portability and scaleability (though you could,
theoretically, get the benefits of both by creating an app that digitally
abstracts physical flashcards the way [Protyping on Paper][pop] does physical
design mockups).

A full-fledged app would have users, the ability create multiple/different decks
of cards so that they could be organized by category, and eventually maybe some
basic social features so that you could import other peoples cards (and upvote
individual cards as particularly good, as a signal to other users searching for cards related to
a particular category).  I decided that all of those features were unecessary to
demonstrate core functionality (and to get my technical learning goals
accomplished), so I chose to implement the app with one card
deck as the MVP.

Process Map: 5 Stages 
=========================
1) Make an in-browser version of the full app, with no persistence and a simple
in memory data-model, using jQuery
2) Create the persistence tier and server that would function as a RESTful api,
receiving and responding to requests by a client with JSON data
3) Hookup the server to the client by refactoring jQuery to make requests of the
server to construct/update the browser's in-memory data model
4) Refactor the client code into an Angular app that separates concerns
5) Enhance the UI/UX and refactor for maintainability and extensibility with
whatever time remains.

The benefits of this process map is that I knew that, whatever problems I might
have, completing (1) would give me enough to have something to demo.

Stage 1: In-Browser jQuery App
===================================================
This process went very quickly.  It's something that I could have done
competently before MakerSquare, though probably not quite as quickly.  The first
step was creating a generic, Bootstrap layout.  Hard-coding a deck object, then
creating event listeners that triggered controller actions that could cycle
through the deck and shuffle it. Then I removed the hard-coded object deck,
implementing the form to create, edit, and delete instances of cards that would
populate the deck object.  B/c there was no persistence tier, the view page
couldn't acces the object created on the manage page.  I could have refactored
into a single-page app that used jquery to update the dom between page views,
but I decided that it was a better use of my time to go ahead withcreating the
server and persistence tier.  If things went terribly wrong, it would be pretty
easy for me to go back and make those changes to salvage the demo.


Stage 2: Making the Server and Persistence
=====================================
-Borrowed a lot of could from earlier sprints
-route to controller
-why mongo?
-railsy
-model very thin (a real production would probably include a lot of validation
here)
-test the server in isolation using postman

Stage 3: Hook up Client to Server
=================================
-show examples of refactoring jquery

Stage 4: Refactoring into Angular
================================
-errors having to do with correctly importing modules
-1 deck factory for 2 different controller/views (make sense, as there is only 1
type of resource that both views/controllers are manipulating)

Step 5: UI/UX enhancement and refactoring
=========================================
-color
-clip animation
-consistent method names at all levels
-mongoose refactoring 

A More Complete Demo Would Have Had
==================================
-tests
-responsive layout (pagination)


[ms]: http://www.makersquare.com
[fm]: https://salty-thicket-9132.herokuapp.com/
[mest]: http://mean.io/#!/
[crud]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[pop]: https://popapp.in/
