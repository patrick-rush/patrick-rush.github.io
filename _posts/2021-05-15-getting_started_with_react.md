---
layout: post
title:      "Getting Started with React: An Intro to Next.js"
date:       2021-05-15 10:00:00 +0000
permalink:  getting_started_with_react_nextjs
---

Last week, I took a close look at Create React App and all of the things that you get out of the box when you use `create-react-app` to bootstrap a React application. But, as I mentioned in my previous post, Create React App is not the only toolchain available to you when you go to create a new React application. While the [React Documentation](https://reactjs.org/docs/create-a-new-react-app.html#recommended-toolchains) recommends Create React App for getting started with React and creating single-page applications, it recommends Next.js for creating and building static and server-rendered websites with Node.js.

If you are unfamiliar with Node.js, the textbook definition found on [Node.js’s website](https://nodejs.org/en/) is “a JavaScript runtime built on Chrome’s V8 JavaScript engine.” In reality, what it does is allow for two-way communication between the client and the server, allowing for the free flow of data between the two. You might check out [this post](https://www.toptal.com/nodejs/why-the-hell-would-i-use-node-js) for a more in depth look at Node.js.

So besides the fact that Next.js expects a Node.js server, what are the benefits of Next.js as a toolchain? The two biggest benefits, in my opinion, are the styling functions and routing solutions included out of the box. 

### Styling
---

By default, Next.js includes [Styled JSX](https://github.com/vercel/styled-jsx), which is a CSS-in-JS library that enables you to include CSS styles directly in JSX within a component. The styles introduced in a component are encapsulated and scoped, meaning that they will not affect other components in your application. By including a `<style jsx>` element in the root element of a component and then filling it with styles using standard CSS syntax, appropriate class names are added to elements within the component so that styles are not applied elsewhere. 

Global styles for a specific component can be manipulated using the `global` attribute in another `<style jsx>` tag, like so: `<style jsx global>`. This will apply styles to all matching elements within your current component. Global styles for your entire app as well as external stylesheets can be imported into a file called `pages/_app.js` like so:

```js
// pages/_app.js
import 'bootstrap/dist/css/bootstrap.css'
// or import ‘../styles.css’

export default function YourApp({ Component, pageProps }) {
	Return <Component {...pageProps} />
}
```
(Above example from [Next.js docs](https://nextjs.org/docs/basic-features/built-in-css-support))

The inclusion of the Styled JSX library allows for some very intuitive manipulation of CSS styles throughout your application.

### Routing
---

Whereas Create React App doesn’t ship with a native routing solution, Next.js has a built in routing solution that is very easy to implement. For each file that is added to the `pages` directory, a corresponding route is made available throughout the app. That route can then be accessed via a `<Link>` component that is imported from `’next/link’` from elsewhere within the app. Nested routes follow the same structure as nested file paths and dynamic routes can be achieved using bracket syntax. This way, routes can be accessed from anywhere within the app, and do not need to be passed around the app within params. 

```js
// pages/app.js

import Link from 'next/link'

export default function Home() {
	return (
		<Link href="/posts">
	        <a>Posts</a>
        </Link>
    )
}
```

In my opinion, this out of the box solution to routing that is available within Next.js is very easy to use and is a bit more intuitive than React Router DOM, the popular solution within Create React App. 

---

After having created several applications using Create React App, getting familiar with Next.js has been really enjoyable. The documentation is excellent and the level of abstraction seems to be a good fit for the kinds of things I need it for at this time, such as creating a personal website. I recommend heading over to [Next.js’s official guide](https://nextjs.org/learn/basics/create-nextjs-app) if you decide to give it a shot. This guide will walk you through all of the basic setup of a React app using Next.js. Thanks so much for taking the time to read this guide to Next.js! 