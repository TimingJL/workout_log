# Learning by Doing 

![Ubuntu version](https://img.shields.io/badge/Ubuntu-16.04%20LTS-orange.svg)
![Rails version](https://img.shields.io/badge/Rails-v5.0.0-blue.svg)
![Ruby version](https://img.shields.io/badge/Ruby-v2.3.1p112-red.svg)

Mackenzie Child's video really inspired me. So I decided to follow all of his rails video tutorial to learn how to build a web app. Through the video, I would try to build the web app by my self and record the courses step by step in text to facilitate the review.


# Project 8: How To Build A Workout Log In Rails       
We have the ability to create / edit / destroy workouts, and add multiple exercises to each workout with the amount of sets & reps for that exercise.


https://mackenziechild.me/12-in-12/8/      


### Highlights of this course
1. Workouts (Posts)
2. Exercises
3. Bootstrap
4. HAML


# Create A Workout Log
```console
$ rails new workout_log
```

Chage directory to the pin_board. Under `Gemfile`, add `gem 'therubyracer'`, save and run `bundle install`.      

Note: 
Because there is no Javascript interpreter for Rails on Ubuntu Operation System, we have to install `Node.js` or `therubyracer` to get the Javascript interpreter.

```console
$ bundle install
```

Then run the `rails server` and go to `http://localhost:3000` to make sure everything is correct.       

### Create our workout model
The first thing we want to do is create a model and a controller for our workout.       
```console
$ rails g model workout date:datetime workout:string mood:string length:string
$ rake db:migrate
```

To be continued...