### Type:
Creational

### TL;DR:
Abstract factory allows you to create groups/families of related code,
but without having to specify their concrete classes when called.

### What problem is it solving?
Let's say you're a dairy farmer, and you currently use cows for your crop.
In your code for milking your cows you call methods Milk() and Feed(), etc.
One day however, you change your main supply of milk to goats. You could
change everywhere in your code that you call your Cow class, and it's related
methods, to Goat class. But wouldn't it be easier if in the first place it had
been typehinted by an overall Type that strictly defines which methods are
shared? Then that you don't have to change a thing, except the concretion that
you have defined in one single place.

### How does it work?
First of all, you need an interface that all of your
concrete classes implement. 

### What does it look like in PHP?

### What are it's drawbacks?

### What design pattern(s) is it used with commonly?
