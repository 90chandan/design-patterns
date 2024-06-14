# design-patterns
**1. Creational Design Pattern -Creational design patterns solve the problems related to object creation. They help to abstract away object creation processes that spread across multiple classes.**

1. Singleton
2. Factory Method
3. Abstract Factory
4. Builder
4. Prototype


**2. Structural Design Patterns : The structural design patterns suggest implementing relationships between classes and objects.**

1. Adapter
2. Bridgec
3. Composite
4. Decorator
5. Facade
6. Flyweight
7. Proxy

 
**3. Behavioural Design Patterns : Behavioral Patterns are concerned with algorithms and the assignment of resposibilities between objects.**

 1. Chain of Responsibility
 2. Comman
 3. Iterator
 4. Mediator
 5. Memento
 6. Observer
 7. State
 9. Stategy
 10. Template Method
 11. Visitor

**===================singleTon Design Patter =============================**

Singleton is a class that allows only a single instance of itself to be created.

Advantages of Singleton Design Pattern
* Singleton pattern can implement interfaces.
* Can be lazy-loaded and has Static Initialization.
* It helps to hide dependencies.
* It provides a single point of access to a particular instance, so it is easy to maintain.

Disadvantages of Singleton Design Pattern
+ Unit testing is a bit difficult as it introduces a global state into an application
+ Reduces the potential for parallelism within a program by locking.

The following compares Singleton class vs. Static methods,
- A Static Class cannot be extended whereas a singleton class can be extended.
- A Static Class cannot be initialized whereas a singleton class can be.
- A Static class is loaded automatically by the CLR when the program containing the class is loaded.

```

public sealed class SingleTon
{
    private static SingleTon _instance = null;
    private static readonly Object padlock = new Object();

    private SingleTon() { }

    public static SingleTon Instance{
        get{
            lock(padlock)
            {
                if(_instance == null){
                    _instance = new SingleTon();
                }
                return _instance;
            }
    }
} 

```
**======================C# Factory Method Design Pattern======================**

The Factory Method design pattern defines an interface for creating an object, but let subclasses decide which class to instantiate. This pattern lets a class defer instantiation to subclasses.

Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

```

public interface INotification
{
    void SendNotification();
}


public class EmailNotification : INotification
{
    public void SendNotification
    {
        Console.WriteLine("Sending email notification")
    }
}


public class SMSNotification : INotification
{
    public void SendNotification()
    {
        Console.WriteLine("Sending sms notification");   
    }
}


public abstract class NotificationFactory
{
    public abstract INotification CreateNotification();
}

public class EmailNotificationFactory : INotificationFactory
{
    public override INotification CreateNotification()
    {
        return new EmailNotification();
    }
}


public SMSNotificationFactory : NotificationFactory
{
    public overide INotification CreateNotification()
    {
        return SMSNotification();
    }
}

class Program {
    static void Main(string[] args)
    {

        Notificationfactory emailFactory = new EmailNotificationFactory();
        INotification emailNotification = emailFactory.createNotification();
        emailNotification.SendNotification();

        NotificationFactory smsFactory = new SMSNotificationFactory();
        INotification smsNotification = smsFactory.CreateNotification();
        smsNotification.SendNotification();
    }
}

```

**===============Abstract Factory===================**

The Abstract Factory design pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.

```
public interface INotification
{
    void Send();
}


public void EmailNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Sending email notification");
    }
}


public void SMSNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Send SMS notification")
    }
}

public interface INotificationFactory
{
    INotification CreateEmailNotification();
    INotification CreateSMSNotification();
}

public class NotificationFactory : INotificationFactory
{
    public INotification CreateEmailNotification()
    {
         return new EmailNotification();
    }

    public INotification CreateSMSNotification()
    {
        return new SMSNotification();
    }
}

class Program
{
    //create a notification factory

    INotificationFactory notificationFactory = new NotificationFactory();

    //create an email notification
    INotification emailNotification = notificationFactory.createEmailNotification();
    emailNotification.Send();

    //create an SMS notification
    INotification smsNotification = notificationFactory.createSMSNotification();
    smsNotification.Send();
}

```
**===================================Builder design pattern===========================================**

The Builder design pattern separates the construction of a complex object from its representation so that the same construction process can create different representations.

**Builder :** This is an interface which is used to define all the steps required to create a product.

**ConcreteBuilder :** This is a class which implements the Builder interface to create a complex product.

**Product :** This is a class which defines the parts of the complex object which are to be generated by the Builder Pattern.

**Director :** This is a class that is used to construct an object using the Builder interface.

 
**WHEN TO USE THIS PATTERN :** When we need to create a complex object in several steps (a step by step approach).

**ADVANTAGES**

- Code is more maintainable and readable.

- Less prone to errors as we have a method which returns the finally constructed object.

**DISADVANTAGES**

- Number of lines of code increases in builder pattern, but it makes sense as the effort pays off in terms of maintainability and readability.

**Conclusion**

So, if your object has only a few constructor arguments, it makes no sense to use the builder pattern. Builder pattern is a good choice when designing classes whose constructors have more than a handful of parameters.

```
using System;
					
public class Program
{
	public static void Main()
	{
		var builder = new NotificationBuilder();
		var director = new Director(builder);
		director.ConstructNotification("Greetings", "Greetings of the day!", "test@gmail.com");
		var notification = builder.Build();	
		notification.PrintDetails();
	}
}

//product
public class Notification
{
	public string Title {get;set;}
	public string Body {get;set;}
	public string Recipient {get;set;}
	
	public void PrintDetails()
	{
		Console.WriteLine("Title : "+ Title + " Body : " + Body+ "Receipient "+ Recipient);
	}
}

//builder interface
public interface INotificationBuilder
{
	void SetTitle(string title);
	void SetBody(string body);
	void SetRecipient(string recipient);
	
	Notification Build();
}

public class NotificationBuilder : INotificationBuilder
{
	private Notification _notification = new Notification();
	
	public void SetTitle(string title)
	{
		_notification.Title = title;	
	}
	
	public void SetBody(string body)
	{
		_notification.Body = body;
	}
	
	public void SetRecipient( string recipient)
	{
		_notification.Recipient = recipient;
	}
	
	public Notification Build()
	{
		return _notification;
	}
}

//Director
public class Director
{
	private INotificationBuilder _builder;
	public Director(INotificationBuilder notificationBuilder)
	{
		_builder = notificationBuilder;
	}
	
	public void ConstructNotification(string title, string body, string recipient)
	{
		_builder.SetTitle(title);
		_builder.SetBody(body);
		_builder.SetRecipient(recipient);
	}
	
}

```
**==============================================Prototype Design Pattern=================================================**

* The Prototype design pattern is a creational pattern that enables the creation of new objects by copying an existing object, known as the prototype. This pattern is useful when creating a new object is costly or complex.
* The Prototype pattern is typically implemented by creating a Clone method in the prototype class. This method creates a new instance of the class and copies the data from the original instance to the new one.

**Example: Document Cloning**
Consider a scenario where you have different types of documents (e.g., resumes, reports) that share some common properties but also have their specific details. Creating each document from scratch every time is costly, so you can use the Prototype pattern to clone existing documents and then modify them as needed.

```
public interface IDocumentPrototype
{
    IDocumentPrototype Clone();
}


public class Document : IDocumentPrototype
{
    public string Title { get; set; }
    public string Content { get; set; }

    public Document(string title, string content)
    {
        Title = title;
        Content = content;
    }

    public virtual IDocumentPrototype Clone()
    {
        return new Document(Title, Content);
    }

    public override string ToString()
    {
        return $"Title: {Title}, Content: {Content}";
    }
}

public class Resume : Document
{
    public string CandidateName { get; set; }
    public string Experience { get; set; }

    public Resume(string title, string content, string candidateName, string experience)
        : base(title, content)
    {
        CandidateName = candidateName;
        Experience = experience;
    }

    public override IDocumentPrototype Clone()
    {
        return new Resume(Title, Content, CandidateName, Experience);
    }

    public override string ToString()
    {
        return $"Title: {Title}, Content: {Content}, Candidate Name: {CandidateName}, Experience: {Experience}";
    }
}


public class Program
{
    public static void Main()
    {
        // Create an original resume
        Resume originalResume = new Resume("John Doe's Resume", "Experience in software development...", "John Doe", "5 years");

        // Clone the original resume
        Resume clonedResume = (Resume)originalResume.Clone();

        // Modify the cloned resume
        clonedResume.CandidateName = "Jane Doe";
        clonedResume.Experience = "3 years in data science";

        // Display the resumes
        Console.WriteLine("Original Resume:");
        Console.WriteLine(originalResume);

        Console.WriteLine("\nCloned Resume:");
        Console.WriteLine(clonedResume);
    }
}


```

**==================================Adapter Design Pattern============**

The Adapter design pattern converts the interface of a class into another interface clients expect. This design pattern lets classes work together that couldn‘t otherwise because of incompatible interfaces.

The Adapter Design pattern is a structural pattern that allows object with incompatible interfaces to work together.It acts as bridge between the old and new interfaces, enabling them to collaborate seamlessly.

```

public interface IWeatherService
{
    float GetTemperatureInCelsius();
}

public class FarenheitWeatherServiceAdapter : IWeatherService
{
    private readonly ExternalWeatherService _externalWeatherService;

    public FarenheitWeatherServiceAdapter(ExternalWeatherService externalService){

        _externalService = externalService;
    }

    public float GetTemperatureInCelsius(){

        //convert Farenheit to celcius

        float temperatureInFarenheit = _externalWeatherService.GetTemperature();

        return (temperatur - 32) *5 / 9;

    }

}

```
**======================================================Bridge Design Patter =================================================**

The Bridge design pattern allows you to separate the abstraction from the implementation,so that the two can vary independently.

The bridge design pattern is a type of structural design pattern which is used to split a large class into two separate inheritance hierarchies (a collection of 'is-a' relationships); one for the implementations and one for the abstractions. These hierarchies are then connected to each other via object composition, forming a bridge-like structure. This pattern is also known as the Handle-Body Design Pattern.

``` 
namespace BridgePatternRealTimeExample
{
    /** 
    This is going to be an interface that acts as a bridge between the Abstraction Layer and Implementation Layer
    The following Implementor Interface defines the operations for all implementation classes. It doesn't have to match the Abstraction's interface.
    In fact, the two interfaces can be entirely different.
     **/

    public interface IMessageSender
    {
        void SendMessage(string Message);
    }
}

 
using System;
namespace BridgePatternRealTimeExample
{
    /** 
    This is going to be a class that implements the Implementor Interface i.e. IMessageSender. It also provides the implementation details for the associated Abstraction class. Each Concrete Implementation corresponds to a specific platform, in this case sending messages using SMS.
    **/

    public class SmsMessageSender : IMessageSender
    {
        public void SendMessage(string Message)
        {
            //Send a message using SMS
            Console.WriteLine("'" + Message + "'   : This Message has been sent using SMS");
        }
    }
}

 
using System;
namespace BridgePatternRealTimeExample
{
    /** 
    This is going to be a class that implements the Implementor Interface i.e. IMessageSenderIt also provides the implementation details for the associated Abstraction class. Each Concrete Implementation corresponds to a specific platform, in this case sending messages using Email.
    **/

    public class EmailMessageSender : IMessageSender
    {
        public void SendMessage(string Message)
        {
            Console.WriteLine("'" + Message + "'   : This Message has been sent using Email");
        }
    }
}

 
namespace BridgePatternRealTimeExample
{
    /**
    This is an abstract class that going to be implemented by the Concrete Abstraction. It contains a reference to an object of type IMessageSender Interface i.e. messageSender and delegates all of the real work to this object (the class that implements IMessageSender Interface). It can also act as the base class for other abstractions.
    **/

    public abstract class AbstractMessage
    {
        protected IMessageSender messageSender;
        public abstract void SendMessage(string Message);
    }
}

using System;
namespace BridgePatternRealTimeExample
{
    /** 
    This is going to be a concrete class which inherits from the Abstraction class i.e. AbstractMessage. This Concrete Abstraction Class implements the operations defined by AbstractMessage class.
    **/

    public class ShortMessage : AbstractMessage
    {
        //The constructor expected an argument of type object which implements the IMessageSender interface

        public ShortMessage(IMessageSender messageSender)
        {
            //Initialize the super class messageSender variable

            this.messageSender = messageSender;
        }

        public override void SendMessage(string Message)
        {
            if (Message.Length <= 10)
            {
                messageSender.SendMessage(Message);
            }
            else
            {
                Console.WriteLine("Unable to send the message as length > 10 characters");
            }
        }
    }
}
 
namespace BridgePatternRealTimeExample
{
    /** 
    This is going to be a concrete class that inherits from the Abstraction class i.e. AbstractMessage. This Concrete Abstraction Class implements the operations defined by AbstractMessage class.
    **/

    public class LongMessage : AbstractMessage
    {
        public LongMessage(IMessageSender messageSender)
        {
            //Initialize the super class messageSender variable
            this.messageSender = messageSender;
        }

        public override void SendMessage(string Message)
        {
            messageSender.SendMessage(Message);
        }
    }
}

 
using System;
namespace BridgePatternRealTimeExample
{
    class Program
    {
        static void Main(string[] args)
        {
            /** 
            Except for the initialization phase, where an Abstraction object i.e. LongMessage or ShortMessage linked with a specific Implementation object i.e. new EmailMessageSender() or new SmsMessageSender(), the client code should only depend on the Abstraction class i.e. AbstractMessage
            **/
 
            Console.WriteLine("Select the Message Type 1. For longmessage or 2. For shortmessage");
            int MessageType = Convert.ToInt32(Console.ReadLine());

            Console.WriteLine("Please enter the message that you want to send");

            string Message = Console.ReadLine();

            if (MessageType == 1)
            {
                AbstractMessage longMessage = new LongMessage(new EmailMessageSender());
                longMessage.SendMessage(Message);
            }
            else
            {
                AbstractMessage shortMessage = new ShortMessage(new SmsMessageSender());
                shortMessage.SendMessage(Message);
            }
 
            Console.ReadKey();
        }
    }
}

```


**=================================================Facade Design Patter ============================================================**

Facade pattern hides the complexities of the system and provides an interface to the client using which the client can access the system. Facade is a structural design pattern that provides a simplified interface to a complex system making it easier to use.

This pattern involves a single wrapper class which contains a set of members which are required by the client. These members access the system on behalf of the facade client and hide the implementation details.

The facade design pattern is particularly used when a system is very complex or difficult to understand because the system has a large number of interdependent classes or its source code is unavailable.

Facade Design pattern falls under Structural Pattern. The Facade design pattern is particularly used when a system is very complex or difficult to understand because the system has a large number of interdependent classes or its source code is unavailable.
 

Example without using Facade Design Pattern
 
```
namespace FacadeDesignPatternRealTimeExample
{
    public class Customer
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public string MobileNumber { get; set; }
        public string Address { get; set; }
        //Any other Properties as per the Business Requirements
    }
}


using System;
namespace FacadeDesignPatternRealTimeExample
{
    public class Validator
    {
        public bool ValidateCustomer(Customer customer)
        {
            //Need to Validate the Customer Object
            Console.WriteLine("Customer Validated...");
            Console.WriteLine($"Name:{customer.Name}");
            Console.WriteLine($"Email:{customer.Email}");
            Console.WriteLine($"Mobile:{customer.MobileNumber}");
            Console.WriteLine($"Address:{customer.Address}");

            return true;
        }
    }
}

using System;
namespace FacadeDesignPatternRealTimeExample
{
    public class CustomerDataAccessLayer
    {
        public bool SaveCustomer(Customer customer)
        {
            //Save the Customer in the Database
            Console.WriteLine("\nCustomer Saved into the Database...");

            return true;
        }
    }
}

using System;
namespace FacadeDesignPatternRealTimeExample
{
    public class Email
    {
        public bool SendRegistrationEmail(Customer customer)
        {
            //Send Registration Successful Email to Customer
            Console.WriteLine("\nRegistration Email Send to Customer...");

            return true;
        }
    }
}

 
            Customer customer = new Customer()
using System;
namespace FacadeDesignPatternRealTimeExample
{
    class Program
    {
        static void Main(string[] args)
        {
            //Step1: Create an Instance of Customer Class
            {
                Name = "Pranaya",
                Email = info@dotnettutorials.net,
                MobileNumber = "1234567890",
                Address = "BBSR, Odisha, India"
            };

            //Step2: Validate the Customer
            Validator validator = new Validator();
            bool IsValid = validator.ValidateCustomer(customer);

            //Step3: Save the Customer Object into the database
            CustomerDataAccessLayer customerDataAccessLayer = new CustomerDataAccessLayer();
            bool IsSaved = customerDataAccessLayer.SaveCustomer(customer);

            //Step4: Send the Registration Email to the Customer
            Email email = new Email();
            email.SendRegistrationEmail(customer);

            Console.ReadKey();
        }
    }
}

```

**Problem with the above Design :**

The problem is now we have many subsystems like Validator, CustomerDataAccessLayer, and Email. And the Client needs to follow the appropriate sequence to create and consume the objects of the above subsystems. And here, there is a high chance that the client might not follow the proper sequence, or the client might forget to use one of the subsystems.

 

Implementing Facade Design Pattern:

```
namespace FacadeDesignPatternRealTimeExample
{
    public class CustomerRegistration
    {
        public bool RegisterCustomer(Customer customer)
        {
            //Step1: Validate the Customer
            Validator validator = new Validator();
            bool IsValid = validator.ValidateCustomer(customer);

            //Step1: Save the Customer Object into the database
            CustomerDataAccessLayer customerDataAccessLayer = new CustomerDataAccessLayer();
            bool IsSaved = customerDataAccessLayer.SaveCustomer(customer);

            //Step3: Send the Registration Email to the Customer
            Email email = new Email();
            email.SendRegistrationEmail(customer);

            return true;
        }
    }
}

 
using System;
namespace FacadeDesignPatternRealTimeExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create an Instance of Customer Class
            Customer customer = new Customer()
            {
                Name = "Pranaya",
                Email = info@dotnettutorials.net,
                MobileNumber = "1234567890",
                Address = "BBSR, Odisha, India"
            };

            //Using Facade Class
            CustomerRegistration customerRegistration = new CustomerRegistration();
            customerRegistration.RegisterCustomer(customer);

            Console.ReadKey();
        }
    }
}

```
**=================================================Decorator Design Pattern ============================================================**

The Decorator Design Pattern allows us to dynamically add new functionalities to an existing object without altering or modifying its structure. This design pattern acts as a wrapper to the existing class. That means the Decorator Design Pattern dynamically changes the functionality of an object at runtime without impacting the existing functionality of the object.

In short, this design pattern adds additional functionalities to the object by wrapping it. A decorator is an object that adds features to another object.

```
public interface Component
{
    void Operation();
}

public class ConcreteComponent : Component
{
    public void Operation()
    {
        Console.WriteLine("Component Operation");
    }
}

public abstract class Decorator : Component
{
    private Component _component;
    
    public Decorator(Component component)
    {
        _component = component;
    }
    
    public virtual void Operation()
    {
        _component.Operation();
    }

}


public class ConcreteDecorator : Decorator
{
    public ConcreteDecorator(Component component) : base(component) { }
    
    public override void Operation()
    {
        base.Operation();
        
        Console.WriteLine("Override Decorator Operation");
    }
}

**Real life example **
Suppose you want to prepare either chicken pizza or vegetable pizza. Then what you need to do is, first you need to prepare the plain pizza. Then the Pizza Decorator will add chicken to the plain pizza if you want chicken pizza or it will add vegetables to the plain pizza if you want vegetable pizza. So, you are getting chicken pizza or vegetable pizza by adding chicken or vegetables to the plain pizza by the Pizza decorator.

 
Step1: Creating the Pizza interface (Component)

namespace DecoratorDesignPatternRealTimeExample
{
    // This is the Base Component interface that defines operations that can be altered by decorators

    public interface Pizza
    {
        string MakePizza();
    }
}

 
Step2: Creating Plain Pizza (Concrete Component)

namespace DecoratorDesignPatternRealTimeExample
{
    // This is the Base Component interface that defines operations that can be altered by decorators

    public interface Pizza
    {
        string MakePizza();
    }
}

Step3: Creating Pizza Decorator (Decorator)

namespace DecoratorDesignPatternRealTimeExample
{
    //Inherited from the Base Component Interface

    public abstract class PizzaDecorator : Pizza
    {
        //Create a Field to store the existing Pizza Object

        protected Pizza pizza;
        
        //Initializing the Field using Constructor
        //While Creating an instance of the PizzaDecorator (Instance of the Child Class that Implements PizzaDecorator abstract class)
        //We need to pass the existing pizza object which we want to decorate

        public PizzaDecorator(Pizza pizza)
        {
            //Store that existing pizza object in the pizza variable
            
            this.pizza = pizza;
        }

        //Providing Implementation for Pizza Interface i.e. MakePizza Method
        //Here, we are just calling the Concrete Component ManufactureCar Method using the existing pizza object
        //We are making this Method Virtual to allow the Child Concrete Decorator classes to override

        public virtual string MakePizza()
        {
            return pizza.MakePizza();
        }
    }
}

 

Step4: Creating Chicken Pizza Decorator (ConcreteDecoratorA )

namespace DecoratorDesignPatternRealTimeExample
{
    //The following Concrete Decorator class will add Chicken to Existing Pizza

    public class ChickenPizzaDecorator : PizzaDecorator
    {
        //Pass the existing pizza while creating the Instance of ChickenPizzaDecorator
        //Also pass the same existing pizza object to the base constructor

        public ChickenPizzaDecorator(Pizza pizza) : base(pizza)
         {
        }


        public override string MakePizza()
        {
        //Overriding the MakePizza method to Chicken
            //First Call the Concrete Components MakePizza Method and then the AddChicken method

            return pizza.MakePizza() + AddChicken();
        }


        private string AddChicken()
        {
            return ", Chicken added";
        }
    }
}

 

Step5: Creating Veg Pizza Decorator (ConcreteDecoratorB)

namespace DecoratorDesignPatternRealTimeExample
{

    //The following Concrete Decorator class will add Vegetables to the Existing Pizza

    public class VegPizzaDecorator : PizzaDecorator
    {
        //Pass the existing pizza object while creating the Instance of VegPizzaDecorator class
        //Also pass the same existing pizza object to the base class constructor

        public VegPizzaDecorator(Pizza pizza) : base(pizza)
        {
        }

        //Overriding the MakePizza method to Vegetables

        public override string MakePizza()
        {
            //First Call the Concrete Components MakePizza Method and then the AddVegetables method

            return pizza.MakePizza() + AddVegetables();
        }

        private string AddVegetables()
        {
            return ", Vegetables added";
        }
    }
}

 
Step6: Client

using System;
namespace DecoratorDesignPatternRealTimeExample
{
    class Program
    {
        static void Main(string[] args)
        {
            //Create an instance of Concrete Component

            PlainPizza plainPizzaObj = new PlainPizza();

            //Calling the MakePizza method will create the pizza without chicken and vegetables

            string plainPizza = plainPizzaObj.MakePizza();

            Console.WriteLine(plainPizza);

            //Adding Chicken to the Plain Pizza
            //Create an instance PizzaDecorator class and
            //pass existing plainPizzaObj as a parameter to the Constructor which we want to decorate

            PizzaDecorator chickenPizzaDecorator = new ChickenPizzaDecorator(plainPizzaObj);

            //Calling the MakePizza Method using the chickenPizzaDecorator object will add Chicken to the existing Plain Pizza

            string chickenPizza = chickenPizzaDecorator.MakePizza();

            Console.WriteLine("\n'" + chickenPizza + "' using ChickenPizzaDecorator");


            //The Process is the same for adding vegetables to the existing pizza

            VegPizzaDecorator vegPizzaDecorator = new VegPizzaDecorator(plainPizzaObj);
            string vegPizza = vegPizzaDecorator.MakePizza();

            Console.WriteLine("\n'" + vegPizza + "' using VegPizzaDecorator");

            Console.Read();
        }
    }
}

```
**=====================================================FlyWeight Design Pattern ==================================**
The Flyweight design pattern is a structural pattern aimed at minimizing memory usage by sharing as much data as possible with similar objects. This pattern is particularly useful when dealing with a large number of objects that share a substantial amount of data, thus allowing for efficient memory usage.

Here's a detailed explanation of the Flyweight pattern in C#, including an example:

Key Concepts
- Flyweight: An interface or abstract class declaring methods that can be used by the flyweights.
- ConcreteFlyweight: Implements the Flyweight interface and adds storage for intrinsic state. Instances of ConcreteFlyweight are shared.
- UnsharedConcreteFlyweight: Not all Flyweight subclasses need to be shared. The Flyweight interface enables sharing but does not enforce it.
- FlyweightFactory: Creates and manages Flyweight objects, ensuring that flyweights are shared properly. When a client requests a flyweight, the factory provides an existing instance or creates one if none exists.
- Client: Maintains references to flyweights and computes extrinsic state.

**Example Scenario**
Let's consider a scenario where we need to create a large number of Tree objects in a forest simulation. Each tree has intrinsic state (shared among trees, such as type, texture, etc.) and extrinsic state (unique to each tree, such as position).


Step 1: Define the Flyweight Interface

```
public interface ITree
{
    void Display(int x, int y);
}

```
Step 2: Implement the ConcreteFlyweight

```
public class TreeType : ITree
{
    private string name;
    private string color;
    private string texture;

    public TreeType(string name, string color, string texture)
    {
        this.name = name;
        this.color = color;
        this.texture = texture;
    }

    public void Display(int x, int y)
    {
        Console.WriteLine($"Displaying tree of type {name} at ({x}, {y}) with color {color} and texture {texture}");
    }
}

```


Step 3: Implement the FlyweightFactory

```
public class TreeFactory
{
    private Dictionary<string, TreeType> treeTypes = new Dictionary<string, TreeType>();

    public TreeType GetTreeType(string name, string color, string texture)
    {
        string key = $"{name}_{color}_{texture}";

        if (!treeTypes.ContainsKey(key))
        {
            treeTypes[key] = new TreeType(name, color, texture);
        }

        return treeTypes[key];
    }
}

```

Step 4: Client Code to Use Flyweights

```
public class Tree
{
    private int x;
    private int y;
    private TreeType type;

    public Tree(int x, int y, TreeType type)
    {
        this.x = x;
        this.y = y;
        this.type = type;
    }

    public void Display()
    {
        type.Display(x, y);
    }
}

public class Forest
{
    private List<Tree> trees = new List<Tree>();
    private TreeFactory treeFactory = new TreeFactory();

    public void PlantTree(int x, int y, string name, string color, string texture)
    {
        TreeType type = treeFactory.GetTreeType(name, color, texture);
        Tree tree = new Tree(x, y, type);
        trees.Add(tree);
    }

    public void DisplayTrees()
    {
        foreach (var tree in trees)
        {
            tree.Display();
        }
    }
}

```

Step 5: Demonstrate Usage

```
class Program
{
    static void Main()
    {
        Forest forest = new Forest();

        forest.PlantTree(1, 2, "Oak", "Green", "Rough");
        forest.PlantTree(2, 3, "Pine", "Green", "Smooth");
        forest.PlantTree(1, 2, "Oak", "Green", "Rough"); // This will reuse the existing "Oak" tree type

        forest.DisplayTrees();
    }
}

```
Explanation
TreeType is the ConcreteFlyweight that stores intrinsic state (name, color, texture).
TreeFactory is responsible for creating and managing the flyweights. It ensures that the same TreeType instance is reused whenever possible.
Tree is a class that uses a TreeType flyweight and adds extrinsic state (x, y coordinates).
Forest is the client that creates and uses Tree objects. It demonstrates how to plant trees and display them.
By using the Flyweight pattern, we minimize memory usage by sharing TreeType objects instead of creating new ones for each tree, thus achieving efficiency when dealing with a large number of similar objects.


**==========================================Composite Design Pattern=================================================**


**=================================================Explain Circuit Breaker Design Pattern in Microservices ? ======================**
**Circuit Breaker Pattern Overview**
The circuit breaker pattern works similarly to an electrical circuit breaker. It monitors for failures in a service call and, when certain thresholds are met, it "trips" the breaker to prevent further calls to the failing service. This allows the failing service time to recover, and avoids overloading it with additional requests.

The circuit breaker has three states:
-- **Closed:** Requests are allowed to pass through. If the requests succeed, the circuit remains closed.
--  **Open:** Requests are blocked and an error is returned immediately without attempting the call. This state is triggered when a threshold of failures is reached.
-- **Half-Open:** A limited number of test requests are allowed through to see if the service has recovered. If these requests succeed, the circuit breaker resets to the closed state. If they fail, the circuit breaker returns to the open state.

**Implementing Circuit Breaker in .NET Core**
In .NET Core, you can implement the circuit breaker pattern using the Polly library. Polly is a .NET resilience and transient fault-handling library that allows you to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback.

**Steps to Implement Circuit Breaker with Polly**
-- Install Polly NuGet Package:
First, install the Polly NuGet package in your .NET Core project:

```
dotnet add package Polly

```
-- Configure Circuit Breaker Policy:
Define a circuit breaker policy in your code. This policy specifies the number of exceptions allowed before opening the circuit, the duration the circuit remains open, and the actions to take on transitions.
```
using Polly;
using Polly.CircuitBreaker;

// Define a circuit breaker policy
var circuitBreakerPolicy = Policy
    .Handle<Exception>()
    .CircuitBreakerAsync(
        exceptionsAllowedBeforeBreaking: 3,
        durationOfBreak: TimeSpan.FromSeconds(30),
        onBreak: (exception, timespan) => {
            // Code to run when the circuit breaks
            Console.WriteLine("Circuit broken!");
        },
        onReset: () => {
            // Code to run when the circuit resets
            Console.WriteLine("Circuit reset!");
        },
        onHalfOpen: () => {
            // Code to run when the circuit is half-open
            Console.WriteLine("Circuit half-open, testing...");
        });

```
-- Use Circuit Breaker Policy in Your HTTP Calls:
Apply the circuit breaker policy to your HTTP calls. You can wrap your HttpClient calls with the circuit breaker policy.
```
using System.Net.Http;
using System.Threading.Tasks;

public class ApiService
{
    private readonly HttpClient _httpClient;
    private readonly AsyncCircuitBreakerPolicy _circuitBreakerPolicy;

    public ApiService(HttpClient httpClient, AsyncCircuitBreakerPolicy circuitBreakerPolicy)
    {
        _httpClient = httpClient;
        _circuitBreakerPolicy = circuitBreakerPolicy;
    }

    public async Task<string> GetDataAsync(string url)
    {
        return await _circuitBreakerPolicy.ExecuteAsync(async () => {
            var response = await _httpClient.GetAsync(url);
            response.EnsureSuccessStatusCode();
            return await response.Content.ReadAsStringAsync();
        });
    }
}

```
-- Register Services in Dependency Injection:
Register your HttpClient and ApiService with dependency injection in the Startup class.

```
using Microsoft.Extensions.DependencyInjection;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHttpClient<ApiService>()
            .AddPolicyHandler(circuitBreakerPolicy);
    }
}

```
**Example Usage**
Here’s how you might use the ApiService in a controller:
```
using Microsoft.AspNetCore.Mvc;
using System.Threading.Tasks;

public class MyController : ControllerBase
{
    private readonly ApiService _apiService;

    public MyController(ApiService apiService)
    {
        _apiService = apiService;
    }

    [HttpGet("data")]
    public async Task<IActionResult> GetData()
    {
        try
        {
            var data = await _apiService.GetDataAsync("https://api.example.com/data");
            return Ok(data);
        }
        catch (Exception ex)
        {
            // Handle exceptions, e.g., log error and return appropriate response
            return StatusCode(500, "Internal server error");
        }
    }
}

```
Benefits
-- Resilience: Prevents cascading failures by stopping calls to services that are down.
-- Recovery: Allows services to recover gracefully without being overwhelmed.
-- Monitoring: Provides hooks to monitor the state and behavior of service calls.
By implementing the circuit breaker pattern with Polly in .NET Core, you can significantly enhance the reliability and resilience of your microservice architecture.



**===============================Proxy Design Pattern============================================**
The Proxy Design Pattern is a structural design pattern that provides a surrogate or placeholder for another object to control access to it. This can be useful for various reasons such as lazy initialization, access control, logging, or remote access.

Concept
In the Proxy pattern, we have a Proxy class that represents the RealSubject class. The Proxy class controls access to the RealSubject class by implementing the same interface.

```
public interface ISubject
{
    void Request();
}

public class RealSubject : ISubject
{
    public void Request()
    {
        Console.WriteLine("RealSubject: Handling request.");
    }
}

public class Proxy : ISubject
{
    private RealSubject _realSubject;

    public void Request()
    {
        if (_realSubject == null)
        {
            _realSubject = new RealSubject();
        }

        Console.WriteLine("Proxy: Logging request.");
        _realSubject.Request();
    }
}

public class Client
{
    public void ClientCode(ISubject subject)
    {
        subject.Request();
    }
}

class Program
{
    static void Main(string[] args)
    {
        Client client = new Client();

        Console.WriteLine("Client: Executing with a real subject:");
        RealSubject realSubject = new RealSubject();
        client.ClientCode(realSubject);

        Console.WriteLine();

        Console.WriteLine("Client: Executing with a proxy:");
        Proxy proxy = new Proxy();
        client.ClientCode(proxy);
    }
}

```

Benefits of Proxy Pattern
- Lazy Initialization: The RealSubject is created only when it is needed.
- Access Control: The Proxy can control access to the RealSubject.
- Logging: The Proxy can log requests before passing them to the RealSubject.
- Remote Proxy: Can be used to represent objects that are in different address spaces.

**Scenario**
Imagine we have a system that sends notifications to users. Sending a notification can be an expensive operation, so we want to control how and when notifications are sent. We can use the Proxy pattern to add this control.

```
public interface INotification
{
    void Send(string message);
}

public class RealNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"RealNotification: Sending message - {message}");
    }
}

public class NotificationProxy : INotification
{
    private RealNotification _realNotification;
    private bool _hasPermission;

    public NotificationProxy(bool hasPermission)
    {
        _hasPermission = hasPermission;
    }

    public void Send(string message)
    {
        if (_realNotification == null)
        {
            _realNotification = new RealNotification();
        }

        if (_hasPermission)
        {
            Console.WriteLine("NotificationProxy: Permission granted.");
            _realNotification.Send(message);
        }
        else
        {
            Console.WriteLine("NotificationProxy: Permission denied.");
        }
    }
}

public class Client
{
    public void SendNotification(INotification notification, string message)
    {
        notification.Send(message);
    }
}

class Program
{
    static void Main(string[] args)
    {
        Client client = new Client();

        Console.WriteLine("Client: Attempting to send notification with permission:");
        NotificationProxy proxyWithPermission = new NotificationProxy(true);
        client.SendNotification(proxyWithPermission, "Hello, user!");

        Console.WriteLine();

        Console.WriteLine("Client: Attempting to send notification without permission:");
        NotificationProxy proxyWithoutPermission = new NotificationProxy(false);
        client.SendNotification(proxyWithoutPermission, "Hello, user!");
    }
}

```




