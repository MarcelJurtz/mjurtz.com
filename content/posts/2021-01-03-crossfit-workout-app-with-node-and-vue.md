---
title: 'Building a CrossFit Workout-App with Node.JS and Vue'
date: 2021-01-03T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2021/01/crossfit-workout-app-with-node-and-vue
categories:
  - development
tags:
  - webdevelopment
  - nodejs
  - vue
---

If you follow my blog a bit more closely, you might know that my most time-consuming hobby is triathlon. Strength training in the gym or at home is and remains part of it, but is more a means to an end. Then in October 2020 I had my first experience with CrossFit - (very) short, very intense workouts without too much equipment, leaving you with a feeling like the one you have after finishing a very hard interval session on the running track - I was immediately captivated. The basic idea of CrossFit workouts are so called WODs (Workouts of the Day), so you don’t usually train repetitive workouts, but always bring some variety into the mix. When you’re a coached CrossFit-Athlete, you usually don’t get to know the contents of todays WOD until you hit the gym (which are called “Box”). The sessions are usually named to make them easier to communicate. If you look up the term "WOD" on any search engine, you'll be inundated with related posts. One of these WODs is be the following:

```
Fran
21-15-9 Reps for Time
- Thrusters
- Pull-Ups
```

There are several different variations of such workouts. But in general, time always plays a big role. One variation is that the session must be completed (single or multiple reps) as quickly as possible (for time). Another one consists of exercises that change every minute (every minute on the minute, EMOM). The third option is that you have to do as many reps as you can in a given time (As many Reps as possible, AMRAP).

As I mentioned, most of the workouts are very spare in terms of equipment. And the stuff that is actually used also seems very "tech-hostile". Those who are looking for machines in CrossFit like you’re used to from training in regular gyms, with which individual muscle groups are trained in isolation, are looking in vain here. Common are barbells and dumbbells, kettlebells, skipping ropes, horizontal bars and the like. More complex equipment is required for cardio exercises, for which rowing ergometers, airbikes or ski ergometers are used.

Usually, CrossFitters train in a "box" similar to a gym - under the guidance of certified trainers. And this is where my problem starts. In the spring, all the gyms in Germany were closed. In the summer, I was then able to do my trial training. Only to directly get punched in the face by the next lockdown. But since the workouts often don't require much equipment, it shouldn't be a problem to do them from home and continue training independently, right? In theory, yes. Practically, though, when I was looking for workouts, I was blown away by the sheer volume of sessions available. The whole thing got worse when I wasn't just looking for sessions completely without equipment, but wanted to adapt them to the limited equipment I have available. It often took me some time to find workouts I didn't already know and which I could do with my equipment. Most of the websites I found only included workout catalogs, but without any options to filter these.

As a developer, situations like this always stick in your head and this situation got me thinking. Also, I wanted to get into fullstack JavaScript development for a while, and saw this as a good opportunity to improve the basics and learn the concepts of node.js and frontend development on a real example.

## The Requirements

My application consists of three main building blocks: A database that contains all available WODs. Besides name, description and excecises, each workout has to define its requirements in terms of equipment.

Furthermore, I need a server application that queries workouts on an API basis and takes available equipment into account via parameterized calls.

Finally, the third component is a frontend for visualizing the training session.

My approach is that the user should be able to specify what equipment is available, whereupon a random workout is suggested that matches those requirements. For now, no search function or catalog display is to be implemented.

Technologically, in addition to node.js for the backend, I chose MongoDB as the database and vue.js as the frontend framework. I don't have any technical reasons for these decisions, I just wanted to try both.

## Workouts

I started with the collection of the workouts. I currently use Google Sheets to manage them, in combination with Google App Sheets which allows me to write my own table functions in JavaScript. I use these to generate a JSON object from the workout. For example, the JSON version of the Fran-WOD above looks like this:

```json
{
  "name":"Fran",
  "description":"21-15-9 Reps for Time",
  "excercises":[
    "Thruster 95lbs",
    "Pull-Ups"
  ],
  "equipment":[
    "barbell",
    "pullupbar"
  ],
  "tag":"Benchmark-WOD",
  "permalink":"0BF8C0D2"
}
```

Each workout has a name, a description, the exercises to be performed, and the equipment needed to perform them. In addition, I generate an 8-digit id for each workout, which I want to use later as a way to uniquely identify workouts to be able to share them via permalink. Additionally, an optional tag can be stored to provide some grouping capability.

In this way I have created a small list of units to be able to test my API later in a practical way.

## The Backend

As soon as I had a small database available, I started to implement the server.

In terms of functionality, the structure is quite straightforward: via API call, an element is retrieved from the WOD database and returned in JSON format. I use express.js for the API and mongoose for the MongoDB access. First I took the JSON structure of the workouts into a mongoose model:

```javascript
const { Schema, model } = require('mongoose')

const WodSchema = new Schema({
    name: {
        type: String
    },
    permalink: {
        type: String
    },
    tag: {
        type: String
    },
    description: {
        type: String
    },
    excercises: Array,
    equipment: Array,
    created: {
        type: Date,
        default: Date.now
    }
})

const Wod = model('wod', WodSchema)
module.exports = Wod
```

Then, I created the code to manage the API-calls:

```javascript
const { Router, query } = require('express')
const Wod = require('../../models/wod')
const router = Router()

router.get('/random', async(req, res) => {
    try {
        // Get the count of all wods
        Wod.countDocuments({ "equipment": { "$nin": req.query.blacklist } }).exec(function(err, count) {

            if (count === 0) {
                throw new Error("No wods found");
            }

            // Get a random entry
            var random = Math.floor(Math.random() * count)
            console.log("found " + count + " elements for blackist")

            Wod.findOne({ "equipment": { "$nin": req.query.blacklist } }).skip(random).exec(
                function(err, result) {
                    if (err) {
                        throw new Error(err.message);
                    } else {
                        res.status(200).json(result);
                    }
                });
        })
    } catch (error) {
            res.status(500).json({ message: error.message })
    }
})

module.exports = router
```

So I have a single GET operation, listening to the path "/random", that passes the complement of the set of available equipment via a "blacklist" query parameter. This means I have a list of equipment available, none of which may appear in my workout. This is what the "$nin" call is for. First, my code checks if there are any elements with corresponding restrictions at all, then it randomly returns one of the matching elements.

Finally, the server.js file follows, in which all this is bound together:

```javascript
const express = require('express')
const app = express()
const mongoose = require('mongoose')
const { PORT, mongoUri } = require('./config.js')
const cors = require('cors')
const morgan = require('morgan')
const bodyParser = require('body-parser')
const wodRoutes = require('./routes/api/wods')
const path = require('path')

app.use(cors());
app.use(morgan('tiny'))
app.use(bodyParser.json());

mongoose
    .connect(mongoUri, {
        useNewUrlParser: true,
        useCreateIndex: true,
        useUnifiedTopology: true,
        useFindAndModify: false
    })
    .then(() => console.log('Connected to mongoDB'))
    .catch((err) => console.log(err));

app.use('/api/wod', wodRoutes)

app.listen(PORT, () => console.log(`App listening on http://localhost:${PORT}`))
```

And that’s it for the server already! By this point, I was able to run the API-Call with Insomnia, and everything seemed to work well.

## The Frontend

I wanted the UI to be super simple and easy to use. The main screen should contain the details of your current workout, and there should be options to load another random workout and edit settings to customize the equipment.

I’ve used Bulma as my CSS-Framework - also just to try it out. I came to the following layout: 

![Layout for the Workout-App](/assets/2021/wod-gen-base-layout.png)

The page contains the title of the workout (“The Running Sandwich”) and its contents. If the workout contains a description, it will be displayed right below the title. The blue “Getting Started”-Tag only appears when the workout has a tag defined.

The refresh-button loads another workout, the settings-button takes you to the settings-screen which looks like this:

![Settings Page for the Workout-App](/assets/2021/wod-gen-settings-layout.png)

The list contains all the equipment that is required by at least one of the workouts, so it might be extended when the workout-database grows. By default, everything is enabled.

But enough of the functionality, let’s get into some technical details. I’ll spare you to go over every little piece of code, you can find out more on that using the linked GitHub-Repo. But there are some parts I had some trouble dealing with, especially because I never used vue before.

There’s this thing with pages and components for example. Without any knowledge, I’ve been thinking of pages as separate, independent pages of content and components as reusable pieces of code, which could be for example the header, footer, or a loading indicator. And while I think that description is correct, it’s not too apply immediately. I was thinking of the header (Refresh- and Settings / WOD-Buttons) as components, as well as the footer. I’m using the Workout- and Settings-Elements only once each, so I thought of them as pages. However, this would make my app a multipage-application and felt like too much overhead for me. So I decided to put both elements in separate components and just toggle their visibility based on the headers button state.

Another major thing I had to get my head around is the way state is communicated between components. I’ve added a new component to display a loading indicator when a new workout is being fetched. This component should toggle visibility based on when a workout is actually loading. For this, I found the vuex store to be useful. Based on the [vue documentation](https://vuex.vuejs.org/guide/#the-simplest-store), this store is used to maintain the global application state and additionally - is reactive. So the communication between store and components should work like a two-way-databinding. So far, so good. I added a store configuration to my project and introduced some properties to it: One that contains the current workout, one that manages the blacklist of elements, and one that just states if a workout is being fetched right now. Oh, just a short info on the blacklist: You might have seen that the settings page manages more of a whitelist for the equipment instead of a blacklist. The mongo-request is easier with a blacklist though. Therefore, I decided to add a whitelist-property to the settings-component and a computed blacklist-property bound to it. Whenever I update the whitelist, I also update the blacklist with the following call:

```javascript
var blacklist = this.equipment
  .map(a => a.id)
  .filter(eq => !this.whitelist.includes(eq));
```

Back to the store: after figuring out how the store works, the rest of the implementation was quite easy: I updated the method to fetch workouts, so that it would call the respective method in the store, rather than to execute it directly. I also added a watcher on the isLoading-Property, which allowed me to bind that to a local property which again is bound to a loading-spinner.

And that’s it - The prototype works, but of course there’s more to come.

## What’s next?

I have a few more things I want this app to have. First off - the hosting. Right now, the app runs on a free heroku instance, just so that I can test it only and share it with my friends. I want to set up a custom domain and run a “production”-version of it, but I haven’t decided on a hosting option yet. I’m thankful for any recommendations!

The next part is that the workout database is very small. Right now, I have about 100 workouts in, but of course that’s nothing compared to the other websites that maintain the catalogs.

Also, I have some technical improvements to make. Right now, there’s nothing preventing the same workout from being loaded twice in a row. Of course, that’s not a big problem, especially if the workout database is getting bigger, but I’d like to update that. I’ve added ids to the workouts to make them shareable by permalinks, but that also isn’t implemented yet. If anybody knows a good way on how to manage urls like /wod/IUSA812A in the application, please let me know!

So that’s it for now - Enough of ideas to improve the app further and further, step by step. But as a simple project, basically just for me to learn vue, this was and still is a great beginning in my opinion. Feel free to share yours!

## Links

* [GitHub-Repository](https://github.com/MarcelJurtz/CF-WOD-Gen)
* [Live Prototype (Might need some time to start up!)](http://wod-gen.herokuapp.com/)