# C#

## Delegate

* the delegate is a type that defines a method signature and it is useful to hold the reference of one or more methods which are having the same signatures
* By using delegates, you can invoke the methods and send methods as an argument to other methods
* the delegate is a reference type and it's type-safe and secure, the delegate are similar to `function pointers` in c++

```csharp
<access_modifier> delegate <return_type> <delegate_name>(<parameters>)
    
public delegate void UserDetails(string name);
```

### Single Cast Delegates

* a delegate which points to a single method, used to hold the reference of a single method 

```csharp
using System;
namespace Tutlane
{
    // Declare Delegate
    public delegate void SampleDelegate(int a, int b);
    class MathOperations
    {
        public void Add(int a, int b)
        {
            Console.WriteLine("Add Result: {0}", a + b);
        }
        public void Subtract(int x, int y)
        {
            Console.WriteLine("Subtract Result: {0}", x - y);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("****Delegate Example****");
            MathOperations m = new MathOperations();
            // Instantiate delegate with add method
            SampleDelegate dlgt = m.Add;
            dlgt(10, 90);
            // Instantiate delegate with subtract method
            dlgt = m.Subtract;
            dlgt(10, 90);
            Console.ReadLine();
        }
    }
}
```



### Multicast Delegates

* a delegate that points to multiple methods is called a multicast delegate, used to hold the reference of multiple methods with a single delegate.
* use `+` or `-` to add or remove method reference, it works with only the methods that are having `void` as return type. 

```csharp
using System;
namespace Tutlane
{
    // Declare Delegate
    public delegate void SampleDelegate(int a, int b);
    class MathOperations
    {
        public void Add(int a, int b)
        {
            Console.WriteLine("Add Result: {0}", a + b);
        }

        public void Subtract(int x, int y)
        {
            Console.WriteLine("Subtract Result: {0}", x - y);
        }

        public void Multiply(int x, int y)
        {
            Console.WriteLine("Multiply Result: {0}", x * y);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("****Delegate Example****");
            MathOperations m = new MathOperations();
            // Instantiate delegate with add method
            SampleDelegate dlgt = m.Add;
            dlgt += m.Subtract;
            dlgt += m.Multiply;
            dlgt(10, 90);
            Console.ReadLine();
        }
    }
}
```

### Pass Method as Parameter using Delegate

```csharp
using System;
namespace Tutlane
{
    // Declare Delegate
    public delegate void SampleDelegate(int a, int b);
    class MathOperations
    {
        public void Add(int a, int b)
        {
            Console.WriteLine("Add Result: {0}", a + b);
        }
        public void Subtract(int x, int y)
        {
            Console.WriteLine("Subtract Result: {0}", x - y);
        }
        public void Multiply(int x, int y)
        {
            Console.WriteLine("Multiply Result: {0}", x * y);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("****Delegate Example****");
            MathOperations m = new MathOperations();
            SampleMethod(m.Add, 10, 90);
            SampleMethod(m.Subtract, 10, 90);
            SampleMethod(m.Multiply, 10, 90);
            Console.ReadLine();
        }
        static void SampleMethod(SampleDelegate dlgt, int a, int b)
        {
            dlgt(a, b);
        }
    }
}
```

### Event

* the event is a message which is sent by an object to indicate that particular `action` is going to happen. the `action` could be caused either by a button click, mouse movement or by some other programming logic. The object that raises an event is called an `event sender`.
* Events will contain a `publisher, subscriber, notification and handler`.The events will enable a class or object tp notify other classes or object
* the **publisher** will determine when an event is raised and the subscribers will determine what action can be taken in response to the event. An event in  C# will have multiple subscriber and the events that have no subscribers will never be raised the subscriber can handle multiple events from multiple publishers
* events are the encapsulated delegates, so we need to define a delegate before we declare an event inside of a class by using event keyword.

```csharp
// Declare the delegate
public delegate void SampleDelegate();
//Declare an event
public event SampleDelegate SampleEvent;
```

```csharp
using System;
namespace Tutlane
{
    class Maths //publisher
    {
        // Declare the delegate
        public delegate void SampleDelegate();
        //Declare an event
        public event SampleDelegate SampleEvent;
        public void Add(int a, int b)
        {
            // Calling event delegate to check subscription
            if (SampleEvent != null)
            {
                // Raise the event by using () operator
                SampleEvent();
                Console.WriteLine("Add Result: {0}", a + b);
            }
            else
            {
                Console.WriteLine("Not Subscribed to Event");
            }
        }
        public void Subtract(int x, int y)
        {
            // Calling event delegate to check subscription
            if (SampleEvent != null)
            {
                // Raise the event by using () operator
                SampleEvent();
                Console.WriteLine("Subtract Result: {0}", x - y);
            }
            else
            {
                Console.WriteLine("Not Subscribed to Event");
            }
        }
    }
    class Operations //subscriber
    {
        Maths m;
        public int a { get; set; }
        public int b { get; set; }
        public Operations(int x, int y)
        {
            m = new Maths();
            // Subscribe to SampleEvent event
            m.SampleEvent += SampleEventHandler;
            a = x;
            b = y;
        }
        // SampleEvent Handler
        public void SampleEventHandler()
        {
            Console.WriteLine("SampleEvent Handler: Calling Method");
        }
        public void AddOperation()
        {
            m.Add(a, b);
        }
        public void SubOperation()
        {
            m.Subtract(a, b);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("****Events Example****");
            Operations op = new Operations(10, 20);
            op.AddOperation();
            op.SubOperation();
            Console.ReadLine();
        }
    }
}
```

### Action

```csharp
public delegate void Action();
```

Inheritance Object->Delegate -> Action

* Encapsulates a method that has no parameters and does not return a value
* The encapsulated method must correspond to the method signature that is defined by this delegate, meaning that the encapsulated method must have no parameters and no return value

```csharp
using System;
using System.Windows.Forms;

public class Name
{
   private string instanceName;
    
   public Name(string name)
   {
      this.instanceName = name;
   }
    
   public void DisplayToConsole()
   {
      Console.WriteLine(this.instanceName);
   }

   public void DisplayToWindow()
   {
      MessageBox.Show(this.instanceName);
   }
}

public class Anonymous
{
   public static void Main()
   {
      Name testName = new Name("Koani");
      Action showMethod = delegate() { testName.DisplayToWindow();} ; //anonymous Method
      showMethod();
   }
}
```



### UnityAction

* Zero argument delegate used by `UnityEvents`
* Unity Actions have no arguments, functions that they call must also have no arguments

```csharp
//Attach this script to a GameObject. Attach a Renderer and Button component to the same GameObject for this example.
//This script will change the Color of the GameObject as well as output messages to the Console saying which function was run by the UnityAction.

using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;

public class UnityActionExample : MonoBehaviour
{
    //This is the Button you attach to the GameObject in the Inspector
    Button m_AddButton;
    Renderer m_Renderer;

    private UnityAction m_MyFirstAction;
    //This is the number that the script updates
    float m_MyNumber;

    void Start()
    {
        //Fetch the Button and Renderer components from the GameObject
        m_AddButton = GetComponent<Button>();
        m_Renderer = GetComponent<Renderer>();

        //Make a Unity Action that calls your function
        m_MyFirstAction += MyFunction;
        //Make the Unity Action also call your second function
        m_MyFirstAction += MySecondFunction;
        //Register the Button to detect clicks and call your Unity Action
        m_AddButton.onClick.AddListener(m_MyFirstAction);
    }

    void MyFunction()
    {
        //Add to the number
        m_MyNumber++;
        //Display the number so far with the message
        Debug.Log("First Added : " + m_MyNumber);
    }

    void MySecondFunction()
    {
        //Change the Color of the GameObject
        m_Renderer.material.color = Color.blue;
        //Ouput the message that the second function was played
        Debug.Log("Second Added");
    }
}
```

### Extension Methods

* Add functionality to a class without access to the class

```csharp
using UnityEngine;
using System.Collections;

//It is common to create a class to contain all of your
//extension methods. This class must be STATIC.
public static class ExtensionMethods
{
    //Even though they are used like normal methods, extension
    //methods must be declared STATIC. Notice that the first
    //parameter has the 'this' keyword followed by a Transform
    //variable. This variable denotes which class the extension
    //method becomes a part of.
    public static void ResetTransformation(this Transform trans)
    {
        trans.position = Vector3.zero;
        trans.localRotation = Quaternion.identity;
        trans.localScale = new Vector3(1, 1, 1);
    }
}
```

* Usage

```csharp
using UnityEngine;
using System.Collections;

public class SomeClass : MonoBehaviour 
{
    void Start () {
        //Notice how you pass no parameter into this
        //extension method even though you had one in the
        //method declaration. The transform object that
        //this method is called from automatically gets
        //passed in as the first parameter.
        transform.ResetTransformation();
    }
}
```



