---
layout: post
title:      "Understanding Arguments"
date:       2020-09-11 23:02:37 +0000
permalink:  understanding_arguments
---


If you know anything about me, you know fitness has been a huge part of my life. As you can imagine, when I was asked to create a CLI scraping or API project based on “something I could fill a room talking about”, naturally I chose something that had to do with fitness. I was a CrossFit coach for a few years, so why not create something that scraped the CrossFit website to view any workout that I wanted to in 2020? I embarked on this task having minimal scraping (albeit coding) experience, but I sure learned a lot. Let me walk you through a big hurdle I had to overcome in how objects are created and how we can extract information from them.

The problem; I was trying to scrape a webpage in which I had to manually manipulate the URL to get the data that I wanted. Although we had spent the last week learning about Object-Oriented Ruby, I still seem have a hard time with the flow, object creation, and calling of their attributes. I built an entire project that passed all of the tests I was hoping for, but was only able to succeed with the help of others, as I didn’t fully grasp what I was coding. So, what did I do? I started from scratch. The goals were the same, but this time I wanted to not only refactor my syntactical vinegar, but gain a better understanding of what my code was actually doing. Here is the scraper class I used to get the information necessary:

```
class CFWorkouts::Scraper 
    
    MONTH_URL = "https://www.crossfit.com/workout/2020/"
    DAY_URL = "https://www.crossfit.com/"

    def self.scrape_months
        doc = Nokogiri::HTML(open(MONTH_URL))
        doc.css("select#monthFilter.form-control.input-sm").text.strip.split("\n                ").slice(1, 13).each do |months|
            month_number, month_name = months.split(" - ")
            CFWorkouts::Month.new(month_name, month_number)

        end
    end

    def self.scrape_days(days)
        doc = Nokogiri::HTML(open(MONTH_URL+days.month_number))
        doc.css("section#archives.section").css(".show a").each do |day|
            name, date = day.text.split
            CFWorkouts::Day.new(name, date)
        end
    end

    def self.scrape_workouts(workout)
        doc = Nokogiri::HTML(open(DAY_URL+workout.date))
        workout.details = doc.css("div._6zX5t4v71r1EQ1b1O0nO2.jYZW249J9cFebTPrzuIl0").text
    end

end
```

This class worked perfectly for my intended use. The issue I was having was the misunderstanding of the arguments `days` and `workout`. At first, I had help to build this class, but I had no idea what I was actually passing through the method. I thought the argument was arbitrary, but how would that allow me to call the dot notation methods `days.month_number` and `workout.date`. I thought that since I had already defined these variables in my `Month` and `Day` class, as well as created new objects within this class by calling the `.new` method, that I would have no problem assigning the arguments. That is where I was all wrong. 

I had to take a step back and ask myself, “What are these methods in need of other than the creation of new objects?” It took quite a while to realize that without the specific scraped data and object creation from the users input within my `CLI` class, that these methods would have no clue what to pass through these methods. Let’s take a look at my `CLI` class methods: 

```
def month_input
		print "\nPick a number to see all the days: "
		input = gets.chomp.to_i
		day = CFWorkouts::Month.all[input-1]
		case input
		when 1..CFWorkouts::Month.all.length
				puts "\nNow, which day would you like to see?"
				CFWorkouts::Day.reset
				list_days(day)
				day_input
		else
				puts "Sorry, what was that?"
				month_input
		end
end

def day_input
		print "\nPick a number to see the workout: "
		input = gets.chomp.to_i
		workout = CFWorkouts::Day.all.reverse[input-1]
		case input
		when 1..CFWorkouts::Day.all.length
				workout_title
				puts "--------------------"
				workout_details(workout)
				puts "--------------------"
				options_menu
		else
				puts "Sorry, what was that?"
				day_input
		end
end

```



At what point within these methods does each variable get specified to be able to use them as arguments? THE INPUT! I needed to set the user input to a variable so that the new object that is created can call on the variable as an argument in my `Scraper` class! This talking back and forth between classes using newly defined objects was a major breakthrough in my understanding of how Object Orientation really works. I have a tendency to believe all arguments are arbitrary, but that simply isn’t the case. While I can name them whatever I want, their significance is much more than just something to be passed through a method. I am only 3 weeks into my software engineering bootcamp, but I have a feeling that understanding Object Orientation is something I will need for my entire programming career, and I have made it my mission to get a deeper understanding with every minute spent coding.

