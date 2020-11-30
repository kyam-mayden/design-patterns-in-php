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
```PHP
interface CarFactory {
  public function createSUV();
  public function createCoupe();
}

class SkodaFactory implements CarFactory
{
  public function createSUV()
  {
    return new SkodaSuv();
  }
  
  public function createCoupe()
  {
    return new SkodaCoupe();
  }
}

class VWFactory implements CarFactory
{
  public function createSUV()
  {
    return new VWSuv();
  }
  
  public function createCoupe()
  {
    return new VWCoupe();
  }
}

class OrderingSystem
{
  private $carFactory;
  
  public function __construct (CarFactory $carFactory)
  {
    $this->carFactory = $carFactory;
  }
  
  public Function createCoupes(int $number): array
  {
    return array_fill(0, $number, $this->carFactory->createCoupe());
  }
  
  public Function createSUVs(int $number): array
  {
    return array_fill(0, $number, $this->carFactory->createSUV());
  }
}
```


### What are its drawbacks?
Abstract Factory is perfect if your system is made up of lots of interchangeable
pieces. Also, it's important that the types (models) of these pieces rarely changes - 
the main changes to the system are the concretions (brands) of these pieces.
If however, you wanted to add a new model that doesn't already exist, like a Sedan,
then you need to update both the interface and all of the concretions to allow for this
new Model. 

### What are its benefits?
- Supports Open/Closed principle
- Also supports single responsibility principle
- You avoid tying your code to concretions of classes

### What design pattern(s) is it used with commonly?
- AbstractFactory classes are usually implemented using the Factory pattern,
but they can also be done using Prototype.
- Often the Concrete Factory is implemented as a Singleton.
