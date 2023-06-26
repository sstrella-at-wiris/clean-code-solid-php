# Rigor Talks

Rigor Talks is a series of presentations delivered by [Carlos Buenosvinos](https://github.com/carlosbuenosvinos), a renowned software developer and advocate of clean code practices in PHP.

In his Rigor Talks, Carlos shares his expertise and insights on various topics related to software development, with a focus on writing high-quality, maintainable, and robust PHP code.

We are going to take a look through some key concepts mentioned in his videos.

For further information, you can check them out [here](https://www.youtube.com/playlist?list=PLfgj7DYkKH3Cd8bdu5SIHGYXh_bPV2idP).

### Guard Clauses
It involves using conditional checks at the beginning of a function to handle exceptional cases or error conditions.

This approach promotes early returns, eliminating unnecessary nesting and making the code more readable and maintainable.

	function divideNumbers($dividend, $divisor) {
	    if ($divisor === 0) {
	        throw new InvalidArgumentException('Divisor cannot be zero');
	    }
	    
	    if ($dividend === null || $divisor === null) {
	        throw new InvalidArgumentException('Dividend or divisor cannot be null');
	    }
	    
	    return $dividend / $divisor;
	}

### Self-encapsulation
We encapsulate data and behavior within a class to ensure data integrity and control its access to it.

We will move our internal variables and setter methods into private setters restricting the change of these fields.

	class Human {
	    private $age;
	
        public function __construct($age)
        {
            $this->setAge($age);
        }

	    private function setAge($age) 
        {
	        $this->checkIfAgeIsPositive($age);
	        $this->age = $age;
	    }

        private function checkIfAgeIsPositive() 
        {
            if ($age < 0) {
	            throw new InvalidArgumentException('Age cannot be negative');
	        }
        }

	    public function age() 
        {
	        return $this->age;
	    }
	}

### Named Constructors
It's a design pattern that provides explicit and descriptive methods to create instances of a class and make it more related to our business logic.

For this, we instance an object using a static function to return an object, without using the `new` like we normally use.

We also set our constructor to private, this will force that our client code will have to instance the class through the named constructor.

This can help us reduce the complexity of object creation and improve the semantic of the code.

	class User {
	    private $username;
	    private $email;
	    
	    private function __construct($username, $email) 
        {
	        $this->username = $username;
	        $this->email = $email;
	    }
	    
	    public static function createRegularUser($username, $email) 
        {
	        return new self($username, $email);
	    }
	    
	    public static function createAdminUser($username, $email) 
        {
	        $user = new self($username, $email);
	        $user->grantAdminPrivileges();
	        return $user;
	    }
	    
	    private function grantAdminPrivileges() 
        {
	        // Logic
	    }
	}

    class UserTest extends TestCase {

        public function testCreateRegularUser() 
        {
            $username = 'regularuser';
            $email = 'regular@example.com';
        
            $user = User::createRegularUser($username, $email);
            
            $this->assertInstanceOf(User::class, $user);
            $this->assertEquals($username, $user->getUsername());
            $this->assertEquals($email, $user->getEmail());
        }
    }


### Test Class
In his example, Carlos, emphasizes the importance of decoupling code for effective testing.

One approach demonstrated in the video is to separate the database connection from the main method being tested. 

This involves creating a new test class and moving the database connection code into a separate protected method.

This new test class will implement the main class, then new method can be created in the new class to return a mocked result instead of actually connecting to the database, eliminating the need for database access during testing. 

This allows for testing the business logic in isolation, independent of the infrastructure components.

We can take a look at the following example for clarification.

	class Invoice {
	    public function isSpanishTaxGreaterThanZero() : bool 
        {
            $connection = new mysqli("localhost", "myusername", "mypassword", "mydatabase");
            $spanishIso2 = 'es';

            $spanishTaxPercent = $connection->query('SELECT taxPercent FROM country WHERE iso2 = \'' . $spanishIso2 . '\'');
 
            return $spanishTaxPercent > 0;
	    }
	}

In order to decouple it, we can just extract the db connection as a protected method and create a method with the same name in our new class `InvoiceTestClass`.

    class Invoice {

        public function isSpanishTaxGreaterThanZero() : bool
        {
            $spanishTaxPercent = $this->getSpanishTaxPercent();
            return $spanishTaxPercent > 0;
        }

        protected function getSpanishTaxPercent() 
        {
            $connection = new mysqli("localhost", "myusername", "mypassword", "mydatabase");
            $spanishIso2 = 'es';

            $result = $connection->query('SELECT taxPercent FROM country WHERE iso2 = \'' . $spanishIso2 . '\'');
            return $result;
	    }
	}

	class InvoiceTestClass extends Invoice {
	    protected function getSpanishTaxPercent() {
	        return 21;
	    }
	}

	class InvoiceTest extends TestCase {
	    public function testCheckIfTaxIsGreaterThanZero() 
	    {
	        $invoiceTestClass = new InvoiceTestClass();
	        $this->assertTrue($invoiceTestClass->isSpanishTaxGreaterThanZero());
	    }
	}

Instead of using the parent `Invoice` class, we use our children `InvoiceTestClass`.

Like this, we will not need to depend on the db connection to test the `isTaxGreaterThanZero` method functionality, our business logic.

### Immutability
The immutability principle states that once an object is created, its state should not be modified, ensuring that it remains constant and unchangeable throughout its lifetime.

To achieve this immutability, we are making the age property private and providing a single method `setAge()` to set the initial age. 

Once the age is set, it cannot be modified externally, preserving the object's state.


    class Human {
        private $age;
    
        public function __construct($age)
        {
            $this->setAge($age);
        }
    
        private function setAge($age) 
        {
            $this->checkIfAgeIsPositive($age);
            $this->age = $age;
        }
    
        private function checkIfAgeIsPositive($age) 
        {
            if ($age < 0) {
                throw new InvalidArgumentException('Age cannot be negative');
            }
        }
    
        public function age() 
        {
            return $this->age;
        }
    
        public function sumAges(Human $other) 
        {
            $sum = $this->age() + $other->age();
            return new Human($sum);
        }
    }

    class HumanTest extends TestCase {
        public function testSumAges()
        {
            $human1 = new Human(20);
            $human2 = new Human(30);
        
            $sumOfAges = $human1->sumAges($human2);
        
            $this->assertEquals(50, $sumOfAges->age());
            $this->assertNotSame($human1, $sumOfAges);
            $this->assertNotSame($human2, $sumOfAges);
        }
    }

By asserting that `$sumOfAges` is not the same instance as `$human1` and `$human2`, we are checking that the sumAges method is following the immutability principle.

Thus creating a new object with the summed age rather than modifying the existing objects.

