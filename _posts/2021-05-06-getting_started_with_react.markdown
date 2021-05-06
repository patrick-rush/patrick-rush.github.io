---
layout: post
title:      "Getting Started with React: An Intro to Create-React-App"
date:       2021-05-05 10:00:00 +0000
permalink:  getting_started_with_react
---

![create-react-app](https://i.imgur.com/QnKni4J.png)

The very first thing I did when I started learning how to build applications with React was run `create-react-app`. Then I sat back and watched as my terminal worked some magic and then voila!, out came a fresh React app, ready to be molded into my new idea. At that point, I was a bit too excited about getting in there and building something to really spend time figuring out what exactly `create-react-app` did for me, but when it came time to build more applications, I began to wonder, what exactly was going on under the hood?

The answer is _a lot_. There are all kinds of things that go into creating a web application: package managers, bundlers, compilers, linters, testing suites, and the list goes on. When you go to create a web application using the React JavaScript library (and many other frameworks as well), you could certainly do so by individually creating the necessary files, installing the required components, and filling in all of the requisite boilerplate, but thankfully, you don’t have to! There are tools designed to do just this. 

In the [React documentation](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app), you will see these tools called Toolchains. A toolchain is a collection of tools that bootstrap an application for you in order to allow you to skip straight to the things that are more specific to the application you are trying to build. Create-React-App is the toolchain that is often recommended to React newbies. It is a good place to start learning about React and is perfect for building single-page applications, according to the React docs. 

Let’s dive into exactly what you get when you use the Create-React-App toolchain.

---
### Directory Structure

First off, you get a basic directory structure that consists of a package directory, a public folder for static files, an application root directory filled with the necessary application components and utility files, and a few other files in the application directory such as a README.md. Not all of the files included in the directory structure are strictly necessary, and some pruning may be in order to trim the app down to the files necessary for the specific application, but this alone can save a good deal of time that would be necessary to build the file structure needed for a React application.

### Bundling and Transpiling

[Webpack](https://webpack.js.org/) is a module bundler that parses through the entire application and creates a dependency graph that it then uses to start a development server that is live and interactive. As code changes in the editor, the app running in the browser automatically reflects that change. [Babel](https://babeljs.io/) is a transpiler (think of it as a translator for programming languages) used by Webpack to turn source code into something that can be understood by a wide range of browsers more easily, avoiding issues with compatibility. The development server can be run using the command `yarn start` or `npm start`. Create-React-App also handles creating a production environment using `yarn build` or `npm run build` which creates a build folder and an optimized version of your code. 

### Linting

Create-React-App includes [ESLint](https://eslint.org/) for making errors apparent during development. 

### Testing

The [Jest](https://jestjs.io/) testing framework is included when using Create-React-App. Jest is designed to work out of the box as a testing framework for React apps, and can be run by using the command `yarn test` or `npm run test`. 


---
This is the core of Create-React-App, though there are many smaller pieces and packages that are included as well that serve various functions ([Husky](https://github.com/typicode/husky), [Lerna](https://github.com/lerna/lerna), [Meow](https://github.com/sindresorhus/meow), [etc](https://github.com/facebook/create-react-app/blob/master/package.json).). 

If you are just starting out with React, or you are creating a single-page application, Create-React-App is an excellent way to get started. Check out the documentation [here](https://github.com/facebook/create-react-app) and then run `npx create-react-app your-app-name` in the directory of your choice!

---

### Resources for this post:
- [React Docs](https://reactjs.org/docs/create-a-new-react-app.html)
- [Create-React-App on Github](https://github.com/facebook/create-react-app)
- [Set Up React Toolchain From the Ground Up](https://dev.to/_elmahdim/set-up-react-toolchain-from-the-ground-up-3340) by Mahmoud Elmahdi
- [You Could Build a Toolchain](https://blog.logrocket.com/creating-a-react-app-toolchain-from-scratch/#:~:text=You%20could%20build%20a%20toolchain,tools%20or%20the%20learning%20curve.&text=You%20could%20build%20a%20toolchain,tools%20or%20the%20learning%20curve) by Adebiyi Adedotun

