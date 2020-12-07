---
layout: post
title:      "To Redux Or Not To Redux"
date:       2020-12-07 07:15:17 +0000
permalink:  to_redux_or_not_to_redux
---


**A short guide on when you should or shouldn't implement Redux in your React app**

As you might (or might not) know Redux is a JavaScript library used for managing state in an application. It is most commonly used in React apps but can also be utilized when working with other frameworks like Vue or Angular. In this guide I'll be refering to it's use in React, as that's where my experience lies. 

When deciding if you should use Redux or not in your application the most important thing to consider is going to be the scale of the application. The second thing is going to be the complexity of the application, specifically when it comes to what the state will managing within the application. I'd argue those are the main two factors as to whether it will be worth implementing Redux, as it can create unnecessary complexity to a small application that only consists of basic actions or simple UI changes, for example. If you are going to be fetching data from multiple data sources or you'd like to make sure your application is scalable for the future, Redux is most likely going to be of good use. Managing state between a growing number of components can become cumbersome and hard to maintain, as well as troubleshoot without the use a third party library like Redux to maintain the state. Predictability and maintainability are strengths that belong to the use of Redux. 

**Reasons to use Redux**
* Predictability due to the state and action being passed to a reducers that are pure functions, always producing the same result
* Maintainability due to the structure of how the code is generally organized (or should be) in an application utilizing Redux
* Makes an application easy to debug due to logging actions and state
* Can easily persist data in state to local storage to restore it after a page refresh
* Can be used for server-side rendering

**Reasons not to use Redux**
* There is no state to manage in your application
* Your application is simple with only a few components that make use of state
* You're still learning the basics of React and don't need to learn about the complexity that comes along with implementing and setting up Redux just yet
* You're only fetching data from a single source per view
* You don't need to manage server-side events
* You have your own method of managing state in your application

To sum it up, Redux can make managing state in a large/complex application much more simple and it's certainly a useful tool for a developer to make use of but it's definitely important to consider if you need it in the first place. 

