---
title: "Climbing the Redux Thunk learning curve"
date: 2020-09-04T08:58:52-04:00
author: Robert Phillips
image : ""
bg_image: ""
categories: ["Code"]
tags: ["Redux","React", "Javascript"]
description: "climbing the Redux Thunk learning curve"
type: "post"
---

## Preface:
I am not an expert. I am just learning React and wanted to learn Thunk and Redux. This is me documenting my learning journey and circling back to try and help others doing the same thing with (hopefully) relevant stuff.

I am linking the most helpful resources, articles, etc. I found below. Those are the experts. My most helpful contribution may be to those climbing the learning curve of unfamiliarity like I just did. Once you’re there, those folks will take you home.

This post assumes the reader is also learning Javascript and is familiar with Redux already. I’ll talk briefly about what Redux is but will mostly jump into how Thunk extends it, what the use case is for Thunk, and give an example use case for it and how I set it up.

## What is Redux and What It Does for Us?
> “Redux was created by [Dan Abramov](https://github.com/gaearon) for a [ talk ](https://www.youtube.com/watch?v=xsSnOQynTHs). It is a “state container” inspired by the unidirectional [ Flux ](http://facebook.github.io/flux/) data flow and functional [ Elm ](https://github.com/evancz/elm-architecture-tutorial/) architecture. It provides a predictable approach to managing state that benefits from immutability, keeps business logic contained, acts as the single source of truth, and has a very small API.”  
> --[Gabriel Lebec](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)

When we load a website and log in to our account, the website pulls lots of data totally relevant to our personalized user experience from its database and perhaps other places around the web to produce the experience we have. Think... all my user preferences, past posts, my location’s weather data, etc. Once that data is fetched, it becomes the “state” of my app and the environment we experience in the app.  All of that is essentially held on to locally. As we interact with our browser liking, toggling filters, deleting, etc. the “state” of the app changes and thus other things on the page.

As programmers, how do we keep up with all of that information and pass it all around our application? I built a project without it and was passing props and state all over the place in my app. There were SO many lines of code and it was hard to keep up with all of it. That experience was good. It made me hungry for [ Redux ](https://redux.js.org/) (and [Redux Context](https://reactjs.org/docs/context.html), check it out). Redux manages and simplifies all of this for us.

### Under the hood:
Inside a connected component we don’t have access to the Redux store directly so we would typically  
> ```javascript
> import { connect } from ‘react-redux’```  

and then, usually at the bottom of our component we  

> ```javascript
> const mapStateToProps = state => {
>   return {
>       stateItemToUse: state.reducer.stateItem
>   }
> }
> const mapDispatchToProps = dispatch => {
>   return {
>       actionToDispatch: () => ({ type: 'DISPATCH TYPE', action: action})
>   }
> }
> export default connect(mapStateToProps, mapDispatchToProps)(componentName)
> ```  

This gives us access to the store inside our component using [ this.props.actionToDispatch ](#) to dispatch an action to our store and update our state based on component logic or we use [ this.props.stateItemToUse ](#) to access data in the store.


## What is THUNK
> Redux Thunk is a middleware that lets you call action creators that return a function instead of an action object. That function receives the store’s dispatch method, which is then used to dispatch regular synchronous actions inside the body of the function once the asynchronous operations have completed.
> [Asynchronous Redux Actions Using Redux Thunk](https://www.digitalocean.com/community/tutorials/redux-redux-thunk#:~:text=Redux%20Thunk%20is%20a%20middleware,the%20asynchronous%20operations%20have%20completed.)

> Redux-Thunk is a middleware that allows our action creators to return a function instead of an action object. The thunk can be used to delay the dispatch of an action, or to dispatch only if a certain condition is met. It looks at every action that passed thrugh the system; and if it’s a function, it calls that function. This is just another tool that can help us decouple our application logic from our view rendering.
> [Front End Curriculum](https://frontend.turing.io/lessons/module-3/redux-thunk-middleware)

## Why Thunk?
1. **Async dispatch**  
We often want to fetch data and save the response data to our Redux store when it comes back but dispatch does not know what to do with a promise. Stay tuned, you'll see how I did it without and then with Thunk.
1. **Components simple and focused on presentation (abstracting logic)**  
Nice to abstract API calls, dispatches, & associated logic out of our component into /services or /actions
1. **DRY!! - Duplication of code**  
It's likely your login, signup pages, and others may follow a similar flow to hydrate your app with data from server after the user has authenticated. I started with that code duplicated in several places. With Thunk, we can combine that fetch into a single action creator function.
1. **Reduction of code, errors, maintenance points**  
One fetch and subsequent dispatch in one place versus several == great improvement!
1. **Functional Purity**  
A core principle of Redux state mgt is that it is built upon and around pure functions. Ripple effects of API calls goes against that instead producing dependencies and more tightly coupling our components thus making our code harder to test and reason about.
1. **Component Coupling:**  
Long, spelled out, detailed fetches, and customized dispatches to store with responses make our components hard to reuse or untether as our application grows... A big thanks to [Sandi Metz](https://www.poodr.com/) for enlightening me with the concepts of dependency injection and inversion control.
1. **Consistency in our API:**  
source: [Full Stack Academy](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)  
We could avoid all this and just store.dispatch inside our async handler...
```javascript
// in an action creator module:
import store from '../store'

const simpleLogin = user => ({ type: LOGIN, user })

const asyncLogin = () =>
    axios.get('/api/auth/me')
    .then(res => res.data)
    .then(user => {
        store.dispatch(simpleLogin(user))
})
 
// somewhere in component:
asyncLogin()
```
But now our components sometimes call store.dispatch(syncActionCreator()), and sometimes call doSomeAsyncThing().
* In the latter case, it’s not obvious that we are dispatching to the store. We cannot identify Redux actions at a glance, making our app’s data flow more opaque.
* What if we later change an action-function from sync to async, or async to sync? We have to track down and modify the call site for that function, in every single component it is used. Yuck!

## Example flow from my app:
Here's how I refactored into Thunk implementation:  
User signs up and a token is fetched from the back end. If the user gets a token from that fetch, we need to do another to fetchData that hydrates the app with all hte basic data needed to begin using the app. We don't want that same user to have to now login with that token. When a user logs in however, the exact same thing happens when the user is validated. We fetchData and hydrate the app for that user. Then, every time this user refreshes the page we use the componentDidMount hook to fetch the same data as well as when returning to the app after closing it (assuming they don't log out).
This all makes sense but after my first pass, I had at least 20 lines of duplicate code for this in App.js, EmployerLogin.js, CaregiverLogin.js, EmployerSignup.js, CaregiverSignup.js. So there is at least 80 lines of duplicate code and at least 5 places that need maintenance over the life of the app. Not to mention, a lot of logic cluttering these components. Right away I could smell this was wrong in the code and wondered how to fix it. As I was building it, I was under a time constraint to produce an MVP and I was ignorant of middleware like Thunk.

