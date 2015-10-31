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

This is where you really start feeling the "magical side" of programming that the authors of SICP are talking.  Using imperative and declarative knowledge hand in 


The Director of Research at Google Likes the Wizard Book
========================================================
The internet is full of people saying nice things about the `Wizard Book`, and I won't take too much time reiterating those points of praise (more references are provided at the bottom of this post).   I will, however, quote [Peter Norvig][pn], a computer scientist who is the Director of Research at Google, whose [Amazon review][pnar] provides a particularly apt takeaway: 

>Programming is a craft that is subject to frequent failure: many projects are started and abandoned because the designers do not have the flexibility, experience and understanding to come up with a suitable design and implementation. SICP gives you an approach that will succeed, but it is an approach based on principles and wisdom, not on a checklist. If you don't understand the principles, or if you are the kind of person who wants to be given a cookbook of what to do rather than to think creatively, or if you only want to work on problems that are pretty much like the problem you worked on last time, then this approach will not work for you. There are other approaches that will be more reproducible for a limited range of simple problems, but there is no better way than SICP to learn how to address the truly hard problems.

This excerpt neatly summarizes what the Wizard Book does and does not provide a developer.  What it doesn't provide is an introduction to bleeding-edge technologies or frameworks that organize and abstract common tasks in some problem domain.  As noted in the last section, itt doesn't even provide instruction in a mainstream, widely-adopted programming language.  What it does provide, though, is a general set of principles and techniques for designing and implementing solutions to any problem that can be approached computationally.  One such technique, functional decomposition, can  