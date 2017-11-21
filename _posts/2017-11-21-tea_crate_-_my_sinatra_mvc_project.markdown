---
layout: post
title:      "Tea Crate - My Sinatra MVC Project"
date:       2017-11-21 20:50:09 +0000
permalink:  tea_crate_-_my_sinatra_mvc_project
---

My web app is called Tea Crate and it helps a user record their tea experience. The app allows a user to create a tea that they've tried. They can create the name, tick off checkmarks or create new inputs for the type of tea they consumed.

The goals I had for the app were:

* set up restful routes
* set up user slug
* create forms to allow checkboxes for Types
* update CSS code
* ensure signup/login/logout worked as they should
* ensure flash messages occured where they should be

![](https://img.memecdn.com/tea-time_fb_301593.jpg)

#### TYPES & CHECKBOXES

A good part of my project was figuring out the checkbox issues with my nested hash for Type. That was the hardest thing to solve. I could not find any strong solutions or advice online about this. I had multiple sessions with Techs and a few with the section lead who would help point me in the right direction. Eventually we took our last session as a study group session so other students can learn. It was a great opportunity and I certainly learned a whole lot about my process and how to use rake console and tux.

During this study group session, we determined one of the issues affecting my goal were the model associations in terms of the attributes. Earlier in my project, I went back and forth about the associations and relationships between classes. Does a tea have many types? Or does a tea only have one type? Do the other attributes fall under types or tea because of its association? After fumbling with my code and with help, it was determined that the other attributes fall under Tea itself while Type only has the type name attribute.

**Model Associations**

```
class User < ActiveRecord::Base
  has_many :teas

  has_secure_password

  def slug
    username.downcase.gsub(" ","-")
  end

  def self.find_by_slug(slug)
    User.all.find {|user| user.slug == slug}
  end
end

class Tea < ActiveRecord::Base
  belongs_to :user
  has_many :tea_types
  has_many :types, :through => :tea_types
end

class Type < ActiveRecord::Base
  has_many :teas, :through => :tea_types
  has_many :tea_types
  has_many :users, :through => :teas
end

class TeaType < ActiveRecord::Base
  belongs_to :tea
  belongs_to :type
end
```

With the updated schema, we had a better grasp of the class Types to ensure the checkbox functions were being created. In the beginning, I hardcoded the checkboxes to see the functionality of the actions. Hardcoding was a temporary fix. I was able to figure out some errors and setup routes that helped me get back on non-hardcoding paths.


Here's the create form with a user's entered info:

![](https://i.imgur.com/rbhDyxd.png)

So in terms of my class Type, it's an array within a nested hash. Below is an example of a newly created tea that's set up as params.

```
[1] pry(#<TeaController>)> params
=> {"tea"=>
  {"tea_name"=>"Organic 100% Double Green Matcha",
   "brand"=>"Republic of Tea",
   "origin"=>"China, Japan",
   "leaves"=>"Teabag",
   "caffeine"=>"Caffeine",
   "pairing"=>"None",
   "brew_time"=>
    ["1 teabag",
     "12 oz",
     "180 F",
     "3 minutes"],
   "tasting_notes"=>"smooth, grassy",
   "comments"=>
    "Fine 100% organic China green tea and organic stone-ground Japanese tencha leaves"},
 "type"=>
  {"type_ids"=>["2"],
   "type_name"=>"Matcha"}}
```

As you can see, the information a user enters is passed to through the form as params. Then, the params can be used throughout our code to build out the objects.

Upon creating the very first tea, we also create the first tea type by adding it's ``` type_name ``` into ``` @tea.types ```. @tea.types being the new tea's type according to how the model association was set up.

```
post '/users/teas' do
    if params.values.any? {|value| value == ""}
      flash[:message] = "Please enter all fields."
      redirect "/users/#{current_user.slug}/teas/new"
    else
      user = current_user
      @tea = current_user.teas.build(params[:tea])
      if !params[:type][:type_name].blank? && @tea.types.nil? #creating the first type by creating its params as type_name
        @tea.types.new(type_name: params[:type][:type_name])
      elsif !params[:type][:type_name].blank? && !@tea.types.nil? #selecting an existing type from the checkbox AND creating a new type (a tea that has more than one type)
        @tea.types << @tea.types.new(type_name: params[:type][:type_name])
        @tea.type_ids = params[:type][:type_ids]
      else params[:type][:type_name].blank? && !@tea.types.nil?
        @tea.type_ids = params[:type][:type_ids] #the type has already been created and is shown as checkboxes
      end
      if @tea.save
        flash[:message] = "New Tea added to crate!"
        redirect "/users/#{current_user.slug}/teas/#{@tea.id}"
      else
        redirect "/users/#{current_user.slug}/teas/new"
      end
    end
  end
```

Once the newly created tea is saved we are redirected to its show page. Here we see the new tea and all its attributes include the type of tea is belongs to.

![](https://i.imgur.com/jYF5TVc.png)

If a user found an error, from an incorrect input or that the tea actually has more than 1 tea type, then the option to edit the tea has a form. The type we created earlier and associated with the tea appears with a check marked on the box, showing us that this was the type originally entered. Here's where things get more complex.

In terms of our logic or how a user would interact with the form determined how we can edit the tea's type.

1. User creates the very first tea, by creating a new type's type_name
2. Upon creation of the new tea, the new type is created
3. Error is found
4. User needs to edit the tea type by either....
   * creating a new type by creating a new type name and deselecting the existing tea type
   * add a new type by creating a new type name and keeping the existing tea type
   * keep or change the existing tea type and DO NOT create a new type with its type_name

Whew! Lots to think about. This affects how the tea will be updated and what attributes will be updated (if they are changed). The most complex updates done in the edit form is the Type class and the checkboxes. Below is the ``` PATCH ``` code. You can see how I determined the separation of complexities. In my walkthrough video (link here) you can watch and hear how I went about solving this issue.

```
patch '/users/:slug/teas/:id' do
    if params.values.any? {|value| value == ""}
      flash[:message] = "Please enter all fields."
      redirect "/users/#{current_user.slug}/teas/#{@tea.id}/edit"
    else
      @user = current_user
      @tea = Tea.find(params[:id])
      @tea.update(params[:tea])
      if !params[:type][:type_name].blank? && @tea.types.nil? #creating the first type by creating its params as type_name
        @tea.types << @tea.types.new(type_name: params[:type][:type_name])
      elsif !params[:type][:type_name].blank? && !@tea.types.nil? #selecting an existing type from the checkbox AND creating a new type (a tea that has more than one type
        @tea.types << @tea.types.new(type_name: params[:type][:type_name])
        newly_added_tea = @tea.types.last.id.to_s
        c = params[:type][:type_ids]
        c << newly_added_tea
        @tea.type_ids = c
      else params[:type][:type_name].blank? && !@tea.types.nil?
        @tea.type_ids = params[:type][:type_ids] #the type has already been created and is shown as checkboxes
      end
      @tea.save
      flash[:message] = "Your Tea has been updated!"
      redirect "/users/#{current_user.slug}/teas/#{@tea.id}"
    end
  end
```

I found that ActiveRecord is very useful but at times difficult to grasp. Since I struggled so much on this subject, I spent a few hours refreshing myself through the [Ruby on Rails Guide - Active Record](http://guides.rubyonrails.org/active_record_basics.html). I've spent a lot of time on this project and during it I had to move forward with Rails as I didn't want to lose any time. Due to this, I had a much better understanding about Active Record, Sinatra, RESTful Routes, rake console, and the controller, model, view associations. There's always more to learn, but I found this refresher period time well spent.

Given that, once I determine how the edit form was to function, I determined that the post route should have similar complex logic. Here's the post route again for your viewing pleasure.

***if checked logic***

Difference between checkmarks and radios:
* checkmarks - can select many options
* radios - can only choose 1

```
  <span>Types:</span>
    <% Type.all.each do |type| %>
      <input type="checkbox" name="type[type_ids][]" value="<%=type.id%>" id="<%=type.id%>" <%= 'checked' if @tea.type_ids.include?(type.id) %>><%=type.type_name%></input>
    <%end%>
```

That's pretty much it for solving the Type, type's type_name, and checkboxes.

![](http://s2.quickmeme.com/img/0e/0eb56acc4305e1726ca06c307cfa518676b9668933a4c9be0ad94bbf2e61797d.jpg)

The remainder of the app was relatively straightforward. The app has your basic signup/login welcome page. Best to remember that when creating these types of access that the database for Users table must include ``` password_digest ``` and not simply "password". This helps when authenticating the user and their password. Also, create a User controller for these routes. It helps to separate specific routes. In my web app, I created a User controller for this reason. The main application controller contains the web page index and the routes for tea information since it doesn't require many control routes.

**Slug vs ID**

During the study group pairing session, a fellow student asked if creating a slug for a username is important. We all discussed that it depends on preference. Some found it was mostly for appearance sake, while others, like myself, find it easier to locate a user. If an app grows very large due to the amount of users, reading numbers as ids can be overwhelming. Then again, that's just my 2 cents.

My app has other fun pages just to make the app feel more fulfilled. I'm interested in building this out as a full functioning app. There's a whole bunch of tea out in the world and I want to record the ones I've tried. I guess you can say the app will serve an amateur tea-botanist (I just made that up) goal of tea collecting. My app is at the basic stages. From my experience, I believe Origins and Pairings will need to be on its own model and function similar to Type. Why? Well, teas have many origins and can be paired with many other items. I personally think most teas shouldn't need any milk, cream, or sugar however, a nice black tea always taste great with milk and cream. I'll add sugar if I feel like. I could go on and on about tea. I'll leave you now and hope you found this blog and my walkthrough helpful.

![](https://i.pinimg.com/originals/7a/d2/eb/7ad2eb6dbb1d9b0bc5e0cf024c5827cc.jpg)






