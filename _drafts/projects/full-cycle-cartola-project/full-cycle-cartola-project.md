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
* golang on the microservice;
* kafka for messages;
* docker/kubernetes for infra;

The interactions between those parts are as follow:

* webbrowser interacts with front-end (next.js);
* front-end interacts with back-end (back-end/admin) and microservice;
* back-end interacts with Kafka;
* microservice interacts with Kafka and it is used as an aggregator;

The object is not mix the responsibility of back-end, which is create the actions of the plays and of players, with the aggregator microservice, which is to consolidate statistics. The microservice service must have resilience to avoir data lost. For that, Kafka is used. The microservice service delivery data through a REST endpoint.

## Understanding technologies



### Next.js

### Django/Python

### Golang/Microservice

### Kafka

### Docker/Kubernetes

## References
