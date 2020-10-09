---
layout: post
title: Delegate In C#
date: 2020-05-24 00:46:41 +0000
---

# Delegate in C#

Delegates in C# are similar to function pointer in C++. A function pointer can hold reference to a function and can be invoked with arguments, if any, just like a function.

This article talks about delegates, and generic delegates namely Action and Func which was introduced with C# 3.0. We will also discuss about Predicate which is a special delegate. Finally, we will talk about events and how delegates are useful to define the signature of event handler in subscriber class.

## Delegate

Delegates in C# inherit from System.MulticastDelegate (which inherits from System.Delegate and, in turn, from System.Object). This class provides a linked list of delegates which are invoked synchronously in order.

### Using named functions:

```c#
    //declare delegate aka function pointer which can 
    //point to a function which returns void and takes no arguments
    public delegate void Function();

    //declare named function
    public void WriteInformation()
    {
        Debug.WriteLine("Got Here");
    }

    public void CallFunction()
    {
        //Assign named function to delegate.
        //Signature of delegate and named function should match
        //thus, providing type safety.
        Function function = new Function(WriteInformation);
        function();
    }
```
Delegates can be combined by calling the Delegate.Combine method, and it must be invoked through the DynamicInvoke() method. The delegates are invoked synchronously in the order in which they appear in Combine(). If an error occurs during execution, an exception is thrown.

```c#
    public void WriteInformation2()
    {
        Debug.WriteLine("Write Information 2");
    }

    public void CallFunction()
    {
        Function function = new Function(WriteInformation);
        Function function2 = new Function(WriteInformation2);
        var del = Delegate.Combine(function, function2);
        del.DynamicInvoke();
        function();
    }
```
### Using Anonymous methods

Instead of created a named function, we can often write an anonymous method inline. If anonymous method uses any variable defined in the function, its reference would be retained until the delegate/function-pointer is valid.

```c#
public delegate void Function(string info);

public void CallFunction()
{
    Function function = delegate (string info) { Debug.WriteLine(info); };
    function("This method is anonymous");
}
```

## Generic Delegates

The above code created custom delegate which takes string parameter. C# 3.0 introduced generic delegates which can take generic parameter. It introduced two generic delegate types `Func` and `Action`

Func is a generic delegate included in the `System` namespace. It has zero or more *input* parameters and one *out* parameter. The last parameter is considered as an out parameter.

An Action type delegate is the same as ***Func*** delegate except that the Action delegate doesn't return a value. In other words, an Action delegate can be used with a method that has a void return type.

### Action

Using Action, you do not need to declare delegate.  Action delegate can have 0 to 16 input parameters.

    /*public delegate void Function(string s); Not needed */
        
    //declare named function
    public void WriteInformation(string s)
    {
        Debug.WriteLine(s);
    }
    
    public void CallFunction()
    {
        /*Function function = new Function(WriteInformation);*/
        //Initializing action by directly assigning method
        Action<string> actionDelegate = WriteInformation;
        actionDelegate('Action delegate');
        
        //Initializing action using new keyword
        Action<string> actionDelegate2 = new Action<string>(WriteInformation);
        actionDelegate2('Same as above')
        
        //Initializing action using anonymous method
        Action<string> actionDelegate3 = delegate(string s){ Debug.WriteLine(s);};
        actionDelegate3('Action using Anonymous method')
        
        //Initializing action using lambda method
        Action<string> actionDelegate4 = s => Debug.WriteLine(s);
        actionDelegate4('Action using lambda method')
    }
### Func

In below code, sum and SomeOperation are equivalent and both can hold a function pointer that takes two integers and return an integer.
In Func, last parameter is the output type and remaining parameters are input.

```c#
//Declaring a delegate
public delegate int SomeOperation(int i, int j);

//Using Func without declaring delegate
Func<int, int, int> sum;
```

Below code defines a func which has zero input parameters.

```c#
Func<int> funcWithZeroInputParams = delegate()
                            {
                                Random rnd = new Random();
                                return rnd.Next(1, 10);
                            };
```

Func with lambda expression

```c#
Func<int> funcWithZeroInputParams = () => new Random().Next(1, 10);
Func<int, int, int> sumTwoNumbers = (x, y) => x + y;
```

## Predicate

A predicate is also a delegate like Func and Action. It represents a method that takes a single input parameter and returns a bool.

It is defined in System namespace as below:

```c#
public delegate bool Predicate<in T>(T obj);
```

```c#
static bool IsLowerCase(string str)
{
    return str.Equals(str.ToLower());
}

Predicate<string> isLower = IsLowerCase;
bool result = isLower("abc");
```

Another way to write above code without using named method

```c#
 Predicate<string> isLower = delegate(string s) { return s.Equals(s.ToLower());};
```

Using lambda expression

```c#
Predicate<string> isLower = s => s.Equals(s.ToLower());
```

## Events

An event is a wrapper around a delegate. It depends on the delegate. Events follows the observer design pattern. There's a publisher class which generates events and there can be one or multiple subscriber classes which subscribe to the event in order to receive notification (in "push" model) as soon as the event is generated. Events can be declared static, virtual, sealed, and abstract. Event handlers are invoked synchronously if there are multiple subscribers.

An event can be declared in two steps:

1. Declare a delegate.
2. Declare a variable of the delegate with `event` keyword.

```
public delegate void Notify(object o);  // delegate

public class EventPublisher
{
    public event Notify ProcessCompleted; // event

    protected virtual void OnNotify(object o)
    {
        ProcessCompleted.Invoke(o); //calls all event handler methods registered with this event
    }
}

public class Subscriber
{
	public void SubscribeToEvent(EventPublisher publisher)
	{
	    publisher.ProcessCompleted += HandleEvent;
	}
	
	//HandleEvent must match the delegate declared by publisher class.
	private void HandleEvent(object o)
	{
	    //Do some work
	}
}
```

Instead of declaring delegate as above by EventPublisher class, we can use two pre-defnied delegates: EventHandler and EventHandler<TEventArgs> 

TEventArgs could be simple or complex type and can be used when we need to send some data back to subscribers. When no data need to be sent, first delegate "EventHandler" should be sufficient.

```c#
class CustomEventArgs : EventArgs
{
    public string CustomData { get; set; }
    public DateTime EventTime { get; set; }
}

public class EventPublisher
{
    public event EventHandler ProcessCompletedWithoutArgs;
    public event EventHandler<CustomEventArgs> ProcessCompletedWithArgs;
    protected virtual void OnNotifyWithoutArgs()
    {
        ProcessCompletedWithoutArgs.Invoke(this, EventArgs.Empty);
    }
    
    protected virtual void OnNotifyWithArgs()
    {
        ProcessCompletedWithArgs.Invoke(this, new ProcessEventArgs(){CustomData="TestData", EventTime = DateTime.Now});
    }
}

public class EventSubscriber
{
    public void SubscribeToEvent(EventPublisher publisher)
	{
	    publisher.ProcessCompletedWithoutArgs += HandleEventWithoutArgs;
   	    publisher.ProcessCompletedWithArgs += HandleEventWithArgs;
	}
	
    public static void HandleEventWithArgs(object sender, CustomEventArgs e)
    {
       
    }
    
    public static void HandleEventWithoutArgs(object sender, EventArgs e)
    {
       
    }
}
```
