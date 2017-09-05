---
layout: post
title:  "Procedural Ruby Pt. 4"
date:   2017-09-05 03:54:05 -0400
---


Finally, done with iterations. :]

![](http://i0.kym-cdn.com/photos/images/original/000/988/454/aa4.jpg)

I do like hashes. They're pretty simple and straight forward. Until we reach iteration. Grrr... I dislike it so much.

**Hashes**

```hash = {"key" => "value", "another_key" => "another value"}```

* Like a dictionary, a hash holds keys (words) with values (definitions). These stored data are grouped into key/value pairs, where the values are related to the keys.

* has no index as it's not a numbered list

* A key can only be used once.

* Adding a second key with the same key name will overwrite it like how we rename a variable.

* **Creating a hash** ----- Similar to an array, we can simply say ```hash = {}```

* **Gettting Data from Hashes** ----- We access values of a hash by using their key ```hash[key]```

* **Adding more data** ----- Use ```[]=``` bracket-equals method ```hash_name[new_key_name]= new_value```


**Symbols**

``` :i_am_a_symbol ```

* the colon symbol is a type of object. It's similar to a string but it can't be changed.

* ```:symbol.object_id``` will output the same id as symbols are immutable (can't change).

* **Using symbols as Hash keys** ----- Since symbols have the same object_id, they take up less memory. It's easier to use symbols than strings as strings take up more memory space.

* When using symbols, we can replace the pointer (=>) in a key/value pair with a colon (:).

     ```my_dictionary = {:word => "definition"}```    to this   ```my_dictionary = {word: "definition"}```


**Hash Iterations**

*Each*

* Using ```.each``` with hash means we iterate over the PAIRS of the key/values.

* Like with other ```.each``` iterations, the original collection is returned.

```
hash.each do |key, value|
   puts "#{key}: #{value}"
end
```

*Collect or map*

* Using ```.collect``` or ```.map``` means we get a return of the collected data that was passed into the block.
 
* Returns a new list of data.

* When using collect or map on a hash, we get an array of the returned values.


----- **KEY-FOR-MIN-VALUES LAB** -----

OMG!!!! this one kicked me in the butt! I couldn't figure it out at all! I found some help via google and stackflow. Here's my code and explaination:

```
name_hash = {:blake => 500, :ashley => 2, :adam => 1}
name_hash = {:blake => 10, :ashley => 50, :adam => 17}
name_hash = {}

def key_for_min_value(name_hash)
  lowest_key = name_hash.reduce do |key, value|
    key.last > value.last ? value : key
  end
    if name_hash == {}
      lowest_key
    else
      lowest_key.first
    end
end

# Since we weren't allowed to use #keys, #values, #min, #sort and #min_by methods, we had to find a way to locate the lowest value, but return the key associated with that lowest value. The method had to pass 3 tests, to show that :adam and :blake were the keys with the lowest value, and if the hash was empty, nil should be returned. It took me some time to figure out how I can find the lowest key/value pair without using the restricted methods. I found an alternative through the REDUCE method.

# Reduce method reduces a list by comparing entries. Each hash gets passed in as an array with 2 elements. Since we're iterating a symbol (the key) through the block, each element in the collection will be passed to the named method of memo (an accumulator value) in which it becomes the new value for the memo. The final value of memo is the returned value for the method.

# Inject "captures" the values or rathers returns the sum of all the values. The key is value.first and value is value.last.

# I defined the iteration into a variable "lowest_key". Within the iteration, as a terany operator we can code "if key.last is greater than value.last then return value, if it's not true then return key."

# After the iteration, we include more logic. If the hash (name_hash) is an empty hash, we return the iteration which should be nil. If the hash is not empty, then we print the key of the lowest value, which is under value.last.
```
----- **end lab** -----

**Nested hashes**

A key in a hash can point to a value that is a collection of objects like an array or another hash.

```
the_foods = {
  "Fruits" => {
	    citrus: ["lemons", "lime", "oranges"],
	    berries: ["strawberries", "blueberries", "gooseberries", "blackberries"]
	}
	"Veggies" => {
	    roots: ["beets", "carrots", "radishes"],
			leafy: ["kale", "lettuce", "bok choy"]
	}
}
```

* the_foods = hash

* key = :citrus and :berries

* values = each key has a value of an array

* to get the first fruit in the citrus array, set a variable first.

```
sours = fruits[:citrus]

sours[1]

#output
"lime"
```

***Adding data to Nested Hashes***

Option 1: Use the ```<<``` method

```
the_foods = {
  "Fruits" => {
	    citrus: ["lemons", "lime", "oranges"],
	    berries: ["strawberries", "blueberries", "gooseberries", "blackberries"]
	}
	"Veggies" => {
	    roots: ["beets", "carrots", "radishes"],
			leafy: ["kale", "lettuce", "bok choy"]
	}
}

the_foods["Fruits"]["berries"] << "raspberries"

#output
   berries: ["strawberries", "blueberries", "gooseberries", "blackberries", "raspberries"]
```


OR

Option 2:

1. Store the value of the first hash key in a variable
2. Store the value of the first hash key's key in question in another variable
3. Add the new data to 2nd variable using ```<<```


```
the_foods = {
  "Fruits" => {
	    citrus: ["lemons", "lime", "oranges"],
	    berries: ["strawberries", "blueberries", "gooseberries", "blackberries"]
	}
	"Veggies" => {
	    roots: ["beets", "carrots", "radishes"],
			leafy: ["kale", "lettuce", "bok choy"]
	}
}

fruits = the_foods["Fruits"]
fruits_of_berries = fruits[:berries}
fruits_of_berries << "raspberries"

puts the_foods

#output
    berries: ["strawberries", "blueberries", "gooseberries", "blackberries", "raspberries"]
```


***Adding Key/Value Pairs to Nested Hashes***

1. Access the level of the hash the added key/value pair will go to
2. Create a new key value on the second level

```
the_foods["Veggies"][:legumes] = ["snap peas", "string beans", "pinto"] 

#output

the_foods = {
  "Fruits" => {
	    citrus: ["lemons", "lime", "oranges"],
	    berries: ["strawberries", "blueberries", "gooseberries", "blackberries"]
	}
	"Veggies" => {
	    roots: ["beets", "carrots", "radishes"],
			leafy: ["kale", "lettuce", "bok choy"]
			legumes: = ["snap peas", "string beans", "pinto"] 
	}
}
```

***Nested Hash Iteration***

iterate one level at a time

```
hash.each do |key, value|
  #first level = |key, value| are the "data"
     #to iterate over the "data" hash, we can use the following line: 
 
       data.each do |key, value|
          #to get to the data inside of this hash, we iterate down to that level
	
	        3rd level hash
		        #some code
	        end
        end
    end
end
```

**More Hash Methods**

``` .values ```
collects all the values in a hash

``` .keys ```
collects all the keys in a hash

``` .min ```
returns the key/value pair whose key is the **lowest** value.

Note: Arrays can return two different things, it's most helpful for ``` .min ```

``` .flatten ```
turns all the data into a flat array


----- **BUILDING-NESTED-HASHES LAB** ----- 

This codealong lab was very fun. I liked how the use of characters from Romeo & Juliet made a great example of hashes and nested hashes.

The bonus was relatively easy, too. Using ```[]=``` to change the status of the hero and heroine is relatively straight forward, however, I did notice an interesting syntax. I've reduce the code a bit to explain what I noticed about the syntax.

In the hash itself, we see the key ``` :hero ``` and ``` :heroine ```. Both their values which are more hashes show a key called ``` status: ```.  In order for us to change the status from "alive" to "dead", we used the ``` []= ```. However, when calling the ``` []= ``` of the nested hashes, the ``` status: ``` must have it's colon moved to the front. I'm not sure how important that detail is but it seemed to matter to pass this lab. 

``` epic_tragedy[:montague][:hero][:status]= "dead"```

```epic_tragedy[:capulet][:heroine][:status]= "dead"```

```
def bonus
  epic_tragedy = {
   :montague => {
     ....
      :hero => {name: "Romeo", age: "15", status: "alive"},
    ...
      ]
   },
   :capulet => {
      ...
      :heroine => {name: "Juliet", age: "15", status: "alive"},
    ...
      ]
   }
  }
......
```
----- **end lab** ----- 

----- **APPLES & HOLIDAYS LAB** ----- 

I really like learning about hashes. I find it easier to understand and to operate/code than arrays.

Debugging with binding.pry was very helpful with this lab. I used it to determine the code for ```#all_winter_holiday_supplies``` It took a couple of pry tests but I figured it out.

```
def all_winter_holiday_supplies(holiday_hash)
  # return an array of all of the supplies that are used in the winter season
  holiday_hash[:winter].values.flatten
  #binding.pry
end

# I placed the binding.pry below the code I thought would work. However, I worked the code in pieces. Please view my pry in image form below and I'll explain.

# We first want to see the entire values of the holiday_hash. I coded holiday_hash.values. This returned the entire values in array format listed in the hash.

# Like an array, I searched the index for the :winter key's values of :christmas and :new_years. I coded holiday_hash.values[0]

# From holiday_hash.values[0], we find the return value gives us the :winter key's values of key's (which are the holidays of :christmas and :new_years. At this point we know all we want is the values of :winter. This means we code holiday_hash[:winter].values

# Lastly, to return the entire values into one whole array per the tests, we include the .flatten method. => holiday_hash[:winter].values.flatten
```

![](https://i.imgur.com/e8FbaEq.png)

This other method was tricky. I chose to iterate through each data instead of one whole string.

First I iterated the top level. The block code indicate seasons as the seasons in the hash being passed through. "data" indicate the key/value pair hash relating to that specific season.

Second, I iterated the "data" with another each method. The block code indicate holidays as the holiday key in the season hash. "supplies" indicate the values of the holidays key.

Third, I converted the holidays into an array so I can split the underscore for any holiday that had 2 words. Taken the array, I iterated it so I can add the capitalize method, which I would then turn the array into a string with .join.

Lastly, I puts the variables of the block code into a string

```
def all_supplies_in_holidays(holiday_hash)
  # iterate through holiday_hash and print items such that your readout resembles:
  # Winter:
  #   Christmas: Lights, Wreath
  #   New Years: Party Hats
  # Summer:
  #   Fourth Of July: Fireworks, BBQ
  # etc.

  ###iterate through everything
  holiday_hash.each do |seasons, data|
     puts "#{seasons}:".capitalize  #top-level, seasons is the seasons in hash. "data" are the hash of key/value pairs
    data.each do |holidays, supplies|
      holidays = holidays.to_s.split("_")
      holidays = holidays.collect {|the_holiday| the_holiday.capitalize }.join(" ")
      supplies = supplies.join(", ")
      puts "  " + "#{holidays}: " + "#{supplies}" #holidays is the holidays listed in the top-level hash which is :winter, :spring .... "value" is the key/value pairs of the holidays hash.
    end
  end
end
```

----- **end lab** ----- 

**REGEX**

REGEX = Regular Expressions for PATTERNS

* Search through strings and blocks of text for pattern
* used for data validation, searching, mass file renaming, finding records in a database.

*metacharacters*

Use a pre-defined shorthand to match specific characters
* \d match digit in text
* \w match word character (letters, numbers, and underscores)
* \W match non-word characters in text
* lots more...

*specific characters*

* use [] = ```/[aeiou]/ #finding vowels in a string```

*ranges*

* can look for single characters of the alphabet ```/[a-z]/```

*repetition*

* use {} denotes a repeatition that occurs the number of times as denoted in the {}
* ```/[aeiou]{2}/ #vowel repeats two times```

**REGREX RUBY METHODS**

Regrex sounds really cool. I'm having difficulties in writing the methods using the metacharacters though.

``` .scan ```

* returns an array of **all** items in the string that match a given REGREX

``` .match ```

* returns first item in the string that matches a given REGREZ as a ```MatchData```

``` MatchData ``` 

* result is usually a boolean which tells us if the pattern exists or not
* \d matches digits
* \w matches words
* /[]/ matches specific single characters. Can be used to look for a range of letters or numbers - /[0-9]/, /[a-j]/
* /[]{number}/ the pattern or character preceding {} will repeat that number of times.

``` .grep ```

* an enumerable method looking for patterns in arrays and hashes
* returns array of matching items from an array (like scan)

``` Capture Groups ```

* using () in regex creates "groups" that can be reference in scan/match/grep methods as indexes in an array.

-----**end REGREX** -----

**DONE with Procedural Ruby**

So this was a very challenging set of practice for Ruby. I've certainly learned a lot and hope to continue on this path of continous learning. Hopefully with a smoother understanding of the labs.

![](http://i0.kym-cdn.com/photos/images/facebook/000/949/928/ab3.jpg)



