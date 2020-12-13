---
layout: post
title:      "Forming Forms in JS"
date:       2020-12-13 11:30:03 -0500
permalink:  forming_forms_in_js
---

This learning block was dedicated towards learning JavaScript for the first time. To say that I was struggling would be a bit of an understatement, but after finishing my project, with the help of my cohort lead and some other students, I have a much better grasp on it all! My project Idea was simple enough, make an application using JavaScript that can track your workouts that you do or want to do. It would do this by having a form on the page to create your workout and the movements, and you would be able to submit to the database, which would be in the back end as an API. Using the stored data in the API, you would then be able to see your workouts with all the associated movements. Here is where things got interesting. 

I first generated the form for my workout in the HTML of my webpage, the workout attributes and associated movements. This was going to end up being difficult, as workouts don’t all have the same amount of movements. My first thought, that I spent way too much time on, was to create an ‘Add Movement’ button that duplicated the section of the form as many times as needed. Sounds neat, right? It turns out, I was having a ridiculously hard time targeting those new forms data. So all the extra forms just went to waste. 

Here comes the solution to that problem! Have a form at the top of the page, that is only for the workout attributes (name, number, goal, rounds), and submit that form as a POST fetch request to the database. I have already written the code to create and append a div card at the bottom of the window to show the workout you created. So when that card is created, I then decided that I would out the form for the movements within that card itself, and therefore able to always associate the new movement to the workout it needed to! Below is the JS/HTML that was for that new form within the workout card:


```
workoutHTML(){
        let completed = this.completed == true ? "checked" : ""
        return `
        <h3 class="title">${this.workout_name} - Workout #${this.workout_number}</h3>
        <p>Workout Goal: ${this.goal}</p>
        <p>Rounds: ${this.rounds}</p>
        <p>Workout Completed: <input data-id="${this.id}" class="toggle" type="checkbox" value="completed" ${completed}</p><br><br>
        <button class="delete">Delete Workout</button><br><br>
        <div id="moveForm">
            <form class="movementForm${this.id}">
                <label for="movement-name">Movement Name:</label>
                    <input type="text" name="movementName">
                <br>
                    <label for="reps">Reps:</label>
                    <input type="text" name="reps">
                <br>
                    <label for="weight">Weight:</label>
                    <input type="text" name="weight">
                <br>
                    <input type="hidden" name=${this.id}/>
                <br><br>
                <input type="submit" value="Add Movement"></input><br><br>
            </form>
        </div>
            `
    }
```


The fact that I only have the one form now associated with the correct workout, I have no need to make duplicates. The form is submitted, and then immediately cleared after the submit button is clicked, making you able to add as many movements as you need with the single form! I then have an event listener on the card itself for a click, so that when you click the card you are able to see all the movements associated with the workout. I want to actually get rid of that at some point, as it seems the event is more trouble than it’s worth, but I also happy with the functionality I was a able to produce from this workaround!
