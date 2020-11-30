### Type:
Creational

### TL;DR:
The Prototype pattern allows your to copy existing objects without having your code be reliant on knowing how the
objects are constructed, or anything to do with its internal structure/dependencies.

### What problem is it solving?
Using our Car analogy, if we had to copy one of our Cars in code, how would we do it? We may have to go through all
of the fields on our current Car and set them on the new Car. But what if any of those properties are private?
What if any of those properties have sub-dependencies?

Also, as we now have to know about every property and dependency of that Car in our Client code, which ties it
heavily to a single implementation. What if we wanted to type-hint our Cars by an Interface - how then would we ensure 
that we can easily and consistently clone them?

### How does it work?
Prototype says that, if an object should be allowed to clone, it should know everything about that cloning procedure.
And so each object should implement a `cloneable` interface, which usually will just enforce a `clone` method.

This `clone()` method will then know everything about how to copy the object and return a new instance.

You can even implement a Prototype Registry, which acts like a DIC container - matching each implementation
of your class against a unique string and allowing you to grab the object straight out of the registry, ready
to copy.

### What does it look like in PHP?
PHP has built-in clone support. By using the `clone` keyword you can copy an object without having to define
any special methods - as long as the properties that are being copied are primitive types.

If any of the properties are objects, you may want to create a copy of that nested object too. To do this,
we make use of the `__clone` magic method:
```PHP
class Prototype
{
    public $numberProperty;
    public $objectProperty;

    public function __clone()
    {
        $this->objectProperty = clone $this->objectProperty;
    }
}

class Client
{
    public function doCloneThings()
    {
        $prototype = new Prototype();
        $clonedPrototype = clone $prototype;
    }
}
```

### What are its drawbacks?
- Cloning objects with circular dependencies could get ugly

### What are its benefits?
- You are able to copy objects without coupling your domain logic to a particular concrete class
- You can copy complex objects easier
- You can get replace repeated initialisation code by just calling `clone $myThingIWantToCopy`
- Isn't based on inheritance, so doesn't come with the problems that inheritance does

### What design pattern(s) is it used with commonly?
- An application might start out using the _Factory_ pattern, and evolve to use _Prototype_ as its code needs
to become more flexible
- _Abstract Factory_ methods can use _Prototype_ instead of _Factory_ methods
- Can be implemented as a _Singleton_
- Can be used as a simple alternative to _Memento_
- Designs using _Composite_ and _Decorator_ can benefit from using _Prototype_ as it stops you from having to
re-create complex objects from scratch if you already have an example of them
