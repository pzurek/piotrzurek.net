---
layout: post
title:  "Pointers in Go. Short tale of asterisk and ampersand."
date:   2013-09-20 21:57:15
tags: [programming, go, golang]
---
A couple of weeks ago I started playing with [Go]. It was my the first ever
time trying a language that close to C in terms of structure and employed concepts.
At first I struggled with... well, almost everything, but soon it started to make more and more sense.
Go's simplicty goes a long way towards making it easy to pick up and run, without having to dig deeper into advanced and elaborate constructs. All this backed up by a very rich and well documented standard library.  
You don't have to understand everything about _go routines_ and _chanels_ to become productive and have fun. You have to however start somewhere and for that some absolute basics are required. For me, **pointers** have been the first of those fundamental building blocks that I wasn't clear how to approach but at the same time knew I couldn't move on without. That's why I devised this, really simple code ilustrating how they work at the lowest possible level. If you decided to read this post maybe it will help you too.

First the basics:

- **&** in front of variable name is used to retrieve the address of where this variable's value is stored. That address is what the pointer is going to store.

- **\*** in front of a type name, means that the declared variable will store an address of another variable of that type (not a value of that type).

- **\*** in front of a variable of pointer type is used to retrieve a value stored at given address. In Go speak this is called dereferencing.

That's a very dry description which wouldn't tell me much without the example below, but before we get there one more thing important to mention. In Go assigning one variable to another one creates a copy of the source value and no reference between them is kept. When the source changes the target variable will not change to follow.

{% highlight go %}
package main
 
import "fmt"
 
func main() {
	var a int   // a variable of type 'int'
	var b int   // another 'int'
	var c *int  // a variable of type 'pointer to int'
 
	a = 42      // assigning value '42' to 'a'
	b = a       // copying the value from 'a' to 'b'
	c = &a      // assigning the address of 'a' to 'c'
 
	fmt.Printf("Values:\n;
	 a = %v\n b = %v\n c = %v\n*c = %v\n", a, b, c, *c)
 
	a = 21      // assigning a new value of '21' to 'a'
 
	fmt.Printf("Values:\n a = %v\n b = %v\n c = %v\n*c = %v\n", a, b, c, *c)
	fmt.Printf("Types:\n a:  %T\n b:  %T\n c: %T\n*c:  %T\n", a, b, c, *c)
}
{% endhighlight %}

When run, the above code will produce to follwing output (without the comments of course):

{% highlight go %}
Values:           // after the initial assignment
 a = 42           // no surprizes here
 b = 42           // exactly what we expect
 c = 0x21015a018  // OK, so that's that an 'address' looks like
*c = 42           // 'unwraped' or 'dereferenced' value of '*c'
Values:           // after the second assignment
 a = 21           // ok, that's to be expected
 b = 42           // ooo... ok, so 'b' is a copy of 'a' stored under different address
 c = 0x21015a018  // hmmm... and this is the same address that we have seen previously
*c = 21           // but the value stored under this addres has changed
Types:            // to ilustrate once more that a pointer is a different type
 a:  int          // than the variable that it points to
 b:  int
 c: *int
*c:  int
{% endhighlight %}

Run this code at [play.golang.org][play].

[go]: http://golang.org
[play]: http://play.golang.org/p/kdwhV55cj8