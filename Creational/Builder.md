### Type:
Creational

### TL;DR:
The Builder pattern allows you to construct lots of similar classes without having to rely on class inheritance
or ugly constructor methods.

### What problem is it solving?
Using our car example, let's say we are running a car factory. We can produce 5 different models, each many variations
such as engine size, number of seats, CD player, sunroof, spare tyre.

We could choose to let a base Car class handle all of this logic for us in its constructor. But quickly this constructor
would start to look ugly:
```PHP
class Client
{
    public function doCarThings()
    {
        $car1 = new Car(new SmallEngine(), 6, null, null, true, ....);
        $car2 = new Car(new LargeEngine(), 4, true, true, null, ....); 
    }
}
$car1 = new Car(new LargeEngine(), 4, true, true, null, ....);
$car2 = new Car(new SmallEngine(), 6, null, null, true, ....);
```
It's easy to see how, in not too much time, many of the constructor arguments will become unused.

The builder pattern allows us to move the construction of an object out of its own class and give that responsibility to
Builders.

### How does it work?
First of all, your Client code needs to not be concerned with knowing about a Car class, but knowing about a Car Builder
class. Then, you can call the Builder and say 'I need a car, with a large engine and a CD player and four seats'. The
Builder then knows how to make and set these components on the Car, and returns the Car to the Client.

```PHP
class CarBuilder
{
public function buildEngine(){};
public function buildSeats(){};
public function buildCDPlayer(){};
public function buildSunroof(){};
public function buildSpareTyre(){};
public function get(){};
}
```
This approach allows you to separate each aspect of constructing a car into a distinct and unrelated step, allowing
each feature of a car to either be set, or not, on its own:
```PHP
$car1 = CarBuilder->buildEngine(new LargeEngine)->buildSeats(4)->buildCDPlayer(true)->get();
```
Notice that for Car1 we didn't need to specify that the sunroof and spare tyre don't exists - it is implied by not
calling their construction methods.

You may need to build different implementations of the same class (Sports Car/SUV/etc), and so you may have a builder
for each type of implementation - so SUVCarBuilder. All that matters then is that each Builder type implements a common interface that
enforces similar builder methods eg buildEngine, etc.

While not fully necessary, you may also implement a Director class to handle the steps by which you construct the
Car, allowing the Builder to just know what to do with those steps. This both allows the specific construction order 
to be reused across your program, and hides the construction logic from your Client.

### What does it look like in PHP?
```PHP
interface CarBuilderInterface
{
    public function buildEngine();
    public function buildSeats();
    public function buildCDPlayer();
    public function buildSunroof();
    public function buildSpareTyre();
// Notice that we don't specify the get() method - this is because it could have a different return type depending
// on the implementation of the interface.
}

class SuvBuilder implements CarBuilderInterface
{
    public function buildEngine(){};
    public function buildSeats(){};
    public function buildCDPlayer(){};
    public function buildSunroof(){};
    public function buildSpareTyre(){};
    public function get(){};
}

class SportsCarBuilder implements CarBuilderInterface
{
    public function buildEngine(){};
    public function buildSeats(){};
    public function buildCDPlayer(){};
    public function buildSunroof(){};
    public function buildSpareTyre(){};
    public function get(){};
}

class Director
{
    private $builder;
    public function make($type)
    {
        if ($type === 'basic') {
            return $this->builder->buildEngine('small')->buildSeats(4)->get();
        } else {
             return $this->builder->buildEngine('large')
                    ->buildSeats(4)
                    ->buildSunRoof()
                    ->buildCDPlayer()
                    ->buildSpareTyre()
                    ->get();
        }
    }
}

class Client
{
    private $director;
    public function doCarThings()
    {
        $car1 = $this->director->make('basic');
        $car2 = $this->director->make('expensive');
    }
}
```
We can see now that the Client class need to know nothing about how to make a car, it just knows where to get one from.
Also by setting a Builder property on the Director, we can easily swap out which Builder it is using and make the whole
operation flexible, to suit the need of constructing any car.


### What are its drawbacks?
- Code complexity increases. In our example, we previously had two classes (Car and Client), whereas now we have
5 classes and an interface - and there are probably a lot more classes to add for car models, etc.

### What are its benefits?
- Allows you to get rid of a 'telescopic constructor' - where the length of your class constructor call changes because
so many of the parameters have default values.
- Allows you to easily construct different implementations of similar products (SUV/sports car) without having to rely on class
inheritance
- Single Responsibility - allow your Client code to only care about its job, and allow other parts of the codebase
to construct the Car class for us.
- The construction code is reusable, making your code less repetitive and more consistent.
- You can make your car in distinct steps, or abstract out those construction steps even further.

### What design pattern(s) is it used with commonly?
- Builder(s) can be implemented as a _Singleton_
- The difference between _Builder_ and _Abstract Factory_ is that the _Builder_ method allows you to construct complicated
objects step-by-step, whereas _Abstract Factory_ allows you to more easily create families of related objects.
- Many applications may start life using the _Factory_ pattern, but as they need flexibility will evolve to use other
patterns, including _Builder_.
