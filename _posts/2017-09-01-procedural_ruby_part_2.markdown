---
layout: post
title:  "Procedural Ruby Part. 2"
date:   2017-09-01 19:23:32 +0000
---


This week I learned about converting data types.

* strings to array
* array to string
* and more...

There are a variety of methods to help us convert data types.

### Converting data types to Array

```
.split
```

converts a string into an array

```
.to_a
```

converts a range to an array


### Converting Array to string

```
.join
```

* .join has an optional argument in which you can insert between each array element when they're a string
* without the argument it connects the array as one whole string

### Oxford Comma Lab

BOY! was this lab hard! However, I do like that this lab touched on the Oxford Comma. I'm all for the Oxford. I don't see how people still get away with not writing in the [Oxford comma rule](https://www.grammarly.com/blog/what-is-the-oxford-comma-and-why-do-people-care-so-much-about-it/).

![](http://i.imgur.com/b0R6GJ1.gif)

I was able to pass two tests but the others were giving me so much trouble. At first I was writing multiple elsif statements but I thought to myself that that was just too much and messy. We're suppose to iterate over array elements, too, which I don't think I did here. Instead, I played around with a variety of array methods to pass the tests for 3+ elements in an array. I may have to redo this lab to include an iteration.

```
def oxford_comma(array)
  if array.size == 2
    array.join(" and ")
  elsif 2 < array.size
    last_word = array.pop     #for arrays who have more than 2 elements, we will remove the last element in the array 
		new_list = array.join(", ")     #then we take the array (minus the original last element) and join the elements into string 
    new_list << ", and #{last_word}"    #once the new list becomes a string of the array, we can add back the original last word through string interpolation along with the ", and " string to make the brand new string in Oxford Comma format.
  else
    array.join
  end
end

#
```

### Deli Counter

Have I mentioned I don't like iterators and loops? Now, I would like to add Enumerators to the list. Arrgh!

There's lots of things to dig and poke around within any language. For this lab and the others I've mentioned recently for procedural ruby were about adding and removing elements from an array including iterating its index numbers. Having to remember or at least understand that the way strings can be added or removed is the same as adding and removing elements from an array.

Below is the #line from Deli Counter lab. Since ```.each``` returns the original array and the tests wants us to return a new array with the person's placement in line (starting at 1 because people count starting with 1), we had to declare another variable to supply this new iteration. In doing so, I've named the new variable ```current_line``` with part of the string. Then I iterated the ```katz_deli``` variable using ```.each``` **AND** its chained *enumerator* ```.with_index``` that included the index I wanted to start with which is ``` (1) ```. Within the ``` |  | ``` I stated the new local variable called ```person``` which is the person's name in line, and ``` index ``` for the position in line. Given my new local variables I string interpolated them into my ```current_line``` variable to complete the expected sentence for the test. After the iteration, we have to output the ```current_line```.

This all seemed so simple, but my not understanding enumerators made it very difficult.

```
def line(katz_deli)
  if katz_deli.length == 0
    puts "The line is currently empty."
  else
    current_line = "The line is currently:"
    katz_deli.each.with_index(1) do |person, index|
      current_line << " #{index}. #{person}"
    end
    puts current_line
  end
end
```

### Collecting and Returning Values

The ```.each``` method always returns the original array.

To have a different return value, we have to explicity tell it to do it. Just like the above Deli Line example. We had to create a ```current_line``` variable to store the new array. In order to called that new array we have to call it outside of the ```.each``` statement.

Given that understanding, there's another method called ```.map``` (also known as ```.collect```) that does that very thing. I came across this during my google hunts of ruby, I had an idea what the examples were saying through StackFlow, but now it makes even more sense. I guess to understand what's going on with ```.map``` we have to know about ```.each``` and how to use it.


#### Reverse Each Word Lab

Wow! I actually was able to pass the first half of the test without wanting to rip my hair out.

irb is THE BEST!!! Playing around in this sandbox return big imaginations. Using irb I can see how my test will operate/run. Here's what I did.

Part 1:

I entered my method the way I think it should be. I created an empty array in a new variable called ```new_sentence```. Next, I converted the sentence argument into an array. By doing this, we can then get every word to reverse by using the ```.each``` method. In this case, the array we're passing is the ```sentence.split```. The block we're stating is "add to the empty ```new_sentence``` array the iterated local variable ```backward_words``` in reverse order. Remember, ```.each``` when given a block will iterate each element of the array it's passed. Once, the block code has iterated, we must call on the ```new_sentence``` outside of the ```.each``` method.

If you see my irb below, when we call this method at line 26, we get each word to reverse, but it's still in an array form. See Part 2 to see what happens next.

```
irb
2.3.1 :020 > def reverse_each_word(sentence)
2.3.1 :021?>     #sentence.split     #ignore this line as it was a comment for something I was testing.
2.3.1 :022 >       new_sentence = []
2.3.1 :023?>     sentence.split.each do |backward_words|
2.3.1 :024 >           new_sentence << backward_words.reverse
2.3.1 :025?>       end
2.3.1 :026?>       new_sentence
2.3.1 :027?>   end
 => :reverse_each_word
2.3.1 :028 > reverse_each_word("Hello there, and how are you?")
 => ["olleH", ",ereht", "dna", "woh", "era", "?uoy"]
```

Part 2.

I've redefined my method but this time, I called ```new_sentence.join``` because we're converting the array into a string. Though, the elements have become one whole string, how can we separate them into their own words? Next, see Part. 3

```
2.3.1 :029 > def reverse_each_word(sentence)
2.3.1 :030?>     #sentence.split     #ignore this line as it was a comment for something I was testing.
2.3.1 :031 >       new_sentence = []
2.3.1 :032?>     sentence.split.each do |backward_words|
2.3.1 :033 >           new_sentence << backward_words.reverse
2.3.1 :034?>       end
2.3.1 :035?>       new_sentence.join
2.3.1 :036?>   end
 => :reverse_each_word
2.3.1 :037 > reverse_each_word("Hello there, and how are you?")
 => "olleH,erehtdnawohera?uoy"
```

Part 3.

I redefined the method to call ```new_sentence.join(" ")```. Why ``` (" ")```? We're still joining the elements to convert the array into a string, but by saying ```(" ")``` each element is given its own space. Now, the string is reversing individual words.

TA DA!!!

```
2.3.1 :038 > def reverse_each_word(sentence)
2.3.1 :039?>     #sentence.split     #ignore this line as it was a comment for something I was testing.
2.3.1 :040 >       new_sentence = []
2.3.1 :041?>     sentence.split.each do |backward_words|
2.3.1 :042 >           new_sentence << backward_words.reverse
2.3.1 :043?>       end
2.3.1 :044?>       new_sentence.join(" ")
2.3.1 :045?>   end
 => :reverse_each_word
2.3.1 :046 > reverse_each_word("Hello there, and how are you?")
 => "olleH ,ereht dna woh era ?uoy"
2.3.1 :047 > exit
```

NOW to see it in ```.collect``` method form.

We don't have to declare a new variable like ```new_sentence``` as an empty array. We simply convert the arugment into a string which is then collected into an iteration ```sentence.split.collect ```. The block code stays the same. The other difference is when we do call on the new sentence which has been "collected" we simply state ```.join(" ")```. The simple method will be connected with ```sentence.split.connect```

```
def reverse_each_word(sentence)
  sentence.split.collect do |backward_words|
    backward_words.reverse
  end
    .join(" ")
end
```

### Yield and Blocks

The ```yield``` keyword stops a code and then executes another blocked code before preceeding with the rest of the code. Let's put it in a traffic stop example since we're playing with the word "yield" :p

When you're driving, you carry on with your route. But then you come up to a stop sign. It's telling you to stop or "yield" because there might be a pedestrian or other cars that need to pass. After the pedestrian or cars pass through, you can continue on with your route.

So in code form ...

* define your method = define/begin your route
* the code in your method = you go on your route
* ``` yield {block of code} ``` = oops, there's a person crossing the street, you must stop (```yield```) for the person (```{block of code} ``` to pass
* the code continues = you go on your route
* end = you end your route


```
def yielding
  puts "the program is executing the code inside the method"
  yield
  puts "now we are back in the method"
end

yielding { puts "the method has yielded to the block!" } #calling the method

#output
the program is executing the code inside the method
the method has yielded to the block!
now we are back in the method
```

Yields can also take parameters. Like the ```.each``` method, the block gets passed a variable in the ```|  |``` which becomes the local variable of that block that we can operate on. The parameter passed to ```yield(parameter)``` would be passed to the varaible in the ``` | | ```.

###### My Collect Lab

This lab was a good example on how to see the process of ```yield``` and making use of  ```binding.pry```.

From my understanding we would need to determine how the method would operate when it's called. Given this, I followed the lab's lesson to solve the lab. Here's how I used the example to pass my test.

* So if my_collect(array) was called to return only the first names of it's array it would look like the below code. The array will be passed into the ```my_collect``` method and the block will operate it's duty of converting the string into an array but only taking the first name by spliting at the spaces.

```
#array = ["Tim Jones", "Tom Smith", "Jim Campagno"]

my_collect(array) do |name|
  name.split(" ").first
end
```

* Given the above method call, we can use ```binding.pry``` to look into the ```yield(parameter)``` to determine if the above method call operates correctly. I placed ```binding.pry``` above the code I want to check which is  ```yield(array[i])```.  I'm checking to see if the above method call was yielded correctly. If you see the first pry, it returned ```"Tim"```. This is correct.

```
[18:57:28] (master) my-collect-v-000
// ♥ ruby lib/my_collect.rb
From: /home/christinetran825-39939/code/labs/my-collect-v-000/lib/my_collect.rb @ line 6 Object#my_collect:
     3: def my_collect(array)
     4:   i = 0
     5:   while i < array.length
 =>  6:     binding.pry
     7:     yield(array[i])
     8:     i += 1
     9:   end
    10:   array
    11: end
[1] pry(main)> yield(array[i])
=> "Tim"
[2] pry(main)> exit
```

* In essence, the method call stated above included a block code that ```yield``` applied during its while loop. That block of code for the method call can be written like so.

```#my_collect(["Tim Jones", "Tom Smith", "Jim Campagno"]) { |name| name.split(" ").first }```

```
require 'pry'

def my_collect(array)
  collection = []
  i = 0
  while i < array.length
    #binding.pry
    collection << yield(array[i])
    i += 1
  end
  collection
end
```

Next up, more fun and games with Enumerators.

![](http://i.imgur.com/iJlqqaC.gif)
