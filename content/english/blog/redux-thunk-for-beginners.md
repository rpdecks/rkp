---
title: "Climbing the Redux Thunk learning curve"
date: 2020-08-18T08:58:52-04:00
author: Robert Phillips
image : "images/blog/thunk-monkey.jpg"
bg_image: "images/blog/thunk-monkey.jpg"
categories: ["Code"]
tags: ["Redux","React", "Javascript"]
description: "what I wish I knew starting out with Thunk"
draft: false
type: "post"
---

## Preface:
I am not an expert. I am just learning React and wanted to learn Thunk and Redux. This is me documenting my learning journey and circling back to try and help others doing the same thing with (hopefully) relevant stuff.

I am linking the most helpful resources and articles I found at the end. They are the experts. My most helpful contribution may be to those climbing the learning curve of unfamiliarity like I just did. Once you’re there, those folks will take you home.

This post assumes the reader is also learning Javascript and is familiar with Redux already. I'll briefly touch on Redux; however, this article mostly involves explaining Thunk as an extension of Redux. I will also go into Thunk use cases as well as provide an example.  

## What is Redux and What It Does for Us?
> “Redux was created by [Dan Abramov](https://github.com/gaearon) for a [ talk ](https://www.youtube.com/watch?v=xsSnOQynTHs). It is a “state container” inspired by the unidirectional [ Flux ](http://facebook.github.io/flux/) data flow and functional [ Elm ](https://github.com/evancz/elm-architecture-tutorial/) architecture. It provides a predictable approach to managing state that benefits from immutability, keeps business logic contained, acts as the single source of truth, and has a very small API.” --[Gabriel Lebec](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)

When we load a website and log in to our account, the app pulls in data for our personalized user experience from its database and other places to "hydrate," or start up the app. Think... all my user preferences, posts, likes, my location’s weather data, etc. Once fetched, that data becomes the “state” of our app and the environment we experience in the app.  All of that is essentially stored locally. As we interact with our browser... liking, toggling filters, deleting, etc... the “state” of the app changes underneath along with the environment we are experiencing (i.e. the page we are on).

As programmers, how do we keep up with all of that information and pass it all around our application? I built an app without it and found myself passing props and state all over the place. There were SO many lines of code and it was really hard to keep up with all of it. That experience was good. For sure, it made me hungry for the [ Redux ](https://redux.js.org/) technology (and [Redux Context](https://reactjs.org/docs/context.html), check it out). Redux manages and simplifies all of this for us.

### Under the hood:
Inside a connected component we don’t have access to the Redux store directly so we need to import the 'react-redux' library. This then gives us access to the two functions, mapStateToProps and mapDispatchToProps you see below.
```javascript
import { connect } from ‘react-redux’
```  

and then, usually at the bottom of our component we  

```javascript
const mapStateToProps = state => {
    return {
        stateItemToUse: state.reducer.stateItem
    }
}
const mapDispatchToProps = dispatch => {
    return {
        actionToDispatch: () => ({ type: 'DISPATCH TYPE', action: action})
    }
}
export default connect(mapStateToProps, mapDispatchToProps)(componentName)
```  

This gives us access to the store inside our component using to dispatch an action and write to our Redux store and update state or to access state data in the store and use it in our component logic.

## What is THUNK
> "Redux Thunk is a middleware that lets you call action creators that return a function instead of an action object. That function receives the store’s dispatch method, which is then used to dispatch regular synchronous actions inside the body of the function once the asynchronous operations have completed."  
> **[Asynchronous Redux Actions Using Redux Thunk](https://www.digitalocean.com/community/tutorials/redux-redux-thunk#:~:text=Redux%20Thunk%20is%20a%20middleware,the%20asynchronous%20operations%20have%20completed.)**

> "Redux-Thunk is a middleware that allows our action creators to return a function instead of an action object. The thunk can be used to delay the dispatch of an action, or to dispatch only if a certain condition is met. It looks at every action that passed thrugh the system; and if it’s a function, it calls that function. This is just another tool that can help us decouple our application logic from our view rendering."  
> **[Front End Curriculum](https://frontend.turing.io/lessons/module-3/redux-thunk-middleware)**

## Why Thunk?
For small simple applications, it's probably not necessary. There are ways to manage, but where Thunk shines is in the convenience it provides as our applications grow in complexity and size.  
1. **Async dispatch**  
We often want to fetch data and save it to our Redux store but dispatch does not know what to do with an async promise. Stay tuned, you'll see how I did it pre & post-Thunk.
1. **Components simple and focused on presentation (abstracting logic)**  
It's nice to be able to abstract API calls, dispatches, and associated logic out of our component into ./services or ./actions
1. **DRY!! - Duplication of code**  
It's likely our login, signup components, and others may follow a similar flow to get users logged in and the app hydrated. I started with that code duplicated in several places. With Thunk, we can combine that similar fetch into a single action creator function and use it in the aforementioned components.
1. **Reduction of code, errors, maintenance points**  
One fetch and subsequent dispatch in one place versus several == great improvement!
1. **Functional Purity**  
A core principle of Redux state management is that it's built upon and around pure functions. The ripple effects of API calls go against that principle and instead produce dependencies and more tight coupling of our components. This makes our code harder to test and reason about.
1. **Component Coupling:**  
Long, spelled out, detailed fetches, and customized dispatches to the store make our components harder to reuse or untether as our application grows... A big thanks to [Sandi Metz](https://www.poodr.com/) for enlightening me with the concepts of dependency injection and inversion control. I highly recommend her book!
1. **Consistency in our API:**  
Consider the following from: [Full Stack Academy](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)  
> We could avoid all this and just store.dispatch inside our async handler...
> ```javascript
> // in an action creator module:
> import store from '../store'
>
> const simpleLogin = user => ({ type: LOGIN, user })
> 
> const asyncLogin = () =>
>    axios.get('/api/auth/me')
>    .then(res => res.data)
>    .then(user => {
>        store.dispatch(simpleLogin(user))
> })
> 
> // somewhere else in our component:
> asyncLogin()
> ```
> But now our components sometimes call store.dispatch(syncActionCreator()), and sometimes call doSomeAsyncThing().
> * In the latter case, it’s not obvious that we are dispatching to the store. We cannot identify Redux actions at a glance, making our app’s data flow more opaque.
> * What if we later change an action-function from sync to async, or async to sync? We have to track down and modify the call site for that function, in every single component it is used. Yuck!

## Example flow from my app:
Here's how I refactored into Thunk implementation:  
User signs up and a token is fetched from the back end. If the user gets a token, we do another (fetchData) to hydrate the app with all the basic data needed to start up the app. We don't want that same user to have to now login with that token. When they signup successfully, we want them to be logged in too. When an existing user logs in however, the exact same thing happens when the user is authenticated. We fetchData and hydrate the app for that user's session. Then, every time this user refreshes the page we use the componentDidMount hook to fetch the same data.  
This all makes sense but after my first pass, I had at least 20 lines of duplicate code to do this in several components. So maybe there's 80 lines of duplicate code and several places to maintain over the life of the app.. not to mention, a lot of logic cluttering these components. How did I get myself into this mess!?! Right away I could smell this code was wrong and wondered how to fix it. At that point, I was under a time constraint to produce an MVP and I was ignorant of middleware like Thunk.  

## Refactoring
Below is what I started with in my Login component. I am going to abstract this whole fetch and all the action dispatches using Thunk by the end of the post.  
```javascript
import React from 'react';
import { connect } from 'react-redux'
import { withRouter } from 'react-router';
import { API_ROOT} from '../services/apiRoot'

handleLogin = token => { //this 
    localStorage.setItem('auth_token', token);
    localStorage.setItem('userType', this.props.userType);
    this.props.setLoginStatus(true)

    fetch(`${API_ROOT}/app_status`, fetchObj)
      .then(res => res.json())
      .then(appData => {
        props.storeUserJobs(appData.jobs)
        props.storeUserData(appData.user)
        if (userType === 'employer') {
          props.storeUserFavorites(appData.employer_favorites)
          props.storeAuthoredReviews(appData.employer_reviews)
          props.storeReviewsAboutMe(appData.caregiver_reviews)
          props.storeCaregivers(appData.caregivers)
        } else if (userType === 'caregiver') {
          props.storeUserFavorites(appData.caregiver_favorites)
          props.storeAuthoredReviews(appData.caregiver_reviews)
          props.storeReviewsAboutMe(appData.employer_reviews)
          props.storeEmployers(appData.employers)
          props.storeAvailableJobs(appData.available_jobs)
          props.storeInterestedJobs(appData.interested_jobs)
        } else { console.log('No userType specific appData stored') }
        props.hydrateComplete()
      })
      .catch(error => console.log(error))
}
```
That's long, I know. I'm learning :) Anyhow, I'll spare you the mapDispatchToProps function where all those actions are configured. I think we get the point here. This is too much stuff to live in one component, let alone several.  

## Thunk setup
To set up Thunk first I needed to run 'yarn add @reduxjs/toolkit'.  
* If you did this to get redux core then you are good. If not, this toolkit is recommended for Redux applications and brings Thunk in with it.  
Next I needed to do the following pertinent things in **store.js**:  
Note what's imported. createStore and combineReducers are probably familiar but 'compose' allows me to combine the 'applyMiddleware' argument with the REDUX DEVTOOLS EXTENSION in the createStore function.
```javascript
import { combineReducers, compose, createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk'

export default createStore(
  rootReducer,
  compose( applyMiddleware(thunk), window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__({trace: true}))
);
```
Then I created this file in this folder: [src/actions/fetches.js](#)
```javascript
import { API_ROOT } from '../services/apiRoot'

export const fetchData = (userType) => {
    const auth_token = localStorage.getItem('auth_token')
    const fetchObj = {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        'Auth-Token': auth_token,
      }
    }
    return (dispatch) => {
        dispatch({ type: 'LOADING_DATA' })
        fetch(`${API_ROOT}/app_status`, fetchObj)
        .then(res => res.json())
        .then(appData => {
            dispatch({ type: 'STORE_USER_JOBS', userJobs: appData.jobs })
            dispatch({ type: 'STORE_USER_DATA', userData: appData.user })
            if (userType === 'employer') {
                dispatch({ type: 'STORE_USER_FAVORITES', userFavorites: appData.employer_favorites })
                dispatch({ type: 'STORE_REVIEWS', authoredReviews: appData.employer_reviews })
                dispatch({ type: 'STORE_REVIEWS_ABOUT_ME', reviewsAboutMe: appData.caregiver_reviews })
                dispatch({ type: 'STORE_CAREGIVERS', caregivers: appData.caregivers })
            } else if (userType === 'caregiver') {
                dispatch({ type: 'STORE_USER_FAVORITES', userFavorites: appData.caregiver_favorites })
                dispatch({ type: 'STORE_REVIEWS', authoredReviews: appData.caregiver_reviews })
                dispatch({ type: 'STORE_REVIEWS_ABOUT_ME', reviewsAboutMe: appData.employer_reviews })
                dispatch({ type: 'STORE_EMPLOYERS', employers: appData.employers })
                dispatch({ type: 'STORE_AVAILABLE_JOBS', availableJobs: appData.available_jobs })
                dispatch({ type: 'STORE_INTERESTED_JOBS', interestedJobs: appData.interested_jobs })
            } else { console.log('No userType specific appData stored') }
            dispatch({ type: 'FINISH_LOADING' })
        })
        .catch(error => console.log(error))
    }
}
```
**Note here:**
1. Action creator fetchData returns a function
    * Typical Redux action creators return objects with ({ type: type, action: action})... this is Thunk and new
1. This function is passed dispatch as an argument and userType
1. The function fetches data async
1. The first thing this action creator does is dispatch "LOADING_DATA'
    * This sets state.loading: true. If/when this function finishes loading that fetched data into store, state.loading is toggled to false triggering a wonderful refresh of our now hydrated app.
1. We are not using dispatch mapped to props like we do in a connected component, rather we use the dispatch function passed in to dispatch actions to the store.  

### Returning to Login.js...  
We now have the following having refactored out the fetch, the action dispatches in mapStateToProps, and a handful of items in mapStateToProps:
```javascript
handleLogin = token => {
localStorage.setItem('auth_token', token);
localStorage.setItem('userType', this.props.userType);
this.props.setLoginStatus(true)
this.props.fetchData(this.props.userType) // a thing of beauty to me
}
```
## Summary
I went on to refactor all the fetches out of these components (editUser, login, etc). I am very happy to have consolidated my fetches outside of my components. Now they are much simpler to work with, read, and reason about. They are also not so closely coupled to the fetches and don't know much (anything really) about the fetch's logic and dispatch. I was able to remove almost all the mapToProps from the connected components.

## Promised Helpful Links
1. [Thunks in Redux](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60) by Gabriel Lebec
1. [Stack Overflow: Why do we need middleware for async flow in Redux?](https://stackoverflow.com/questions/34570758/why-do-we-need-middleware-for-async-flow-in-redux) answered by Dan Abramov
1. [Stack Overflow: Dispatching Redux Actions with a Timeout](https://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559) answered by Dan Abramov