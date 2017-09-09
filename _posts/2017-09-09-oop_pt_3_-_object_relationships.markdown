---
layout: post
title:  "OOP Pt. 3 - Object Relationships"
date:   2017-09-09 05:29:12 +0000
---


The relationship between objects was hard to understand through the readings.

So I decided to break down what I've learned.

In OOP Ruby, two classes (models) can relate to each other - "belongs to" or "has many"

**"belongs to"**

* builds a one-on-one connection between the two classes (models).

* model the relationship by giving one class an attribute of the other class.
  * this creates a setter and getter for the first class' other class's attribute 

Example: Every song has an artist. So, in class Song, we reference the artist's name in ```attr_accessor :artist, :name```. In class Artist we reference the artist's name in ``` attr_accessor :name ```

```
class Song
  attr_accessor :artist, :name, :genre
 
  def initialize(name, genre)
    @name = name
    @genre = genre
  end
end

#calling new Song:
ninetynine_problems = Song.new("99 Problems", "rap")

----

class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
  end
end

#calling new Artist: 
jay_z = Artist.new("Jay-Z")

---
#calling has many relationship

ninetynine_problems.artist = jay_z
 
ninetynine_problems.artist.name
  # => "Jay-Z"
```

Breaking it down:

1. "ninetynine_problems" is initialized by class Song and ``` .new ```

   ``` ninetynine_problems = Song.new("99 Problems", "rap") ```

2. "jay_z" is initialized by class Artist and ``` .new ```

   ``` jay_z = Artist.new("Jay-Z") ```

3-A. We take the instance of class Song ``` ninetynine_problems ``` and call the ``` .artist ``` method on it from the attr_accessor to equal the value of the instance from the class Artist ``` jay_z ```

* the artist attribute is equal to the strings ("99 Problems", "rap") but needs to be equal to an instance of the Artist class, stored in the jay-z local variable

   ``` ninetynine_problems.artist = jay_z ```


3-B. Then, we take 3-A and call the ``` .name ``` method from the class Artist which gives us "Jay-Z".

* although the class Song's **artist** attribute is a setter and getter, we have to set up a separate class for Artist to give it an individual song's artist attribute equal to the instance of that class.

   ```
	 ninetynine_problems.artist.name
     => "Jay-Z"
   ```

**Has many**

* builds one-to-many connection between two models
* each instance of the model has some or no instances of another model
* initialize an empty collection

Here, we're adding an empty array collection with the @songs

```
class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
end
```

Then we add the items to the collection - but think of responsibility. Who's responsibility is it to add new items?

```
class Song
  attr_accessor :artist, :name, :genre
 
  def initialize(name, genre)
    @name = name
    @genre = genre
  end
	
	def artist_name    #returns the name of a given song's artist
    self.artist.name
  end
end

#calling methods

ninetynine_problems = Song.new("99 Problems", "rap")
crazy_in_love = Song.new("Crazy in Love", "pop")

---

class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
	
	def add_song_by_name(name, genre)    
    song = Song.new(name, genre)   #create a new song using name and genre as arguments
    @songs << song      #adds the song into the collection
    song.artist = self      #tells a song that it belongs to an artist (self). The .artist method called on song is a setter and since song is being passed in as an argument we set it equal to the artist (self)
  end

  def songs
    @songs   #@songs return the @songs array
  end

end

#calling methods

jay_z = Artist.new("Jay-Z")

```

Once songs can be added to the instance @song through ``` add_song ```, we have to expose that instance with an instance method of ``` songs ```.

```
#calling methods

jay_z.add_song(ninetynine_problems)
jay_z.add_song(crazy_in_love)
 
jay_z.songs
```

Breaking it down (converting instances not just strings):

1. Initialize instances of class Song with arguments of a song title and a song genre as strings

   ```
   ninetynine_problems = Song.new("99 Problems", "rap")
   crazy_in_love = Song.new("Crazy in Love", "pop")
   ```

2. Initialize an instance of class Artist with argument of an artist's name as a string

   ``` jay_z = Artist.new("Jay-Z") ```

3. call the ``` .add_song ``` method on the class Artist instance ``` jay_z ``` with the argument of the class Song instances ``` ninetynine_problems  and crazy_in_love ``` as arguments.

	 ```
	 jay_z.add_song(ninetynine_problems)
   jay_z.add_song(crazy_in_love)
 
   jay_z.songs
	    # =>[#<Song:0x007fa96a878348 @name="99 Problems", @genre="rap">, #<Song:0x007fa96a122580 @name="Crazy in Love", @genre="pop">]
   ```
	 
From all these codes, we now have a collection (array) of songs and its genres. With a collection (array) of data, we can iterate them to collect their genres.

```
jay_z.songs.collect do |song|   ##.songs is the instance method of class Artist
  song.genre
end
  # => ["rap", "pop"]
```

# **PHEW!!**
I'm hoping my break down in understanding object relationships made sense to my brain.

![](http://i.imgur.com/RSXKJ5E.jpg)
