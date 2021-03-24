---
layout: post
title:      "Nested Routes in Class Based React"
date:       2021-03-24 02:34:44 +0000
permalink:  nested_routes_in_class_based_react
---

While working on my latest project, an app for keeping track of when to take care of your houseplants, I wanted to make sure that I built the app in such a way that the user could link to specific parts of the app, even a specific plant's information card. Additionally, they should be able to reload the page while viewing a specific plant or care event, and continue to see that specific plant or event when the page is done reloading. Seems easy enough. It became a bit of a headache though, so let me walk you through my final solution. 

First off, some context:

I've got a React app using a Redux store, Thunk middlewear is handling fetch calls to my Rails API, and React Router DOM is taking care of the routing. 

Starting out in `App.js`, 

![app.js](https://i.imgur.com/pLihW1I.png)

I'm importing `BrowserRouter` as `Router`, `Switch`, and `Route`. In this case, I have a separate functional component taking care of my Navbar which is hanging out right above the Switch.

Now, if you're building a hook based React application, the best way to handle nested routes is using the [`useRouteMatch()`](https://reactrouter.com/web/example/nesting) hook available from React Router DOM from which you can destructure `URL` and `path` variables to manage the nesting of routes. However, I'm working with class based React, and I have an app that is small enough that it seemed impractical to refactor the whole app just to accommodate nested routing. So this was my solution. 

When your router matches a specific path, it looks for one of three render methods: `component`, `render`, or `children`. They each function a little bit differently, but regardless of which you choose, you'll get three built in props with each of these: `match`, `location`, and `history`. 

Let's look at `match` first. `match` holds information about the current route including the path and url. It also includes any params present in the route. 

The router looks like this,

![router](https://i.imgur.com/vKRkO9n.png)

so when a user enters `http://reallycoolapp.com/plants/35`, `match` will look like this:

![match](https://i.imgur.com/xPDrERE.png)

So, with `match.params.plantId`, we get "35"! That's what we need to find the correct plant and render it to the user! 

The path `"plants/:plantId"` renders the component `PlantPageContainer`, so let's have a look at what happens when we get there.

![PlantPageContainer](https://i.imgur.com/FGtmvu6.png)

After making the appropriate imports, within the class, we define a constructor. In the React component lifecycle, the very first thing to execute is the constructor. Now, in all of my reading and searching, it was often repeated that `componentDidMount()` was a great place for fetch calls. But what nobody seemed to talk about is then dispatching actions on the data retrieved by those fetch calls before your component has a chance to render. That is where I ran into trouble. Depending on the nested route the user wants to visit, there's a good amount of information that is needed from the server. Those fetch calls have to complete before we can call the `setPlantToActive` action that provides plant information to components down the line. 

I found that the only way to get that information before components tried to render, without using the unsafe `componentWillMount()` was managing all of that information in the constructor. That way, when the component mounts, the necessary information is present.

So, in the constructor, in order to appropriately respond to nested routes, after fetching all of the plants that need to show up in our sidebar (making sure to return our fetch call when it is defined), we throw on a `.then` statement, which is where we check to see if the `match` prop has any params. If it does, we'll want to go find that plant in the store, set it to active, and fetch the notes and events associated with it. If there is no `plantId` in match, then we move along and simply render the welcome message along with the list of plants in the sidebar. Once all of that information is in place, we can go ahead with rendering the component! 

So, to recap, in order to imitate the functionality of nested routes, we define a nested route in the `router` located in `App.js` using a colon to indicate that we'll be passing a value in in that position, and we pass that route a component to render when it is matched. Then the component, which in this case is a container component for rendering plants, calls upon its constructor, which is where we make sure we have all of the information necessary to render the component and all of its children. In order to set the current plant that has been passed into the component via the url, we fetch all of the plants first so that when we dispatch this action: 

![action](https://i.imgur.com/Wo94fec.png)

and then dispatch this case in `reducers/plants.js`:

![set_current_plant](https://i.imgur.com/l3rPfpf.png)

there are plants in the store to iterate over. This prevents us having to make another fetch call to the API just to retrieve this specific plant, which we already have the information for in the store. [Here's a short clip](https://i.imgur.com/GhPJ3Ox.mp4) demonstrating navigating directly to a specific plant via a nested route.

