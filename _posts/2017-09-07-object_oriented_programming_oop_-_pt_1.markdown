---
layout: post
title:  "Object Oriented Programming (OOP) - Pt. 1"
date:   2017-09-07 13:47:28 -0400
---

## **EDITS made for clarity**

OOP is very interesting in the way it operates. It creates objects and exposes methods or interfaces to interact with those objects. These objects are named as real-world nouns who receives methods that describe the behavior or functionality.

**Objects**

* created from Classes.
* are *instances* (created) individuals who have different information from other objects.
* since an object is an instance of its own class, it knows its own attributes (properties). Other words, it knows *itself*

**Class / Classes** tell us...

* what an object should be (its attributes)
* what an object should be capable of doing - behave (functions)

   ```
   class Dog
     #some code to describe a dog
   end
   ```

A class is like an outline. To build this outline, we usually begin by creating (instantiate) a class. **Instantiating** instances of classes can be done literally by creating new instances of a class on itself.

* Why is that? ``` .new ``` is an **initialize method** (also known as a constructor) and it is automatically triggered whenever a new object is created.

   ```
   class Dog
   end
 
   fido = Dog.new
   fido #=> #<Dog:0x007fc52c2d7d20> #every object has its own id differenciating it from other objects
 
   snoopy = Dog.new
   snoopy
	 #=> <Dog:0x007fc52c2d4170> #every object has its own id differenciating it from other objects
   ```

Once a new object is created, we determine whether or not their behavior is identical to other objects. If they are and the objects share the same class, we write **instance methods**.

**Instance methods**.

* methods defined within the object’s class and has access to any instance variable.

* represents an object’s ability to have logic.

* responsible for “setting”/"writing" and “getting”/"reading" methods.

   ```
   class Dog
    def bark
      puts "Woof!"
    end
   end
 
   fido = Dog.new      #initializing a new object/instance
   fido.bark 
	 # => "Woof!"    #calling the instance method of bark onto the object/instanced we called fido
 
   snoopy = Dog.new     #initializing a new object/instance
   snoopy.bark 
	 # => "Woof!"    #calling the instance method of bark onto the object/instanced we called snoopy
   ```

When behaviors are defined, we must add attributes to the objects. To do that we create **Instance variables**.

**Instance variables**

* track different information on different objects who share the same class
* they are scoped at the object (or instance) level, written with the @ symbol in front of it.

   ```
   class Dog
     def name=(dog_name)
       @this_dogs_name = dog_name    #instance variable
     end
 
     def name
       @this_dogs_name    #instance variable
     end
   end
	 
	 lassie = Dog.new      #initializing a new object/instance called lassie
   lassie.name = "Lassie"  #calling the instance method  .name onto the object we called lassie
 
   puts lassie.name
	 
	 #output
	 => Lassie
   ```

Earlier I mentioned that **Instance Methods** are responsible for getter and setter methods. This means that certain methods allows an instance variable to be defined and stored.

   * Setters and Getters are usually paired. When setting an instance we need to add a getter method.
   
   * With getter and setter methods, object's states can be exposed and changed.

   * Setter/Writer methods are defined with an = appended to the getter method followed by the (argument_name). It sets a property or instance variable.
   
   * Getter/Reader method. It returns attributes stored in an instance variable. To make the class’s method attribute writable, a “setter” or writer method needs to be defined.

Going back to the class Dog example:



To reduce the amount of getter and setter methods code and to simplify our class, we can use **Object Accessors.**

**Object Accessors** 

* are macros (shortcuts) that abstracts instance methods especially if the setter and getter perform the same function for every instance method.
   
   * ``` attr_accessor ``` : takes a symbol as an arugment which it uses to create the method name for the getter and setter methods. It replaces two method definitions into one line.
   
   * ``` attr_reader ``` : creates getter method only, not paired with setter method. Only retrieves the instance variable
   
   * ``` attr_writer ``` : creates setter method only, not paired with getter method.
   

Here's the refactored code of the class Dog

   ```
   class Dog
     
	 attr_accessor :name
		 
   end
	 
	 lassie = Dog.new      #initializing a new object/instance called lassie
   lassie.name = "Lassie"  #calling the setter/writer method to equal a value of string "Lassie"
 
   puts lassie.name   #calling the getter/reader method again to see the value of string "Lassie"
	 
	 #output
	 => Lassie
	 ```
  
Below is an example of how the outline for a class with attr_readers & attr_writers can be built : 
  
```
#the macro way
	
class className
 attr_writer :attribute
 attr_reader :attribute
end	
		
#the explicit way
	
class className
	
 def name=(name)
  @name = name
 end
 
 def instance_method_name
  @name
 end
end

#combining the writer and reader methods into one macro - attr_accessor

class className
   attr_accessor :name
end
```

Here's a fun example of ``` attr_reader ```.

*----- Meowing Cat lab*

```
#the goal

maru = Cat.new   #the instance maru was initialized with the class name and the .new method
maru.name = "Maru"    #we want the method .name to give us the name of the object maru

maru.name
# => "Maru"

maru.meow
# => "meow!"
# => nil

#the code

class Cat
  attr_accessor :name   #with this accessor of :name, the setter and getter methods knew upon the object maru's initialization that its name would be called with the object.
  def meow   #this instance method called meow can be called by the object maru so we can hear it say "meow!"
    puts 'meow!'
  end
end
```

*----- end lab*

**Initialization**

Any Ruby class can produce a new instance of itself with the ``` .new ``` method. Remember ``` className.new ```?

Though if we want each instance of the class to be created with certain attributes, we have to define it in the ``` .initialize ``` method.

Whenever a new object/instance is created with ``` .new ``` the argument (the attribute) that was called with it will be passed to the initialize method and invoke that method.

Here's the class Dog example again:

```
class Dog
  def initialize(breed)
    @breed = breed
  end
 
  def breed=(breed)
    @breed = breed
  end
 
  def breed
    @breed
  end
end

lassie = Dog.new("Collie")    #calling the .new on the class Dog to instatiate a new object called lassie. It's attribute of its breed "Collie" will now be passed into .initialize(breed) as the breed argument.
 
lassie.breed #=> "Collie"
```

Here's an example:

*----- Object Initialization lab*

The class Dog has an initialize method that accepts two arugments. 1) for the dog's name that's stored in the @name instance variable. 2) the dog's breed stored in the @breed instance variable. IF the Dog has no breed, it's defaulted to "Mutt."

```
class Dog
  def initialize(name, breed="Mutt")
    @name = name
    @breed = breed
  end
end
```

*----- end lab*

***How do we determine what is a reader, writer, or accessor?***

Now that we discussed initialize and object accessors, how do we determine which methods should be a reader, a writer, or accessor?

Usually methods are getters/readers when they aren’t modifiable. Meaning we don’t want anyone else to touch it. It makes sense to place those reader methods as instance variables in the initialize method. Then we expose that variable under ``` attr_reader ```.

Here's an example from the lab:

*----- OO Basics with Class Constants*

Note: Class Constants are shared throughout all instances.

When we want to ad some customization to a method, we must:

* define that method's setter method
* remove/don't include the attr_accessor for that method because we don't need to create a reader AND writer for that method
* add an attr_reader for that method, instead because Ruby needs to still generate a reader.

In this lab, we defined the genre= method to integrate with the class constant (GENRES = [ ]). Then genre is added as an attr_reader after being removed from the attr_accessor.

```
Class::CONSTANT
class Book
  attr_accessor :author, :page_count     # remove the attr_accessor for genre
  attr_reader :title, :genre     # add an attr_reader for genre

  GENRES = [ ]

  def initialize(title)
    @title = title
  end

  def turn_page
    puts "Flipping the page...wow, you read fast!"
  end

# create the writer for genre and add the logic for the class constant
  def genre=(genre)
    @genre = genre
    GENRES << genre
  end  
end
```

*----- end lab*


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
    roster[grade] ||= [] #part 2b ||= means if roster[grade] has a value, leave it alone and dont't change the value, other wise, set roster[grade] to y.
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

Hm.... So far I'm understanding how classes operate, create new objects (instances), and how to get and set functionality. I have a feeling it's going to get more complicated from here even though it's a really easy concept. -_-

![](http://i.imgur.com/DH1zh2j.gif)


