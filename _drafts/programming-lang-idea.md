---
layout: post
---

( *Note* this is just an idea) 

# The Byte only idea
So a few days ago i was thinking about programing languages that handles everything as if it was a object, like ruby does. I have no idea how to even do that. How can a string or int be an object because at some level there must be raw memory accessing to store the raw byte's representing the string or int. This got me thinking how would a programming language look that could only declare variables as a byte or byte array, and no other data types. (side note i still don't know how ruby actually goes about doing this xD)

# Other Data types
At this point you're probably wondering, "only bytes, uh gross", well have no fear OOP is here. Consider this:
{% highlight csharp linenos %}
var strA = "Hello world";
var intA = 20;
{% endhighlight %}
looking at the code sample we can say that ```strA``` is an string, and that ```intA``` is int type,
this is very convenient for the developer. Now comes the question of how do we make this happen in byte only.
The sultion is quit simple it looks something like this:
{% highlight csharp linenos %}
//we create a wrapper class for our new data type
public class string
{
	//store the raw asci
	private byte[] _buffer;

	/*
		take the raw bytes
	*/
	public string(byte[] data)
	{
		_buffer = data;
	}
}

public class number
{
	//store the raw bytes
	private byte[] _buffer;

	/*
		take the raw bytes
	*/
	public number(byte[] data)
	{
		_buffer = data;
	}
}

void main()
{
	var strA = "Hello world";
	var intA = 20;
	
	/*
	the compiler can then comes and translate that code to this
	*/
	string strA = new string( new byte[] {
		/* the ascii of  Hello world */
		72, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100,
	});
	
	var intA = new number(new byte[] { 
		20 
		/* 20 is less that 255 so only one byte is needed */ 
	});
}

{% endhighlight %}
now lets say we want to combine to strings together like this:
{% highlight csharp linenos %}
var foo = "Hello " + "world";
{% endhighlight %}

We can take the to values and use operator overides to make this happen like this:
{% highlight csharp linenos %}
/* inside the string class */
public void overide operator + add(string toAdd)
{
	//note none of the code will compile its just to prove the point
	_buffer.append(toAdd._buffer);
}

//now the compiler translates
var foo = "Hello " + "world";

//to
var foo = "Hello ".add("world");
{% endhighlight %}
Now you will notice i did not name the int class integer but intsted I named it number, the idea being, why have a int8, 16, 32 , 64 or float. Why not have one generic class called number that incompises all the complexities that is a number. The ```_buffer``` array can expand as the bit size of the number changes, allowing the number to be as big or accurate as you need it to be, as for operators same story as the string

That is the basic idea and two questions remain, how fast will it be and why?

How fast will it be?
it will be considerably slower than using the cpu's building data types native instructions to do manipulations
therefor i can see it being a practical aproch in a interpited enviroment, maybe in a jit situation.

Why?
i dont know, seems a little pointless.
