---
layout: post
title:      "A simple React process Tutorial:"
date:       2018-11-20 21:21:50 +0000
permalink:  a_simple_react_process_tutorial
---


1).  Lets start with creating a React app:


	a). First, create a folder to work out of.  Mine is called my_react_example_blog
	
	b). Open your Developer tool and it's command line (I'm using Visual Studio Code, and its terminal).
	
	c). Open up your newly created folder named my_react_example_blog and make sure your terminal is in that folder.
	
	d). type: npm -g create-react-app and then press enter
	
	e). After that type: create-react-app button-app  (that's the name of this example)
	
	f). Once it is complete- type: cd button-app in the terminal to enter the folder.
	
	g). Type npm install to make sure all nodes are installed. typing: npm start will open up the app in chrome.
	

2). Now is time for Basic React app setup:

	a). Now if you are using github desktop, open it up, and click File -> Add Local Repository.  Add the folder for this app, and publish it.
	
	b). next type: npm i bootstrap and press enter.  Then add this line to the top of index.js file in src folder:
		import "bootstrap/dist/css/bootstrap.css";
		
	c). In your App.js file delete everything within the return area besides the <div></div> portion.
	
	d).  Also Delete the import Logo phrase form the top of App.js
	
	e). Now To test that everything is working - type `<h1>Hello World</h1>` between the <div>s in return(); and then npm start in console.
	

3). Time to build the basic functionality:

	a). In the App.js delete the Hello World h1 and type <Button /> in between the divs.
	
	b). At the top of App.js type import Button from './components/Button';
	
	c). Create a folder called components in src folder, and within that create a Button.jsx file.
	
	d). In Button.jsx type imrc and press tab to get the import react statement.  and type: cc press tab, and type: Button.  This creates the Button class as a code snippet.
	
	e). Now type `<h1>Hello World</h1>` Within the Button's return function.  That should now render on your chrome browser.
	
	f). Next, Delete the Hello World statement, and within the return statement place:
	
```
	
      <div className="col-1">
        <button onClick={() => this.props.onIncrement()} className={this.props.classAssign()}>
          Press Me
        </button>
      </div>
		
```
	
	g). That has set us up on the lowest level.  Now create a file in src folder called buttons.jsx
	
	h). Write this code in there. 

		<div>
			<Button onIncrement={onIncrement} classAssign={classAssign} />
		</div>



	i). Now in App.js within the component assign the state - state = { val: 0 };
	
	j). Next, set up your constructor with constructor(props){ super(props); }
	

4). Setting up the actual rendering and functionality:

	a). Within the Render and return method place a <div className="containr"> <Buttons onIncrement={this.handleIncrement} classAssign={this.classAssign} /></div> 
	
	b). Those statements actually put the component on your browser screen!
	
	c). Now its time to describe these functions start with handleIncrement = props => {}; and classAssign = props => {};
	
	d). Within handleIncrement place const value = this.state;  and then value.val++;  and finally this.setState({ value });
	
	e). And for classAssign, check out the code below 
	
	`  classAssign = props => {
    let classes = "btn btn-secondary btn-sm ";
    if (this.state.val / 1 === 1) {
      classes += "badge-primary";
    } else if (this.state.val / 2 === 1) {
      classes += "badge-success";
    } else if (this.state.val / 3 === 1) {
      classes += "badge-danger";
    } else if (this.state.val / 4 === 1) {
      classes += "badge-warning";
    }
    console.log(this.state.val);
    return classes;
  };`



5).  Your button should now be changing between 4 colors, and the page should never be refreshing!  Thats a basic react setup!
