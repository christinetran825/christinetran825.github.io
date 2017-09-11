---
layout: post
title:  "OOP - Pt. 2 - Self & Class Methods/Variables"
date:   2017-09-07 21:15:46 -0400
---

## **Edit for clarity**

I'm struggling with the concept of self. I understand what it's all about but applying it as a method and/or to instance variables is confusing. In one of the lessons video, Avi touched on the subject of Class Memoization. Learning about that cleared up some confusion I had about OOP and class objects.

**Class Memoization**

When creating a class, we're teaching it to remember the instances of itself. Memoization allows us to reduce the amount of coding/work we have to do to avoid duplicated steps.

[Gavin Miller](http://gavinmiller.io/2013/basics-of-ruby-memoization/) explained memoization as a pattern:

1. Perform some work
2. Store the work result
3. Use the stored results in future calls
4. ** most common pattern to memoize a class is using the conditional assignment operator ``` | |= ```. *This operator occured in one of the labs. it means if the value on the left has a value, leave it alone and dont't change the value, other wise, set the value on the left to the value on the right.*

``` x | |= y  =>  if x has a value, then it equals x, if not then x = y ```

For a class to know itself, we refer to it as ``` self ```.

**Self**
 
Self is the object itself. It can refer to different things depending on where it's used. 

* In an instance method, self will refer to the instance itself.

* In a class method, self will refer to the class itself (the entire class) ``` def self.method_name ```

* ``` self ``` is a thing that will receive the method call

```
class Dog
 
  attr_accessor :name
 
  def initialize(name)
    @name = name
  end
 
  def bark
    "Woof!"
  end
 
end

fido = Dog.new("Fido")    #create a new instance of Dog
fido.name     # access/call on the object fido's name which is "Fido" by calling name method (which is attr_accessor)
  => "Fido"
fido.bark      # access/call on the object fido to bark by calling bark method
  => "Woof!"

```

*----- OO Counting Sentences*

* The last instance method of this lab was tough. I didn't know that we could call on ``` .split ``` with mutliple characters using REGREX.

```
class String

 ... other codes

 def count_sentences
  #binding.pry
	self.split(/\.|\?|\!|\!!|\.../).delete_if { |word| word.size < 2}.size
 end
end
```

* Using the binding.pry I split the sentence itself at the punctuations listed. This new splitted sentence would now equal to a new variable I called new_split_sentence

* new_split_sentence = ``` self.split(/\.|\?|\!|\!!|\.../) ```

* new_split_sentence.size   #I then determined this new splitted sentences size as an array

* Putting it all together, I didn't have to call on the new splitted sentence but on the sentence itself. From there I would delete any word size less than two characters

*----- end of lab*

On the subject of self, it can call on its own variable and method similar to how an instance calls on its instance variable and instance method. They are called **Class variables** & **Class methods**.

**Class variables**

* stores information about the class as a whole and is accessible to the entire class because it has **class scope**

* written with two @@ symbols

* a class level variable is used for instance memoization (remembering all instances of a class)

* can be equal to an empty array as they store lists of data.
   
    ``` @@class_variable_name ```

**Class methods**

* methods that can be called on the class directly without having to instantiate any objects

* stores behavior that doesn't relate to individual objects

* usually denoted with ```self``` in the method name
   
    ```
    def self.class_method_name
       #some code
    end
    ```

!!! Before adding new attributes or functions to our class, we must determine whose **responsibility** it is to enact that behavior or attributes. So designing objects means either the class itself or the instances of the class is responsible for that object.

```
class Album

 @@album_count = 0
 
 def initialize   #instance_method
  @@album_count += 1 #increment the number of albums by 1 every time a new album object is initialized
 end
 
 def self.count
  @@album_count
 end

end

Album.new
Album.new
Album.new

Album.count
=> 3
```

Every individual album instance should have a release date attribute. That attribute can be defined as an instance variable called ``` @release_date ``` and its instance method ``` release_date ``` will expose that variable.

The instance variable ``` @release_date ``` is set equal to a value using the ``` release_date=() ``` setter method. The getter method of ``` release_date ``` returns the value of ``` @release_date ```.

This is how we can execute it:

```
album = Album.new
album.release_date = 1991
album.release_date 
  # => 1991
```


Determining the responsibility means we should also understand the two types or styles of methods - Public and Private methods.

**Public Methods**

* can be called outside the scope of the class declaration, like on the instance of the class or the class itself. They are explicit receivers

* ``` def self.all ``` defines a class method that exposes the value in a class variable ``` @@all ``` It acts as a class reader for class variables

**Private Methods**

* can't be called by an explicit receiver

* only be called within the context of the defining class - the receiver of a private method is always ``` self ```

* depended on by other methods

* ``` initialize ``` method is a private method
 
Within the ``` initialize ``` method, we only get one copy of the initialized object. We can save the objects upon initialization by adding the ``` self ``` to it's class variable that's set as a collection (this could be an array or a hash). New instances can be added to this storage variable (as an array or hash) every time a new instance was created signaled by the ```self``` keyword in the ``` initialize ``` method. A class method can access and print out the individual instances stored in the class variable.

```
class className
@@all = []

 def initialize(name)
  @name = name
  @@all << self   #self is now referring to whichever instance the method is being called on, not the class itself - this is method scope
 end

 def self.all   #this self refers to the class on which the class method will be called - this is class scope
  @@all.each { |x| some code } ##method that exposes the value of a class variable making it accessible outside of the class
 end
end
	
className.all
=> outputs the @@all array
```
	
Here's the lab example

*----- Puppy Lab*

```
class Dog
  attr_accessor :name   #has a name
  
  @@all = [ ]   

  def initialize(name)    #.new initializes an argument of a name
    @name = name
    @@all << self     #adds new dog to the @@all array. New dog instances/objects are called self
  end

  def self.all
    puts @@all.collect { |dog| dog.name } #class method that puts out the name of each dog
  end

  def self.clear_all   #class method that empties the @@all array of all existing dogs
    @@all.clear
  end
end
```

*----- end lab*

It's at this point where I start having trouble with OOP - advanced OOP.

**Class methods**

As mentioned earlier, a class method exposes the value in a calss variable. It acts as a class reader for class variables. Here are some class methods that return exisiting instances or create new instances, and manipulate class level data.

1.  ``` def self.all ``` defines a class method that exposes the value in a class variable ``` @@all ```. The class is providing access to all its instances through the ``` self.all ``` method

2. ``` .find_by_(instance attribute) ``` is a ***class finder method***

    * class finder methods are responsible for finding instances based on attributes or conditions

    * Since ``` self.all ``` allows us to access the instances in ``` @@all ```, we can use the ``` .detect ``` or ``` .each ``` method to find a specific instance/object equaling its value
    
       * This action can be done within a class method like ``` .find_by_(instance attribute) ``` We're making the class itself responsible for knowing its instance
       
       * Within a class method, we can call another class method with the class itself. ``` self ``` is now within the class method so ``` self ``` is class itself

          ```
          class Person
            attr_accessor :name
             @@people = []
 
            def self.all
             @@people
            end
 
            def initialize(name)
             @name = name
             self.class.all << self
            end
 
            def self.find_by_name(name)
             self.all.detect{|person| person.name == name}
            end
          end

          Person.find_by_name("Ada Lovelace")
          Person.all.detect { |p| p.name ==  "Ada Lovelace" }
          ```

3. **Custom Constructor**

Going back to the ``` .initalize ``` method, we learned that it's never modified. When an instance needs more functions/behaviors, we can extend the initialize method with a custom constructor.

* We wrap the ``` .new ``` method which can be saved into a class variable because there are times when not every single new initialized instance/object.

* By creating ``` .create ``` class method, we can extend the functionality/behavior of initializing and creating the instance.

   ```
   def self.create(name) #custom constructor
	  song = self.new(name)
		song.save
		return song
   end
   ```

4.** Class Operators**

Class operators manipulate class-level data - puttings data, capitalization, deleting elements/data in an collection

normalizing a person's name:

```
def normalize_name
  self.name.split(" ").collect{|w| w.capitalize}.join(" ")
end
 
def self.normalize_names
  self.all.each do |person|
    person.name = person.normalize_name
  end
end
```

deleting elements/data: uses Array#clear method

```
def self.destroy_all
   self.all.clear
end
```

*----- Advanced Class Methods Lab - Song.rb*

More practice with self and learning class constructors through this lab

Build class constructors that interact on the class data of ``` @@all ```

```
class Song
  attr_accessor :name, :artist_name
  @@all = []

  def self.all
    @@all
  end

  def save
    self.class.all << self
  end

* song.create => *self.create* initializes a song and saves it to @@all

  def self.create
    song = Song.new 
    song.save 
    song
  end

* song.new_by_name  => *self.new_by_name* takes string of song and returns a song instance with that name as its name property

  def self.new_by_name(song_name)
    song = self.new 
    song.name = song_name
    song
  end

* song.create_by_name => *self.create_by_name*  takes string of song and returns a song instance with that name as its name property AND the song saved into @@all. #calls the self.create instance method on the song because we want to save the song which is done in the self.create method.

  def self.create_by_name(song_name)
    song = self.create     
    song.name = song_name
    song
  end

* song.find_by_name => *self.find_by_name* takes string name of a song and returns matching instance of the song with that song name.

  def self.find_by_name(song_name)
    self.all.detect { |song|
      song.name == song_name
    }
  end

* song.find_or_create_by_name => *self.find_or_create_by_name* takes a string name of a song and either returns a matching song instance with that name or create a new song with the name and returns the song instance.

* #uses the find_by_name method to find the song name that returns the matching instance if the song has the same song name.

* #uses the create_by_method

  def self.find_or_create_by_name(song_name)
    self.find_by_name(song_name) || self.create_by_name(song_name)
  end
	
* song.alphabetical => *self.alphabetical*	

  def self.alphabetical
    self.all.sort_by { |song|
      song.name
    }
  end

* song.new_from_filename => *self.new_from_filename*

  def self.new_from_filename(filename)
    new_file = filename.split(" - ") #'initializes a song and artist_name based on the filename format'
    artist_name = new_file[0]
    song_name = new_file[1].gsub(".mp3","")

    song = self.new
    song.name = song_name
    song.artist_name = artist_name
    song
  end

* song.create_from_filename => *self.create_from_filename*

  def self.create_from_filename(filename)
    new_file = filename.split(" - ") #'initializes and saves a song and artist_name based on the filename format'
    artist_name = new_file[0]
    song_name = new_file[1].gsub(".mp3","")

    song = self.create
    song.name = song_name
    song.artist_name = artist_name
    song
  end
	
* song.destroy_all => *self.destroy_all*

  def self.destroy_all
    self.all.clear
  end
end

```

*----- end of lab*

I think this is going to be a rough learning course this time.

![](https://i.imgflip.com/hn557.jpg)


