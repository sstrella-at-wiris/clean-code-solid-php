# Clean Code

The following sources and examples are based on the book "Clean Code: A Handbook of Agile Software Craftsmanship" by Robert "Uncle Bob" Martin.

Robert C. Martin, widely known as "Uncle Bob," is a highly respected software engineer, author, and speaker. 

He is considered a prominent figure in the software development community, advocating for principles such as clean code, test-driven development, and SOLID principles. 

Uncle Bob's passion lies in promoting high-quality, maintainable software and empowering developers to embrace professionalism in their craft.

Through his extensive writing and speaking engagements, he has made a significant impact on the industry, shaping the way developers approach software development.

## Chapter 1: Clean Code

### What is Clean Code?
The code can be measured with either "good" or "bad" in the code review or by how many minutes it takes you to talk about it.

Code is clean if it can be understood easily – by everyone on the team.

Clean code can be read and enhanced by a developer other than its original author. With understandability comes readability, changeability, extensibility and maintainability.

You should add value to the business with your code.

Clean code offers quality and understanding when we open a class.

It is necessary that your code is clean and readable for anyone to find and easily understand. Avoid wasting others' time.

## Chapter 2: Meaningful Names

### Use Intention-Revealing Names
Choose names that clearly and accurately communicate the purpose of the variable, function, or class. A good name should allow a reader to quickly understand the intention of the code.

    // Bad example
    $var = 5;
    if ($var > 10) {
        echo "Value is greater than 10.";
    }
    
    // Good example
    $numberOfStudents = 5;
    if ($numberOfStudents > 10) {
        echo "Number of students exceeds the maximum limit.";
    }

### Avoid Disinformation
Avoid using names that suggest incorrect assumptions about what the code does. Names that suggest different meanings than their actual purpose can lead to confusion and bugs.

    // Bad example
    $isAvailable = false;
    if (!$isAvailable) {
        Log::error("Item not found.");
    }
    
    // Good example
    $isItemAvailable = false;
    if (!$isItemAvailable) {
        Log::error("Item is not available.");
    }

### Make Meaningful Distinctions
Avoid using names that are too similar to other names in the same context. Similar names can lead to confusion, especially when the difference in meaning is subtle.

    // Bad example
    function copyFiles($source, $destination) { /* ... */ }
    function copyFilesFromRemoteServer($source, $destination) { /* ... */ }
    
    // Good example
    function copyLocalFiles($source, $destination) { /* ... */ }
    function copyRemoteFiles($source, $destination) { /* ... */ }

### Use Pronounceable Names
Choose names that are easy to say and sound natural in conversation. This makes it easier for developers to communicate about the code.

    // Bad example
    $cclsttn = new ClientConnection();
    $dnmpstn = new DumpStation();
    $mstbnr = new MasterBanner();
    
    // Good example
    $clientConnection = new ClientConnection();
    $dumpStation = new DumpStation();
    $masterBanner = new MasterBanner();

### Use Searchable Names
Choose names that can be easily searched for within the codebase. This makes it easier to find and reference code when making changes.

Avoid using single-letter names (a, b, c, ...) and numeric constants (1, 2, 3, ...), they are unsearchable names.

Single-letter names can ONLY be used as local variables inside short methods.

    // Bad example
    $nums = [3, 5, 7, 9];
    for ($i = 0; $i < count($nums); $i++) {
        // ...
    }
    
    // Good example
    $primeNumbers = [3, 5, 7, 11];
    for ($i = 0; $i < count($primeNumbers); $i++) {
        // ...
    }

### Avoid Encodings
Avoid using prefixes or other encoding techniques to convey information about the type or scope of a variable. 

We don't need to prefix member variables with m_ anymore - classes and function should be small enough.
    
    // Bad example
    class Part {
        private $m_dsc;
    
        public function setName($name) {
            $this->m_dsc = $name;
        }
    }

    // Good example
    class Part {
        private $description;
        
        public function setName($name) {
            $this->description = $name;
        }
    }

### Avoid Mental Mapping
Avoid using names that require readers to perform mental mapping to understand the meaning. The code should be as clear as possible.

    // Bad example
    function process($data) {
        $d = [];
    
        foreach ($data as $record) {
            $name = $record[0];
            $age = $record[1];
    
            $d[] = [$name, $age];
        }
    
        return $d;
    }
    
    // Good example
    function processRecords($records) {
        $processedRecords = [];
    
        foreach ($records as $record) {
            $name = $record['name'];
            $age = $record['age'];
    
            $processedRecords[] = ['name' => $name, 'age' => $age];
        }
    
        return $processedRecords;
    }

### Class Names
Class names should be nouns that describe the class's purpose. They should be as specific as possible.

    // Bad example
    class DataManager { /* ... */ }
    
    // Good example
    class UserRepository { /* ... */ }

### Method Names
Method names should be verbs or verb phrases that describe what the method does. They should be as specific as possible.

    // Bad example
    class Order {
        public function Add() { /* ... */ }
        public function Save() { /* ... */ }
    }
    
    // Good example
    class Order {
        public function addProduct() { /* ... */ }
        public function saveToDatabase() { /* ... */ }
    }
 
### Don't be cute
Avoid using puns or jokes in your variable or function names. They may seem clever at first, but they can quickly become confusing.
    
    function countMyChickens() {
        // ...
    }
    
    function countAnimals($species) {
        // ...
    }

### Pick One Word per Concept
Be consistent in the names you choose for the same concept throughout your code. Don't use synonyms or similar words to refer to the same thing.

    // Bad example
    $userManager = new UserHandler();
    $user = $userManager->get_user();
    $userManager->create_user($user);
    $userManager->delete_user($user);
    
    // Good example
    $userRepository = new UserRepository();
    $user = $userRepository->findUserById(1);
    $userRepository->save($user);
    $userRepository->delete($user);

### Don't Pun
Be consistent in the names you choose for the same concept throughout your code. Avoid using the same word for two purposes.

    // Bad example
    class Adder {
        public function add($a, $b) {
            return $a + $b;
        }
    }
    
    // Good example
    class Calculator {
        public function sum($a, $b) {
            return $a + $b;
        }
    }

## Chapter 3: Functions

### Small Functions
Small functions are easier to read, understand, and maintain than large ones.

A few things to consider: 

- Functions should be at maximum 20 lines long.
- Lines should not be 150 characters long.
- The blocks within if, else, while, and so on should be one line long. Probably that line should be a function call.
- The nested structures (blocks) should be one or two at maximum.


		// Bad Example
		function calculateOrderTotal($orderItems, $shippingMethod) {
			$subtotal = 0;
			foreach ($orderItems as $item) {
				$subtotal += $item['price'] * $item['quantity'];
			}
			$tax = calculateTax($subtotal);
			$shipping = calculateShippingCost($shippingMethod);

			return $subtotal + $tax + $shipping;
		}
		
		// Good Example
		function calculateOrderSubtotal($orderItems) {
			$subtotal = 0;
			foreach ($orderItems as $item) {
		    		$subtotal += $item['price'] * $item['quantity'];
			}

			return $subtotal;
		}
		
		function calculateOrderTotal($orderItems, $shippingMethod) {
			$subtotal = calculateOrderSubtotal($orderItems);
			$tax = calculateTax($subtotal);
			$shipping = calculateShippingCost($shippingMethod);

			return $subtotal + $tax + $shipping;
		}

### Do One Thing
Functions should do one thing and do it well, with a clear purpose and minimal side effects..

    // Bad example - function doing multiple things
    function calculateSum($numbers) {
        $sum = 0;
        for ($i = 0; $i < count($numbers); $i++) {
            $sum += $numbers[$i];
        }
        return $sum;
    }

    // Good example - functions doing one thing
    function calculateSum($numbers) {
        $sum = 0;
        foreach ($numbers as $number) {
            $sum += $number;
        }
        return $sum;
    }

### Switch Statements
Avoid complex switch statements in functions. Instead, use polymorphism and object-oriented design for maintainable and extensible code.

    // Bad example
    function calculatePrice($product, $quantity) {
        $price = 0;
    
        switch ($product) {
            case 'apple':
                $price = 0.5;
                break;
            case 'banana':
                $price = 0.4;
                break;
            case 'orange':
                $price = 0.3;
                break;
            default:
                $price = 0;
                break;
        }
    
        return $price * $quantity;
    }
    
    // Good example
    abstract class Product {
        abstract public function getPrice();
    }
    
    class Apple extends Product {
        public function getPrice() {
            return 0.5;
        }
    }
    
    class Banana extends Product {
        public function getPrice() {
            return 0.4;
        }
    }
    
    class Orange extends Product {
        public function getPrice() {
            return 0.3;
        }
    }
    
    function calculatePrice(Product $product, $quantity) {
        return $product->getPrice() * $quantity;
    }

This approach adheres to the [Open-Closed Principle](../SOLID/SOLID_PRINCIPLES.md#the-openclosed-principle), allows for easy extensibility and improves the maintainability.

### Descriptive Names
Functions should have descriptive names that accurately reflect what they do.

Keep in mind they can be long, but long and descriptive is better than short and unclear.

Take your time choosing a name, this will improve the clarity of the code, mostly in your mind.

    // Bad example
    function calculate($order) {
         // ... code to calculate the shipping cost ..
    }
    
    // Good example
    function calculateShippingCost($order) {
        // ... code to calculate the shipping cost ...
    }

### Function Arguments
Functions should have a small number of arguments, ideally zero or one.

Triads (three arguments) should be avoided when possible.

Avoid using more than three unless there isn't another way around.

    // Bad example
    function calculateTotal($items, $taxRate, $shippingMethod) {
        $subtotal = calculateSubtotal($items);
        $tax = $subtotal * $taxRate;
        $shipping = calculateShippingCost($shippingMethod);
        return $subtotal + $tax + $shipping;
    }

    // Good example
    function calculateTotal($order) {
        $subtotal = calculateSubtotal($order['items']);
        $tax = calculateTax($subtotal);
        $shipping = calculateShippingCost($order['shippingMethod']);
        return $subtotal + $tax + $shipping;
    }

### Structure Programming
Every function and every block within a function, should have a consistent control flow, this means a single entry point and a single exit.

There should only be one return statement in a function, no break or continue statements in a loop.

    // Bad example
    function calculateSum($num1, $num2) {
        if ($num1 > 0 && $num2 > 0) {
            return $num1 + $num2;
        } else {
            return 0;
        }
    }

    // Good example 
    function calculateSum($num1, $num2) {
        $sum = $num1 + $num2;
        return $sum;
    }

### No side effects
Functions should minimize their side effects and clearly document any that they do have.

    // Bad example
    function updateUserStatus($userId, $status) {
        
        $db->query("UPDATE users SET status = '$status' WHERE id = $userId");

        Log::debug($userId . "status updated to" . $status);

        $notification->sendNotification($userId, "Your status has been updated"); 
            
        return true;
    }
    
    // Good example
    function updateUserStatus($userId, $status) {
    $updated = false;

        if ($db->query("UPDATE users SET status = '$status' WHERE id = $userId")) {
            $updated = true;
        }
        
        return $updated;
    }

### Prefer exceptions than return error codes
Using exceptions instead of error codes is a cleaner and more reliable way to handle errors and exceptions in code.

If you use exceptions instead of returned error codes, then the error processing code can be separated from the happy path code.

    // Bad example
    if ($this->deletePage($page) == 'success') {
        if ($this->registry->deleteReference($page->name) == 'success') {
            if ($this->configKeys->deleteKey($page->name->makeKey()) == 'success') {
                Log::write('debug', 'Page deleted');
            } else {
                Log::write('error', 'ConfigKey not deleted');
            }
        } else {
            Log::write('error', 'Delete reference from registry failed');
        }
    } else {
        Log::write('error', 'Delete failed');
        return 'error';
    }

    // Good example
    try {
        $this->deletePage($page);
        $this->registry->deleteReference($page->name);
        $this->configKeys->deleteKey($page->name->makeKey());
        CakeLog::write('debug', 'Page deleted');
    } catch (Exception $e) {
        CakeLog::write('error', $e->getMessage());
        throw new Exception('Delete failed');
    }

#### Extract Try/Catch blocks
Try/catch blocks are ugly and confuse the structure of the code and mix error processing with normal processing (happy path).

Extracting the blocks into functions will make it less confusing.

    // Bad example
    function processOrder($order) {
        try {
            // Process the order
            $result = process($order);
            // Do something with the result
            return $result;
        } catch (Exception $e) {
            // Handle the exception
            logError($e);
            return false;
        }
    }

    // Good example
    function processOrder($order) {
        $result = tryProcess($order);
        // Do something with the result
        return $result;
    }
    
    function tryProcess($order) {
        try {
            // Process the order
            return process($order);
        } catch (Exception $e) {
            // Handle the exception
            logError($e);
            return false;
        }
    }

#### Don't Repeat Yourself (DRY)
The DRY (Don't Repeat Yourself) principle promotes code reuse and avoiding duplication. 

It improves maintainability, reduces bugs, and enhances readability by keeping code concise and eliminating redundancy.
    
    // Bad example
    function calculateTax($amount) {
        $taxRate = 0.15;
        $tax = $amount * $taxRate;
        return $tax;
    }
    
    function calculateTotalAmount($amount) {
        $taxRate = 0.15;
        $tax = $amount * $taxRate;
        $totalAmount = $amount + $tax;
        return $totalAmount;
    }
    
    // Good example
    function calculateTax($amount) {
        $taxRate = 0.15;
        $tax = $amount * $taxRate;
        return $tax;
    }
    
    function calculateTotalAmount($amount) {
        $tax = calculateTax($amount);
        $totalAmount = $amount + $tax;
        return $totalAmount;
    }

## Chapter 4: Comments

#### Comments Do Not Make Up for Bad Code
Comments should be a last resort and not a first choice. Good code should be self-explanatory and not require comments to understand.

#### Explain Yourself in Code
Code should be written in a way that is clear and self-explanatory, with variable and function names that accurately reflect their purpose.

    // Bad example
    // Check if the array has any elements
    if(count($array) == 0) {
        // Code here
    }

    // Good example
    if(empty($array)) {
        // Code here
    }

#### Good Comments
If comments are necessary, they should be clear, concise, and to the point. Comments should explain why code is written a certain way, not how.

    // Bad example
    // This function adds two numbers together
    function addNumbers($a, $b) {
        return $a + $b;
    }   
    
    // Good example
    /** 
    * Adds two numbers together and returns the result.
    * @param int $a The first number to add.
    * @param int $b The second number to add.
    * @return int The sum of $a and $b.
    */
    function addNumbers($a, $b) {
        return $a + $b;
    }

Examples of good comments:

 - Legal comments
 - Informative comments
 - Explanation of intent
 - Clarification
 - Warnings
 - TODO 
 - Amplification

#### TODO comments
"Todo comments" are comments that are left in the codebase to indicate incomplete or unfinished work.

While they may be helpful reminders for the developer who wrote them, they can be confusing and misleading for others who come across them later.

Use them with purpose and be clear.

    // TODO: add error handling
    function doSomething($parameter) {
        // do something with the parameter
    }

### Bad Comments
Comments that are redundant, misleading, or inaccurate can be worse than no comments at all. 

Comments that explain bad code are a sign that the code should be refactored instead.

    // Bad Example - redundant

    // Set the variable to 5
    $var = 5;

    // Bad Example - misleading

    // Calculate the sum of the numbers
    $result = $a - $b;
    
    // Bad Example - innacurate

    // This loop runs 10 times
    for ($i = 0; $i < 5; $i++) {
        // some code here
    }

Some examples of bad comments:

- Mumbling comments
- Redundant comments
- Misleading comments
- Journal comments
- Mandated comments
- Noise comments
- Position markers 
- Closing brace comments
- Commented-out code 

#### Commented-Out Code
Probably the worse in my opinion, commented-out code should be removed instead of left in the codebase. Version control systems can be used to retrieve old code if necessary.

    // function oldFunction() {
    //   // Some old code here
    // }

***
> Above all, don’t use a comment when you can use a function or a variable.
***

## Chapter 5: Formatting

#### The Newspaper Metaphor: 
Apply the concept of a newspaper to code formatting, where important information is placed upfront and less significant details follow. 

This metaphor helps structure the code in a way that emphasizes the most critical aspects and improves code readability.

### Vertical Formatting

#### Vertical Openness between Concepts:
Use vertical spacing to create visual separation between different concepts, such as between methods, classes, or logical blocks.

This improves code readability by making it easier to distinguish and understand different parts of the codebase.

#### Vertical Density:
Balance vertical spacing to avoid excessive gaps or tightly packed code. Aim for a visually appealing code layout that provides enough breathing room between code elements while maintaining readability.

    // Bad
    function calculateTotal($items) {
        $total = 0;
    
        foreach ($items as $item) {
            $subtotal = $item->getPrice() * $item->getQuantity();
            
            $total += $subtotal;
        }
        
        return $total;
    }

    // Good
    function calculateTotal($items) {
        $total = 0;
        foreach ($items as $item) {
            $subtotal = $item->getPrice() * $item->getQuantity();
            $total += $subtotal;
        }

        return $total;
    }

#### Vertical Distance:
Manage vertical distance between related code elements, such as grouping related lines of code together, to improve code organization and make the relationships between them more apparent.

    // Bad
    function validateUser($user) {
        if (empty($user->getName())) {
        return false;
        }
        if (empty($user->getEmail())) {
        return false;
        }
        if (empty($user->getPassword())) {
        return false;
        }
        return true;
    }
    
    // Good
    function validateUser($user) {
        if (empty($user->getName())) {
        return false;
        }
        
        if (empty($user->getEmail())) {
            return false;
        }
        
        if (empty($user->getPassword())) {
            return false;
        }
        
        return true;
    }

We should consider the following concepts:

- **Variable declarations**: Variables should be declared as close to their usage as possible. Local variables should appear at the top of each function.
- **Instance variables**: they should be declared at the top of the class, because, in a well-designed class, they are used by many (if not all) of the methods of the class.
- **Dependent functions**: If one function calls another, they should be vertically close, and the caller should be above the callee.
- **Conceptual Affinity**: the stronger that affinity, the less vertical distance there should be between them.

#### Vertical Ordering:
We want functions call dependencies to point in the downward direction. A function that is called should be below a function that does the calling.

Like a newspaper, what's more important comes first and less detailed.

We can see previous concepts applied in one example:

    class OrderProcessor {
        private $shippingAddress;
        private $userEmail;
        private $OrderNumer;

        public function processOrder($order) {
            $this->validateOrder($order);
            $this->saveOrder($order);
            $this->sendConfirmationEmail($order);
        }

        private function validateOrder($order) {
            // Validate the order data
            // ...
        }
        
        private function saveOrder($order) {
            // Save the order to the database
            // ...
        }
        
        private function sendConfirmationEmail($order) {
            // Send a confirmation email to the customer
            // ...
        }
    }

### Horizontal Formatting

#### Horizontal Openness and Density:
Maintain a balance between horizontal spacing and compactness to make the code readable without excessive gaps or crowding.

#### Horizontal Alignment:
We can align code elements, such as assignment operators or method arguments, vertically to enhance readability and make code structure more visually consistent.

But strict horizontal alignment is not always useful and can sometimes hinder readability. 

While it may look aesthetically pleasing, it can make the code more difficult to read and maintain, especially when changes are made to the code.

Remember to prioritize readability and consistency over strict horizontal alignment.
    
    // Not necessary
    class Configuration {
    private $appName   = "My App";
    private $version   = "1.0";
    private $env       = "development";
    
    private $dbHost    = "localhost";
    private $dbUser    = "root";
    private $dbPass    = "password";
    private $dbName    = "my_database";
    }


    // Good
    class Configuration {
    private $version = "1.0";
    private $appName = "My App";
    private $env = "development";
    
    private $dbUser = "root";
    private $dbPass = "password";
    private $dbHost = "localhost";
    private $dbName = "my_database";
    }

#### Indentation: 
Use consistent indentation with proper spacing to visually represent the hierarchical structure of the code. 

This helps improve readability and makes it easier to understand the code's logical flow.

    // Bad
    function calculateTotal($items) {
    $total = 0;
    foreach ($items as $item) {
    $subtotal = $item->getPrice() * $item->getQuantity();
    $total += $subtotal;
    }
    return $total;
    }
    
    // Good
    function calculateTotal($items) {
        $total = 0;
        foreach ($items as $item) {
            $subtotal = $item->getPrice() * $item->getQuantity();
            $total += $subtotal;
        }
        return $total;
    }

### Team Rules:
Establish and follow team-specific formatting rules or style guides to ensure consistent formatting across the codebase. 

> Consistent formatting practices enable easier code reviews, reduce confusion, and make the codebase more maintainable.

## Chapter 6: Objects and Data Structures

#### Data Abstraction
Hiding implementation is not about putting another layer of functions between the variables but rather exposing abstract interfaces that allow users to manipulate the essential data hiding unnecessary details about the implementation.

    // Bad Example
    class Point {
        public $X;
        public $Y;
    }
    
    
    // Good Example
    interface Point {
        public function getX();
        public function getY();
        public function setCartesian($x, $y);
        public function getR();
        public function getTheta();
        public function setPolar($r, $theta);
    }

By using an interface, we abstract the implementation details of the Point class. 

Any class that implements this interface must provide the necessary methods to work with point coordinates, ensuring consistency and adherence to the contract defined by the interface.

#### Data/Object Anti-Symmetry
> Data structures expose data and have no behavior.  Data structures make it easy to add functions without the need to modify existing structures.

    class Rectangle {
        public $width;
        public $height;
    }
    
    class Circle {
        public $radius;
    }
    
    function calculateRectangleArea(Rectangle $rectangle) {
        return $rectangle->width * $rectangle->height;
    }
    
    function calculateCircleArea(Circle $circle) {
        return pi() * $circle->radius * $circle->radius;
    }
    
    $rectangle = new Rectangle();
    $rectangle->width = 5;
    $rectangle->height = 10;
    echo "Rectangle Area: " . calculateRectangleArea($rectangle) . "\n";
    
    $circle = new Circle();
    $circle->radius = 7;
    echo "Circle Area: " . calculateCircleArea($circle) . "\n";

> Objects expose behavior and hide data.  Objects make it easy to add classes without the need to modify existing functions.

    interface Shape {
        public function calculateArea();
    }
    
    class Rectangle implements Shape {
        private $width;
        private $height;
        
        public function __construct($width, $height) {
            $this->width = $width;
            $this->height = $height;
        }
        
        public function calculateArea() {
            return $this->width * $this->height;
        }
    }
    
    class Circle implements Shape {
        private $radius;
    
        public function __construct($radius) {
            $this->radius = $radius;
        }
    
        public function calculateArea() {
            return pi() * $this->radius * $this->radius;
        }
    }
    
    function calculateArea(Shape $shape) {
        return $shape->calculateArea();
    }
    
    $rectangle = new Rectangle(5, 10);
    echo "Rectangle Area: " . calculateArea($rectangle) . "\n";
    
    $circle = new Circle(7);
    echo "Circle Area: " . calculateArea($circle) . "\n";

#### The Law of Demeter 

> The Law of Demeter says that a method M of class C should only call the methods of these
>- C itself.
>- M's parameters.
>- Any objects created within M.
>- C's direct component objects.
>- A global variable, accessible by C, in the scope of M. 

In simpler words, objects should only communicate with their immediate neighbors and should not have knowledge of the internal workings of other objects.

    // Breaks law
    $outputDir = $ctxt->getOptions()->getInternalScratchDir()->getAbsolutePath();

In the following example, we assume that the `$ctxt` object has a method `getOutputDirectoryPath()` that encapsulates the logic of retrieving the output directory path. 
This method internally handles the necessary calls to other objects, such as `$ctxt->getOptions()->getScratchDir()->getAbsolutePath()`, but the caller doesn't need to be aware of this implementation detail.

    // Follow law
    $outputDir = $ctxt->getOutputDirectoryPath();

#### Train Wrecks 
Avoid long chains of method invocations, known as "train wrecks", that can lead to tight coupling and reduced readability.

	// Bad
	$result = $object->method1()->method2()->method3()->method4()->method5();

	// Good
	$method1Result = $object->method1();
	$method2Result = $method1Result->method2();
	$result = $method2Result->method3(); // Improved readability

#### Data Transfer Objects
This is a form of a data structure which is a class with public variables and no functions and sometimes called DTO.
 
You can use DTOs to transfer data between different layers of an application or between systems, focusing on data structure rather than behavior.

    class Address {
        private $street;
        private $city;
        private $state;
        private $zipCode;
    
        public function __construct($street, $city, $state, $zipCode) {
            $this->street = $street;
            $this->city = $city;
            $this->state = $state;
            $this->zipCode = $zipCode;
        }
    
        public function getStreet() {
            return $this->street;
        }
    
        public function getCity() {
            return $this->city;
        }
        public function getState() {
            return $this->state;
        }
    
        public function getZipCode() {
            return $this->zipCode;
        }
    }

> Important: Avoid mixing object-oriented code and data structures (hybrids), as it can lead to confusion and compromise the benefits of both paradigms.

## Chapter 7: Error Handling

#### Use Exceptions, Not Return Codes
As seen in Chapter 3: _"Functions"_, it describes the use of exceptions for error handling instead of relying on return codes or special values.

#### Write Your try-catch-finally Statement First
It is really important to plan the error handling code upfront before writing the main logic.

This help us to define what the user should expect, no matter what happens with the code executed in the _try_.

We can have tests that force the _catch_ and add behaviour to our handler to satisfy the tests.

#### Provide Context with Exceptions
Include relevant context information in the exception messages to aid in diagnosing and debugging errors.

    // Bad 
    try {
        // Code that may throw an exception
    } catch (Exception $e) {
        // Handling the exception without providing context
        Log::error($e->getMessage());
    }
    
    // Good 
    try {
        // Code that may throw an exception
    } catch (Exception $e) {
        // Handling the exception with context
        Log::error("An error occurred: " . $e->getMessage());
    }

#### Define Exception Classes  

If you are using third-party APIs, it could be useful to wrap it and define the exceptions related to the errors we're receiving by their source.

Also, by wrapping, you won't be tied to the particular vendor's API design choices, instead, you can define an API you are conformable with. 

#### Define Normal Flow
Structure the code in such a way that the normal flow of the program is not disrupted by error handling code. 

Separate error handling from the main logic to maintain clarity and readability.

    // Bad 
    function processOrder($order) {
        if ($order->isCancelled()) {
            return null; 
        }
        // Process the order
        return $order->getTotal();
    }
    
    // Good 
    function processOrder($order) {
        if ($order->isCancelled()) {
            throw new OrderCancelledException("Order is cancelled.");
        }
        // Process the order
        return $order->getTotal();
    }

#### Don't Return Null
Avoid returning null from a method, as it can lead to null reference exceptions and make the code more error-prone. 

Instead, consider returning empty collections, optional values, or using special case objects.

    // Bad Example
    function findUser($userId) {
        // Logic to find the user
        if ($userExists) {
            return $user;
        } else {
            return null;
        }
    }
    
    // Good Example
    function findUser($userId) {
        // Logic to find the user
        if ($userExists) {
            return $user;
        } else {
            throw new UserNotFoundException("User not found.");
        }
    }

#### Don't Pass Null
Returning null from methods is bad but passing null as a parameter to a method is worse.

Null values can cause unexpected behavior and make the code more difficult to reason about. 

Instead, use default values, or separate methods to handle different scenarios.

    // Bad Example
    function processOrder($order, $discount = null) {
        if ($discount !== null) {
            // Apply the discount
        }
        // Process the order
    }
    
    // Good Example
    function processOrder($order, $discount = 0) {
        if ($discount > 0) {
            // Apply the discount
        }
        // Process the order
    }

## Chapter 8: Boundaries

#### Exploring and Learning Boundaries & Learning Tests Are Better Than Free
Using third-party code can greatly accelerate our development process. 

However, comprehending its inner workings and seamlessly integrating it into our project can present challenges.

By writing tests for the third-party code, we can gain a deeper understanding of its functionality and ensure that it aligns with our expectations. 

These tests also serve as a safety net in the event of any changes or compatibility issues, allowing us to catch and address them proactively before they impact our project.

#### Using Code That Does Not Yet Exist 
Defining interfaces, contracts, or placeholders for code that is not yet implemented, allowing other parts of the system to interact with and depend on the expected behavior without the actual implementation being present.

> _Clean Boundaries: It's better to depend on something **you** can control than on something you don't control, lest it end up controlling you._

## Chapter 9: Unit Tests

#### Three Laws of TDD
The three laws guide the development process in Test-Driven Development (TDD), emphasizing writing tests before implementing the code.

1- You are not allowed to write any production code unless it is to make a failing unit test pass.

2- You are not allowed to write more of a unit test than is sufficient to fail, or fail to compile.

3- You are not allowed to write more production code than is sufficient to pass the one failing unit test.

This means we will be writing tests along the production code, which will eventually cover all of our codebase.

#### Keeping Tests Clean
Unit tests should be well-structured, readable, and maintainable, following the same clean code principles as production code.

> _Test code is just as important as production code._

#### Clean Tests
Clean tests are easy to read and understand. They have clear and descriptive names, well-structured code, and minimal complexity. 

They should be easily comprehensible by any developer.

#### One Assert & Concept per Test
Unit tests should ideally have a single assertion, focusing on testing one specific behavior or aspect of the code in isolation, to ensure clarity and maintainability of the tests.

Same applies for concepts, having one concept per test avoids long test functions that mix unrelated behaviors.

#### F.I.R.S.T
Unit tests should follow these five other rules:

- Fast: Should run quickly to be executed often.
- Independent: Don't rely on each other.
- Repeatable: Run in any environment. 
- Self-Validating: Boolean output, pass or fail. 
- Timely: Written before the production code is done.

## Chapter 10: Classes

### Class Organization
The class structure should typically start with constant declarations (if any), followed by properties, and then methods.

- Constants: If your class needs to define any constants, they should be declared at the beginning of the class before any properties or methods.

- Properties: Class properties represent the data or state of an object. They should be declared after the constants and before the methods.

- Methods: Methods define the behavior and actions that can be performed by an object. They should be declared after the properties.

Within each section (constants, properties, methods), it's beneficial to maintain a consistent order and grouping. 

You can order properties and methods alphabetically or categorize them based on their functionality. 

The goal is to make it easy for developers to locate and understand the different elements of the class.

#### Encapsulation
Keep variables and utility functions `private` whenever it's possible, if not, make it `protected` for tests to have access.

### Classes should Be Small!
> The size of a Class should be measured by its responsibilities, not its size.

#### The Single Responsibility Principle (SRP) 
It states that class should have only one reason to change, meaning it should have a single responsibility or purpose.

When a class has a single responsibility, it tends to be smaller and focused on a specific task or functionality. 

Small classes also promote better separation of concerns and improve code re-usability.

#### Cohesion
Classes should have a small number of instance variables, each of the methods of a class should manipulate one or more of those variables.
The more variables a method manipulates, the more cohesive that the method is to its class.

### Organizing for Change
> Change is continual and every change put us to the risk that the system no longer works as intended.

#### Isolating from Change
We can introduce interfaces and abstract classes in a way that isolates and minimizes the impact of potential changes, making it easier to modify or extend the system without affecting other parts.


## Chapter 11: Systems

### Separate constructing a system from using it
The process of constructing a system, such as setting up dependencies and initializing components, should be separated from the code that uses the system.
This promotes flexibility and testability.

Some principles we can follow to achieve this are:

#### Separation of Main:
Instead of having the main function directly handle the core logic, extract the core logic into a separate class or module.

    // main.php
    $system = new System();
    $system->run();

    // System.php
    class System {
        public function run() {
            // Core logic here
        }
    }

#### Factories: 
Utilize a factory class to create instances of complex objects, providing a centralized place for construction logic and enabling easy changes or variations in object creation.

    class ObjectFactory {
        public function createObject() {
            // Construction logic here
            return new Object();
        }
    }

#### Dependency Injection: 
Use dependency injection to decouple dependencies and make them explicit.

Rather than directly instantiating dependencies within a class, pass them as parameters or inject them through setters or constructors.

    class MyClass {
        private $dependency;
    
        public function __construct(Dependency $dependency) {
            $this->dependency = $dependency;
        }
    }

### Scaling up

> Designing a system to handle scalability involves considering factors such as performance, concurrency, and distributed architectures. 
> Scalability should be planned from the early stages of development.

### Test Drive the system architecture:
We can start a software project with a "naively simple" but nicely decoupled architecture, delivering value quickly and adding more infrastructure as we scale up. 

This means, that we can start simple, but we must sustain the ability to change course in response to evolving circumstances.

> In summary, systems must be clean too. An invasive architecture overwhelms the domain logic and impacts agility.

## Chapter 12: Emergence

Here are the rules that are given by Kent Beck to create good designs:

- Run all tests: They verify that the system behaves as expected.
- Eliminate duplication: Duplicated code brings additional work.
- Express the intention of the programmer: Use more expressive code to facilitate maintenance. Choose good names for functions, classes and tests shouldn’t be small and well written.
- Minimize the number of classes and methods: Following this pattern can ignore it if the classes are very small.
- Apply all knowledge to improve the design during refactoring: Increase cohesion, reduce coupling, separate responsibilities, reduce classes and methods, choose the best names.
  
Even applying it once, you will not be able to have good software. 
You need to do this over and over again to achieve continuous improvement.

