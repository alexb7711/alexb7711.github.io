---
layout: post
title: Simple Callback Methods In C++
categories: C/C++
---

I have been spending a lot of time as of late creating a small framework for developing games. It is in an extreme infancy still, but it has given me a lot of chances to practice and learn a lot of new techniques. One of the things I've always known about that would be useful are callback methods, but I never really had the time to implement it up until today. The concept is not to difficult to understand (we will go into how it works later), but the actual application is a little more complicated. Not by much, but enough to give me a hard time for a few hours. 

To start, lets cover what a callback method is and how it can be used to your advantage. 

# Callbacks 
A callback is a piece of code that is used to execute (call back) a specified argument. In other words, if you pass a pointer of a function (foo) to another function (bar) as an argument and execute it, then the function pointer (foo) is a callback. As stated on [GeeksForGeeks](https://www.geeksforgeeks.org/).

> In C, a callback function is a function that is called through a function pointer.

That is specific enough for our purpose right now. If you would like read more about Callbacks, here is a link to [Wikipedia](https://en.wikipedia.org/wiki/Callback_(computer_programming)).

# Why Would You Want To Use A Callback?
As will all things, it is situational as to why you would like to use it. It doesn't solve all your problems, but it does make a lot of tasks quite easy. It is useful when you can make generic code that can apply different logic. Now what do I mean by that? For example in "[The C Programming Language](https://www.amazon.com/dp/0131103628?tag=duckduckgo-brave-20&linkCode=osi&th=1&psc=1)" the example they give is a single method that can be used to call different types of sorting methods. They use the same call to sort, but apply different sorting methods on the same bit of data. 

Another more useful (but more complex) reason to use callbacks are in the case of message driven applications. In this case, you have a bunch of methods that have the parameters of the form:

``` C
foo(std::string message, void* data)
{...}
```
stored as a list. Every time you have an event occur, you can send out a message of certain type and pass the data. The callback handler will simply loop through the entire list of callback functions and attempt to pass the data to them. If the method is of the type `message`, then it will accept the data. Now because the data is passed in as a type `void*`, it can then be cast to any type.

# A Simple Example
Below is a simple class that uses a callback to count to 10. To start, you can see that we have a method called `callback(int count)` that all that it knows is to print the integer that it gets if it is smaller than 10. `caller(void (Foo::*fnc)(int))` is the function that is going to be providing the values to `callback`, and `run()` is just the driving code that will be called from `main()`. 

``` C++
#include <cstdio>

class Foo
{
public:
  Foo()
  {}

  void callback(int count)
  {
    if (count > 10) return;
    printf("%d\n", count++);
  }

  void caller(void (Foo::*fnc)(int))
  {
    for (int i = 1; i < 12; ++i)
      (this->*fnc)(i);
  }

  void run()
  {
    caller(&Foo::callback);
  }
};

int main()
{
  Foo foo;
  foo.run();
}
```

Looking specifically at the parameter list of `caller(void (Foo::*fnc)(int))`, `void (Foo::*fnc)(int)` is used to specify that we are going to be passing a function that has a `void` return type, it is a part of the `Foo` class, `*fnc` is just the pointer to the method that is being passed (we don't know what it is yet), and `(int)` which is the argument that `*fnc` can take. It is important to take not of the parenthesis. They *must* look like this in order specify that we are passing a function pointer rather than a `void*`.

At this point, we don't know what method is coming in, but we know what the method is going to look like and from what object that method is a part of. Now lets look at the implementation of `callback`.

``` C
void caller(void (Foo::*fnc)(int))
{
  for (int i = 1; i < 12; ++i)
  (this->*fnc)(i);
}
```

The tricky part here is how we dereference the function. Like how the parenthesis were important in the argument list, the parenthesis are important in how we dereference the function pointer. You must also specify `this->` for the compiler to understand that the function call is from the class type `Foo`. 

As for `run`, that is simple. We just need to pass the pointer to the function which is straightforward. Again, we have to specify the owner of the method by placing `Foo::` in front of the function name.

```C
void run()
{
  caller(&Foo::callback)
}
```

# Shortcomings
Pretty simple once you have done it once, right? It can't be all good though as you probably guessed from the heading. The drawback of this type of declaration is that the method calls must be from inside the same class. Why is that the case though? Couldn't you just do something like:

``` C
void caller(void (Bar::*fnc)(int))
{...}
```

and call a method from `Bar`? The problem with this is that you can know the class definition, but you can't know which instance you are referring to. In the way outlined above, the `this->` pointer indicates which instance you are talking about, so it all works out. But if you have 3 instances of `Bar`, using this method you can't possibly know which instance you are referring to. 

The good news though is that with minor adjustments you can know!

Rather than doing it the way we stated above, try:

``` C
void caller (void (*fnc)(int), void* instance)
{...}
```

Everything will be more or less the same. The only difference is now that you have the instance of the object, you can static cast it to the correct type and then dereference the method. Pretty slick, right? It requires a little more work, but using this method you can potentially create a message handler class!

# My Problem That Was Fixed With Callbacks
As for my case, it is a lot simpler than some of the examples I gave above. My problem is that I wanted to open and parse two different files that will similar formatting but contain two different types of data. More specifically, for my game framework I want to be able to load/save files from a text file. To do this I am currently employing two files: `Maps.txt` and `Dictionary.txt`. `Maps.txt` looks like:

```
[Default]
0 0 1
1 1 0
1 0 0
```

Where `[Default]` is the name of the map, and the matrix underneath specified the type of tiles that will be placed in that specific area. But what do those tiles mean? Thats what `Dictionary.txt` is for. That file looks like this:

```
[Default]
0 0 256 64
1 0 128 64
```

Where the first character is the identifier that will be used in `Map.txt`, the second is the `x` location on the asset sheet, then the `y` position on the asset sheet, and the width/height of the texture (I'm assuming squares). Because they both have similar ways of being parsed, the only part that is going to change is how the data will be stores. Rather than writing two separate methods that will open the file and loop through it the same way, why not create one method that does that, but implements different ways of storing the data.

Similarly to the examples given above I wrote a generic method that will execute a callback:

``` C++
/*
 *--------------------------------------------------------------------------------------
 *       Class:  TileMap
 *      Method:  TileMap :: readFromFileCallback
 * Description:  Callback method used to read from config files. 
 *--------------------------------------------------------------------------------------
 */
void TileMap::readFromFileCallback(void (TileMap::*method)(std::ifstream&, std::string&),
                                   std::string& map_name)
{
  std::ifstream file_in(m_map_text_file);
  std::string line;

  if (file_in.is_open())
  {
    while (std::getline(file_in, line))
    {
      (this->*method)(file_in, map_name);
    }

    file_in.close();
  }
   else
     printf("ERROR: COULD NOT LOAD FILE!\n");

  return;
}

```

The method that is accepted is a `void` method that has a `ifstream` reference and a `string`. I'm also passing a separate string `map_name` that is used in `method` to figure out which file is being parsed!

Super simple, and now I can use this one method to parse different files of the same formatting style!

# References
* [GeeksForGeeks](https://www.geeksforgeeks.org/callbacks-in-c/)
* [The C Programming Language](https://www.amazon.com/dp/0131103628?tag=duckduckgo-brave-20&linkCode=osi&th=1&psc=1)
