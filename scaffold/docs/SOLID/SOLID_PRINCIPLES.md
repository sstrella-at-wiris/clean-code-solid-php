# SOLID Principles in PHP

## Single Responsibility Principle
 
The Single Responsibility Principle (SRP) states that every class should have only one reason to change.

This means, that a class should have only one responsibility. 

When a class has multiple responsibilities, it becomes more difficult to maintain and test, and can lead to problems in the long run.

### Example:

In this example, UserManager class has two responsibilities: creating a new user and sending a welcome email to the user.

    class UserManager {
        private $db;
    
        public function __construct(Database $db) {
            $this->db = $db;
        }
    
        public function createUser($username, $password) {
            // Validation logic here
    
            // Save the user to the database
            $this->db->insert('users', [
                'username' => $username,
                'password' => $password,
            ]);
    
            // Send a welcome email to the user
            $mailer = new Mailer();
            $mailer->sendEmail($username, 'Welcome to our site!');
        }
    }

This code violates the SRP because if either the way we save users to the database or the way we send welcome emails changes, the UserManager class will need to be modified. 

We can refactor this, by splitting the responsibilities into two separate classes:

    class UserCreator {
        private $db;
    
        public function __construct(Database $db) {
            $this->db = $db;
        }
    
        public function createUser($username, $password) {
            // Validation logic here
    
            // Save the user to the database
            $this->db->insert('users', [
                'username' => $username,
                'password' => $password,
            ]);
        }
    }

    class EmailSender {
        public function sendWelcomeEmail($username) {
            $mailer = new Mailer();
            $mailer->sendEmail($username, 'Welcome to our site!');
        }
    }

Now we have achieved:
- Easier maintenance because changes can affect only one aspect of the system
- Improved testability because it has fewer dependencies, and we can be more specific 
- More organized code and easier to understand

## The Open/Closed Principle

The Open/Closed Principle (OCP) states that "software entities (classes, modules, functions, etc.) should be open for extension but closed for modification." 

This means that you should be able to add new functionality to a system without changing the existing code.

This can be achieved by using techniques such as inheritance, interfaces, and abstract classes.

### Example:

Let's say we have a User class with a getDiscount() method that calculates a discount based on the user's type:

    class User {
        protected $type;

        public function __construct($type) {
            $this->type = $type;
        }
    
        public function getDiscount() {
            switch ($this->type) {
                case 'regular':
                    return 0.05;
                case 'premium':
                    return 0.1;
                default:
                    throw new \Exception('Invalid user type');
            }
        }
    }

We can refactor this example by using object-oriented design and interfaces: 

    interface UserType {
        public function getDiscount();
    }
    
    class RegularUser implements UserType {
        public function getDiscount() {
            return 0.05;
        }
    }
    
    class PremiumUser implements UserType {
        public function getDiscount() {
            return 0.1;
        }
    }
    
    class User {
        protected $type;
    
        public function __construct(UserType $type) {
            $this->type = $type;
        }
        
        public function getDiscount() {
            return $this->type->getDiscount();
        }
    }
    
    $regularUserType = new RegularUser();
    $user1 = new User($regularUserType);
    echo $user1->getDiscount(); // result: 0.05
    
    $premiumUserType = new PremiumUser();
    $user2 = new User($premiumUserType);
    echo $user2->getDiscount(); // result: 0.1

In this refactored example, we have created an interface UserType that defines the getDiscount method.

Now we just need to instantiate the userType we need.

Like this, we removed the switch case and delegated the responsibility of calculating the discount to the concrete implementations of UserType.

This makes the User class open for extension (in case we want to add new user types by implementing the UserType interface) but closed for modification (we don't need to modify the User class to add new user types).

## Liskov Substitution Principle

The Liskov Substitution Principle (LSP) states that if a class A is a subtype of class B,
then objects of class B should be replaceable with objects of class A without affecting the correctness of the program.

This means that subclasses should be able to be used in place of their parent class without causing any issues or breaking the code.

Basically:

    Class A
    {
        public function doSomething(){ /* .... */}
    }

    Class B extends A
    {
    
    }

### Example:

    interface LogRepositoryInterface {
        /**
         * Gets all logs.
         * @return array
        */
        public function getAll();
    }

    class FileLogRepository implements LogRepositoryInterface {
        public function getAll() {
        // Fetch the logs from the file and return an array
        return $logsArray;
        }
    }

    class DatabaseLogRepository implements LogRepositoryInterface {
        public function getAll() {
        // fetch Logs from model Log and call toArray() function to match the return type.
        return Log::all()->toArray();
        }
    }

Both classes implement the `LogRepositoryInterface` by having a `getAll` method.

`FileLogRepository` read logs from file and return an array and `DatabaseLogRepository` reads logs using the `all()` method that returns a Collection type, so we can call `toArray()` method on collection to make the array.

If we don't call the `toArray()` method and return a Collection it violates LSP that leads to type checking on client class.

In summary, this principle is just an extension of the Open/Closed Principle, and it means that we must make sure that new derived classes are extending the base classes without changing their behavior.

We could follow these conditions to avoid LSP violation:

- The return type of the method should match the base type.
- Exception types must match the base class.

## The Interface Segregation Principle

The Interface Segregation Principle (ISP) suggests that a class should not be forced to implement interfaces it doesn't need. 

Instead, it's better to have several smaller interfaces that are specific to particular use cases.

By having smaller, more specific interfaces, each class can implement only the interface(s) that are relevant to their behavior without being forced to implement unnecessary methods.

### Example:

In this example, we have three separate interfaces that each define a single behavior: CanFly, CanSwim, and CanWalk.

We then have three classes, Bird, Fish, and Dog, each of which implements only the interface(s) that are relevant to their behavior.

    interface CanFly {
        public function fly();
    }
    
    interface CanSwim {
        public function swim();
    }
    
    interface CanWalk {
        public function walk();
    }
    
    class Bird implements CanFly, CanWalk {
        public function fly() {
            echo "The bird is flying.<br>";
        }
    
        public function walk() {
            echo "The bird is walking.<br>";
        }
    }
    
    class Fish implements CanSwim {
        public function swim() {
            echo "The fish is swimming.<br>";
        }
    }
    
    class Dog implements CanWalk {
        public function walk() {
            echo "The dog is walking.<br>";
        }
    }
    
    $bird = new Bird();
    $bird->fly(); // Outputs: The bird is flying.
    $bird->walk(); // Outputs: The bird is walking.
    
    $fish = new Fish();
    $fish->swim(); // Outputs: The fish is swimming.
    
    $dog = new Dog();
    $dog->walk(); // Outputs: The dog is walking.

## Dependency Inversion Principle

The Dependency Inversion Principle (DIP) states that high-level modules/classes should not depend on low-level modules/classes directly, but instead, they should depend on abstractions.

It means that a higher level class should not need to know the implementation details of the low-level class, the low-level class should be hidden behind an abstraction.

## Example:

    <?php
    class MySqlConnection {
      public function connect() {}
    }
     
    class Post{
      private $dbConnection;
      public function __construct(MySqlConnection $dbConnection) {
        $this->dbConnection = $dbConnection;
            $this->dbConnection->connect();
      }
    }

The problem with this example is that our post-class depends on a concrete class MySqlConnection. 

In case we need to support another DB we have to change our Post class.

To avoid this we can, instead of passing a concrete class type in the constructor we pass a DbConnection interface that is implemented by MySqlConnection class.

So it'll look like this:

    interface DbConnectionInterface {
        public function connect();
    }
    
    class MySqlConnection implements DbConnectionInterface {
        public function connect() {}
    }
    
    class Post {
        private $dbConnection;
        public function __construct(DbConnectionInterface $dbConnection) {
            $this->dbConnection = $dbConnection;
            $this->dbConnection->connect();
        }
    }

So now there is no need to change Post class for new DB type. 

The only thing that we need to support the new DB is that it must implement the interface DBConnectionInterface.