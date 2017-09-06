---
layout: post
title:  "Ruby Readings"
date:   2017-09-06 12:25:26 -0400
---


Procedural Ruby was a struggle but I did it! Woohoo!

Before I start with Object Orientation, I read some Ruby books/resources that were recommened to review what I've learned.

**RUBY MONSTAS**

I enjoyed [Ruby Monstas](http://ruby-for-beginners.rubymonstas.org/print.html) the most. It's tailored for beginners with easy to read information about the basics of Ruby. It doesn't cover Yield though, which is fine. 

**Learning to Program** means we must learn a new language and use that language to solve problems by writing code and describing the solution to the problem.

A lot of season developers or just the programming community can get caught up with new features of a language or things we beginners aren't fully aware or familiar with. I've come across this when looking for answers through Google, Stack Overflow, various sites, and even the documentation. It's one way to I learn I guess by exposing ourselves to how others think and code. Though it's more important to understand the basic concepts first so you can enjoy the learning process.

Three new things I learned from Ruby Monstas were: Escape Sequences, Defining strings with %, and Modules.

**Escape sequences**
Control characters are non-printable characters but have visual effects. Only works in double quote strings.

``` \n ``` newline character

```
puts "one\ntwo\nthree"

#output
one
two
three
```

**Defining strings with % **

% makes making changes to quotes easy.

``` %[any-character]The actual string[the same character]


"A String"
'A String'
%(A String)
%{A String}
%|A String|
```

```
message = %(The given email address "#{address}" does not look like a valid email address.)

message = %(The given email address does not look like a valid email address.)

message = %(The given email address doesn't look like a valid email address.)
```

An array of people without the quotes

```
people = %w(
  Anne
  Elizabeth
  Erica
  Iryna
  Johanna
  Juliane
  Katja
  Katrin
  Maria
  Renate
  Sureka
  Miriam
  Zazie
  Anja
)
people.shuffle.each_slice(2) do |pair|
  puts pair.join(', ')
end
```

w in ```%w(...)``` is "words." An array defined this way only contain strings.

```
%w(1 2 3 4) would result in the same array as ["1", "2", "3", "4"]
```

**Modules**

They make methods between classes sharable

```
  def cream?
    true
  end
end

class Cookie
  include Cream
end

cookie = Cookie.new
p cookie.cream?
```

**NAMING THINGS IN PROGRAMMING**

A computer langugage is no different than a language we speak read, and write with. It's all about communication. Though to be a great communicator you have to know the basics and become a better writer.

As mentioned in Peter Hilton's "[How to Name Things: The Hardest Problem in Programming](https://www.slideshare.net/pirhilton/how-to-name-things-the-hardest-problem-in-programming)", becoming a better writer and improving on our vocabulary can help us adopt better naming practices. Telling jokes is a great exercise. Jokes have double-meanings especially with puns. Be actionable, not vague. A thesarus will help out a lot.

**Other Resources**

There were a lot of resources I came across. I found [Codecademy](https://www.codecademy.com/articles) has articles on a variety of languages. Some of these articles have a short summary of glassaries for references. I'm still reading the [Poignant Guide book](http://poignant.guide/book/) on Ruby. I refer to these and the other resouces and readings when I get stuck on a lab. I found [Launch School](https://launchschool.com/books)'s open bookshelf highly informative. There are a variety of languages they offer free readings on.

I plan on doing some parrallel practice on Ruby through Codeacademy, [rubymonk](https://rubymonk.com/), and [Hackerrank](https://www.hackerrank.com). 

I'm sure I'll find more awesome resources along my journey with coding. These are just a few that have helped me so far and I hope it helps you. You can always visit libraries, too. They're like big brains scattered throughout the world wanting to share what they know.

![](http://i.imgur.com/T4iO2la.jpg)


