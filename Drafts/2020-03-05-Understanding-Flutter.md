---
layout: post
title: Understanding Flutter 
categories: Flutter
---

Flutter is a very simple framework once you get the hang of it. If you are not used to GUI development though, it can feel a little overwhelming due to the sheer amount of methods at your disposal. It is a similar feeling to starting Python or C#, familiar C syntax but now you have the paradox of choice for method calls that do similar things. 

As with all things, the best way to learn how everything works is just by working with the framework. The goal of this post is to give some direction in what worked for me to make this idea click. 

# General Understanding
To premise the little task to understand how Flutter works a little better lets talk about the idea of "everything is a widget".

It is an easy enough thing to understand, but what the hell is a widget? Well a widget is more or less just a class. Wow. 

![Wow](https://media.giphy.com/media/2rqEdFfkMzXmo/giphy.gif) 

Now the bulk of your work comes in just the return statement of a virtual method called `build()` as shown below:

``` dart
class MyApp extends StatelessWidget 
{
  @override
  Widget build(BuildContext context)
  {
    return MaterialApp
    (
      title: 'Welcome to Flutter',
      home: Scaffold
      (
        appBar: AppBar
	(
          title: Text('Welcome to Flutter'),
        ),
        body: Center
	(
          child: Text('Hello World'),
        ),
      ), // Home
    );   // Material App
  }	 // build
}	 // MyApp
```
In this case, you are returning a `MaterialApp()` class that takes optional parameters (indicated by `title:`, `home:` and any other thing in parentheses that the format of a word with a colon at the end of it). The only things these classes are doing are containing other basic classes organized in a way that can be reused easily. Or in other words, you could just copy and paste what is in these classes (for the most part) in the spot that you are declaring the "widget" or object.

For example, the `appBar:` optional parameter can be replaced by your own custom bar if the default `AppBar` that comes with the Flutter framework does not suit your needs. Just about anything can be redone utilizing the more basic object types and grouping them into your own class and adding some functionality to them. 

# Create Your Own Button
Now comes the fun part. Create your own button that can be any color, have any text, and runs a custom function that is passed. I will add the code snippet below, but before looking at it, take a shot at it. 

After you give that a shot, now try using it. What I did was use the default Flutter program that counts the amount of times that but plus button has been pressed and remap it to the new button that I created. If you are stuck, I'll have comments in the code snippet below that will hopefully give you some insight.

---

``` dart
import 'package:flutter/material.dart';

class ColorButton extends StatelessWidget
{
  // Private Member Variables
  MaterialColor _boxColor; 
  String        _boxText;
  Function      _function;

  //============================================================================
  // Constructor
  ColorButton(Function function, {MaterialColor color = Colors.grey, 
              String text = "Default Text"})
	      // function is a required parameter of Function type. If you want
	      // you can think of it as a pointer to a method that you are going
	      // to call when you press the function. Look up lambas as well,
	      // those work well with this. I'll make a post on those later.

	      // color is an optional parameter that defaults to a grey button

	      // text is another optional parameter that is used to display 
	      // the text on the button
  {
    _boxColor = color;
    _boxText  = text;
    _function = function;
  }

  //============================================================================
  // Build method
  @override 
  Widget build(BuildContext context)
  {
    // Grab width of screen
    final double _screenWidth = MediaQuery.of(context).size.width;

    return GestureDetector // Gesture detector detects taps and can do different
    			   // things based on taps or swipes. This case we will
			   // call the method passed when the button is tapped.
    (
      onTap: _function,
      child: Container 	   // It contains
      (
        padding: EdgeInsets.symmetric(horizontal: 30.0),
        constraints: BoxConstraints.expand // Give the box its dimensions
        (
          height: 147.0,
          width: _screenWidth * 0.80,
        ),
        decoration: BoxDecoration // Give the box its color and shape
        (
          color: _boxColor,
          borderRadius: BorderRadius.circular(10.0),
        ),                                                     // Box Decoration
        child: Center // Center the text vertically
        (
          child: Row // Alight the text at the left side of the box
          (
            mainAxisAlignment: MainAxisAlignment.start, 
            children: 
            [
              Text(_boxText, textAlign: TextAlign.center), // Apply button text
            ]
          )                                                               // Row
        ),                                                             // Center
      ),                                                            // Container
    );                                                        // GestureDetector
  }
}
```

That is all that is really to it! Flutter is pretty easy once you start grasping its concepts. This is what make that connection in my brain and it has been pretty smooth sailing every time I want to try to do something new. Hopefully this helps you as well!
