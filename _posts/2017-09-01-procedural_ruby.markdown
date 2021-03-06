---
layout: post
title:  "Procedural Ruby Part. 1 - Methods & Iterations"
date:   2017-09-01 01:17:04 -0400
---


Everything seems to make sense through the lessons and reading, but when applying what I learned into my labs have been a BIG challenge.

Now that I've completed the Intro to Ruby course, the beginning part of the Procedural Ruby course was somewhat smooth sailing until I reached the looping, arrays, and iterations lessons. I guess the Intro to Ruby course was really a taste for a smoother learning curve. Loops and Iterations are the most challenging subjects I've come across. Oh, how I despise them.

I'm feeling like that mouse right now.

![](http://i.imgur.com/YRFyVhy.gif)

Here's what I'm understanding about these confounded methods.

**Loops**

* Tells program to do the same thing over a certain amount of time or until a certain condition is met. It's not explicit (literal), it's abstract in the way it removes details.

* If there's no stopping point for the loop to make the we'll be taken into an infinite loop. To stop we must tell the loop to take a ```break``` OR have it ```count``` to a certain stopping point.


**Arrays**

* Stores a list of data. Each data (called an element) has a position (index) in the array that start at 0, because computers count starting from 0.

* An array can be full of strings, numbers, or a combination of both.


*Methods to access elements in Arrays and to manipulate Arrays*

* Adding

   * shovel ```<<```  : adds items to the END of the array
   
   * ```.push``` : add items to the END of the array (similar to  << )
   
   * ```.unshift``` : adds element to the FRONT of an array; when called the argument can be added

* Removing

   * ```.pop``` : removes LAST item from the END of the array; returns the element that was removed
   
   * ```.shift``` : removes FIRST item from the FRONT of the array; returns the element that was removed

* Accessing elements

   * ```.first``` : access first element
   
   * ```.last``` : access last element
  
* Other methods to manipulate arrays - (https://docs.ruby-lang.org/en/2.0.0/Array.html#method-i-delete_at)

   * ```.sort``` : sorts the array in no particular order; returns a new array which is generally stored into another variable

   * ```.reverse``` : reverses the array

   * ```.include?``` : returns a boolean if the element submitted with the include?(element) is listed in the array

   * ```.size``` : returns the number of elements in the array; the return value is the actual number of elements in the array, starting from 1


**Iteration and Abstraction (-_-)**

**Methods** are behaviors that objects are capable of. Most methods are posed as questions and return a relevant value while others are posed as commands that change the object/system.

**Iterators** are methods on a collection of data (arrays and hashes) that take a block code so individual members of that array will have something happen to it. These iterators like ``` .each ``` ``` .collect ``` and ``` .select ``` are **chainable** meaning there's added functionality on top of each method.

**Different iterators have different return values.** ``` .each ``` will always return the original collection on which it was called. To return a different return value, we must explicity tell the method to do so. We can do that with ``` .collect ``` or ``` .map ``` methods. 

This is what I didn't understand about Enumerables/Enumerators. When calling a method on an object it returns another object which we can then call on another method onto that new object.

``` .each ```

```
variable_name.each do |local_variable_name|
   #block body
end
```

OR

```
variable_name.each { |local_variable_name| }
```

**Block**

The block body is a big piece of code set between do/end or { }.

The variable.each will take each element one at a time through ech iteration through a variable that's declared in the block body. That variable is the variable (in this case, ```local_variable_name``` that's set inside the pipes ```| |```

The variable inside ```|  |``` has become a local variable and acts like an argument because it now takes the value of ```variable_name```'s elements from the collection of data.

*----- Example 1 *
```
array = ["apple", "pear", "orange"]

array.each do |item|
  puts item
end

#prints:
#"apple"
#"pear"
#"orange"
```

With the ```.each``` method, it always returns the original collection that was called. We say "called" because ```.each``` is a method and we're invoking/calling the method on the variable. In the above example, the ```.each``` method is invoking/calling on array to " ``` do |item| puts item end ```"

Here's another example of a lab that I was able to solve with not a lot of trouble:

*----- Lab 1*
```
array = [1, 2, 3]
def square_array(array)
  new_array = []
  array.each do |index|
    new_array << (index ** 2) #squaring array's indices (plural for index)
  end
  new_array
end

#returns
[2, 4, 9]

#since .each returns an original array, we need to call on a NEW array that would have the results of array's indices being squared. If not, we'll always get the original array [1, 2, 3]. That's why I declared new_array with an empty array which would later have array's indices being squared shovel into new_array
```
*----- end Lab*


The problem is my not understanding **[Enumerators](http://ruby-doc.org/core-2.4.1/Enumerator.html#M000303).** 

Most methods have 2 forms: a block form and a non-block form which you can chain together.

1. ```.each_index```

```
#.each_index = .each method + .index method. This new method (.each_index) is the same as .each, except now, with the chained .index method, it's passing the index of the element instead of the element itself

array = ["apple", "pear", "orange"]

array.each_index {|index| puts "#{index}: #{array[index]}" }

#prints:
#0: "apple"
#1: "pear"
#2: "orange"
```

2. ```.each.with_index```

```
# Similar to Example 1, we have .each method + .with_index method. This new method (.each.with_index) is the same as .each, except now the method takes an optional parameter and the index can start from a number other than 0.

array = ["apple", "pear", "orange"]

array.each.with_index(2) {|index| puts "#{index}: #{array[index]}" }

#prints:
#2: "apple"
#3: "pear"
#4: "orange"
```

Taking what I learned so far about Enumerators, I still struggled trying to figure out the conference_badge lab. It took a long time, but I think I figured it out, since I passed the test.

*----- Conference badge Lab *
```
attendees = ["Edsger", "Ada", "Charles", "Alan", "Grace", "Linus", "Matz"]

def assign_rooms(attendees)
  new_attendees = []
  attendees.each_with_index do |name, index|
    new_attendees << "Hello, #{name}! You'll be assigned to room #{index+1}!"
  end
  new_attendees
end

{[ "Hello, Edsger! You'll be assigned to room 1!",
   "Hello, Ada! You'll be assigned to room 2!",
   "Hello, Charles! You'll be assigned to room 3!",
   "Hello, Alan! You'll be assigned to room 4!",
   "Hello, Grace! You'll be assigned to room 5!",
   "Hello, Linus! You'll be assigned to room 6!",
   "Hello, Matz! You'll be assigned to room 7!"
]}
```
*----- end Lab*

That's all for now with my battle with Enumerators, enumerables, Loops, and iterations. There would be more on this with future labs I work on following this....

![](http://i.imgur.com/d29Chjj.jpg)
