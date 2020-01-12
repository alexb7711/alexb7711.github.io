---
layout: post
title: Starting App Development on Linux
categories: Flutter App-Development
---

## Starting App Development
I have never developed a mobile app before, so this search is coming from fresh eyes. I tried a few years back but failed pretty miserably. Essentially, I got to the point where I installed Flutter because I read about it on a Reddit post and it sounded neat... and that was about it. I had no idea how to use it. This was back when I was starting to program and hoped that all programming would come naturally (it didn't).

As expected, I gave up and moved on. Granted, I gave it minimal effort to start, but at the time it was a lot to take in to start and seemed daunting, confusing, and a lot of work that I just did not want to put.

With more experience under my belt, here are some thoughts (and my choice) for mobile app development.

## Background and Development Environment
My background stems from pretty heavy C/C++ programming, Python, MatLab, and most recently C#. I've programmed for about five years. Over the years, I have learned to love developing under Linux for its flexibility and Unix-like environment, which is what I will use to develop. More specifically, Manjaro Linux (specifics on the entire setup later).

## Options as a Linux Developer
Naturally the first thought would be to develop in the native code. So, Kotlin and Swift come to mind. That introduces two new languages, and if you want to develop for both platforms quickly (which I would like to try), those two are out the window.

Going full circle to a few years ago, we get back to Google's Flutter, and Facebook's React Native. They both provide a single code base for cross-platform applications. 

Let's compare the two.

## Flutter
Flutter is Google's cross-platform SDK that was released in May of 2017. It is free and open source, uses dart as its language of choice, and comes jam packed with utilities.

The big things to know about Flutter is:

* React-style framework
* Allows for hot reloading
* Everything is a widget
* All widgets are made of smaller widgets (aggressive nesting)
* Statefull and Stateless widgets (dynamic and static pages)
* Comes with Material Design (Google's app flavor) and Cupertino (iOS app flavor).

### Example Code
The following code is pulled from [Flutter's codelab](https://flutter.dev/docs/codelabs/layout-basics):

``` dart
import 'package:flutter_web/material.dart';
import 'package:flutter_web_test/flutter_web_test.dart';
import 'package:flutter_web_ui/ui.dart' as ui;

class MyWidget extends StatelessWidget {          // Extend Stateless/Statefull Widget
  @override                                       // Always override
  Widget build(BuildContext context) {            // Required to define the class
    return Row(                                   // Widget
      mainAxisAlignment: MainAxisAlignment.start, // Widget properties
      children: [                                 // Children that live in side the property
        BlueBox(),                                // Etc...
        BlueBox(),
        BlueBox(),
      ],
    );
  }
}

class BlueBox extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      decoration: BoxDecoration(
        color: Colors.blue,
        border: Border.all(),
      ),
    );
  }
}
```

The code above is a demonstration of how alignment works in Flutter. As you can see above, I placed some comments stating the general flow of how Flutter works. 

Dart is a C-styled language, but Flutter changes that more to the React style. Coming from a C heavy background, the code seems somewhat messy and unorganized. However, it does seem efficient in terms of how much is typed versus how much actual work as been done toward the app. One thing to point out about Dart: because it is a relatively new programming language, it is not as widely supported like JavaScript is. ~~For example, while writing this post the Markdown preview doesn't recognize Dart and cannot highlight it properly~~ (This has since been fixed). However, it is starting to be more supported such as for vim which has plugins for dart syntax highlighting and flutter support. 

## React Native
React Native, on the other hand, is Facebook's take at the cross-platform SDK. However, Facebook's version was released March 2015. It is written in JavaScript, and seems to have more recently tried to pick it up more on the "jam packing with utilities" front (based off some older posts I've read it seemed more of a skeleton framework to build off of, sort of like SFML - an API for C/C++ typically for game development -, correct me if I'm wrong). From comments and blog posts about React-Native, it seems like a good choice for those who are moving from traditional React.

The big things to know about React Native is:

* React-style framework
* Allows for hot reloading
* Uses JSX within the JavaScript
    * JSXis a syntax for embedding XML within JavaScript

### Example Code
The following code is pulled from [Facebook's Github](https://facebook.github.io/react-native/docs/tutorial):

``` javascript
import React, { Component } from 'react';
import { StyleSheet, Text, View } from 'react-native';

const styles = StyleSheet.create({
  bigBlue: {
    color: 'blue',
    fontWeight: 'bold',
    fontSize: 30,
  },
  red: {
    color: 'red',
  },
});

export default class LotsOfStyles extends Component {                         // App Extends component
  render() {                                                                  // All the stuff you want to draw
    return (
      <View>                                                                  // XML-like styling
        <Text style={styles.red}>just red</Text>
        <Text style={styles.bigBlue}>just bigBlue</Text>
        <Text style={[styles.bigBlue, styles.red]}>bigBlue, then red</Text>
        <Text style={[styles.red, styles.bigBlue]}>red, then bigBlue</Text>
      </View>
    );
  }
}
```

The code above demonstrates how to style text in React Native. Comments were placed in this as well talking about general trends of the SDK.

Having developed with the ROS (Robot Operating Systems) framework (which uses some XML), this SDK does feel a little more familiar and comfortable at first glance. Albeit comfortable and somewhat more intuitive feel, it does also seem more verbose. 

## My Choice
If you have done any previous research before reading this, you shouldn't be too surprised to hear that I've decided to try Flutter. Here is quick list why:

* Developed by Google, so it should be more stable for Android at least... Right?
* I've seen a lot less complaining about Flutter being more stable than React Native
* More full-featured
* Darts more C-styled nature (Can't really escape the React styling though)

Something worth mentioning is that both SDK's provide great resources on their respective websites (such as trying the live code). It is a pretty even playing field on this front. Although, I have heard/read that Flutter's documentation is more easily read and understood. 

## Conclusion
There are a lot routes to take for app development, and none of them are necessarily better than another. For me, I would like to develop an app for both iOS and Android and do not want to spend the time learning and porting the same app across the two languages. 

Flutter and React Native are good candidates, however researching into the two it seems that Flutter is SDK of choice for those new to the game and React Native is good for those who are coming from React. This, plus Flutter's more full-featured package and C-styled nature, makes it a more tempting place to start.

<!-- If you enjoyed this article, be sure to follow up by reading:  -->

<!-- * How to install Flutter -->
<!-- * Starting with Flutter -->
