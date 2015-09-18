---
layout: post
title:  "Managing Complexity with Black-Box Abstraction: An Introduction to the Wizard Book"
date:   2015-09-18
categories: [sicp, lisp, computer science, program design, black-box abstraction]
---

*This blog post is adapted from a lightning-talk I gave at MakerSquare on 9/18/2015*

The main purpose of this talk is simply to make you aware of the [Wizard Book][sicp]: what it is and why I think working through it makes a great complement and possible next step to the training we get at [MakerSquare][ms].  As way of introducing the distinctive style and thinking of the authors, I will focus on their concept of "black-box abstraction", a simple tool that allows developers to manage the complexity of scaling systems.

"Wizard Book", Whaa?
====================
The first question you are likely to ask is, "what do mean, `Wizard Book`?"  The `Wizard Book` is the nickname for Harold Abelson, Gerald Jay Sussman 
and Julie Sussman's  `*The Structure and Interpretation of Computer Programs* (1986)`.  It's colloquially known as the `Wizard Book` because of the illustration on the front cover:

![SICP](https://mitpress.mit.edu/sicp/full-text/book/cover.jpg)

The wizard on front cover is itself, however, a reference to a powerful metaphor for programming that the authors use to introduce the book:

> We are about to study the idea of a *computational process*. Computational processes are abstract beings that inhabit computers. As they evolve, processes manipulate other abstract things called *data*. The evolution of a process is directed by a pattern of rules called a *program*. People create programs to direct processes. In effect, we conjure the spirits of the computer with our spells.

>A computational process is indeed much like a sorcerer's idea of a spirit. It cannot be seen or touched. It is not composed of matter at all. However, it is very real. It can perform intellectual work. It can answer questions. It can affect the world by disbursing money at a bank or by controlling a robot arm in a factory. The programs we use to conjure processes are like a sorcerer's spells. They are carefully composed from symbolic expressions in arcane and esoteric *programming languages* that prescribe the tasks we want our processes to perform.


Released in 1986, the `Wizard Book` was used as the textbook for the introductory programming course at MIT, originally taught by the authors, until 2008. An [online version][sicp] is made freely  available by MIT Press.

The Director of Research at Google Likes the Wizard Book
========================================================
The internet is full of people saying nice things about the `Wizard Book`, and I won't take too much time reiterating those points of praise (more references are provided at the bottom of this post).   I will, however, quote [Peter Norvig][pn], a computer scientist who is the Director of Research at Google, whose[Amazon review][pnar] of the book provides a particularly apt summary of its merits: 

>Programming is a craft that is subject to frequent failure: many projects are started and abandoned because the designers do not have the flexibility, experience and understanding to come up with a suitable design and implementation. SICP gives you an approach that will succeed, but it is an approach based on principles and wisdom, not on a checklist. If you don't understand the principles, or if you are the kind of person who wants to be given a cookbook of what to do rather than to think creatively, or if you only want to work on problems that are pretty much like the problem you worked on last time, then this approach will not work for you. There are other approaches that will be more reproducible for a limited range of simple problems, but there is no better way than SICP to learn how to address the truly hard problems.

There is a lot in Norvig's review that gets at what the Wizard Book does and does not do for a developer.  What it doesn't do is provide you with an introduction to bleeding-edge technologies, frameworks that organize and abstract common tasks in some programming domain, or even instruction in a language that's widely used in the contemporary workplace.  What it does do, however, is provide of general set of principles that can be applied to designing and implementing any solution to a problem that can be approached computationally.  One relatively non-technical principle, which I will focus in this talk, is the idea of block-box abstraction: a toool that allows the programmer to manage the complexity of systems as they grow. 

Why Should JavaScript Developers Work through the Wizard Book?
=============================================================
The `Wizard Book` famously works through the problems and principles of computer science in an arcane programming language, [Scheme][scheme], which is a dialect of [LISP][lisp] designed by one of the book's authors.  As suggested I earlier, Scheme is not a widely used language.  However, it's design is at the heart of most of the powerful features of JavaScript: first-class functions and closures.  As Brendan Eich has said,

>JavaScript was meant to be "doing Scheme" in the browser

Declarative Vs. Imperative Knowledge
====================================
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

Second Technique for Taming Complexity: Black-Box Abstraction
=============================================================

        16 -> [sqrt] -> 4 
                         \
                          4,5 -> [sum] -> 9
                         /
        25 -> [sqrt] -> 5 

1. Seems to be over-engineering what is easy now, but when systems 
 get huge this is very important.
2. With millions of lines of code, you simply can't know the details / implementation.
    With black-box abstraction, you don't __need to know__ all you need to know when working in your black box is the interface of the surrounding black boxes and your own interface
3. Makes test driven-development easier
    --as soon as you have the box boxes, you can write your unit and integration tests

Abstracting Procedures that Yield Procedures
============================================

    16 -> [squareRooterX] -> squareRooter16

**Newton's Method**

       16 = x^2
     f(x) = x^2 - 16

     x[n+1] = x[n] - f(n)/f'(n)

     x[0] = 1
     x[1] = 1 - (1^2 - 16)(2) = 8.5
     x[2] = 8.5 - (8.5^2 - 16)/2 = 

    [squareRootFunction], 16 -> [NewtonMethodizer] -> squareRooter16


[scheme]: https://en.wikipedia.org/wiki/Scheme_(programming_language)
[lisp]: https://en.wikipedia.org/wiki/Lisp_(programming_language)
[sicp]:https://mitpress.mit.edu/sicp/ 
[ms]: http://www.makersquare.com
[pn]: http://norvig.com/
[pnar]: http://www.amazon.com/review/R403HR4VL71K8
