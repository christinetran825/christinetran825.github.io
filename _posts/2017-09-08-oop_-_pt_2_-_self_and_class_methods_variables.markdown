---
layout: post
title:  "OOP - Pt. 2 - Self & Class Methods/Variables"
date:   2017-09-08 01:15:46 +0000
---

I'm struggling with the concept of self. I understand what it's all about but applying it as a method and/or to instance variables is confusing. In one of the lessons video, Avi touched on the subject of Class Memoization. Learning about that cleared up some confusion I had about OOP and class objects.


**Class Memoization**

When creating a class, we're teach it to remember the instances of itself. Memoization allows us to reduce the amount of coding/work we have to do to avoid duplicated steps.

[Gavin Miller](http://gavinmiller.io/2013/basics-of-ruby-memoization/) explained memoization as a pattern:

1. Perform some work
2. Store the work result
3. Use the stored results in future calls
4. ** most common pattern to memoize a class is using the conditional assignment operator ``` | |= ```. *This operator occured in one of the labs.*

For a class to know itself, we refer to it as ``` self ```.

**Self**
 
Self is the object itself. It can refer to different things depending on where it's used. 

* In an instance method, self will refer to the instance itself.

* In a class method, self will refer to the class itself (the entire class) ``` def self.method_name ```

* ``` self ``` is a thing that will receive the method call

Within the ``` initialize ``` method, we only get one copy of the initialized object. We can save the objects upon initialization by adding the ``` self ``` to it's class variable that's set as a collection (this could be an array or a hash). New instances can be added to this storage variable (as an array or hash) every time a new instance was created signaled by the ```self``` keyword in the ``` initialize ``` method. A class method can access and print out the individual instances stored in the class variable.

```
class className
 @@all = []
 
 def initialize(name)
  @name = name
  @@all << self
 end
end
```

Class/custom constructor (like initialize) still makes an instance/new object but it's also saving it without overriding initialized. It's extending the class's functionality. It's denoted with the ```self``` keyword along with the method name.

```
def self.create(name) #custom constructor
  song = self.new(name)
	song.save
	return song
end
```

Before adding new features or functions to our class, we must determine whose **responsibility** it is to enact that behavior or functionality. So designing objects means either the class itself or the instances of the class is responsible for that object.

**Class Variables & Methods**

```
class Album
 
  def release_date=(date)
    @release_date = date
  end
 
  def release_date
    @release_date
  end
end
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

The class Album is itself an object because objects are a bundle of code that contains attributes and behaviors. This means a class can have its own variables and methods called **class variables** and **class methods**.

   * Class variables stores information about the class as a whole and is accessible to the entire class because it has **class scope**. It's written with two @@ symbols. A class level variable is used for instance memoization (remembering all instances of a class). Class variables can be equal to an empty array as they store lists of data.
   
``` @@variable_name ```

   * Class methods are methods that can be called on the class directly without having to instantiate any objects. They store behavior that doesn't relate to individual objects. It's usually denoted with ```self``` in the method name
   
```
def self.class_method_name
   #code
   end
```

**Public Methods** can be called on the instance of the class or the class itself. They are explicit receivers.

**Private Methods** can't be called by an explicit receiver. It can only be called within the context of the defining class. The receiver of a private method is always ``` self ```. The ``` initialize ``` method is a private method.

**Methods are always calling on an object**

Class Constants are shared throughout all instances.

```
Class::CONSTANT
class Book
  attr_accessor :author, :page_count  # remove the attr_accessor for genre
  attr_reader :title, :genre  # add an attr_reader for genre

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

*----- Class Variables and Methods Lab - Song.rb*

This is another tough lab I faced. It took me a while to understand how ``` self ``` works with in an instance variable and as a class method.

#Song class does the following:

* tracks the number of songs it creates => *@@count - keeps track of new songs created from Song class*
* shows all the artists of existing songs => *@@artists*
* shows all the genres of existing songs => *@@genres*
* keeps track of the number of songs of each genre it creates => *counts number of songs of each genre*
* reveal the number of songs each artists is responsible for => *counts number of songs of each artist*

#class Song initializes an individual song with a name, artist, and genre => *attr_accessor* AND *initialize*

```

class Song
  attr_accessor :name, :artist, :genre

  @@count = 0
  @@artists = []   #contains all of the artists of exisiting song as was pushed from initialize method
  @@genres = []  #contains all of the genres of exisiting song as was pushed from initialize method

  def initialize(name, artist, genre)
    @name = name
    @artist = artist
    @genre = genre
    @@count += 1
    @@genres << genre   #initialized and pushed to [ ] to save
    @@artists << artist     #initialized and pushed to [ ] to save
  end

  def self.count  #total number of soungs created
    @@count
  end

  def self.genres   #returns array of all the genres of exisiting songs; no duplicates
    @@genres.uniq
  end

  def self.artists    #returns array of all artists of existing songs; only unique artists, no repeats
    @@artists.uniq
  end

  def self.genre_count     #returns hash; keys are names of each genre; each genre key point to value that is the number of songs that have that genre
    genre_count = {}
    @@genres.each do |genre|
      if genre_count[genre]
        genre_count[genre] +=1
      else
        genre_count[genre] = 1
      end
    end
    genre_count     #returns the new hash key/value pair
  end

  def self.artist_count     #retuns hash key/value pair; similar to genre_count but for artists
    artist_count = {}
    @@artists.each do |artist|
      if artist_count[artist]
        artist_count[artist] +=1
      else
        artist_count[artist] = 1
      end
    end
    artist_count
  end

end
```

*----- end of lab*

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


