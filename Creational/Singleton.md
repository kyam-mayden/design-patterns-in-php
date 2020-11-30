### Type:
Creational

### TL;DR:
Create only a single instance of a class throughout the whole application, while providing a means to access
this single instance.

### What problem is it solving?
There may be parts of your application that you don't want to duplicate access to. For example, if every time
you wanted to connect to your Database in your application you spun up a new Connection, you would soon have multiple
concurrent Database connections for a _single application instance_, and soon MySQL (or whatever DB engine you are using)
would refuse to offer any more. This would then affecting other instances of your application connecting.

If we allow each instance of the application to have only one single Database connection, we remove this problem.

However, if we are only allowing a single instance of a particular Class then we need to be able to access that
instance anywhere in our code. So, we also need to provide global access to the single Instance.

Because of this, Singleton breaks the Single Responsibility Principle - it both restricts the amount of instances,
and provides access to them.

### How does it work?
Singleton will hide the ability to create a new Instance from anywhere outside of the class. To enable creation,
it will implement a static `getInstance()` method that will check if an instance already exists, and return it if it does.

Otherwise, it will create a new Instance and return it, as well as setting it on the Class to be retrieved at a later
date.

### What does it look like in PHP?
```PHP
class DatabaseConnection
{
    private static $instance;
    private function __construct()
    {
        // make the object
    }

    public static function getInstance()
    {
        if (self::$instance === null) {
            return new Self();
        }
        return self::instance;
    } 
}

class Client
{
    public function doDatabaseThings()
    {
        $brokendDb = new DatabaseConnection(); // will fail as __construct is private
        $realDb = DatabaseConnection::getInstance();
    }
}
```

### What are its drawbacks?
- As mentioned before, Singleton violates SRP
- It can make your code difficult to Unit Test, as a lot of mocking frameworks rely on inheritance to produce their
objects.
- Is global access to an object a good thing? This could allow our objects to know too much about each other.
### What are its benefits?
- Ensures that only one instances of a class can exist at any one time
- The instance that is created is available _globally_
### What design pattern(s) is it used with commonly?
- Many other patterns, such as _Abstract Factory_, _Builder_ and _Prototype_ can be implemented as a _Singleton_
- A _Facade_ class can be transformed into a _Singleton_