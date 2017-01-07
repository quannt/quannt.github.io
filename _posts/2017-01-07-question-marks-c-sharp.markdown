---
layout: post
title: "Four question marks you should know in C#"
categories: [programming, c#]
fullview: false
---

There are 4 question marks `?` used in C# syntax which I think is pretty confusing sometimes. Here I will attempt to explain them to you.

### 1)  The ?: Operator (since Visual Studio 2003)
The format : `condition ? first_expression : second_expression;`

If `condition` evaluates to `true`, `first_expression` shall be returned, otherwise `second_expression`.

Example

{% highlight c# %}
Random rnd = new Random();
var randomNo = rnd.Next();

var status = randomNo % 2 == 0 ? "Generated number is an even number." : "Generated number is an odd number.";
Console.WriteLine(randomNo);
Console.WriteLine(status);

{% endhighlight %}

### 2)  The Nullable Types (since Visual Studio 2005)

Definition on msdn

> Nullable types can represent all the values of an underlying type, and an additional null value.


How do you use a nullable, let say nullable integer. Simple

{% highlight c# %}
int? x = 0; // x can hold any integer values plus the null value
x = null;
var isXNull = (x == null) ? true: false;
Console.WriteLine(isXNull); // True

x = 15;
isXNull = (x == null) ? true: false;
Console.WriteLine(isXNull); // False
{% endhighlight %}
 
### 3) The Null-conditional Operators (since Visual Studio 2015)

Example of a pretty common mistake

{% highlight c# %}
public static void Main() {
 var student1 = CreateStudent("Quan", 27);
 Console.WriteLine(student1.Name); // Quan
 var student2 = CreateStudent("", 27); // student2 is null because no student name is given to the fiction method CreateStudent
 Console.WriteLine(student2.Name); // System.NullReferenceException: Object reference not set to an instance of an object.
}
{% endhighlight %}

The method `CreateStudent` returns a `null` value instead of creating a new student if the student name is not given. Trying to access its `Name` property after that will throw a `NullReferenceException`

What you can do (without the Null-conditional Operators)

{% highlight c# %}
var student2 = CreateStudent("", 27); // student2 is null
if (student2 != null){
 Console.WriteLine(student2.Name); // No more System.NullReferenceException
}
{% endhighlight %}

What you can do (with the Null-conditional Operators)
{% highlight c# %}
var student2 = CreateStudent("", 27); // student2 is null
Console.WriteLine(student2?.Name); // No more System.NullReferenceException
{% endhighlight %}

What happended? Thanks to the question mark after `student2`, you are telling C# that `student2` may be null. If `student2` is indeed `null`, C# will not try to access `Name` anymore, it returns `null` instead and `Console.WriteLine` will happily ignore the `null` value and only add a new line.

### 4) The ?? : Null-coalescing operator Operator (since Visual Studio 2005)
This is a special case of the ?: Operator in section 1

The format : `asking_value ?? just_in_case;`

if `asking_value` is `null`, `just_in_case` is returned, otherwise `asking_value` is returned.

Example 
{% highlight c# %}
int? x = null; // x is nullable so it can be null
var y = x ?? 0; // if x is null, set y to 0, otherwise set y = x;
// which is essentially the same as
var y = (x == null) ? 0 : x;

{% endhighlight %}