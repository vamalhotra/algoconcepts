# Memory issues with closures in C#

A closure in C# is implemented via anonymous methods or lambda functions. If we use any variables inside a closure, those variables will be available as long as closure is visible. Under the hood, compiler generates a dynamic class for each closure and stores all the variables needed by closure. This results in memory for referenced variable not freed as long as as closure is available for use in the code. The lesson is to be careful when using memory intensive variables inside the closure. Sometimes, we just need a small bit of information inside the closure from a large variable outside the closure. It is a good idea to just pass the smaller information inside the closure.



Ref. 

1. [What is closure](https://csharpindepth.com/Articles/Closures)
2. Under the Hood of .NET Management by Chris Farrell