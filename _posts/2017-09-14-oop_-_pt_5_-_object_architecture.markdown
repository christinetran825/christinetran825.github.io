---
layout: post
title:  "OOP - Pt. 5 - Object Architecture"
date:   2017-09-14 18:30:21 +0000
---

Here's what I've learned so far on the last big section of OOP.

**OOP GOALS**

> write code that accommodates future change that doesn't require large amounts of changes.

Most of my blog posts act as a note taking place to showcase how I go about thinking of code/programming early in my coding journey.

**Inheritance** was a fun topic.

It's a heirarchical way of writing our classes between a variety of other classes.

* ``` super ``` class (parent class)

* subclasses has access to all of the methods in its parent (super class)

* ``` super ``` keywords augments a method inside a class. It refers it to inherit any functionality of the same method defined in the parent/super class.

Inheritance using ``` < ``` syntax means that a class is a type of something

``` class className < parent className ```

``` class BMW < Car ```

``` BMW::CAR ``` gives BMW class access to all of Car class without stating that a BMW is a type of car.


**Module**

aren't heirarchical but collects and bundles a group of methods and making them available to any classes.

* ``` include ``` keyword signify to our classes that we can use all of the methods of the included module as ``` instance ``` methods.

* ``` extend ``` keyword signify to our classes that we can use all of the methods of the included module as ``` class ``` methods.
 
* ``` metadata ``` shareable class method
 
* ``` :: ``` = references the parent and child relationship of the nested modules as a name-space. It carries all the public methods/items over to the inheriting class or module.

*If you have a module whose methods you would like to be used in another class as instance methods, then you must include the module.*

*If you want a module's methods to be used in another class as class methods, you must extend the module.*

**Keyword Arguments**

passes arguments into a method
like hashes, argument name is the key, its value is value.

```
def happy_birthday(name:, current_age)
  puts "Happy Bday, #{name}."
	current_age += 1
	puts "You're now #{current_age} years old!. You're growing up so fast!"
end

output
 "Happy Bday, Sarah."
 "You're now 3 years old!. You're growing up so fast!"
```

**Mass Assignment** and **Metaprogramming**

Mass Assignment = instantiates a new instance

Metaprogramming = writing code that writes code for us.

Goal: to abstract away the parent class' dependency on specific attributes

```
class User
  attr_accessor :name, :user_name, :age, :location, :bio
 
  def initialize(attributes)
    attributes.each {|key, value| self.send(("#{key}="), value)}
  end
end
```

```attributes.each {|key, value| self.send(("#{key}="), value)}``` => iterating the key/value pairs of the attributes hash. Keys become the name of a setter method. The value associated with the key is th ename of the value needed to pass to that method.

``` .send ``` method abstracts away the specific method call.

``` self.send(key=, value) ``` send method calls the method name that's the key's name with an arugment of the value.

**ERRORs**

```
begin 
  raise YourCustomError
rescue YourCustomError
end
```

**GEMs**

retrieving data from external sources

**Scraping**

The act of parsing a web page's HTML and pulling data from the HTML

NOKOGIRI
* parses HTML and collects data from it
* large HTML strings = nested nodes(called NodeSet)
* series of methods extracting info from the nested nodes
* uses CSS selectors (id and class attributes) to get specific pieces of info from the HTML doc

    * Nested nodes shows the hierarchal elements of a parent and children element
    * it can be iterated over a collection of elements of the HTML doc using [ ] and dot notation

OPEN-URI
* making HTTP requests
* gives usueful methods to make different types of requests


Now I'm off to complete the final projects before I start on the CLI GEM PROJECT.

Wish me luck!

![](http://www.news-people.fr/images/boutique/Tricot-Super-Mario-77663.jpg)


