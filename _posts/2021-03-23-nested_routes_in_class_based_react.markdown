---
layout: post
title:      "Nested Routes in Class Based React"
date:       2021-03-23 22:34:44 -0400
permalink:  nested_routes_in_class_based_react
---

While working on my latest project, an app for keeping track of when to take care of your houseplants, I wanted to make sure that I built the app in such a way that the user could link to specific parts of the app, even a specific plant's information card. Additionally, they should be able to reload the page while viewing a specific plant or care event, and continue to see that plant or event when the page is done reloading. Seems easy enough. It became a bit of a headache though, so let me walk you through my final solution. 

First off, some context:

I've got a React app using a Redux store, Thunk middlewear is handling fetch calls to my Rails API, and React Router DOM is taking care of the routing. 

Starting out in `App.js`, 

![app.js](https://i.imgur.com/pLihW1I.png)

I'm importing `BrowserRouter` as `Router`, as well as `Switch` and `Route`. In this case, I have a separate functional component taking care of my Navbar which is hanging out right above the Switch.

Now, if you're building a hook based React application, the best way to handle nested routes is using the [`useRouteMatch()`](https://reactrouter.com/web/example/nesting) hook available from React Router DOM from which you can destructure `URL` and `path` variables to manage the nesting of routes. However, I'm working with class based React, and I have an app that is small enough that it seemed impractical to refactor just to accommodate nested routing. So this was my solution. 

When your router matches a specific path, it looks for one of three render methods: `component`, `render`, or `children`. They each function a little bit differently, but regardless of which you choose, you'll get three built in props with each: `match`, `location`, and `history`. 

`history` will be used down the road for redirecting, but let's look at `match`. `match` holds information about the current route including the path and url. It also includes any params present in the route. 

The router looks like this,

![router](https://i.imgur.com/vKRkO9n.png)

so when a user enters `http://reallycoolapp.com/plants/35`, `match` will look like this:

![match](https://i.imgur.com/xPDrERE.png)

So, with `match.params.plantId`, we get "35"! That's what we need to find the correct plant and render it to the user! 

The path `"plants/:plantId"` renders the component `PlantPageContainer`, so let's have a look at what happens when we get there.

Originally, I was triggering a dispatch within the constructor that fetched all of the plants, and attaching a `.then` statement to handle fetching events and notes associated with that plant. I had seen that `componentDidMount()` was a good place for fetch calls, but was having trouble with having the fetch complete before my page tried to render. Afterall, it wasn’t fetching data until after the component mounted, which happens after `render`. This is what my constructor looked like:

![PlantPageContainer](https://i.imgur.com/FGtmvu6.png)

This obviously had a negative effect on performance, and just didn’t follow the convention that I was familiar with, but it took some time to figure out what the true culprit was for the errors I was getting, and it wasn’t what I expected.

When I was building my reducers, I had set up an initial state that set the initial value of `currentPlant` to `null`. So, when components rendered, they tried to extract values from a non-existent object which raised errors. If I had instead defined that initial state as an empty object, no errors would be raised. Calling a key on an empty object just returns nothing. So, I went back and changed my reducer’s initial state to this: 

![initialState](https://i.imgur.com/Fwgg1x7.png)

Now, I also used some logic in some of my container components to determine whether a plant was displayed or a welcome message. This logic broke when there was an empty object, so I defined a helper method that checks whether the `currentPlant` is an empty object or an actual plant instance.

![helper](https://i.imgur.com/zTBTbcW.png)

Now that all this is set up, we can dispatch fetch calls from `componentDidMount()` so that the component can render and then fill in with the information that is fetched. 

Now, the question of nested routing: 

So, in `componentDidMount()`, which now looks like this:

![componentDidMount](https://i.imgur.com/34mtnxi.png)

in order to appropriately respond to nested routes, after fetching all of the plants that need to show up in our sidebar (making sure to **return** our fetch call when it is defined), we throw on a `.then` statement, which is where we check to see if the `match` prop has any params. If it does, we'll want to go find that plant in the store, set it to active, and fetch the notes and events associated with it. If there is no `plantId` in match, then we move along and simply render the welcome message along with the list of plants in the sidebar. 

So, to recap, in order to imitate the functionality of nested routes, we define a nested route in the `router` located in `App.js` using a colon to indicate that we'll be passing a value through in that position, and we pass that route a component to render when it is matched. Then the component, which in this case is a container component for rendering plants, renders and dispatches the appropriate actions to fetch plants. In order to set the current plant that has been passed into the component via the url, we fetch all of the plants first so that when we dispatch this action: 

![action](https://i.imgur.com/Wo94fec.png)

and then dispatch this case in `reducers/plants.js`:

![set_current_plant](https://i.imgur.com/l3rPfpf.png)

there are plants in the store to iterate over. This prevents us having to make another fetch call to the API just to retrieve this specific plant, which we already have the information for in the store. [Here's a short clip](https://i.imgur.com/GhPJ3Ox.mp4) demonstrating navigating directly to a specific plant via a nested route.

