---
layout: post
title:  "Procedural Ruby Pt. 3"
date:   2017-09-02 18:31:50 +0000
---


**Boolean Enumerators**

1.  ```.all?``` 

 every iteration, every loop of the block MUST be true

2. ```.none?```

 is the opposite of ```.all?``` as every iteration, every loop of the block DOESN'T have to be true

3. ```.any?```

 returns true if AT LEAST one element is true; evaluates truthiness

4.  ```.include?```

 returns true if comparing the actual contents of a known value and if the given object exists in the element. If there's no match, returns false.

**Search Enumerators**

1. ```.select```

Instead of having to create a new variable to push the new array into and calling it like the ```.each``` method, we're using ```.select``` to get the return value which will be the new array that caused the block code to pass.

returns an array with the elements that the block evaluated to true for every iteration

```
[1,2,3,4,5].select do |number|
  number.even?
end 

#output
[2,4]
```

2. ```.detect``` OR ```.find```

same method, 2 different names
returns the FIRST element that makes the block true
returns a single object

3. ```.reject```

returns an array with the elements that the block evaluated to false.

**Sorting data**

sorting strings

letter Z is always > than letter A

operators compare strings by looking at the first letter of each string and compare them to the alphabet.

1. ```.sort```

yields 2 elements to a block
the block is the comparator so it needs to compare 2 elements
returns 0 if the 2 elements are the same
returns -1 if first element is less than the second
returns 1 if first elements is greater than the second; sort will switch the locations of first element and second element

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

**The spaceship operator ( <=> )**

Not sure why, but after seeing those words and the symbol, I though of Marvin the Martian and his Space Modulator

![](http://i.imgur.com/RKSIw4D.jpg)


Anywho, the spaceship operator is also called the combined comparison operator. It replaces the if and elsif logic to compare the two elements.

```
array = [7, 3, 1, 2, 6, 5]
 
array.sort do |a, b|
  a <=> b
end
 
#output
[1, 2, 3, 5, 6, 7]
```

**Sorting Lab**

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

Up next, the final stretches of Procedural Ruby.

![](http://i.imgur.com/I8MDT9I.gif)
