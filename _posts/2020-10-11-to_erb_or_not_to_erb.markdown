---
layout: post
title:      "To ERB or not to ERB?"
date:       2020-10-11 11:59:32 -0400
permalink:  to_erb_or_not_to_erb
---


Of course, that is rhetorical. Of course we ERB! Today I want to take the time to chat a little bit about how I got to play around with my `.erb` files while building my first ever Sinatra Application. We had been taught in pervious lessons here at Flatiron School how to create forms within our `.erb` files, but what we had learned was a little too basic for my application (pun intended). 

My app was designed to keep track of all the disc golf discs that you own. What I found interesting to learn what how to manipulate the form drop down menus. It was no problem creating a drop down menu:

``` 
<label for="speed">Speed:</label>
        <select name="speed" id=“speed">
            <option value=“1">1</option>
            <option value="2">2</option>
						<option value="3">3</option>
            <option value="4">4</option>
        </select>
```

Here we have the variable I am looking to set (speed) and I have multiple options in my menu. What I wanted, instead of just having a blank menu with no name, was prompting the user to actually pick a number, to ensure they don’t forget. Since I was validating the data given by the user, they MUST select from this menu. To do this, you have to have an `<option>` tag above the first value, set to nothing (`””`) and make the text unable to be selected. Let me show you what I am talking about:

```
<option value="" selected disabled>Choose a Number</option>
```

As you can see from the text above, the value is nothing and the selection (“Choose a Number”) is disabled. This makes the menu look much cleaner, and gives the user a better experience. Let's put it all together! 

```
    <label for="speed">Speed:</label>
        <select name="speed" id="speed">
            <option value="" selected disabled>Choose a Number</option>
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
            <option value="4">4</option>
				</select>
```

Now you have a beautiful drop down menu that, hopefully, won't be ignored by your user! Happy coding!
