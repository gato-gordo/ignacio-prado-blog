---
layout: post
title:  "Functional Decomposition: An Introduction to the Wizard Book, Part I"
date:   2015-09-18
categories: [sicp, lisp, computer science, program design, black-box abstraction]
---

*These next two blog posts are adapted from a lightning-talk I gave at MakerSquare on 9/18/2015*

The main purpose of this talk is simply to make you aware of the [Wizard Book][sicp]: what it is and why I think it makes a great complement to the training we get at [MakerSquare][ms].  By way of introducing the distinctive style and thinking of the authors, I will focus on their concepts of "functional decomposition" and "black-box abstraction", simple techniques for designing and managing the complexity of scaling systems.

"Wizard Book", Whaa?
====================
The first question you are likely to ask is, "what do mean, `Wizard Book`?"  The `Wizard Book` is the nickname for Harold Abelson, Gerald Jay Sussman 
and Julie Sussman's  *The Structure and Interpretation of Computer Programs* (1986).  It's colloquially known as the `Wizard Book` because of the cover illustration:

![SICP](https://mitpress.mit.edu/sicp/full-text/book/cover.jpg)

The cover illustration is itself a reference to a powerful metaphor for programming that the authors use to introduce the book:

> We are about to study the idea of a *computational process*. Computational processes are abstract beings that inhabit computers. As they evolve, processes manipulate other abstract things called *data*. The evolution of a process is directed by a pattern of rules called a *program*. People create programs to direct processes. In effect, we conjure the spirits of the computer with our spells.

>A computational process is indeed much like a sorcerer's idea of a spirit. It cannot be seen or touched. It is not composed of matter at all. However, it is very real. It can perform intellectual work. It can answer questions. It can affect the world by disbursing money at a bank or by controlling a robot arm in a factory. The programs we use to conjure processes are like a sorcerer's spells. They are carefully composed from symbolic expressions in arcane and esoteric *programming languages* that prescribe the tasks we want our processes to perform.


Released in 1986, the `Wizard Book` was used as the textbook for the introductory programming course at MIT, originally taught by the authors, until 2008. An [online version][sicp] is made freely  available by MIT Press.

Scheme & the Roots of Functional JavaScript
=============================================================
The `Wizard Book` works through the problems and principles of computer science in [Scheme][scheme], a dialect of [LISP][lisp] designed by Gerald Jay Sussman, one of the book's authors.  Though LISP/Scheme has a reputation for being an arcane language, it's design has been very influential.  Brendan Eich, the creator of JavaScript, [has said][be] that

>JavaScript was meant to be "doing Scheme" in the browser

First-class and higher-order functions, closures, and some of the other functional-proramming features of JavaScript, for example, have their roots in Scheme.  For a JS developer, then, learning Scheme through the `Wizard Book` can lead to a deeper understanding of functional programming. With Scheme as a point of comparison, you can abstract away from the implementation details of functional programming in JS and get a clearer picture of it as a generalized paradigm for building systems to solve computational problems. 

Declarative Vs. Imperative Knowledge
====================================
At the core of true functional programming is a style designing procedures that avoid manipulating programming state and typically act by chaining together operations on immutable values.  The beginnings of functional programming in a looser sense, however, 

**Declarative Knowledge**

What is it?

Square root of x is a number y such that y * y = x and y >= 0;

**Imperative Knowledge**

How to find a square root?

***Guess and Check*** 

1.  Make a guess at the square root
2.  Check if guess is good enough
3.  If guess is good enough, stop
4.  Improve guess and check again

***Improving Guess***

1.  Binary Search
2. Heron of Alexandria's Algorithm

***Heron of Alexandria's Algorithm***

1. Make a guess
2. Check if guess is good enough
3. If guess is good enough, stop
4. Make new guess average of old guess and (target / old guess)

Desk Checking Heron of Alexandria's Algorithm
=============================================


<table class="table table-striped">
<thead>
    <th>Guess</th><th>Iteration Equation</th><th>Result</th><th>Square</th><th>Difference</th>
</thead>
<tbody>
    <tr>
        <td>1</td>
        <td>1/2(1+16/1)</td>
        <td>8.5</td>
        <td>72.2500</td>
        <td>56.2500</td>
    </tr>
    <tr>
        <td>8.5</td>
        <td>1/2(8.5+16/8.5)</td>
        <td>5.9111</td>
        <td>26.9471</td>
        <td>10.9471</td>
    </tr>
    <tr>
        <td>5.1911</td>
        <td>1/2(5.1911+16/5.9111)</td>
        <td>4.1366</td>
        <td>17.1114</td>
        <td>1.1114</td>
    </tr>
    <tr>
        <td>4.1366</td>
        <td> 1/2(4.1366+16/4.1366)</td>
        <td>4.0022</td>
        <td>16.0170</td>
        <td>0.0170</td>
    </tr>
</tbody>
</table>


First Technique for Taming Complexity: Functional Decomposition
===============================================================

**Functional Decomposition As Tree**

                        sqrt
                         |
                       sqrter
                       /    \
             good enough     improve
             /         \        |
          square       abs     avg


**Functional Decomposition in JavaScript**

{% highlight js %}
function sqrter(guess, target){
    if(goodEnough(guess, target)){
        return guess;
    } else {
        sqrter(improveGuess(guess, target));
    }
}
function improveGuess(guess, target){
    return average(guess, (target/guess));
}
function average(){
    return sum(arugments)/arguments.length;
}
function sum(){
    return  Array.prototype.slice
                           .call(arguments)
                           .reduce(function(cur, prev){
                              return cur + prev;
                           }, 0);
}
function goodEnough(guess, target){
    return Math.abs(target - Math.pow(guess, 2)) < 0.01;
}
{% endhighlight %}


**Functional Decomposition in Scheme**

{% highlight scheme %}
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x)
                 x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001))

(define (sqrt x)
  (sqrt-iter 1.0 x))

{% endhighlight %}



[scheme]: https://en.wikipedia.org/wiki/Scheme_(programming_language)
[lisp]: https://en.wikipedia.org/wiki/Lisp_(programming_language)
[sicp]:https://mitpress.mit.edu/sicp/ 
[ms]: http://www.makersquare.com
[pn]: http://norvig.com/
[pnar]: http://www.amazon.com/review/R403HR4VL71K8
[be]: https://brendaneich.com/tag/history/
