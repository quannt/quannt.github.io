---
layout: post
title: "Replacing text in Visual Studio 2015 using Regular Expression"
categories: [programming]
fullview: false
---
Skip to *How* if you just want to know the instruction. 

### Why
---
So recently I had to maintain a legacy web application written in VB.NET using .NET 1.1 (yay!). I upgraded it to be compatible with .NET 4.6, which was fine at first but then I hit a problem with this particular line of code

{% highlight VB.NET %}
If Session("UserId") <> "" Then
' Do something here
End If
{% endhighlight %}

It turns out that if `Session("UserId")` contains a numeric data types, VB.NET (well being VB.NET) would naively convert the value on the right hand side to be the same type with the left hand side and do the comparison. This would of course not work because you cannot really convert an empty string to a numeric value. *(I don't even want to start pointing how wrong this is. The fact that the original developer thought it was okay to compare a numeric value with a string or the fact that he didn't give a shit about safely type casting or maybe the fact that he used VB.NET?*).  So I decided to quickly write a helper method 

{% highlight VB.NET %}
﻿Imports System.Web.SessionState

Module StringHelper
    Public Function SafeSessionToString(ByVal session As Object) As String
       If (session Is Nothing) Then
           Return ""
        End If
        Return session.ToString
    End Function
End Module
{% endhighlight %}
The point of this new method is to make sure VB.NET always compare a string with a string, no more & no less.
The new code would look like this
{% highlight VB.NET %}
If SafeSessionToString(Session("UserId")) <> "" Then
' Do something here
End If
{% endhighlight %}
This worked greats except that there were around 400 instances of this type of code in the code base. Let's be optimistic and assume that I can replace each instance in 30 seconds, that would take me more than 3 hours. There was no way I was gonna spend 3 hours copy pasting this. 

### How
---
 What I wanted was to find all instances of `Session("blahblahblah") <>` and replace them with `SafeSessionToString(Session("blahblahblah")) <>`
 
 Press `Ctrl + F` to open the search and replace diaglog, Press `Alt+E` to turn on searching by Regular Expressions. 
 
* In the Find field, I typed in `(Session\(\"[a-zA-Z]+\"\) <>)` to match all the usages (I assume you have some knowledge on RegEx and need no explanation on this). The only thing to note here is to wrap your Regular Expression in a `()`.
* In the Replace field, type in `SafeSessionToString($1)` and press `Alt + A` to replace all. Think of `$1` as a variable that stores the value of the text that matches your Regular Expression.

### End
---
Thanks for reading. I hope this post helps someone on the Internet. If you have question, feel free to leave a comment below.
 
