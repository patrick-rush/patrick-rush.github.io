---
layout:     post
title:      "React Component Initial State for Nested Objects"
date:       2021-05-23 10:00:00 +0000
permalink:  react_state_nested_objects
---

# React Component Initial State for Nested Objects

At least a few times in my life, I have been tripped up by the situation I’m about to describe, so I’m going to take this opportunity to write out what I’ve learned this time in hopes that perhaps I will save someone else a few minutes of their life (even if that person is me in three months) (I really think I have the hang of it this time).

Here’s the scenario: you are building a React app that fetches data from an API. Let’s say that data is an array of nested objects representing movie theaters, the movies they are showing, and a few details about each movie. Your objects look something like this:

```js
    {
        Name: "Belcourt Theater",
        Phone: "(615) 846-3150",
        Address: {
            Address1: "2102 Belcourt Ave",
            Address2: "",
            City: "Nashville",
            StateOrProvince: "Tennessee",
            Country: "US",
            PostalCode: 37212
        },
        Movies: [
            {
                Title: "You've Got Mail",
                Runtime: 119,
                ScreenNumber: 3,
                Description: "Struggling boutique bookseller hates corporate chain store owner that just moved in across the street.",
                Cast: [
                    {
                        Name: "Meg Ryan",
                        Role: "Kathleen Kelly"
                    },
                    {
                        Name: "Tom Hanks",
                        Role: "Joe Fox"
                    }
                ]
            }
        ]
    }

```


Accessing information inside of a nested object is not a problem at all. Once you’ve got the object, simply chaining keys (plus a little bracket notation for arrays) will get you where you need to be (`<object>.Movies[0].Cast[0].Name == “Meg Ryan”`). But that’s not exactly what we’re talking about today. 

When you go to drop this object in state (either in Redux or a component’s local state), hopefully you are overwriting a default value. The biggest reason you need to have a default value when you initialize state is this: when you first render that component, you will be fetching the data from the API. That’s an asynchronous action, and thanks to the React component lifecycle, the fetch call that you most likely make within a `useEffect()` hook will not be finished by the time the component completes its first render. When it is trying to render, calling something like `undefined.Address` is going to raise an error, whereas something like `{}.Address` will not.

This is the tricky part though. Whereas `{}.Address` will not raise an error, it will evaluate to `undefined`, so if you then tried something like `{}.Address.City`, now you’ll find yourself with an error yet again, because you are trying to call `.City` on `undefined`!

There are two solutions to this that I have found. There is the *Validation solution* and the *Scaffold solution* (made up names, hoping they stick). Let’s look at the Validation solution first.

*Validation Solution*
---

In the Validation solution, what we are doing is validating that the prop passed into the component is in fact an object and not `undefined`. We do this before we render anything. 

```js
function isPropNotEmpty(obj) {
    return !!obj && obj.constructor === Object && Object.keys(obj).length > 0;
}
```

In this example, we have a function that takes in the object we wish to evaluate. We first make sure that the prop has a value at all by forcing it into a boolean and checking its truthiness. There are a couple of ways to do this, but I like the double bang operator `!!`. Then, if it does exist, we check to make sure it is an object by calling `.constructor` on it and checking to make sure that that is equivalent to `Object`. Finally, we want to make sure that the object is not empty, so we count the number of keys it contains and make sure that that number is greater than zero. 

In this case, we have wrapped all of this in a function that returns the evaluation of this expression, either `true` or `false`. We’ll want to return something from the component, so using an if/then statement is a good choice:

```js
if (isPropNotEmpty(prop)) {
    return (
        <div>{prop.Theater.Movies[0].Name}</div>
    )
} else {
    return (
        <div>Nothing to see here!</div>
    )
}
```

*Scaffold Solution*
---

As I mentioned before, best practice is to set meaningful default values when initializing state in React. If you know that that piece of state will eventually be an object, setting it to an empty object when it initializes helps prevent some weird behavior. However, if you are expecting that piece of state to become a nested object, and you know the shape of the objects that will populate it in the future, you can use an empty version of that object (a scaffold) to populate initial state, like so:

```js
    {
        Name: "",
        Phone: "",
        Address: {
            Address1: "",
            Address2: "",
            City: "",
            StateOrProvince: "",
            Country: "",
            PostalCode: 0
        },
        Movies: [
            {
                Title: "",
                Runtime: 0,
                ScreenNumber: 0,
                Description: "",
                Cast: [
                    {
                        Name: "",
                        Role: ""
                    }
                ]
            }
        ]
    }

```

This way, any values you try to access will return something and not cause an error, but they also won’t affect what the user sees on the first render. This is with the exception of the initial state for numbers, which should be set to `0` if you are okay with there being a zero in that place to start. If you aren’t, you can try passing `parseInt(‘’)` in as the initial value. 

With this solution, your presentational component shouldn’t be affected at all. 

If you know the shape of the nested objects that you will be receiving, the Scaffold solution is a good one, however, it can be difficult to scale to very large dimensions, and may not work if you don’t know the exact shape of your nested objects (such as if you are retrieving data from a large online API). If you are unsure about any of these things, using the Validation solution works just as well, you just have to decide what you will return from your presentational component in the event that the validation fails.

As always, thanks for reading, and I hope this helps!