---
layout: post
title: "Using as and is for safe casting in C#"
categories: [general]
tags: [c#, programming]
fullview: false
---

Doing a cast in C# is telling the compiler to do an explicit conversion to convert the type of an object from one to another, and by explicit it means that you are aware that data may be truncated during the operation. For example : converting a decimal to a float.

Let's take a look at this example
{% highlight C# %}
private void button1_Click(object sender, EventArgs e)
{
	Button button = (Button)sender; // doing an explicit cast here
	button.Text = "Processing...";
}
{% endhighlight %}

The problem with the above code is if the event handler is no longer called by the Button type, it throws an InvalidCastException.

C# provides you two solutions

Using *as*
{% highlight C# %}
private void button1_Click(object sender, EventArgs e)
{
	Button button = sender as Button;

	if (button != null)
	{
		button.Text = "Processing...";
	}
	else
	{
	 	//Do what you need to do here
	}
}
{% endhighlight %}

Using *is*
{% highlight C# %}
private void button1_Click(object sender, EventArgs e)
{
	if (sender is Button)
	{
		Button button = (Button)sender;
		button.Text = "Processing...";
	}
	else
	{
	 	//Do what you need to do here
	}
}
{% endhighlight %}
and voila, no more InvalidCastException.


Now what should you use is the next thing you should consider, *as* or *is* or just fail fast and catch the InvalidCastException. The answer is it depends on you and your application. Take a look at [this StackOverflow answer](http://stackoverflow.com/a/2139818/1487831) for more discussion on what to use.

##### References
* Defensive Coding in C#, from [pluralsight](http://www.pluralsight.com/courses/defensive-coding-csharp)
* How to: Safely Cast by Using as and is Operators (C# Programming Guide), from [msdn](https://msdn.microsoft.com/en-us/library/cc488006.aspx)

