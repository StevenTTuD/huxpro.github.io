---
layout: post
title: Discover Meteor Ch5：Routing
published: true
date: 2014-08-05 07:18
tags:
  - Meteor
  - javascript
comments: true

---
現在要做的事情是點選http://myapp.com/posts/post_id，就可以對應到post的網頁。


加入iron router這個package。
$ mrt add iron-router

#知識補充站

##Iron Router
Not only does it help with routing (setting up paths), but it can also take care of filters (assigning actions to some of these paths) and even manage subscriptions (control which path has access to what data). (Note: Iron Router was developed in part by Discover Meteor co-author Tom Coleman.)

##Router Vocabulary

- **Routes**: A route is the basic building block of routing. It's basically the set of instructions that tell the app where to go and what to do when it encounters a URL.
- **Paths**: A path is a URL within your app. It can be static (/terms_of_service) or dynamic (/posts/xyz), and even include query parameters (/search? keyword=meteor).
- **Segments**: The different parts of a path, delimited by forward slashes (/).
- **Hooks**: Hooks are actions that you'd like to perform before, after, or even during the routing process. A typical example would be checking if the user has the proper rights before displaying a page.
- **Filters**: Filters are simply hooks that you define globally for one or more routes.
- **Route Templates**: Each route needs to point to a template. If you don't specify one,the router will look for a template with the same name as the route by default.
- **Layouts**: You can think of layouts as one of those digital photo frames. They contain all the HTML code that wraps the current template, and will remain the same even if the template changes.
- **Controllers**: Sometimes, you'll realize that a lot of your templates are reusing the same parameters. Rather than duplicate your code, you can let all these routes inherit from a single routing controller which will contain all the routing logic.

##The /lib folder
Anything you put inside the /lib folder is guaranteed to load first before anything else in your app . This makes it a great place to put any helper code that needs to be available at all times.
A bit of warning though: note that since the /lib folder is neither inside /client or /server, this means its contents will be available to both environments.

#Layout, Template與{{yield}}
This {{yield}} helper will define a special dynamic zone that will automatically render whichever template corresponds to the current route
![](https://lh5.googleusercontent.com/fF4eK0g64rmCDr44cdBam6d--3ldM46wZK0Oalhftws=w1192-h986-no)

#Routing的使用方式

2. 建立layout
3. 建立route.js