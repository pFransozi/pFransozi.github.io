<!-- ---
layout: page
title: full cycle cartola project
description: a seven-day course to develop an application similar to Cartola FC.
img: 
importance: 1
category: programming
--- -->
## Introduction

This is a project based on a seven-day course from [Full Cycle Imersão](imersao.fullcycle.com.br), which aims to develop an application similar to [Cartola FC](https://ge.globo.com/cartola/), just for a pedagogical purpose.

The course can be followed accessing the link above and the source code of the application can be accessed [here](https://github.com/devfullcycle/imersao11).

"Cartola", here, isn't in the sense of a hat. It's a person who manages a soccer team and, for that, who tries to take advantages from that position.

## Business Requirements

The business requirements propose along the fist class were as next:

1. The Cartola (player) can choose the soccer players of his team based on a list of players; Each soccer player has a price;
2. The Cartola's team start with a budget, in way to constraint initially the acquisition of soccer players; The budget will change according to flow cash in buying or selling actions;
3. The Cartola can select a player to play, since the team does not in playing;
4. The aim: to select the best players to play and to have the most cash on hand;

## Basic Stack

The project is a full stack one, including technologies of infra, front-end and back-end. Let's take a look at a brief description:

* next.js on the front-end;
* python/django on the back-end/admin;
* golang on an aggregator service;
* kafka for messages;
* docker/kubernetes for infra;

The interactions between those parts are as next:

* webbrowser interacts with front-end (next.js);
* front-end interacts with back-end (back-end/admin) and aggregator service;
* back-end interacts with Kafka;
* aggregator service interacts with kafka;

The goal is not to mix responsibility of back-end, which is to create actions of the plays and players, with the aggregator service, which is to consolidate statistics. The services must have resilience to avoid data lost. For that, Kafka is used. The aggregator service delivery data through a REST endpoint.