---
layout: post
title:      "Artphilia - My Rails Project"
date:       2017-12-23 19:37:15 +0000
permalink:  artphilia_-_my_rails_project
---


What can I say, Rails was fun. It was challenging and still is.

My Rails project is called Artphilia. Artphilia is an webapp that allows a user to keep a record of non-artists (illustrators, painters, etc.) and their artworks. A user can note if they own the artwork or not. If they do own the artwork, they can say if its signed and if it's an original or print piece. I really like this idea and hope to create another called Bibliophilia for book authors/illustrators. Lots of ideas brewing.... :]

With this app, I didn't use Devise. I personally dislike Devise. I tried it at first with the app thinking it'll be something I can learn about. Unfortunately, it was difficult. So I chose to move forward with setting up the Omniauth and app on my own terms.

### Set up

The first step I did was add the **gems** I knew I would use.

```
gem 'pry'

# social media login
gem 'omniauth'
gem 'omniauth-facebook'

# hides api keys/secrets
gem 'dotenv-rails'

#boostrap
# gem 'bootstrap-sass', '~> 3.2.0'
gem 'bootstrap'
gem 'autoprefixer-rails'
```

I added the bootstrap gems so I can set up my css style. However, I begin my project with the functionality then deal with the css. By making sure the majority of the functionality and logic works I can set up the css style to determine how and where I want to put my partials.

**Omniauth**

I followed the lesson/lab on Omniauth to add the facebook authorization. It was still a struggle when setting up the sessions controller.

```
added the omniauth file to initializers

config < initializers < omniauth.rb

Rails.application.config.middleware.use OmniAuth::Builder do
  provider :facebook, ENV['FACEBOOK_KEY'], ENV['FACEBOOK_SECRET']
end

I set up my app key and secret via facebook's developer page and added that to my .env file

Artphilia < .env

FACEBOOK KEY = ****
FACEBOOK SECRET = ****

Artphilia < .gitignore

I added the .env at the very bottom of the file
```

###  Database & Associations/Models

Thinking about setting up my models were relatively easy except when it came to medium/media. Medium/Media is what the artwork is made of - ink, oil, acrylic, mix, etc... Apparantly, medium is singular while Media is plural. This was problematic for me because I always thought the plural of medium was mediums. Rails doesn't agree. haha.... Here's a description about [media medium](https://www.thoughtco.com/media-medium-and-mediums-1689581).

After jumping that hurdle, I was able to move forward with **associations**. However, it proved another issue. I debated between does ```an Artwork have many Media and a Medium has many Artworks``` OR ```A Medium has many Artworks and an Artwork belongs to a Medium``` I decided to go with the has many to many associations. So the associations between the models would be as follows:

```
a User has_many Artists

an Artist belongs_to a User
an Artist has_many Artworks, :dependent => :destroy
an Artist has_many Media, through Artworks

an Artwork belongs_to an Artist
an Artwork has_many ArtworkMedia
an Artwork has_many Media, through ArtworkMedia

a Medium has_many ArtworkMedia
a Medium has_many Artwork, through ArtworkMedia

ArtworkMedium
belongs_to Artwork
belongs_to Medium
```

I've set up my **database** by the following.

```
rails g resource User

    t.string "name"
    t.string "email"
    t.string "password_digest"
    t.string "uid"    #for omniauth

rails g resource Artist

    t.string "name"
    t.string "website"
    t.string "discovered"
    t.integer "rating"
    t.string "notes"

rails g resource Artwork

    t.string "title"
    t.string "exhibition"
    t.string "user_owned"
    t.string "signed"
    t.string "original"
    t.integer "rating"
    t.string "comments"
		t.integer "artist_id"

rails g migration add_artist_to_artwork  artist:references  #=> this adds the artist id to artwork
		 
rails g resource Medium

    t.string "name"

rails g resource ArtworkMedia

    t.integer "artwork_id"
    t.integer "medium_id"

```

### Controllers

I setup the application controller with helper methods.

```
helper_method :current_user, :logged_in?, :log_user_in #let these methods be used in views

#check if user logged in
	
def logged_in?
  !!current_user
end

#if user is current user
	
def current_user #returns current logged in user (if any)
  User.find_by(id: session[:user_id])
end

def log_user_in
  session[:user_id] = @user.id
end
```

With the helper method set up, I made sure to set up the user's omniauth in the User model so I can include it in the Sessions Controller # create.

```
#find or create by oauth user_id

  def self.set_user_from_omniauth(uid)
    #user logged in via OAuth
    first_or_create(uid: uid) do |u|
      u.name = auth['info']['name']
      u.email = auth['info']['email']
      #setting a random secure password if a new user
      u.password ||= SecureRandom.base58
    end
  end
```

### Validations

Validations were pretty simple. I did struggle with the website and email regex though.

For example, when I was setting up the medium/media checkbox for the form, I had to make sure that the attribute setter doesn't include any blanks, despite the validation of presence :true was included for :name.

```
def media_attributes=(medium_attributes)
    medium_attributes.values.each do |medium_attr|
      if !medium_attr[:name].blank?
        medium = Medium.find_or_create_by(medium_attr)
          self.media << medium
      end
    end
  end
```

For an artist's website, I want to make sure that if an artist doesn't have a website then the user can leave it blank. The below before_action method of make_website would handle the blank entry as entering the website as "None".

```
def make_website
    if self.website != VALID_WEBSITE_REGEX && self.website.blank?
      self.website = "None"
    elsif self.website != VALID_WEBSITE_REGEX && !self.website.blank?
      self.website = "http://#{website}"
    else
      self.website
    end
  end
```

###  Views / Layouts & Partials

*Static*

I've created a static Welcome Index & Homepage.

In the welcome controller, I included 2 actions - index and home. Index is the static page where  a user can log in via Facebook, create a new account, and log in via a normal setup. Home is the homepage where when a user is logged in, they see 3 options of seeing all their artists, a 2 other links that are coming soon.

I included a background image for the index page for differentiation purposes. However, I chose to forgo that for the time being, perhaps for the next project with Rails + Javascript.


*Partials*

I created a variety of partials for headers, form_errors, and flash messages. The form_errors and flash messages partial were placed into the Shared folder since they would be used throughout a variety of other views. The header partial was placed within the Layout folder so it can be used within the application view.

For my Artist#show view I decided to include the template partial of artworks/index because I wanted to allow a user to see an Artist's show page that also includes their list of Artworks. The user can still view the Artist's Artworks on its own Artworks#show page.

On the Artworks#show page I added a star image as placeholder so that a user can see how it will look if an artwork image can be uploaded.

###  Routes

Since artworks belong to an artist, I wanted to nest the routes like so:

```
resources :artists do
  resources :artworks
end

the link shows up as: artist/1/artworks/2
```

With this setup, I had to make sure that I had a before action in Artworks controller to include finding the parent or in this case find the artist.

```
def find_the_artist
  @artist = Artist.find(params[:artist_id]) #find the parent
end

def set_artwork
  @artwork = Artwork.find(params[:id])
end
```


### Summary

In the end of my project, I struggled most with associations It's difficult because there can by many scenarios. I can understand paths, views, and enough of partials. It was tough determine how an association can be set up because then it determines how you set up the object's id in a table. Without that proper set up the app can go crazy.

That is all with my project, I hope... 

![](http://i.imgur.com/fxwWcqg.jpg)



