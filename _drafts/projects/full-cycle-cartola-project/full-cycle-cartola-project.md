---
layout: page
title: full cycle cartola project
description: a seven-day course to develop an application similar to Cartola FC.
img: 
importance: 1
category: programming
---
## Introduction

This is a project based on a seven-day course from [Full Cycle Imersão](imersao.fullcycle.com.br), which aims to develop an application similar to [Cartola FC](https://ge.globo.com/cartola/). The similarity has the pedagogical approach.

The course can be followed accessing the link above and the source code of the application can be accessed[here](https://github.com/devfullcycle/imersao11).

"Cartola", here, isn't in the sense of a hat. It's a person who manages a soccer team and, for that, who tries to take advantages from that position.

## Requirements

The business requirements propose along the fist class were rank as next:

1. Just a clone from "Cartola FC" application;
2. The "cartola" (player) can choose the players of his team based on a list of players;
3. Each player has a price for negotiation;
4. The "cartola"'s team start with a budget, in way to constraint initially the acquisition of players;
5. The budget will change according to flow cash in buying or selling actions;
6. The "cartola" can select a player to play since the team does not in playing;
7. The aim: to select the best players to play and to have the most cash on hand;

## Technologies and basic architecture

Technologically speaking, this project uses:

* next.js on the front-end;
* python/django on the back-end/admin;
* golang on an aggregator service;
* kafka for messages;
* docker/kubernetes for infra;

The interactions between those parts are as follow:

* webbrowser interacts with front-end (next.js);
* front-end interacts with back-end (back-end/admin) and aggregator service;
* back-end interacts with Kafka;
* aggregator service interacts with kafka;

The objective is not mixing the responsibility of back-end, which is creating actions of the plays and of players, with the aggregator service, which is consolidating statistics. The services must have resilience to avoid data lost. For that, Kafka is used. The aggregator service delivery data through a REST endpoint.

## Understanding technologies

### next.js

Next.js is a popular JavaScript framework for building server-rendered React applications. It was created by Vercel (formerly known as Zeit) and is designed to make it easy for developers to create fast and scalable React applications. Next.js allows developers to write server-rendered React applications using server-side rendering (SSR), which can improve the performance of their applications and provide a better user experience. It also includes features such as automatic code splitting, which helps to improve the loading times of applications, and built-in support for CSS-in-JS libraries. Overall, Next.js is a powerful tool for building React applications that can be easily deployed and scaled.

Sure, from a technical and architectural perspective, Next.js is a framework for building server-rendered React applications. This means that when a Next.js application is loaded, the server will generate the initial HTML for the page, which is then sent to the client. This initial HTML includes the React components that make up the page, and the client-side JavaScript then takes over and "hydrates" the components, which means it binds the necessary event handlers and attaches the components to the existing HTML. This allows the application to be interactive on the client side, while still benefiting from the initial rendering on the server.

One of the key advantages of this approach is that it can improve the performance of the application, since the initial HTML is already generated on the server and does not have to be generated on the client. This can make the application feel faster and more responsive to the user. Additionally, server-side rendering can also improve the performance of applications for search engines, since the initial HTML includes the content of the page, which can be indexed by search engines.

Another important aspect of Next.js is its support for automatic code splitting. This means that Next.js will automatically split the JavaScript code of an application into multiple smaller chunks, and only load the chunks that are needed for a particular page. This can help to improve the loading times of an application, since the client does not have to download and execute unnecessary code.

Overall, Next.js is a well-designed framework that makes it easy for developers to create fast and scalable React applications using server-side rendering and other performance-enhancing features.

You might choose to use Next.js in any situation where you want to create a server-rendered React application. Some specific examples of when Next.js might be a good choice include:

If you want to create a web application that is optimized for performance and has fast loading times
If you want to create an application that is SEO-friendly, since server-side rendering can improve the performance of your application for search engines
If you want to create a large or complex application that requires code splitting to improve loading times and reduce the size of the JavaScript bundle
If you want to use server-side rendering to improve the perceived performance of your application and provide a better user experience.
Ultimately, the decision of whether to use Next.js will depend on your specific project requirements and goals. It is a powerful framework that can be a great tool for building high-performance React applications, but it may not be the best choice for every situation.

Yes, there are many popular apps that are built using Next.js. Some examples of popular apps that use Next.js include:

Twitch: The popular live streaming platform is built using Next.js.
ESPN: The website and mobile app for the sports network use Next.js.
Ticketmaster: The ticket sales and event discovery platform uses Next.js.
Zendesk: The customer service and support platform uses Next.js for its web app.
AWS AppSync: The real-time data synchronization service from Amazon Web Services is built using Next.js.
These are just a few examples of popular apps that use Next.js. There are many other well-known apps and websites that are built using the framework.

### Django/Python

### golang/microservice

### kafka

### docker/docker-compose

Docker and Docker Compose are both tools used for deploying and managing applications. Docker is a platform that allows you to package and run applications in containers, while Docker Compose is a tool for defining and running multi-container Docker applications.

Docker allows you to package an application and its dependencies in a virtual container that can be run on any Linux server. This makes it easy to deploy and run applications in a consistent environment, regardless of the underlying infrastructure.

Docker Compose, on the other hand, is a tool for defining and running multi-container Docker applications. It allows you to use a YAML file to configure your application's services, and then start and stop them all at once using a single command. This makes it easy to manage and deploy complex applications that are made up of multiple components.

In summary, Docker is a platform for running applications in containers, while Docker Compose is a tool for defining and running multi-container Docker applications. Both tools are commonly used together to develop and deploy applications in a consistent and efficient manner.

### sqlc

## References
