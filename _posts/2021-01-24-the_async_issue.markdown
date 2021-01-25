---
layout: post
title:      "The Async Issue"
date:       2021-01-25 01:07:12 +0000
permalink:  the_async_issue
---


Over the course of my time here at Flatiron school I have learned so much! Starting with the fundamentals of basic Ruby and vanilla JavaScript, and ending with React and Redux. It has been a wild ride, full of cramming and crying at times! Today I want to focus specifically on one of the issues I ran into developing my final project (of which we used all of our combined knowledge).

The issue that I was dealing with was the fact that my page would render so fast that the data I wanted to render on the page wasn’t able to display! Let’s take a step back from the big picture and dig into the details. The application I created was a weather application, where a user can type in the name of their city and then be able to see the forecast for said city. To do this, in my code, I needed to fetch the data from an external API source that had the data that I needed for all the cities and their forecasts. 

In my `actions` folder I have a component specifically for all of my fetch requests (getting the data, persisting that city into my backend database, etc). With the fetch request that I am using, of course I am also dispatching the information gathered and saving what I want into my state.

Fetch request:

```
export const fetchWeather = (name) => {
    return(dispatch) => {
        return fetch(`https://community-open-weather-map.p.rapidapi.com/weather?q=${name}`, {
            method: "GET",
            headers: {
                "x-rapidapi-key": API_KEY,
                "x-rapidapi-host": "community-open-weather-map.p.rapidapi.com"
            }
        })
        .then(response => response.json())
        .then(data => {
            dispatch({type: "SET_CITY", payload: data})
        })
        .catch(err => {
            console.error(err);
        })
    }
}
```

Action:

```
case "SET_CITY":
            return {
                ...state,
                name: action.payload.name,
                temp: action.payload.main.temp,
                temp_max: action.payload.main.temp_max,
                temp_min: action.payload.main.temp_min,
                desc: action.payload.weather[0].description,
                icon: action.payload.weather[0].icon,
                main: action.payload.weather[0].main
            }
```

Now that is all well and good, and if you didn’t know, fetch requests like this run asynchronously. Meaning it doesn’t run in line with the event loop. It does this because it takes time for the data to be requested and returned. The problem I was then running into was that my page would be rendered, and while rendering tried to display data that hasn’t come back yet from my fetch request! 

My `render()` method started out with all the JSX written directly into it. Because of this, it would error out, saying that it can’t display something with a value of `null`. What do I do here?! Callback functions to the rescue! 

Instead of putting all of my beautiful JSX directly into the `render()` method, I define a function within the same class component first, and then call this function within my `render()` method: 

```
renderWeather() {
    if(this.props.city.icon == null){
      return <Loading />
    }else{

      const img = `http://openweathermap.org/img/wn/${this.props.city.icon}@2x.png`

      return(
        <div className="forecast">
          <h3 style={{textShadow: "2px 2px #1565c0"}}>{this.props.city.name}</h3>

          <img src={img}></img>

          <h4><u>Temperature</u></h4>
            <p>{Math.round((this.props.city.temp-273.15)* 9/5 + 32)}℉</p>

          <h4><u>High</u></h4>
            <p>{Math.round((this.props.city.temp_max-273.15)* 9/5 + 32)}℉</p>

          <h4><u>Low</u></h4>
            <p>{Math.round((this.props.city.temp_min-273.15)* 9/5 + 32)}℉</p>

          <h4><u>Description</u></h4>
            <p>{this.props.city.desc}</p>

          <input 
          onClick={
            () => {this.props.addFavorite(this.props.city.name)
            this.props.history.push('/favorite')}
          }
          type="submit"
          className="btn red lighten-1"
          value="Save City"/>
          <br/><br/>
        </div>
      )
    }
  }

  render() {
    return (
      <>
      {this.renderWeather()}
      </>
    );
  }
}
```

Due to the order of events when rendering your work in the application, and the fact that callback functions also are asynchronous, everything now works! If you are having trouble with your data returning `null`, but are confident that your code is written correctly, it might be beneficial to try and remember when things are running synchronously or asynchronously and that might fix all the issues!
