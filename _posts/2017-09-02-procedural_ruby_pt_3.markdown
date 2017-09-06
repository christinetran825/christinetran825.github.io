---
layout: post
title:  "Procedural Ruby Part 3 - more Iterations + Enumerators"
date:   2017-09-02 14:31:51 -0400
---

**Enumerators** are strange.

These 2 link sums up Enumerators and Enumerables very well - (https://blog.carbonfive.com/2012/10/02/enumerator-rubys-versatile-iterator/)
(https://rossta.net/blog/what-is-enumerator.html)

Enumerables add functionality to the main collection classes (Arrays and Hashes) with the ability to sort. The class must provide a ``` .each ``` method that yields each element of the collection.

Enumerators are Enumerables plus external iteration. They are a class which allows both internal and external iteration. Most methods have two forms: a block form where the contents are evaluated for each item in the enumeration, and a non-block form which returns a new Enumerator wrapping the iteration.

Default Enumerator is ``` .each ``` but it can get messy.

There are a few types of Enumerators like Boolean and Search.

**Boolean Enumerators**

* ```.all?``` : every iteration, every loop of the block MUST be true

* ```.none?``` : is the opposite of ```.all?``` as every iteration, every loop of the block DOESN'T have to be true
 
* ```.any?``` : returns true if AT LEAST one element is true; evaluates truthiness
 
* ```.include?``` : returns true if comparing the actual contents of a known value and if the given object exists in the element. If there's no match, returns false.

**Search Enumerators**

These enumerators help refine a collection to only matching elements by filtering.

*  ``` .select ``` : Instead of having to create a new variable to push into the new array and calling it like the ```.each``` method, we're using ```.select``` to return an array with the elements that the block evaluated to true for every iteration.
    
    ```
		[1,2,3,4,5].select do |number|
		   number.even?
		end 
		
		#output
		[2,4]
		```

*  ``` .detect ``` OR ``` .find ``` : It's the same method, just 2 different names. It returns the FIRST element that makes the block true by returning a single object

*  ``` .reject ``` : Returns an array with the elements that the block evaluated to false.


*----- Cartoon Collections Lab*

This lab was really fun. It took some time and mulitple testing, but I liked how I can start seeing how the enumerators methods worked.

```
def roll_call_dwarves(dwarves)# code an argument here
  # Your code here
  new_dwarves = []
  dwarves.each.with_index(1) do |name, index|
    new_dwarves << "#{index}. #{name}"
  end
  puts new_dwarves.join(" ")
end

#output
dwarves = ["Dopey", "Grumpy", "Bashful"]
1. Dopey
2. Grumpy
3. Bashful

def summon_captain_planet(array)# code an argument here
  # Your code here
  array.collect do |names|
    names.capitalize << "!"
  end
end

#output
fruits = ["apple", "banana", "orange"]
summon_captain_planet(fruits)
=> ['Apple!', 'Banana!', 'Orange!']

def long_planeteer_calls(array)# code an argument here
  # Your code here
  array.any? do |words|
    words.size > 4
  end
end

#output
array = ["earth", "wind", "fire", "water", "heart
=> true

def find_the_cheese(array)# code an argument here
  # the array below is here to help
  cheese_types = ["cheddar", "gouda", "camembert"]

  array.find do |cheese|
    cheese_types.include?(cheese)
  end
end

#output
array = ["banana", "cheddar", "sock"]
=> cheddar
```
*----- end lab*

**Sorting data**

As mentioned above, enumerables add functionality to the main collection classes (Arrays and Hashes) with the ability to sort. The class must provide a ``` .each ``` method that yields each element of the collection.

* ``` .sort ```
    
    * yields 2 elements to a block
    * the block is the comparator so it needs to compare 2 elements
    * returns 0 if the 2 elements are the same
    * returns -1 if first element is less than the second
    * returns 1 if first elements is greater than the second; sort will switch the locations of first element and second element
    
		```
		array.sort do |a, b|
		   if a == b
			   0
			elsif a < b
			   -1
			elsif a > b
			   1
			end
		end
		```

*note: When sorting strings, the letter Z is always > than letter A. Operators compare strings by looking at the first letter of each string and compare them to the alphabet. *

**The spaceship operator ( <=> )**

Not sure why, but after seeing those words and the symbol, I though of Marvin the Martian and his Space Modulator

![](http://i.imgur.com/RKSIw4D.jpg)

Anywho, the spaceship operator is also called the combined comparison operator. It replaces the if and elsif logic to compare the two elements of the ``` .sort ``` method.

```
array = [7, 3, 1, 2, 6, 5]
 
array.sort do |a, b|
  a <=> b
end
 
#output
[1, 2, 3, 5, 6, 7]
```

*----- Sorting Lab*

I'm not sure why this method had be stumped. I have to rembmer that without index being explicitly stated, we're iterating the ```|word|``` (the element from the array that's being passed) and checking its character position. From the second index (or the 3rd letter of the word), we can replace it with a "$".

```
#taking an array as an input, change the 3rd character of each element to a dollar sign.

def kesha_maker(array)
  array.each {|word| word[2] = "$" }
end
```

```
#Add an "s" to each word in the array except for the 2nd element in the array

def add_s(array)
  array.collect do |word|
    if array[1] == word
      word
    else
      word + "s"
    end
  end
end
```
*----- end lab*

Up next, the final stretches of Procedural Ruby.

![](http://i.imgur.com/I8MDT9I.gif)
