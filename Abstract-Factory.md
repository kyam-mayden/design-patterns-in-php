### Type:
Creational

### TL;DR:
Abstract factory allows you to create groups/families of related code,
but without having to specify their concrete classes when called.

### What problem is it solving?
Let's say you're a car dealer, and you deal in both different brands
of car (Skoda, Volkswagen, etc) as well as different models (Hatchback,
SUV, etc).

Today you want to order a bunch of Skoda's:

_"Give me a Skoda SUV, a Skoda Coupe, a Skoda Convertible."_

Tomorrow, you want to order a load of VW's:

_"Give me a VW SUV,a VW Coupe and a VW Convertible"_

Would it not be easier to say:

_"Whichever brand I have selected, give me a SUV, a Coupe and a Convertible"_

This means that you only have to decide (and update) in one place what the
brand of car is.

### How does it work?
First of all, you need an interface that all of your concrete classes
implement. This interface needs to define all of the classes that your concrete
car factory will instantiate, eg getCoupe, getSUV etc. 

These will then return a Skoda coupe, or a VW SUV, depending on the concretion.

### What does it look like in PHP?



### What are it's drawbacks?
Abstract Factory is perfect if your system is made up of lots of interchangeable
peices. Also, it's important that the types (models) of these peices work rarely changes - 
the main changes to the system are the concretions (brands) of these peices.
If however, you wanted to add a new model that doesn't already exist, like a Sedan,
then you need to update both the interface and all of the concretions to allow for this
new Model.

### What design pattern(s) is it used with commonly?
