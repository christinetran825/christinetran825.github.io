---
layout: post
title:      "CLI Gem - SeedPicker"
date:       2017-10-11 18:07:20 +0000
permalink:  cli_gem_-_seedpicker
---


For my project I chose to work with Vegetable seeds. There are a bunch of types, categories, and varieties. I thought it’ll be fun. I chose rareseeds.com because I like their store in Petaluma, CA. I never forgot my experience there enjoying all the various seeds I didn’t know existed.

With the website chosen, I was ready to build the CLI.

After multiple study groups about this project, I realized that the entire CLI concept and the project was about relationships. In a nutshell, we’re building a non-visual user experience interface. The CLI provides information and asks a user for input to that information. When the user provides an input the CLI will fetch that input and provide more information in relationship.

So, with all that in mind, I had to go back to my design and marketing (specifically consumer behavior) background to answer questions about a user’s relationship to the interface (program).

Before I dive into the relationships, I want to note a couple of important things I’ve learned from working on this project for the last 3 weeks.

1. When choosing a website to scrape for a project like this, make sure that the design (HTML) is consistent throughout associated pages using Inspect of the developer tools.

   * Why? After working tirelessly on scraping rareseeds.com, I had a problem scraping a couple of the seeds. They seemed like they were in the same structure but upon inspection, they were off. There were other seeds that had an entirely different set up, too. Those had to be applied and scraped much more differently. More on this issue later.
 
2. Type or write out how you would like the CLI to look like. No coding; just simple text. This gives you an idea of how things should be set up. It also helps you work out the relationships between objects and the user’s relationship with the CLI.
 
3. Take a lot of breaks in between coding sessions. There were times when I was testing many things that eventually I lost track of what I was trying to solve. Other times, I had placed a method in a certain place and the code didn’t work. Then, a day after, I added that method to the same spot to see what happens and the code worked… weird….
 
4. Ask for help - study groups, slack, instructors, google, books, etc...

![](http://ragefaces.memesoftware.com/faces/svg/misc-jackie-chan.svg)


### **CREATING RELATIONSHIPS**

First, think about the 5Ws and the unique H - WHO, WHAT, WHEN, WHERE, WHY, & HOW

In general the WHO is the USER. In terms of the USER’s relationship with the CLI we can apply the remaining Ws and How.

Before I dive into that relationship, let me mention that overall, there are multiple relationships going on with our entire program:
* User & Interface (program)
* Interface & Scraped site’s structure
* GEMs relationship with its dependencies and files.
* GEM files relationship with GitHub and the repository


#### **Building GEM relationships**

First, we create our GEM. We verify the files to confirm that all files and their codes are accounted for. Then we initialize it via git to our GitHub and newly created repository.

Second, we create our executable file and CLI file. From these files and the existing files created during our GEM creation, we load our dependencies to ensure that the files are reading each other.

It took me a couple of days to get started with this project. I ran into a lot of trouble with bundling the gem, connecting it to my GitHub repo, and fumbling with technical stuff with Learn IDE. In the beginning I was lost on how to bundle my new gem and copy the spec.md file to my new added GitHub repo. I referenced help on Slack and the Git & GitHub lessons and was able to get pass this hurdle. Boy, was that aggravating.

![](http://fredericiana.com/media/wp/2012/05/rubygems.jpg)


### **CLI & User Relationship**

The CLi and User communicates with each other. CLI usually asks User something - a question, a request, or a prompt. In some cases, options are provided. The User must respond with an answer by entering a response to those questions. One can’t do anything without the other. That’s why the program needs to operate on the CLI and User relationship before we code, since what we’re coding would need to build on other types of relationships (more on that later).

Here’s how I wanted my program to operate:

1. CLI welcomes User
1. CLI shows User a list of vegetable seeds
1. CLI asks User to choose a seed by entering a number that corresponds to that seed
1. User selects a seed by entering its number
1. CLI shows User that seed’s information - the varieties and its description
1. CLI asks User to choose a variety for more details by entering a number that corresponds to that variety
1. User selects a variety by entering its number
1. CLI shows User that variety’s information - price and the variety’s description
1. CLI asks User if they want to choose another seed or leave the program by entering list or done, respectively
1. User either enters list to view another seed or enters done to leave the program
   * IF User enters list, CLI takes User back to step 2-10
   * IF User enters done, CLI shows a goodbye message
1. Program ends

As I mentioned before, certain seeds had a different design structure. This meant my CLI had to operate a bit differently for these seeds. Here’s how the CLI operates when those **category** seeds are chosen.

1. CLI welcomes User
1. CLI shows User a list of vegetable seeds
1. CLI asks User to choose a seed by entering a number that corresponds to that seed
1. User selects a seed by entering its number
1. CLI shows User that seed’s information - the **category** of varieties and the **category’s** description
1. CLI asks User to choose the **category** of varieties for more details by entering a number that corresponds to that **category** of variety
1. User selects a **category** of variety by entering its number
1. CLI shows User the **category** of variety’s information - the varieties and that **category’s** seed’s description 
1. CLI asks User to choose a variety from the **category**
1. User selects the variety by entering its number
1. CLI shows User the variety’s information - price and the variety’s description
1. CLI asks User if they want to choose another seed or leave the program by entering list or done, respectively
1. User either enters list to view another seed or enters done to leave the program
   * IF User enters list, CLI takes User back to step 2-12
   * IF User enters done, CLI shows a goodbye message
1. Program ends

Now that we have the operation down, we** DRAFT THE CLI TEMPLATE - how the CLI should look**

*CLI - as seen when choosing majority of seeds*

```

Welcome to Baker Creek Heirloom Seeds RareSeeds

Here's our collection of vegetable seeds.

------------- Vegetable Seeds -------------
1. Amaranth
2. Artichoke & Cardoon
3. Asparagus
4. Beans
5. Beetroot
6. Bok Choy
7. Broccoli
8. Brussels Sprouts
9. Cabbage
10. Carrots
11. Cauliflower
12. Celery & Celeriac
more seeds...


^ - ^ Please choose a seed by its number.

    #user input => 1

----- Group: A - Amaranth -----

 Varieties:
 1. Aurelia's Verde Amaranth
 2. Dreadlocks Amaranth
 3. Elena's Rojo Amaranth
 4. Elephant Head Amaranth
 more varieties...

 Description:
 (Amaranthus sp.) Direct... #continues description of parent seed.

^ - ^ Please choose a variety by its number.

    #user input => 1

-------- Group A: Amaranth - Aurelia's Verde Amaranth --------

 Price: $2.50

 Description:
 Native amaranth from Guatemala... #more description of variety seed.

^ - ^ Type list if you want to choose another seed. Type done if you are done.

    #user input => done

^ - ^ Goodbye!!!"

          ( Thank you for viewing our vegetable seed collection. Seed you again, soon!!! )
            ,
    (\ /)  ,
    ( . .)
    c(")(")

```

*CLI - as seen when choosing either Gourds, Melons, Peppers, Squash, or Tomatoes*

```

Welcome to Baker Creek Heirloom Seeds RareSeeds

Here's our collection of vegetable seeds.

------------- Vegetable Seeds -------------
...
21. Fennel
22. Garden Berries
23. Garlic
24. Gourds
25. Grains and Cover Crops
26. Greens
27. Greens, Oriental
28. Ground Cherries
...

^ - ^ Please choose a seed by its number.

    #user input => 24

----- Group: G - Gourds -----

Varieties:
1. Gourds
2. Edible Gourds
3. Luffa Gourds

Description:
Gourds can be ….

   ~ ~ ~ Please choose the category of varieties ~ ~ ~
  
   #user input => 1

   -------- Group G: Gourds - Gourds --------
   Gourds - Varieties:
   
   1. Accocha or Lady’s Slipper Gourd,  Cyclanthera pedata
   2. Big Apple Gourd
   3. Birdhouse Gourd
   .... more seeds
   17. Zucca Gourd

   Category Description:
   (Lagenaria siceraria unless stated otherwise).....

   ~ ~ ~ Please choose the category's varieties ~ ~ ~

   #user input => 1
	 
   -------- Group G: Gourds - Gourds - Accocha or Lady’s Slipper Gourd,  Cyclanthera pedata:  --------
   
	 Price: $5.00

   Variety Description:
   This vegetable related to cucumbers....

^ - ^ Type list if you want to choose another seed. Type done if you are done.

    #user input => done

^ - ^ Goodbye!!!"

          ( Thank you for viewing our vegetable seed collection. Seed you again, soon!!! )
            ,
    (\ /)  ,
    ( . .)
    c(")(")

```


#### **Building Object Relationships**

![](https://i.imgflip.com/1kxbxp.jpg)

Trying to determine objects that the CLI uses in order to create the program was the hardest part. Computers/programs exist to provide functions for Users to interact with. To create those functions, we must provide the computers/programs data to work with. These datas are in a form called objects.

Here’s how I determined objects for my CLI:

CLI’s goal is to show a list of vegetable seeds. The CLI’s object is a list of vegetable seeds, but each vegetable is an object of that list. So the CLi’s main object is one individual instance of a vegetable seed.

Q: What is the object?
A: An instance of a vegetable seed

Q: What is a vegetable seed?
A: An object that has…
   * a Name
   * a Description
   * a URL to its varieties of seeds
   * many varieties of seeds

Q: What is a variety seed?
A: An object that has…
   * a Name
   * a Description
   * a Price

Q: What is a category of seeds?
A: An object that has…
   * a Description
   * a URL to its variety of seeds

Q: What is a category of seeds’ variety seed?
A: An object that has…
   * a Name
   * a Price
   * a Description

Upon determining these individual instances and their attributes, we can apply them to our CLI as a class instance

Q: What is a vegetable seed? *=> object (class: Seeds)*
A: An object that has…* => attributes*
   * a Name *=> @parentseedname = parentseedname*
   * a Description *=> @parentseeddescription = parentseeddescription*
   * a URL to its varieties of seeds *=> @parentseedurl = parentseedurl*
   * many varieties of seeds

Q: What is a variety seed?* => object  (class: Varieties)*
A: An object that has… *=> attributes*
   * a Name *=> @varietyseedname = varietyseedname*
   * a Description *=> @varietyseeddescription = varietyseeddescription*
   * a URL to its own page *=> @varietyseedurl = varietyseedurl*
   * a Price *=> @price = price*

Q: What is a category of seeds? *=> object (class: Category)*
A: An object that has… *=> attributes*
   * a Description *=> @categorydescription = categorydescription*
   * a URL to its variety of seeds *=> @categoryurl = categoryurl*

Q: What is a category of seeds’ variety seed? *=> object (class: Category*)
A: An object that has… *=> attributes*
   * a Name *=> @categoryvarietiesname = categoryvarietiesname*
   * a Price *=> @categoryvarietiesprice = categoryvarietiesprice*
   * a Description *=> @categoryvarietiesdescription = categoryvarietiesdescription*
   * a URL to its own page *=> @categoryvarietiesurl = categoryvarietiesurl*

As you can see, the classes of Seeds, Varieties, and Category are connected with one another to determine the objects and its attributes. And each of these objects depend on the (class: SCRAPER). This class helps us scrape the attributes from the rareseeds website that defines and connects the attributes to the objects.


#### **Scraping the websites**

For this CLI, I had to scrape 5 different web pages. Like I said, make sure the website isn’t too complex unless you want a challenge. I didn’t expect this issue, so I neither wanted a challenge nor did I expect one. :p

1.) Main vegetable seed page
   * parent seed name 
   * parent seed url => we need this url so we can scrape the Parent seed page

2.) Parent seed page
   * parent seed description
   * variety seed name
   * variety seed url => so we can scrape the Variety seed page
   * price

3.) Variety seed page 
   * variety seed description
   * category seed url => we need this url so we can scrape the Category seed page

4.) Category seed page
   * category varieties name 
   * category varieties url => we need this url so we can scrape the Category varieties seed page

5.) Category varieties seed page
   * category varieties description

Without (class: SCRAPER) we can’t build the CLI.


**Scraping the sites - using binding pry, Nokogiri, and open-uri**

Main vegetable seed page => "https://www.rareseeds.com/store/vegetables/"
  
doc = Nokogiri::HTML(open("https://www.rareseeds.com/store/vegetables/"))
doc.css(".sitebody .grid_3 .lnavwrpr ul.lnav li a").first.attr('href') => "/store/vegetables/amaranth/"
doc.css(".sitebody .grid_3 .lnavwrpr ul.lnav li a").first.text => "Amaranth"

*Applying the scraped info to a Scraper class method:*

def self.scrape_parent_seeds #scraping the parent seed for the url and parent_seed_name
    doc = Nokogiri::HTML(open("https://www.rareseeds.com/store/vegetables/"))
    doc.css(".sitebody ul.lnav li a").collect do |the_seeds|
      seed = SeedPicker::Seeds.new ##!!!!! CALLING a new instance OBJECT !!!!!!!!
      seed.parent_seed_url = the_seeds.attribute("href").value
      seed.parent_seed_name = the_seeds.text
      seed
    end
  end

*NOTE: THE LINKS FOR THE URL TO BE SCRAPED MUST NOT BE A FULL LINK  => "/store/vegetables/amaranth/" *

You can view my walkthrough video for more details about the Scraper class and the process. It’s very complex.


#### **THE CLI**

Our collected data can now be written on our (class: CLI). As the User selects a seed that seed is passed through the entire program for the CLI. Since that seed is being passed, we include as a parameter on certain methods.

You can view my walkthrough video for more details about the CLI class.


## **FINAL THOUGHTS**

PHEW………

This was a monolithic project. It was a large learning process, too. I struggled in the beginning because it was tough to get a hang of object relationships, despite my having finished all the labs. I guess I had to build my own CLI to really see the entire object relationships and program operations. I’m so glad there was help when I needed it. Spending 3 weeks on this was tough. I didn’t think I could finish or understand what I was doing.

I’m nervous about the upcoming assessment and refactoring of my code. Hopefully it won’t take me long to do it.

I won't be publishing this gem because of the website's design issue. At least I think it's an issue. If they do change the formatting, my gem (if published) will not work the same.

In the end, I enjoyed learning about Ruby and how to apply it when building CLI. It really is a neat and nice language.

![](https://i.pinimg.com/236x/e8/11/ff/e811ff8d47e1b7f3cea6e71982d01c93--loony-tunes-memorial-cards.jpg)



