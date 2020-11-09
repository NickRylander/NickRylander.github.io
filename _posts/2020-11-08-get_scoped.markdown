---
layout: post
title:      "Get Scoped"
date:       2020-11-08 23:53:19 -0500
permalink:  get_scoped
---


Scope methods are incredible in Rails, and keeps your code DRY (Don’t Repeat Yourself). It uses queries so that you are able to chain into more complex queries. In my Rails Project for Flatiron I ran into some difficulty creating my first scope method. My Rails application was a vinyl collection app, making it so that a user could manage his or her vinyl collections! One of the requirements for the project was to define a scope method somewhere in the app. I chose to make mine for finding the song with the longest duration.

Creating scope methods can be done in multiple different ways. That being said, you must define your scope in the model in which you are trying to query the data. In my case, this means I would have to write it in my `Song.rb` model. 

```class Song < Application Record
	# scope method goes here
End```

You could make the scope using the standard form of method building:

```def self.scope_name
	# query goes here
End```

Or you could make it using a little more syntactical sugar:

`scope :scope_name, -> { #query goes here }`

The method about is a little cleaner and takes up less lines of code (a very good thing). I do want to note that it would be best to put that method below your class name, not just thrown somewhere in the middle of you model (that should be very empty to keep things nice).

With all of that in mind, how on earth do I query to find out which song on that vinyl has the longest duration?! My duration is stored as an integer in my database. So I will need to figure out a way to take that integer, compare it to all of the others stored in that database, and return that title of that song.

After almost an hour of trying different ways to get the perfect query to work, I finally figured it out!

`scope :longest_song, -> { order(“duration desc”) }`

This is calling the scope method I aptly name `longest_song` and set the query to order the songs in the vinyl in descending order (from longest duration or ‘highest integer’ to shortest duration or ‘lowest integer’). After I produced that query, since I wanted the user to be able to view the “Longest Song” on the index page, I set an instance variable in the `SongsController` and then called that instance variable in my `index.html.erb` view for songs, and made sure to ask for only the first instance. I will show you below:

Controller: 

``` class SongsController < Application Controller

	def index
		@long_song = Song.longest_song
	end

end ```

View:

`<p>Longest Song: <%= @long_song.first.title %></p>`

So, if you navigate the to the songs index page, you will see

`Longest Song: #title of whichever song is the longest`

In conclusion, scopes can be finicky, and honestly I really had to rack my brain to remember how SQL queries are constructed. At the end of that day, this is a really fun way to give the user some pretty cool data!
