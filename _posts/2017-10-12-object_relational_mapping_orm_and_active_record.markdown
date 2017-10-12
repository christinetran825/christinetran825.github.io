---
layout: post
title:      "Object Relational Mapping (ORM) & Active Record"
date:       2017-10-12 22:15:41 +0000
permalink:  object_relational_mapping_orm_and_active_record
---


ORM connects objects of an application to tables in a relational database. This means ORMs can access a relational database using OOP languages (like Ruby) that cuts down on repetition. The properties and relationships of the objects are stored and accessible without writing SQL statements directly.

OOPs can manage database data by “mapping’ or ‘wrapping’ database tables to classes and instances of classes to rows in the tables. This means:

* Classes are mapped to tables inside a database
* Instances of the classes are mapped to the database’s table rows

**Mapping Ruby Classes to Database Tables**

Mapping classes to a table persists data in an organized fashion.
* Create database
* Create class table
   * Convention to pluralize name of the class to create name of table
      * class Song => songs table.
* Write a class method within the class, with column names that match the attr_accessors of the class.

```
class Song
 
  attr_accessor :name, :album, :id
 
  def initialize(name, album, id=nil)
    @id = id
    @name = name
    @album = album
  end
 
  def self.create_table
    sql =  <<-SQL 
      CREATE TABLE IF NOT EXISTS songs (
        id INTEGER PRIMARY KEY, 
        name TEXT, 
        album TEXT
        )
        SQL
    DB[:conn].execute(sql)
   @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
  end
 
end
```
Ruby deals with objects. Database deals with raw data.

Ruby objects are not being saved into the database. A Ruby’s object’s attributes are used to create new rows in the database table which is the instance. Remember, an instance is a table’s row. 

Inserting data into a table with save method

```
def save
    sql = <<-SQL
      INSERT INTO songs (name, album) 
      VALUES (?, ?)
    SQL
 
    DB[:conn].execute(sql, self.name, self.album)
 
  End
```

Creating new instances and saving it

```
def self.create(name:, album:)
    song = Song.new(name, album)
    song.save
    song
  End
```

**Mapping Database Tables to Ruby Classes**

Mapping stores raw data describing a given Ruby object in a table row. To reconstruct a Ruby object from the stored data, that same row in the table is selected. When querying the database, methods are coded to access those data and convert into an instance of the appropriate class (Ruby objects).

Convert the database into Ruby objects:
SQLite returns an array of data for each row
[id, data_name, data_value]

```
def self.new_from_db(row)
  instance_name = self.new 
  instance_name.id = row[0]
  instance_name.name =  row[1]
  instance_name.length = row[2]
  instance_name  # return the newly created instance
End
```

Return an array of rows from the database that match the query. 

```
class Song
 def self.all
    sql = <<-SQL
      SELECT *
      FROM songs
    SQL
 
    DB[:conn].execute(sql)
  end
end
```

Now to iterate each row and use ``` self.new_from_db ``` to create new Ruby object for each row.

```
class Song
  def self.all
    sql = <<-SQL
      SELECT *
      FROM songs
    SQL
 
    DB[:conn].execute(sql).map do |row|
      self.new_from_db(row)
    end
  end
end
```

```
class Song
  def self.find_by_name(name)
    sql = <<-SQL
      SELECT *
      FROM songs
      WHERE name = ? #? Is placeholder where the name parameter is passed in (bound parameters)
      LIMIT 1
    SQL
 
    DB[:conn].execute(sql, name).map do |row|
      self.new_from_db(row)
    end.first
  end
end
```

**Updating records in ORM:**

* Get/find attributes from the row in database table
* Recreate them in a Ruby object
* Make changes to that object using Ruby methods
* THEN save all updated attributes back into database

The goal of dynamic ORM is to define a series of methods that can be shared by any class. So, we need to avoid explicitly referencing table and column names.

!!!!! metaprogramming = writing code that writes code for us !!!!!

```
def update
    sql = "UPDATE songs SET name = ?, album = ? WHERE id = ?"
    DB[:conn].execute(sql, self.name, self.album, self.id)
end
```

```
def save
  if self.id
    self.update
  else
    sql = <<-SQL
      INSERT INTO songs (name, album) 
      VALUES (?, ?)
    SQL
    DB[:conn].execute(sql, self.name, self.album)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
  end 
end
```

Avoid duplications

```
def self.find_or_create_by(name:, album:)
    song = DB[:conn].execute("SELECT * FROM songs WHERE name = ? AND album = ?", name, album)
    if !song.empty?
      song_data = song[0]
      song = Song.new(song_data[0], song_data[1], song_data[2])
    else
      song = self.create(name: name, album: album)
    end
    song
end
```

**Dynamic ORMs**

* Create database table
* Metaprogramming based on that table
* Set up database
* Build attr_accessors from column names
* Define table name

```
class Song
  def self.table_name
    self.to_s.downcase.pluralize
  end
End
```

Define column names
```
def self.column_names
  DB[:conn].results_as_hash = true
 
  sql = "PRAGMA table_info('#{table_name}')"
 
  table_info = DB[:conn].execute(sql)
  column_names = []
 
  table_info.each do |column|
    column_names << column["name"]
  end
 
  column_names.compact
end
```

Metaprogramming attr_accessors

```
self.column_names.each do |col_name|
    attr_accessor col_name.to_sym
  End
```

Abstract initialize method
```
def initialize(options={})
  options.each do |property, value|
    self.send("#{property}=", value)
  end
End
```

Write ORM methods

```
def table_name_for_insert
  self.class.table_name
End

def col_names_for_insert
  self.class.column_names.delete_if {|col| col == "id"}.join(", ")
End

def values_for_insert
  values = []
  self.class.column_names.each do |col_name|
    values << "'#{send(col_name)}'" unless send(col_name).nil?
  end
  values.join(", ")
End

def save
  sql = "INSERT INTO #{table_name_for_insert} (#{col_names_for_insert}) VALUES (#{values_for_insert})"
 
  DB[:conn].execute(sql)
 
  @id = DB[:conn].execute("SELECT last_insert_rowid() FROM #{table_name_for_insert}")[0][0]
End

def self.find_by_name(name)
  sql = "SELECT * FROM #{self.table_name} WHERE name = #{name}"
  DB[:conn].execute(sql)
End
```

![](http://www.washingtonpost.com/wp-srv/special/entertainment/metro-love/img/map.png)

## **Active Record**

Maps existing database table to a class and writes methods to be shared with other classes. It gives functionality through inheritance. Something similar to Dynamic ORM is ActiveRecord. It maps database tables to Ruby classes as a framework. As noted by the rubyonrails guide, Active Records...:

* Represents models and their data
* Represents associations between those models
* Represents inheritance hierarchies through related models
* Validates models before they get persisted to the database
* Performs database operations in an object-oriented fashion

Active Record is a Ruby gem which provides us with a library of code through installation. These codes gives us a lot of functionality.

The pattern begins by:
* Connecting to a database
* Creating a table
* Add Active Record’s ``` Base ``` methods to our class by inheritance ``` ActiveRecord::Base ```

```
class Student < ActiveRecord::Base
end
```

Types of base methods
```
.column_names
.create
.find
.find_by
.save
attr_accessors
```

Active Records automatically has methods that read and alters data stored within its tables. This is called CRUD (Create, Read, Update, and Delete)

*Create* Active Records objects can be created from a hash, a block, or have their attributes manually set. This method returns the object and save it to the database
```
.create
```

*Read* Accessing data.
```
.all
.first
.find_by
.where
```

*Update* When an Active Record object is received, its attributes can be altered and save to the database
```
.update
.update_all
```

*Delete*
```
.destroy
```

![](http://markvsql.com/wp-content/uploads/2016/11/Meme.jpg)

### **Migration & Rake**

To manage a database, a language called migrations does the job. Migration uses ``` rake ``` that supports the migration. Rake is a tool that automates certain jobs through ``` rake tasks``` A task is defined through the command line to execute those jobs.

Rake is a part of Ruby and needs only be created in a file called ``` Rakefile  ```

In this file, the task is defined: 

```
Task + name of task as a symbol + block that has the code to be executed

task :hello do
  # the code we want to be executed by this task 
end

```

In Terminal, when entering ``` rake hello ``` the code is executed back.

``` rake -T ``` when run in the terminal gives a list of available Rake tasks and its descriptions. For these tasks to work, they need descriptions.

```
desc 'outputs hello to the terminal'
task :hello do 
  puts "hello from Rake!"
end
```

Rake tasks can be grouped through ``` namespace ```

```
namespace :greeting do 
desc 'outputs hello to the terminal'
  task :hello do 
    puts "hello from Rake!"
  end
 
  desc 'outputs hola to the terminal'
  task :hola do 
    puts "hola de Rake!"
  end
end
```

In terminal we can use either of those grouped Rake tasks by entering:

```
rake greeting:hello

rake greeting:hola
```

How Rake and Migration interact is by ``` rake db:migrate ```. Database tables are created and then migrated using a rake task. Within migration, there are a few rake tasks involved:

* task dependency
   * Classes and databases are loaded within config/environment.rb. The task must access this file. That access needs a rake task to run before migrate task is run.

* ``` rake db: seed ```
   * Seed task seeds database with dummy data. A file should be in the db directory and has code that create instances of the class. Then, a rake task is defined to execute that code.


*Rake console*

This starts up the Pry console for us to play with our code. ``` rake db:migrate ``` would need to be run before we entering rake console in terminal.


![](https://cdn.iconscout.com/public/images/icon/premium/png-256/rake-fall-leaves-gardening-tool-garden-326006f8ecf84a62-256x256.png) ![](https://cdn.iconscout.com/public/images/icon/premium/png-256/rake-fall-leaves-gardening-tool-garden-326006f8ecf84a62-256x256.png) ![](https://cdn.iconscout.com/public/images/icon/premium/png-256/rake-fall-leaves-gardening-tool-garden-326006f8ecf84a62-256x256.png)

#### Setting up Migration

Migration helps alter the database schema over time without having to write SQL. It keeps records of the alteration so nothing is altered completely without history. Active Record can reverse the migration by rolling it back to its previous state.

* Alters database in a structured and organized manner
* Allows for describing transformations using Ruby
* Version control for database
* Executed migrations are tracked by ActiveRecord in database to avoid duplicates
* Applying schema changes made easy

Example
```
# db/migrate/01_create_artists.rb
 
class CreateArtists < ActiveRecord::Migration
 ...some code
end
```
The class inherits ActiveRecord’s ``` ActiveRecord::Migration ``` module. This ``` ActiveRecord::Migration ``` module allows the class CreateArtists to generate the artists table.

**Connect to a database**

* Connect to a database where a table is created using SQL.

**Create a table**

* Typically the files within ``` db/migrate ``` generates the tables with its columns. The file .../01_create_artists.rb would usually have the class set up like so:
 
```
class CreateArtists < ActiveRecord::Migration
def change
    create_table :artists do |t|
      t.string :name
      t.string :genre
      t.integer :age
      t.string :hometown
    end
  end
end
```
* Each column will state the data type followed by its name as a symbol
* By default, create_table creates a primary key called id.

**Running migrations**

* Run ``` rake db:migrate ``` in terminal.
* Then extend the class associated with the table with ``` ActiveRecord::Base ```.
   * The table ``` artists ``` would have its own class and file called artist.rb where the ActiveRecord::Base will connect to the ``` artists ``` database

```
# artist.rb
 
class Artist < ActiveRecord::Base
end
```

**Using migrations to alter/update an existing table:**

A few types of alteration methods

```
* Add_column :table_name, :column_name, data type for the new column
* Remove_column :table_name, :column_name, data type for the new column
* change_column :table_name, :column_name, data type for the new column
```

A new migration file must be created to modify any existing tables then saved before running ``` rake db:migrate ``` again.

```
# db/migrate/02_add_favorite_food_to_artists.rb
 
class AddFavoriteFoodToArtists < ActiveRecord::Migration
  def change
    add_column :artists, :favorite_food, :string
  end
end
```

### **Active Record Querying Methods**

Finding information and datasets using Active Record can be done from a variety of methods. http://guides.rubyonrails.org/active_record_querying.html

Here are some.
```
.sum
.where(conditions, params)
.minimum
.maximum
.desc
.asc
```

![](http://media.istockphoto.com/photos/little-sketchy-man-searching-with-magnifier-picture-id531011479?k=6&m=531011479&s=612x612&w=0&h=QWX-MbYt_DVIkzM297JPVrvpPovTmxvWc1WhFhhN7cE=)

### **Active Record Associations**

Classes can be built to associate with one another but uses a lot of code to do it. Active Record associations does this job with less code. http://guides.rubyonrails.org/association_basics.html

Types of relationships between models:

belongs_to
has_one
has_many
has_many :through
has_one :through
Has_and_belongs_to_many

These relationships are created by using macros - belongs_to, has_many, etc…

Viewing the guides on rubyonrails would provide more details on this topic.

![](https://cdn.meme.am/instances/500x/76908715/dont-need-an-object-relational-mapping-if-you-have-no-relation.jpg)


## **Final Thoughts**

Active Record is a really great tool! I'm happy to have learned it. There's much more learning and practicing to be done but powerful the framework is when creating and finding data. I hope to keep up with this topic and build my skills from it.


