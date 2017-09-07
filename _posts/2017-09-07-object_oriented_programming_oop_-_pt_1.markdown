---
layout: post
title:  "Object Oriented Programming (OOP) - Pt. 1"
date:   2017-09-07 13:47:28 -0400
---


OOP is very interesting in the way it operates. It creates objects and exposes methods or interfaces to interact with those objects. These objects are named as real-world nouns who receives methods that describe the behavior or functionality.

Objects are created from classes.Objects are individuals and have different information from other objects but they are instances (or created) from the same class. Since an object is an instance of its own class, it knows its own attributes (properties). 

A **class** defines these attributes along with the behaviors (functionalities) like an outline. It tells us what an object should be made of and what it's capable of doing. To build this outline of a class, the process usually begins by creating (instantiate) a class.

**Instantiating** instances of classes can be done literally by creating new instances of a class on itself ``` .new ``` => ``` className.new ```. An **Initialize method** (is a constructor) gets triggered whenever a new object is created with ``` .new ```.  It’s used when we need to pass argument(s) when initializing a new instance of a class. This method takes in an argument and sets an **instance variable (@instance_name)** equal to that very argument.

**Instance variables** track different information on different objects who share the same class. Because of this, instance variables are scoped at the object (or instance) level, written with the @ symbol in front of it.  When setting an instance variable in the initialize method, we need to call it so it can return that instance variable in the Instance method. Then the class can call on the ``` .new ``` method. When ``` .new ``` is called with an argument, it passes that argument to the ``` .initialize ``` method and invokes it.

```
   class className
     def initialize(attribute)
      @attribute = attribute
     end
   end
	 
   object_name = className.new("attribute")
   ```

However, when objects share the same class and identical behaviors (functionalities) they are defined as **instance methods**. These methods are defined within the object’s class and has access to any instance variable. It represent an object’s ability to have logic. They are responsible for “setting” and “getting” methods.

   *Note: Dot notation is a method asking the object to perform something. ``` .methods ``` returns an array of all the methods and messages an object responds to. It answers the question, “What methods do you respond to?”*

   * Getter = reader method. It returns info (property) stored in an instance variable. To make the class’s method attribute writable, a “setter” or writer method needs to be defined.

   * Setter methods are defined with an = appended to the getter method followed by the (argument_name). It sets a property or instance variable.

   * Instance.property = “something”

   * Setters and Getters are usually paired. When setting an instance we need to add a getter method.

To access an object's attribute which is stored in the ``` @attribute ``` intance variable, we have to create a method that will return the attribute. We can call that instance method ``` attribute ``` and it's job is to return the value in the ``` @attribute ``` instance variable.

```
class className
 def initialize(attribute)
  @attribute = attribute
 end
 def attribute #getter method
  @attribute
 end
 def attribute=(new_attribute) #setter method
  @attribute = new_attribute
 end
 def instance_method_name
  @attribute #does something
 end
end
	 
object_name = className.new("attribute")
object_name.attribute
object_name.attribute = "new_attribute"
```

To reduce the amount of getter and setter methods code and to simplify our class, we can use **Object Accessors.**

Object Accessors helps us with the abstraction of instance methods.
   
   * ``` attr_accessor ``` : takes a symbol as an arugment which it uses to create the method name for the getter and setter methods. It replaces two method definitions into one line.
   
   * ``` attr_reader ``` : getter method only, not paired with setter method. Only retrieves the instance variable
   
   * ``` attr_writer ``` : setter method only, not paired with getter method.
   

Here's the refactored code:

```
class className
 attr_accessor :attribute
	 
 def initialize(attribute)
  @attribute = attribute
 end
 
 def instance_method_name
  @attribute #does something
 end
end

object_name = className.new("attribute")
object_name.attribute
object_name.attribute = "new_attribute"
```

*How do we determine what is a reader or accessor?*

Usually the methods are readers when they aren’t modifiable. Meaning we don’t want anyone else to affect it. It makes sense to place those reader methods as instance variables in the initialize method. Then we expose that variable under attr_reader.
Notice the difference between the first group of code for the class Book and the one below when attributes accessors, readers, and writers are mentioned.

With getter and setter methods, object's states can be exposed and changed.

```
def instance_method_name
	@attribute #does something #the instance method referenced the instance variable @attribute
end
		 
def instance_method_name
	attribute #does something #the attribute getter method created within the attr_accessor
end
```

*------ OO School Domain Lab*

This was one of the hardest lab I've done. I'm still not sure if I understand domain models very well.

* Part 1: The class School is initialized with a name and a roster that is an empty hash. The roster's hash key is the grade levels and the value of each key is an array of the students' names.

* Part 2a: students are added to the school by called the ``` add_student ``` method that takes an arugment of the student's name and grade level. 

* Part 2b: Since we're adding the student's name and grade level to the empty @roster hash in part 1 (the key/values are set up as an array), we can't push the arguments into the empty hash. So we have to create a new key and point it to an empty array...

* Part 2c: then push the arguments into that array.
 
* Part 3: The grade method takes an argument of a grade and returns all of the students in that grade.
 
* Part 4: Get a sorted list of all the students where the strings in the student arrays are sorted ABC. We get the returned sorted students within their respective grade in hash key/value pairs

```
class School
  attr_accessor :name, :roster

  def initialize(name) #part 1
    @name = name
    @roster = {}
  end

  def add_student(student_name, grade) #part 2a
    roster[grade] ||= [] #part 2b ||= is a conditional assignment operator (this is part of the memorization topic) the values will now be stored.
    roster[grade] << student_name #part 2c
  end

  def grade(student_grade) #part 3
    roster[student_grade]
  end

  def sort  #part 4
    sorted = {} 
    roster.each do |grade, students| #iterate through each grade and student
      sorted[grade] = students.sort #grade is the key in the sorted hash. Value is the students sorted in ABC order.
    end
    sorted #the new sorted roster.
  end
end
```

*------ end lab*


*----- OO Counting Sentences*

* The last instance method of this lab was tough. I didn't know that we could call on ``` .split ``` with mutliple characters.

* With some help, I found that we can use REGEX.

```
class String

 ...

  def count_sentences
    #binding.pry
    #new_split_sentence = self.split(/\.|\?|\!|\!!|\.../)
    #new_split_sentence.size
    self.split(/\.|\?|\!|\!!|\.../).delete_if { |word| word.size < 2}.size
  end
end
```
*----- end of lab*


Hm.... So far I'm understanding how classes operate, create new objects (instances), and how to get and set functionality. I have a feeling it's going to get more complicated from here even though it's a really easy concept. -_-

![](http://i.imgur.com/DH1zh2j.gif)


