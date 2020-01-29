React Redux 

1. Thunk 


npm install -g create-react-app
create-react-app name
cd name
npm start

npm install  redux redux-thunk react-redux

Now create a folder in src directory name ACTION
and create a file name fetchActions.js under the folder action  can be named as action types or action creators 

create a folder name REDUCER in src directory 
and a file name index.js under the folder reducer 

create a file store.js in the src dircetory
now create a initial store 

in the store.js 

******************************************************************
                     Store.js
               **********************

import {createStore, applyMiddleware} from "redux"                   
import asyncReduceer from "./reducer"
import thunk from "redux-thunk"

const store = createStore(asyncReducer, applyMiddleware(thunk))             --createStore is used for creating the redux store 
                                                                            --applyMiddleware will be used for adding the thunk middleware. 
export default store
*******************************************************************

What is a middleware?

Well it is nothing but a piece of code that sits between your actions and your reducers. 
It takes your actions does something to it before passing it down to the reducer. 
Think of it like a middle-man or a bridge between redux store and your react app


Now in the index.js inside reducers

*********************************************************************
                         Index.js/Reducers
                  ********************


const initialState ={
  userData:{},                            -- The userData is an object that will house all the user related information that we will get from our API.
isFetching:false,                         --The isFetching property will be used to load the loading indicatior depending upon when the API request is made.
isError:false                             -- isError is used to render an error message in case we do not get back any user data.
};

const asyncReducer =(state = initialState,action)=>{
return state;
}

export default asyncReducer

*********************************************************************


Now head on to the main index.js file in the src directory and import the store and provider from react-redux 

**********************************************************************
                   Index.js
             ********************

import store from "./store"
import {Provider} from "react-redux"

<Provider store={store}>        --  Provider component that allows us to access the store state from our components
</App>
</Provider>

***********************************************************************

Now head up to fetchaction.js in action

***********************************************************************
*******************FETCHACTION.JS**************************************

We are making use of action creators.
Action creators are just functions that returns an action object.
we have three action creators each of them returning an action.
A little refresher before going forward.
Actions are plain old javascript objects which has a mandatory type property.
This property defines what sort of action/event is taking place in the application.

**************************************************************************

import store from "../store"

export const fetch_post =()=>{                     The first action creator fetch_post is responsible for starting the fetch request. It would be used mainly for showing the loading indicator.
return{
type:"FETCH_USER"
};
};

export const fetched_post =post=>{               The second action creator receive_post will be called when we get back the data from github. 
return{
type:"FETCHED_USER",
data:post
};
};

export const recieve_error =()=>{             Finally receive_error is an action creator that will be called only when we have an error in getting our data back from github's servers.
return{
type:"RECIEVE_ERROR"
};
};
 
***************************************************************************
         ***************Understanding Thunk******************
***************************************************************************
         A THUNK IS A FUNCTION THAT RETURNS ANOTHER FUNCTION	

***************************************************************************
For Example-
function say() {
  return function something() {
    //code here
  };
}

**************************
1.The above function say() is a thunk because it returns another function in this case something().
2.Redux reducers are pure functions hence we can't do any async operations inside the reducers and actions are just plain old objects.

3.Our dispatch method inside the store accepts an action object as its parameter what the redux thunk middleware does is that if say an action creator
  returns a function instead of an object then it simply executes that returning function.


