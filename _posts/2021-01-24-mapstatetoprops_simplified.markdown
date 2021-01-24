---
layout: post
title:      "mapStateToProps Simplified"
date:       2021-01-24 19:37:19 +0000
permalink:  mapstatetoprops_simplified
---


This last month of my time here at Flatiron School I got the opportunity to learn about Redux. After spending time learning all about Redux and its uses, we come across the function `mapStateToProps()`. I will get into that a little later. Redux is an open-source JavaScript library for managing an applications state. First, let’s dive into what the state of an application really is.

According to the Redux webpage, state is something in the Redux API that that refers to the single state value managed by the store. It represents the entire state of a Redux application. What does this mean exactly? The store allows you to save values (of whatever you would like to save) that pertains specifically to your application, and be able to use those values within different components of your applications code. As an example, I created a weather app where I wanted to to set the values in the store with things such as; the city name, the current temperature, the high and low for the day, as well as an icon that showed what the weather was (a sun when it’s sunny, or clouds when it’s overcast). 

Now, after you have created a store within your application and have set the state within one or more of our components, we need to make sure that these state-full components connect to the store. After importing the use of your store in your component: 

`import { connect } from ‘react-redux’;`

You will then need to export the store at the bottom of you component so that other components in your application will be able to use the data within the store:

`export default connect(mapDispatchToProps)(ComponentName);`

Now that we have the store all set up with your persisted data, and we are dispatching it to other components, we can connect our components using the `mapStateToProps()` function.

`mapStateToProps()` is a function within Redux that takes our state from the original component and allows us to use the state within the current component. For example, in my weather app, once I had the values I wanted stored within my state, I was able to collect them in a component called `Forecast,js` where I display the values on the app to the user. 

The function itself was written as such:

```
const mapStateToProps = state => {
	return {
		city: state
	}
}
```

Above you can see that I am taking in the entire state as an argument and passing it into the `mapStateToProps` function. Then the function itself returns the state as a value to the key of my choosing, `city`. Now to display these attributes with the component, you just need to call this key and then whichever attribute you need!

Here is an example of some JSX that will display the current temperature. Keep in mind the external API that I was using gave me the temperature in Kelvin, so there was some math involved to convert that into Fahrenheit:

`<p>{Math.round((this.props.city.temp - 273.15) * 9/5 + 32)}</p>`

