---
title: "Fundamentals of Database"
description: ""
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "cs6400"
weight: 100
toc: true
---

## 1. Introduction

This lesson will teach us about:

* Databases
* Database Management Systems
* The topics in this course

## 2. Overview

We'll get a high-level overview of what a database is. We'll define it as a model of reality. That raises the questions:

1. Why use models at all? And
1. When we do use them, why use a database management system to create them?


## 3. Why Use Models?

Models:

* Can be used to examine or manage part of the real world
* are cheaper than experimenting on the real world

Models of reality include:

* Maps: this is the example we'll use in the lecture
* Model of the economy: Can be helpful to see how new laws and policies affect the real economy. It can be tough to find a model everyone agrees on, though.
* Tsunami warning systems: It may allow us to save lives, livestock, etc.
* Traffic signals: Can be helpful when we want to decide where to build roads

## 4. A Map is a Model of Reality

Here's a map, but it feels like something's wrong.

{{< img src="map-is-a-model-of-reality.png" alt="Upside down world map" class="border-0" >}}

We're used to North being on top, so the map feels wrong. Let's flip it around:

{{< img src="map-is-a-model-of-reality-2.png" alt="Squeezed inverted world map" class="border-0" >}}

Wait, this map feels squeezed horizontally. The problem is that conventionally, we're used to maps being somewhat true to scale. Another problem here is that the map is blue for the land, we usually use it for ocean. This is like a math teacher writing the functions $x(f), y(g)$, we're used to seeing $f(x), g(y)$.

Let's see another map.

{{< img src="map-is-a-model-of-reality-3.png" alt="Good world map" class="border-0" >}}

This map appears right. True to scale—although we know that the Mercator projection distorts the size of land near the poles, and the color of the land is green.

## 5. Example v3

(n.b. Don't look it me, that's the title of the lecture)

Let's look at a more detailed map. It's a map of Atlanta, Georgia, USA.

{{< img src="map-of-atlanta.png" alt="Map of Atlanta, Georgia, USA" class="border-0" >}}

On this map are:

* Interstates:
    * 285
    * 85
    * 75
    * I-20
* Smaller ones:
    * A yellow one
* Neighborhoods like EastPoint


Overall, we have:

* An alphabet to spell out words
* Colors to show different kinds of roads
* Symbols to illustrate things like forests

It's also important to know that there may be errors in a model. If we drew a random line on the map and pretended a road were there, it wouldn't exist in the real world. Similarly, country lines on a map are great to represent abstract ideas but they don't manifest physically in the real world. By the way, on the map, North is up and it's true to scale.

**Using a map may be a better idea than to experiement with reality,**  Let's say I need to get to the varsity from Georgia Tech. GA Tech is the star, and the Varsity is inside the city, I can look it up on the map and get there. I need to cross the interstate, and go south a half mile to get to the Varsity on the right.

In theory, I don't even need to use the map, I know the Varsity is in the city and that the 285 goes all the way around the city. I can drive all around until I hit the 285, and keep driving on it, ensuring I don't repeat roads until I hit the Varisty.

## 6. Message to Model Makers

The professor's message to model makers, A model:

* Is a means of communication, and the users should have some knowledge in common to communicate. For example, in the maps of the world, the model's common knowledge was:
    * North was up
    * It was true to scale
    * The colors have to be correct
* Only emphasizes relevant aspects of reality useful to the purpose at hand.
* Is described in a language. In the map of Atlanta, Georgia, we saw a language of symbols, letters, etc, and it reflected reality
* May have errors like a road in the incorrect location
* May have features don't exist in reality, like contour lines on a mountain, date lines (e.g. the [International Date Line](https://en.wikipedia.org/wiki/International_Date_Line)), etc.