---
layout: post
title:  "Persistence with the Web Storage API for Client-Side Apps"
date:   2015-10-30 
categories:  localStorage
---

I led my first [MakerPrep][msp] class on Wednesday, 10/28/2015.  In the final week of the course, we introduce them to [jQuery][jq] DOM manipulation and event handling.  I thought it would be nice for them to see that they can build a functioning web app with just those tools, so the entire class was taken up with going through a basic to-do app.   I broke down the build down into 7 Steps (each corresponding to a commit in the repo I was using) that I had them attempt to implement one at a time (repo [here][todogit], demo of full app [here][demo]).

One feature that's missing from a lot demo apps that get built as part of tutorials for various client-side JS libraries and frameworks (such as jQuery) is some form of persistence, since connecting to and configuring a database is typically done server-side.  The [Web Storage API][ls], however, provides facilities for persistence through a [Storage][so] object interface that can be used by client-side apps with relatively undemanding persistence needs. The Storage object is structured as as a simple set of key-value pairs (where the values are always strings), along with some utility methods and abstractions.  

For my to-do I app, I used the `window.localStorage` object to save to-do lists and associate them with usernames.  The entire feature required less than 10 lines of code:


{% highlight js %}
var user = prompt("Enter a username: ");

$('#save-button').on('click', function(event){
    var list = $('ol').html();
    localStorage.setItem(user, list);
});

if(localStorage.getItem(user)){
    $('ol').append(localStorage.getItem(user));
};});
{% endhighlight %}

 There are obvious limits to using `localStorage` as a persistence solution.  The amount of data you can put into it is capped at around 5Mb. It would also be unwieldly for a system that required storing and querying data with any non-trivial relational structure.  However, when you need to store just a little bit of data and that data is naturally represented as key/value pairs (such as usernames and their associated task lists), it provides a viable solution that can make your client-side demo or prototype apps close to fully functional.

[jq]: https://api.jquery.com/
[todogit]: https://github.com/gato-gordo/tasks
[demo]: http://tasks.ignacioprado.net
[ls]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API
[so]: https://developer.mozilla.org/en-US/docs/Web/API/Storage
[msp]:  http://www.makersquare.com/part-time
