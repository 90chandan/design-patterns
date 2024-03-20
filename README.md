# design-patterns
Creational Design Pattern -Creational design patterns solve the problems related to object creation. They help to abstract away object creation processes that spread across multiple classes.

a.Singleton
b.Factory Method
c.Abstract Factory
d.Builder
e.Prototype


2.Structural Design Patterns : The structural design patterns suggest implementing relationships between classes and objects.

a.Adapter
b.Bridgec
c.Composite
d.Decorator
f.Facade
g.Flyweight
h.Proxy

 

===================singleTon Design Patter =============================

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
} 

======================C# Factory Method Design Pattern======================

The Factory Method design pattern defines an interface for creating an object, but let subclasses decide which class to instantiate. This pattern lets a class defer instantiation to subclasses.


Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.


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


===============Abstract Factory===================

The Abstract Factory design pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.

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

===================================Builder design pattern===========================================

The Builder design pattern separates the construction of a complex object from its representation so that the same construction process can create different representations.

//product

public class Notification
{
    public string Title{get;set;}
    public string Body {get;set;}
    public string Recipient {get;set;}
    public string Channel {get;set;}

    public void PrintDetails()
    {
        Console.WriteLine("Title :"+Title+" Body :"+Body+"Recipient:"+Recipient);
    }
}

//builder

public interface INotificationBuilder
{
    void SetTitle(string title);
    void SetBody(string body);
    void SetRecipient(string recipient);
    void SetChannel(string channel);
    Notification Build();
}

//concreate builder

public class EmaiNotificationBuilder : INotificationBuilder
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

    public void SetRecipient(string recipient)
    {
        _notification.Channel = "Email";
    }

    public Notification Build()
    {
        return _notification;
    }
}

//director

public class NotificationDirector
{
    private INotificationBuilder _notificationBuilder;

    public NotificationDirector(INotificationBuilder notificationBuilder)
    {
        _notificationBuilder = notificationBuilder;
    }

    public void ConstructNotification(sting title, string body, string recipient, string channel)
    {
        _builder.SetTitle(title);
        _builder.SetBody(body);
        _builder.SetRecipient(recipient);
        _builder.SetChannel(channel);
    }
}

public static void Main(){
     //use the builder to create an email notification

     var emailBuilder = new EmailNotificationBuilder();
     var director = new NotificationDirector(emailBuilder);

     director.ConstructNotification("welcome","Greetings of the day!",user@user.com,"email");
     var emailNotification = emaiBuilder.Build();
     Console.WriteLine(emailNotification);
}

==================================Adapter Design Pattern============

The Adapter design pattern converts the interface of a class into another interface clients expect. This design pattern lets classes work together that couldn‘t otherwise because of incompatible interfaces.

The Adapter Design pattern is a structural pattern that allows object with incompatible interfaces to work together.It acts as bridge between the old and new interfaces, enabling them to collaborate seamlessly.

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
