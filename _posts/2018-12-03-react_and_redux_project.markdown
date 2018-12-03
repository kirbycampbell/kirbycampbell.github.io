---
layout: post
title:      "React & Redux Project"
date:       2018-12-03 21:23:30 +0000
permalink:  react_and_redux_project
---


Hello fine people of Flatiron.  I have just completed the final project and am incredibly excited to have finished FlatIron's Community driven program after 8 months!  I have learned so so so much about the foundations of functional programming, Database management, Authorization, Rails, Javascript, and React/Redux.  The curriculum was incredibly expansive, allowing exploration while guiding me through the many layers of being a fullstack dev.  

I plan to continue self teaching and expanding my knowledge on React/Redux and learn some Angular.js and Vue.js as well.  I want to be as job ready as possible come February 2019, when I plan to begin applying to tech companies in the Portland Oregon area.  

My goal is to continue with my projects from Flatiron, and combine them into one interactive, live on the web website, to showcase the skills I have picked up over the past 8 months.  

To showcase some of my final project coding I'll explain here: 

My Project was a cocktail manager app as well as a bar finder.  I wanted to do a few challenging things with this project specifically: integrate some CRUD functionality with my own Rails API, and connect to another external API with async interactions with my React/Redux front end.  So I chose to use Google's Places API to do a search for Bars in Redux.  This section of the code only dealt with Redux managing the state, and React putting out the forms and input fields.  My Cocktail manager side connected over to my Rails Api and actually stored them all in a database, in which Redux would load the API info up into my React containers.  

My Bar search functionality looked like this:

```
<form
            className="col s12"
            onSubmit={e => {
              e.preventDefault();
              this.props.googleSearch(formFields.search.value);
            }}
          >
            <input
              ref={input => (formFields.search = input)}
              placeholder="Search for Bar"
            />
            <input className="btn waves-effect waves-light" type="submit" />
          </form>
```

The Bottom portion of the above code shows the input field connecting to a self defined let formFields = {}; So, that is how I captured the form input.  Then on Submit - it prevents the default reload action, and connects to my googleSearch custom function that I built and sends in the formField variable that was typed in when submitted.  

This then gets connected to the mapStatesToProps Redux function and Connect export below like so:

```
const mapStatesToProps = state => {
  return {
    bars: state.bars
  };
};

export default connect(
  mapStatesToProps,
  { googleSearch }
)(BarList);
```

The googleSearch function that I built lives in the actions folder as barActions.jsx.  This file looked like this:

```
export const googleSearch = query => {
  return dispatch => {
    return fetch(
      `https://maps.googleapis.com/maps/api/place/textsearch/json?query=${query}+Bar&key=${googleKey}`
    )
      .then(response => response.json())
      .then(bar => {
        dispatch(makeSearch(bar.results));
      })
      .catch(error => console.log(error));
  };
};

export const makeSearch = bars => {
  return {
    type: types.GOOGLE_BAR,
    bars
  };
};
```
This action function takes in the search term, dispatches a fetch action to the google maps api.  I mixin the query sent in variable in the html line and place my secret google API key in the end portion.  Next the response is converted to json.  After that the returned info (called bar) is passed in to another function call in the action file called makeSearch.  Google comes back with a field called results, so I made it bar.results so that the state would be receiving the relevant information.  Next makeSearch takes in the returned info and exports its action type as GOOGLE_BAR.  

The reducer then receives this search result action call:

```
import * as types from "../constants/actionTypes";

export default (state = [], action) => {
  switch (action.type) {
    case types.GOOGLE_BAR:
      console.log(action.bars);
      return action.bars;

    default:
      return state;
  }
};

```

I always console.log() different pieces to see what is getting passed around.  This reducer finally gets reduced in my index.js reducer.  This ends up looking like this:

```
import { combineReducers } from "redux";
import drinkReducers from "./drinkReducers";
import barReducers from "./barReducers";

const rootReducer = combineReducers({
  drinks: drinkReducers,
  bars: barReducers
});
export default rootReducer;

```

As you can see in this portion, the bars portion of the state is taking in the barReducers which puts it onto the Redux managed state.  

Finally back on the barList Container the page maps out the new state of the Search results like so:

```
{this.props.bars.map(bar => {
          return (
            <div className="card" key={bar.id}>
              <h2>{bar.name}</h2>
              <p>{bar.formatted_address}</p>

              <p>{bar.rating}</p>
```

So this little journey has been a great example of the flow of Redux using React.  A form may be rendered on the page visible to the user, the user submits info which gets sent to the action file, which is then sent to the Reducer file, and then the index.js combine reducer, and finally back to the original user visible page.  This example shows you one half of my project, or possibly even 1/3 as the other portion contains Full CRD functionality as of now, and connects to my rails api backend.  
Check it out to see my work, and it's never done, so there will be more to come!

https://github.com/kirbycampbell/oceanx
