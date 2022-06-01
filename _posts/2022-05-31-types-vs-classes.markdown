---
layout: post
title:  "Types vs Classes"
date:   2022-05-31 14:32:06 -0500
categories: object-oriented-design
---
## Types
A _type_ in terms of object oriented programming can be defined as a set of requests an object can respond to. More technically speaking, it is just a set of method signatures. In most modern object-oriented languages like Java and C#, a type can be defined through an _interface_:  
Java and C#:  
{% highlight java %}
interface Adder {
    int add(int num1, int num2);
}
{% endhighlight %}

The same can be achieved in C++ (C++11), despite the lack of support for the interface keyword:  
{% highlight cpp %}
public class Adder {
    public:
        virtual int add(int num1, int num) = 0;
}
{% endhighlight %}

In the example above, `Adder` is a _type_. In order for any object to be an `Adder`, it must be able to respond to a request for `add`. In other words, the object must have a concrete implmentation for a method that takes 2 `int`s as arguments, and returns an `int` at the end of its execution. That method must be named `add`.

All other objects that interact with an `Adder` are agnostic to everything except the interface of said `Adder`:  
{% highlight java %}
import java.util.Scanner;

public class Calculator {
	private Adder myAdder;
	private Scanner console;

	public Calculator(Adder adder) {
		this.myAdder = adder;
		this.console = new Scanner();
	}

	public void displayAdd() {
		int num1 = getNumFromUser();
		int num2 = getNumFromUser();
		int res = myAdder.add(num1, num2);
		System.out.printf("The result is: %s", res);
	}

	private static int getNumFromUser() {
		System.out.println("Enter a number: ");
		int res = console.readInt(System.in);
		return res;
	}
}
{% endhighlight %}

In the above, the `Calculator` class is _composed_ of a `Scanner` to get user input, and an `Adder` that will handle all `add` requests an instance of `Calculator` receives. The `Calculator` is completely clueless as to how the `int` that is returned from `myAdder.add(...)` comes to be. It is **only** aware of the return type (`int`) of add.  

{% highlight java %}
public class FastAdder implements Adder {
	public int add(int num1, int num2) {
		return num1 + num2;
	}
}

public class SlowAdder implements Adder {
	public int add(int num1, int num2) {
		for (int i = 0; i < num2; i++) {
			num1++;
		}
		return num1;
	}
}
{% endhighlight %}

The `Calculator`'s myAdder could be an instance of either `SlowAdder` or `FastAdder` and still be valid.

## Classes
Classes have been used up to this point in the article, but have yet to be properly defined. A _class_ consists of all member variables and method implmentations that an object is made up of. Therefore by the nature of classes, all classes that do not implement predefined interfaces define their own types. The `Calculator` class in the above example did not implement any interfaces. It's type is implied in its declaration: an object that can respond to `displayAdd` requests.

Classes can be of multiple types:  
{% highlight java %}
// introducing a new interface
interface Type2 {
	void myMethod();
}

class MyClass implements Adder, Type2 {
	//... implementations for both add and myMethod;
}
{% endhighlight %}

The class `MyClass` is both an `Adder` and a `Type2` if it implements all the abstract methods contained in both interfaces.

Classes also differ from types by virtue of _instantiation_. Objects can be instantiated from classes; the same is not true of interfaces.
