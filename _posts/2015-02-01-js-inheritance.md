---
layout: post
title: JavaScript Inheritance Pitfall
feature-img: "img/sample_feature_img.png"
---

Very often, while working on some JavaScript app, you will try to build a class hierarchy to encapsulate code in reusable and separate components. As you know, there are no native mechanisms in nowadays JavaScript, for creating classes and supporting inheritance. Usually people use some library that allows then to do so. Even when we use a popular battle-tested framework there are things that can go wrong. In the following post I am going to show you one of them.

I am going to use [Simple JavaScript Inheritance](http://ejohn.org/blog/simple-javascript-inheritance/) library by John Resig(the creator of jQuery) for the following examples. Please, note that the problem described here, is not specific to this library.

Lets look at the following example.

```javascript
var Person = Class.extend({
  skills: ['eat', 'sleep'],
  showSkills: function() {
      console.log(this.name + ': ' + this.skills.toString());   
  }
});

var Ninja = Person.extend({
  init: function(){
    this.skills.push('fight');
    this.name = 'Ninja';
  }
});

var Painter = Person.extend({
  init: function(){
    this.skills.push('paint');
    this.name = 'Painter';
  }
});
```

So, we have one base class ```Person```. Also, there are ```Ninja``` and ```Painter``` that are extending ```Person``` by adding more skills to the ```skills``` array. So the ninja can eat, sleep and fight and the painter can eat, seep and paint.

Let's create one object of each type.

```javascript
var ninja = new Ninja();
var painter = new Painter();
````

And then let's try to call the ```showSkills``` method.

```javascript
ninja.showSkills(); // outputs eat, sleep, fight, paint
painter.showSkills(); // outputs eat, sleep, fight, paint
```

This is really weird. Why does ```ninja.showSkills();``` print ```paint``` as one of the ninja's skills? It has nothing to do with it because this skill is added only in the constructor of the ```Painter``` class. The states of those two objects should be isolated and some changes in the state of the first object should not change the state of the second object. This is opposite to what we see here. Stupid JavaScript.

If you don't believe me go [try it](http://jsfiddle.net/stoitsev/BjX3m/).

The reason for this strange behavior is that in JavaScript everything is passed by reference. Also, you have to remember that JavaScript uses functional scope. When you declare the ```skills``` array in the ```Person``` class definition, an object for that array gets created(because arrays are objects) and a reference for that object is saved in the ```skills``` variable. 

When you create new instance of class ```Painter```, the pointer to the ```skills``` object is copied and we access the same place in memory where the original array is. In the constructor of the object, we add ```paint``` as a skill to the array and because this point to the same original array declared in ```Person```, we actually modify it.

When you create second instance of class ```Ninja``` the same thing happen. In the constructor of the class we access the same array declared in ```Person``` and when we add ```fight``` as an element to it, we actually add it to the original array again. So after the two objects are created, ```paint``` and ```fight``` are added to the same ```skills``` array.

When you try to print the ```skills``` array from some of the instance, you access the object that is declared in the ```Person``` class, which contains both ```paint``` and ```fight```. That's why they are printed as a result of ```showSkills``` call.

If you want to see a solution to the problem, __ask in the comments bellow__ and I'll post it over here. 

This is my first blog post in this new blog so stay tuned for more.
