---
title: "Using Mapbox GL JS API with React"
date: 2020-06-17T11:07:10+06:00
author: Robert Phillips
image : "images/blog/blog-post-4.jpg"
bg_image: "images/featue-bg.jpg"
categories: ["Javascript"]
tags: ["API","Javascript", "React"]
description: "impression & experience first time using this API"
draft: false
type: "post"
---


For our first React map/routing project, my team decided to use the Mapbox GL API to display locations on a map of that a dispatcher needed to interact with to assign and plan work.  

**In this post I will:**  
1. Briefly describe what Mapbox GL JSis and does
2. Show a little example of how we used it
3. Share my impression after this first use
4. Helpful links

## What is Mapbox GL?
> “Mapbox GL JS is a JavaScript library that uses WebGL to render interactive maps from vector tiles and Mapbox styles. It is part of the Mapbox GL ecosystem, which includes Mapbox Mobile, a compatible renderer written in C++ with bindings for desktop and mobile platforms.”  
see also: https://docs.mapbox.com/mapbox-gl-js/api/  

Basically, this is a tool we can use to put a very interactive map interface into our webpage which we are all so familiar with these days (think Uber, Google Maps, Waze, etc).  

Speaking of Uber, their super smart engineers developed a package for using Mapbox with React (the Javascript framework we used to build the front end of our project). That package is react-map-gl. All the heavy lifting of graphics, mapping, routing, calculating, etc is done by this API and this library. This package brought in the maps, markers, popups, event and screen controls, all the user interactions you need, layers, geolocation, and much more.  

For the Rails backed we used a great Ruby gem named mapbox-sdk with had some awesome geolocation logic for resolving locations, even those with partial information (ie the name of a park, etc).  

## Our project
We wanted to take a big list of home healthcare patients, plot them all on a map, and then use that graphical view to more intelligently make nurse assignments based on geography, plan routes, etc. Furthermore, we wanted to be able to filter views of that map, interact with map pins, assign nurses, delete appointments. For this, we married a Rails backend with a Javascript front end using styled Material UI React components.  

I found a number of guides out there to show you how to get started. The Mapbox docs are great for this too. I’ll leave links to a guide I found and a tutorial I watched to get set up and familiar with tool at the bottom.
Using Mapbox markers

![map screenshot](/images/blog/dispatch-map.png)

Example code with comments:  
```
  import React from 'react'
  import ReactMapGL, { Marker, Popup } from 'react-map-gl';
  import '../App.css';

  const mapMarkers = (userData, setSelectedAppointments, setPopupState) => {
    const nurses = mapNurses(userData.nurses, setSelectedAppointments, setPopupState) || [];
    const patients = mapPatients(userData.patients, setSelectedAppointments, setPopupState) || [];
    return nurses.concat(patients) || null;
  }

  const handleClick = (user, setSelectedAppointments, setPopupState) => {
    setSelectedAppointments(user.id)
    setPopupState(user) // put appt data into state to be displayed on popup's window
  }

  // const mapNurses = (nurses, setSelectedAppointments, setPopupState) => {
  // } looks like mapPatients below...

  const mapPatients = (patients, setSelectedAppointments, setPopupState) => {
    if (patients) {
      return patients.map(patient => {
        return <Marker
          key={patient.address}
          latitude={patient.latitude}
          longitude={patient.longitude}
        >
          <button className='marker-btn' onClick={() => handleClick(patient, setSelectedAppointments, setPopupState)}>
            {/* get your own images to style your pins. nurses style != patient style */}
            <img src='patient-pin.png' alt='patient-pin' />
          </button>
        </Marker>
      })
    }
  }

  const renderPopup = (stateObj, setPopupState, updateRenderedItem) => {

    return (
      stateObj && (
        <Popup
          className="popup"
          tipSize={5}
          anchor="top"
          longitude={stateObj.longitude} // click on marker feeds lat/long to state which is passed
          latitude={stateObj.latitude} // here as prop so popup shows up near the selected marker pin
          closeOnClick={false}
          onClose={() => setPopupState(null)}
        >
          <div>
            <b>{ stateObj.name }</b><br />
            { stateObj.address }<br />
            <button onClick={() => {
              updateRenderedItem('apptDetails') // change from map view to appt view
              setPopupState(null) // reset state which effectively closes popup
            }}>
              Appt Details
            </button >
          </div>
        </Popup>
      )
    );
  }

  const MapContainer = props => {
    return <ReactMapGL
      {...props.viewport}
      mapboxApiAccessToken={props.mapboxApiAccessToken}
      mapStyle='mapbox://styles/rpdecks/ckbczsigy1q5m1ilf2qhgsphi' // lots of map styles available, or customize your own
      onViewportChange={props.handleViewportChange} // allows to drag map inside grid
    >
      {props.userData.user_type !== 'patient' && mapMarkers(props.userData, props.setSelectedAppointments, props.setPopupState)}
      {/* this renders the marker popup based on popupState which is set by clicking on a marker. If you click, it pops up. If you close, state is wiped. */}
      {renderPopup(props.popupState, props.setPopupState, props.updateRenderedItem)}
    </ReactMapGL>
  }

  export default MapContainer;
```
## Impression?
I like Mapbox a lot. Our first experience was very good. I found it easy to get started and use. A short video tutorial and we had our map up and running. Popups and markers were a breeze too.  

Wins for devs?  

**1. Docs:** extensive and well written  
**2. Free!** Until you start generating a lot of traffic.  
**3.** Lots of **tutorials** online with substantial user base  
**4. Uber:** They’re smart and their package is powerful and improving on the regular (react-map-gl)  
**5. Great looking** maps with plenty of options and customization ability