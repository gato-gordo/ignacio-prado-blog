---
layout: post
title:  "Making a MEAN Viable Product: a Two-Day Sprint Reflection"
date:   2015-09-02
categories: [project management, agile, MEAN stack, product design]
---

[MakerSquare][ms] structures the first half of its program around short sprints that have different
learning objectives.  We recently completed a sprint where we were tasked with
creating a minimum viable product (MVP) in 36 clock hours (in my case, it represented about 20 man-hours). I built a basic [flashcard app][fm] and want to reflect here on my 
goals, approach, and process throughout the sprint. I can't make the full
repository public until the end of the program at the end of October, but I will walk through code samples from different stages of the sprint.



The Task:  Build a Working Prototype of a Product in 36 Hours (and practice using the full MEAN stack)
=====================================================================================
The task we were given was to create a MVP in 36 hours that
could be demoed to the rest of our MakerSquare cohort and staff.  The
pedagogical goal was to get us to learn how to manage scope so that we could have a working
prototype to show.  My personal technical goal was to create an app that could do basic [CRUD][crud] on one
resource, utilizing the full [MEAN stack][mest] ([AngularJS][ang] for the client, [Node.js][node]/[Express][express]
for the server, and [mongoDB][mongo] for persistence).

*To scroll through code samples, simply drag with two-fingers in the direction you want to scroll.*

The Product Idea: Basic Flashcard App
=====================================
I love flashcards as a way of learning technical theory or recipes, but they are
not very portable once they start to accumulate (which they usually do
quite rapidly).  I've often thought that a web app that could be used to
make, store, and access flashcards would be useful to overcome these portability
and scaleability limits.  As with ebooks or digital music, you would be trading the benefits of having a
tangible physical object for that portability and scaleability.  [As an aside, you could, theoretically, get the benefits of both by creating an app that abstracts digital versions of physical flashcards the way [Prototyping on Paper][pop] does physical design mockups.  You could also close the digital-physical loop by having an easy mechanism for exporting your library of digital flashcards so that they could be printed onto cardboard of standard index-card size.]

A full-fledged app would have users, and the ability create multiple/different decks
of cards so that they could be organized by category.  Eventually, it would also work well as an app with some basic social features.  In the social version, you might be able to search through other users' cards on the site and import them into your own decks.  You would also be able to upvote others'cards as particularly good or useful, so they would show up higher in a ranked search for cards in a particular category.

For the purposes of creating a MVP to demo, however, I ultimately decided that all of these additional features were both unreasonable to tackle in the time I had and unecessary to demonstrate core functionality (as well as unecessary to accomplishing my technical learning goals).  I thus chose to implement the MVP with no concept of distinct users or distinct decks.

My Process Map: 5 Stages 
=========================
1. **Make an in-browser version of the full app**, with no persistence and a simple in-memory data model, using jQuery.

2. **Create the persistence tier (using mongoDB) and a server (using NodeJS) that would function as a RESTful api (using Express)**,
receiving and responding to requests by an http client (I used [PostMan][pm]) with JSON data.

3. **Hookup the server to the client** by refactoring the jQuery into a client that make requests of the
server to construct/update the browser's in-memory data model (the client
would also render the html and respond to user actions by translating them into intents to do something on the server).

4. **Refactor the client code into an Angular app** that separates the logic for
updating the UI from the logic that makes changes to the data model, while taking advantage of Angular's [two-way data binding][twdb]. 

5. **Enhance the UI/UX** and **refactor code at all levels** for maintainability and extensibliity.

The benefits of this process map is that I knew that, whatever problems I might
have, completing (1) would give me enough to have something to demo.

Stage 1: Creating In-Browser jQuery App
=======================================
This stage was completed very quickly.  It's something that I could have done
competently before MakerSquare, though probably not quite as quickly.  I decided
early on that the app would have two views: (i) one for viewing and cycling through
the cards in the deck, and (ii) one for managing the deck by adding, editing, or
deleting cards in it.

After creating a generic [Bootstrap][boot] layout that would be common to both pages, I
tackled (i) first in isolation.  I used a simple hard-coded array of JS objects to represent my deck, along with some associated methods for cycling through it:
{% highlight js %}
var javascriptDeck = {
    currentCard: 0,
    forward: function(){
        var notLast = this.currentCard !== this.cards.length - 1;
        this.currentCard  = notLast ? this.currentCard + 1 : 0; 
        return this.currentCard;
    },
    backward: function(){
        var notFirst = this.currentCard !== 0;
        this.currentCard = notFirst ? this.currentCard - 1 : this.cards.length - 1;
        return this.currentCard;
    },
    cards: [
        {
            front: "Closure",
            back: "A function that retains ongoing access to the variables of the context in which it was created (even after the outer function calls it was created within have returned).",
        },
            {
            front: "Non-blocking i/o",
            back: "In non-blocking i/o, the function that initiates the i/o returns immediately, allowing other code to execute while the i/o operations run in the background.  Operations that depend on the result of the i/o operations are handled by either callbacks, promises, or some other asynchronous form of execution that are registered with the non-blocking function.",
        },
        {
            front: "Higher-order Function",
            back: "A function that operates on other functions by either taking them as input or returning them when invoked.",
        }, [..]
{% endhighlight %}
The rest of the jQuery was listening for clicks to trigger the `#forward` and `#backward` methods that would update the DOM with the current card (as well as toggling the front/back view of the card when you clicked on it). I then removed the hard-coded `cards`array and implemented the form to create, edit, and delete instances of cards that would dynamicaly populate the deck array.  Because there was no persistence tier, the view-deck page couldn't access the cards created on the manage-deck page.  I could have refactored the code at this point into a single-page app so that both views would have access to same the cards, but I decided that it was a better use of my time to go ahead with creating the server and persistence tier.  If things went terribly wrong, it would be pretty easy for me to go back and make those contemplated changes to salvage the demo.

Stage 2: Creating the Server and a Persistence Tier
=====================================================
In setting up my server, I borrowed a lot of boiler plate structure from our earlier sprints.  My main task was creating a router with RESTful urls that mapped to controller actions for creating, reading, updating, and deleting cards.  Those controller actions would (1) call model methods that would update the database and (2) respond to the client with the results of those updates.  Consequently, most of my server logic was in the controller.

My naming of controller actions was very Railsy, because that's how I first learned to do CRUD: `#index` (to get all cards), `#read` (to get an individual card), `#create` (to make a new card), `#update` (to change data of an existing card) and `#destroy` (to remove an existing card).  The corresponding routes looked like so: 
{% highlight js %}
app.post('/', cardController.create);
app.get('/:card_id', cardController.read)
app.get('/', cardController.index);
app.put('/:card_id', cardController.update);
app.delete('/:card_id', cardController.destroy);
{% endhighlight js %}
These routes had all been configured using [Expresss middleware][em] (a mechanism for filtering all handling of server requests through sequential processing functions or "middleware") to be prefaced with '/api/cards':
{% highlight js %}
app.use('/api/cards', cardRouter);
{% endhighlight %}
Because the `Card` model could inherit a lot of methods from [Mongoose][mongoos]/mongoDB, it was super thin: 
{% highlight js %}
var CardSchema = new mongoose.Schema({
  front: {
    type: String,
    required: true,
    unique: true
  },
  back: {
    type: String,
    required: true
 } 
});
{% endhighlight %}
A production app would have done more data validation in the model, as well as add more properties to the schema like timestamps for storing when the card was created or last modified. I decided, however, that this was an unproductive use of time for the demo.  

As I said earlier, most of the server logic was in the controller actions.   For an example, here is the `#update` method: 
{% highlight js %}
  update: function(req, res, next){
    var id = req.params.card_id, 
        front = req.body.front, 
        back  = req.body.back,
        update = Q.nbind(Card.findByIdAndUpdate, Card);
    update(id, {"front": front, "back": back}, {"new": true})
      .then(function(card) {
        res.json(card);
      })
      .fail(function (error) {
        next(error);
      });
  },
{% endhighlight %}
The first three variable declaration/assignments simply pull data from the client request.  The `:card_id` was part of the REST endpoint and gets accessed through `req.params`.   The data in the request body reaches the controller as JSON already-parsed by [body-parser][bp], Express middleware that was installed and required as a Node module.  As a result of including  this module, parsing the JSON in the body of every request takes just one line of code:

 `app.use(bodyParser.json());`.

The fourth variable uses the [Q library][q], which transforms asynchronous functions designed to take callbacks into functions designed to use the JS promises API, as described [here][qwiki]. The first argument to `Q#nbind` is the asynchronous function being transformed---in this case, `#findByIdAndUpdate`. The other arguments get bound to the parameters of the returned function, similar to the way to the core JS `Function.prototype.bind` function works.  `#nbind` then returns a function that uses the promises api and has the bound parameters just specified.

`#findByIdAndUpdate` is a Mongoose method ([docs][fbiau]) that does what it says:

1. The first argument is the id the method uses to locate the mongoDB document (grabbed from the RESTful url parameter as described above).
2.  The second argument is the set of keys/values being updated in the mongoDB document.
3.  The third argument is an option specifying that the returned result of the query should be the new/updated mongoDB document.  

While I don't have time to fully go into promises in this post, the `#then` invocation, as used in the `#update` method provided above, handles the happy path of a database query and `#fail` handles the sad path.  As the actual [promises api][papi] specifies, the `#then` block can be used to handle failure as well as success, and `#fail` has been deprecated in favor of `#catch`. Consequently, this pattern I use to handle the happy and sad paths of database queries in all my controller methods will need to be refactored (I only read about the deprecation of `#fail` after completion of the MVP).  Finally, the upated card back is sent back as JSON to the client, which it can use to update its in-memory data model of the deck of flashcards. 


Stage 3: Hooking up the Client to Server
========================================
Since most of the code in this stage got refactored in Stage 4, I'll  give just one example of how the jQuery client issued a request to update a card on the server and then used the server's response to update its in-memory data-model.  First, the form submission is handled that contains the information to pass to the server:
{% highlight js %}
[...]
var values = $(this).serializeArray();
var action = $(this).find('[type="submit"]').text();
[...]
} else if (action === 'Edit'){
  var index = values[0]['value'];
  var id = values[1]['value'];
  var card = javascriptDeck.parseCardForm(values.slice(2));
  javascriptDeck.putCard(card, index, id);
}
{% endhighlight %}
The `id` is pulled from a hidden input field and will be used to construct the RESTful endpoint to update the card resource (`index` is only used on the client side to update the in-memory deck of cards array).  The card returned from the invocation of `javascriptDeck#parsedCardForm` makes up the body of the request to the server ( `{ front: frontValue, back: backValue }` ).   The request gets made in `javascriptDeck#putCard`:
{% highlight js %}
  putCard: function(card, index, id){
    var deck = this;
    var url = cardsServerUrl + '/' + id;
    $.ajax({
      url: url,
      method: 'PUT',
      data: JSON.stringify(card),
      contentType: "application/json",
      dataType: 'json'
    }).
    then(function(data){
      deck.updateCard(data[0], index);
    });
  },
{% endhighlight %}
The ajax call returns a promise handled by the `#then` method, which takes a callback.  The `data` parameter to the callback is an array whose first element is the updated card, which is passed to an `#updateCard` method invoked on the deck object.  That method takes care of the in-memory update. The `index` parameter is used to track the in-memory location of what is being updated: 
{% highlight js %}
updateCard: function(card, index){
  this.cards[index] = card;
  this.renderTable();
}, 
{% endhighlight %} 
The last line of code re-draws the table of existing cards to reflect the updates to the data model.  It also provides a good segway to explain part of what refactoring the client into Angular will accomplish.  With jQuery, keeping the DOM up-to-date with our in-memory data model must be done through explicitly written <em>imperative</em> code.  Angular directives, by contrast, are <em>declarative</em> in the way html is.  Putting them in a view creates a [two-way data binding][twdb] between that part of the view and a data-model represented in a client-side controller.   The framework itself does the work of updating either side of the declared relationship when the other side changes.  This allows you to write less and more modular code for a dynamic app, though it takes more overhead to set up.

Stage 4: Refactoring into Angular
================================
As described at the end of the last stage, the jQuery client had to update view with explicit programmatic logic that manipulated DOM.  In code, the process looked like this:
{% highlight js %}
  renderTable: function($el){
    $el = $el || this.$el;
    $el.html('');
    var $table = $('<table class="table"><tbody></tbody></table>');
    this.cards.forEach(function(card, index){
      $table.append(
        '<tr>' +
          '<td>' + card.front +'</td>' +
          '<td><a class="edit" href="' + index + '">Edit</a></td>' +
          '<td><a class="delete" href="' + index + '">Delete</a></td>' +
        '</tr>'
      );
    });
    $el.append($table);
  },
{% endhighlight %}
In Angular, by contrast, we have static html with embedded Angular directives:
{% highlight js %}
<table class="table table-striped">
  <tbody>
    <tr ng-repeat="card in cards">
      <td>{% raw %} {{card.front}} {% endraw %}</td>
      <td><a ng-click="editCard($index)">Edit</a></td>
      <td><a ng-click="removeCard(card._id, $index)">Delete</a></td>
    </tr>
  </tbody>
</table>
{% endhighlight %}
This html snippet never has to be explicitly re-rendered when the number cards or content in invidual cards changes.  The re-rendering happens automatically when the in-memory data model changes, and the data model changes through the registered click handlers (with more time, I would have re-factored this into custom directives, so that there was no in-line logic with explicit function calls handling clicks).

The logic in Angular for updating the deck data model is actually fairly similar to the way the jQuery version of the client worked.  For updating individual cards, I used the Rails convention that an `#edit` action provides a view with a form and an `#update` action handles the submission of the form by invoking a method that ultimately changes persisting data (in Rails, that would be a model method that extends ActiveRecord; in Angular, it's a factory or service method). The only direct result of the `#edit` action is updating the values of several data model variables: 
{% highlight js %}
$scope.editCard = function(index){
  $scope.formCard = $scope.cards[index];
  $scope.formHandler = $scope.updateCard;
  $scope.currentCard = index;
  $scope.formAction = "Edit";
}
{% endhighlight%}
Re-setting these variables in the controller, however, automatically redraws the card form on the manage-deck view. It also changes the purpose of that form from that of a default "create-card form" into that of an "update-card form".  This is again done through the two-way data binding provided Angular:
{% highlight js %}
<form id ="form-card">
  <div class="form-group">
    <label for="form-card-front">Front</label>
    <input name="front" type="text" ng-model="formCard.front"class="form-control" id="form-card-front" placeholder="30 characters or less">
  </div>
  <div class="form-group">
    <label for="form-card-back">Back</label>
    <textarea rows="15" name="back" class="form-control" ng-model="formCard.back" id="form-card-back" placeholder="300 characters or less"></textarea>
  </div>
  <button type="submit" class="btn btn-primary" ng-click="formHandler(formCard, currentCard)">{{formAction}}</button>
</form>
{% endhighlight %}
In this snippet, all of the following values get automatically updated to reflect the values just set in `#editCard`: `formCard.front`, `formCard.back`, `formHandler`, and `formAction`.  The user can then edit the existing front and back of the card to whatever new values she likes, and the controller's `#updateCard` action will handle the submission.  The only thing the `#updateCard` action does is call the corresponding factory method, which is passed the new values grabbed from the form: 
{% highlight js %}
$scope.updateCard = function(card, index){
  Deck.updateCard(card).then(function(cards){
    $scope.cards[index] = cards[0];
    $scope.setDefaultForm();
  });
}
{% endhighlight %}
The factory `#updateCard` method looks very similar to the `#putCard` method from my jQuery client that was described in Stage 3:
{% highlight js %}
  var updateCard = function(card){
    var url = '/api/cards/' + card._id;
    return $http({
      method: 'PUT',
      url: url,
      data: {front: card.front, back: card.back }
    })
    .then(function(resp){
       return resp.data;
    });
  };
{% endhighlight %}
One difference, however, is modularity.  In my jQuery client, all of my programmatic logic--whether for updating a view, handling a form submission, or making a request of the server--was defined as methods of a single `javascriptDeck` object.  That was not a necessity.  I could have re-factored my design to be more modular, and I probably would have if I knew the jQuery client was meant to be more than transitional. By contrast, Angular is designed to be modular by default.  I would have actively had to work against the grain of the framework to have business logic, UI logic, and calls to the server mixed together.  So, with jQuery, you have to do work to put modularity in.  With Angular, the corresponding work would be to take it out.

Completing the walkthrough of Stage 4, the server responds with exactly the same data as it did to the put request described in Stage 3.  This is with good reason: the server hasn't changed!  Consequently, the callback to the `#then` block returned by the server request, as well as the code for updating the in-memory data model, is basically identical.  Again, though, how the view gets redrawn as a result of the changes on the server is different.  In jQuery, there was explicit DOM manipulation in `#renderTable` (and in a method I did not walk through, `#populateFormCard`, which changed the html and input values in the Edit/Create form).  By contrast, in Angular, `#setDefaultForm` simply updates values in the data model, and those changes in value trigger updates in the view through Angular's two-way data binding: 
{% highlight js %}
 $scope.setDefaultForm = function(){
    $scope.currentCard = 0;
    $scope.formCard = {};
    $scope.formAction = "Create";
    $scope.formHandler = $scope.addCard;
}
{% endhighlight %}
The form is thus set back to the default state it had before the `#edit` action was triggered by a form submission.  It's ready for the submission of a new card to add to the deck.

Stage 5: UI/UX enhancement and refactoring
==========================================
The demo was functionally complete after Stage 4.  With my remaining time before the demo, I tinkered with the interface, user experience, and refactoring.  One nice UX enhancement was the "flipping the card" animation, which was brought to you by a google search for "flip flashcard CSS animation" and this [nice walk through][ffc] by [kara][kara]. A lot of my refactoring came as a result of reading through [mongoose documentatin][mongoose] to find more abstracted methods for my queries.  Instead of, for example, splitting a query into a `#find` query and an `#update` query in a success callback, the Mongoose method `#findByIdAndUpdate` does a combined query in simpler, cleaner code.  I also refactored to have as much parallelism as possible in the naming of related methods on the client and server (e.g., the `#updateCard` method in the client controller issues a put request that ultimately cashes out in an `#updateCard` controller action on the server).  I also changed the color scheme to be consistent with one that I am trying to use for all of my MakerSquare apps that I work on alone.  The hope is to provide a more consistent experience when going through my portfolio, rather than it just looking like a jumble of unrelated projects.  For example, this is [Twittler][twittler], which I made as part of a pre-course assignment.

A More Complete Approach in this Sprint Would Have. . .
=======================================================
Overall, I was very pleased with how I approached the sprint and what I accomplished.  The only feature I tried and failed to implement was a "shake" animation when shuffling the deck (similar to what [Stripe does][shake] to the purchase order form when you submit it with invalid data).  The two main things that I have identified that could have improved my approach for the sprint were:

1. Incorporating a BDD/TDD workflow
2. Making the UI responsive by default

With respect to (1), I am still not fluent enough in writing tests for a JavaScript app to manage the overhead that BDD/TDD adds while still producing enough features in the two days I had.  I particularly regret this as getting better at BDD/TDD was a goal I set for myself at the beginning of the summer. (2) is always important, but it was particularly important for this project, as one of my main reasons for doing flashcards as a web app was getting portability. Having an app that works well on phone-sized screens would be the easiest way to achieve maximum portability.  

Those large reservations aside, the sprint provided me with a good foundation for moving into the second half of MakerSquare's program.  The emphasis in the first half is on acquiring a solid grounding in (a) Computer Science fundamentals and (b) different parts of the JavaScript stack for web development.  In the second half of the program, we apply (a) and (b) in creating and extending full-featured web applications.

[ms]: http://www.makersquare.com
[fm]: https://salty-thicket-9132.herokuapp.com/
[mest]: http://mean.io/#!/
[crud]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[pop]: https://popapp.in/
[q]: https://github.com/kriskowal/q
[qwiki]: https://github.com/kriskowal/q/wiki/API-Reference
[bp]: https://github.com/expressjs/body-parser
[em]: http://expressjs.com/guide/using-middleware.html
[fbiau]: http://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate
[papi]: https://promisesaplus.com/ 
[twdb]: https://docs.angularjs.org/tutorial/step_04
[pm]: https://www.getpostman.com/
[ang]: https://angularjs.org/
[node]: https://nodejs.org/en/
[express]: http://expressjs.com/
[mongo]: https://www.mongodb.org/
[boot]: http://getbootstrap.com/
[ffc]: http://codrspace.com/kara/implementing-flashcard-flip-with-css3-transitions/
[kara]: https://github.com/kara
[mongoos]: http://mongoosejs.com/
[mongoose]: http://mongoosejs.com/docs/guide.html
[twittler]: http://twittler.ignacioprado.net/
[shake]: https://github.com/mduvall/dissecting-delight/blob/master/stripe-checkout/shake-animation/index.html
