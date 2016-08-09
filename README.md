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


### Create our workout controller
```console
$ rails g controller workouts
```


Then we want to add an index and we want the ability to create workout as well as edit and delete.      
In `config/routes.rb`
```ruby
Rails.application.routes.draw do
	resources :workouts
	root 'workouts#index'
end
```

In `app/controllers/workouts_controller.rb`
```ruby
class WorkoutsController < ApplicationController
	def index
	end
end
```

Before we create the index view, we wanna to add some rubygems.       
Open up the `Gemfile`
```
gem 'simple_form', github: 'kesha-antonov/simple_form', branch: 'rails-5-0'
gem 'haml', '~> 4.0.5'
gem 'bootstrap-sass', '~> 3.2.0.2'
```
Go back to our terminal. Run `bundle install` and restart our server.

#### Bootstrap
https://github.com/twbs/bootstrap-sass          

To use bootstrap-sass, first we need to rename `application.css` to `application.css.scss` in `app/assets/stylesheets/`.       
And then import Bootstrap in `app/assets/stylesheets/application.css.scss`:
```scss
@import "bootstrap-mincer";
@import "bootstrap";
```
Note:        
File to import not found or unreadable: 
bootstrap-sprockets Only for Twitter Bootstrap 3, bootstrap-sprockets is used.


Then in `app/assets/javascripts/application.js`, we import `//= require bootstrap-sprockets` under `//= require jquery` like:
```js
//= require jquery
//= require jquery_ujs
//= require bootstrap-sprockets
//= require turbolinks
//= require_tree .
```

#### Simple Form
https://github.com/plataformatec/simple_form        

In our terminal, we need to run the generator.     
Simple Form can be easily integrated to the Bootstrap. To do that you have to use the bootstrap option in the install generator, like this:
```console
rails generate simple_form:install --bootstrap
```


```
===============================================================================
  Be sure to have a copy of the Bootstrap stylesheet available on your
  application, you can get it on http://getbootstrap.com/.

  Inside your views, use the 'simple_form_for' with one of the Bootstrap form
  classes, '.form-horizontal' or '.form-inline', as the following:

    = simple_form_for(@user, html: { class: 'form-horizontal' }) do |form|
===============================================================================
```



In `app/views/workouts`, we create a new file and save it as `index.html.haml`
```haml
%h1 This is the Workouts#Index placeholder
```



# Ability To Create A Workout
We next need the ability to create a workout.       
So, we're going to define a few actions in `app/controllers/workouts_controller.rb`
```ruby
class WorkoutsController < ApplicationController
	before_action :find_workout, only: [:show, :edit, :update, :destroy]
	def index
	end

	def show
	end

	def new
		@workout = Workout.new
	end

	def create
		@workout = Workout.new(workout_params)
		if @workout.save
			redirect_to @workout
		else
			render 'new'
		end
	end

	def edit
	end

	def update
	end

	def destroy
	end

	private

	def workout_params
		params.require(:workout).permit(:date, :workout, :mood, :length)
	end

	def find_workout
		@workout = Workout.find(params[:id])
	end
end
```


In `app/views/workouts`,we create a new file and save it as `new.html.haml`.
```haml
%h1 New Workout

= render 'form'

= link_to "Cancel", root_path
```


In `app/views/workouts`,we also create an another new file and save it as `_form.html.haml`.
```haml
= simple_form_for(@workout, html: { class: 'form-horizontal' }) do |f|
	= f.input :date, label: "Date"
	= f.input :workout, label: "What area did you workout?", input_html: { class: "form-control" }
	= f.input :mood, label: "How were you feeling?", input_html: { class: "form-control" }
	= f.input :length, label: "How long was the workout?", input_html: { class: "form-control" }
	%br/
	= f.button :submit
```
![image](https://github.com/TimingJL/workout_log/blob/master/pic/new_job.jpeg)


In `app/views/workouts`,we create a new file and save it as `show.html.haml`.
```haml
#workout
	%p= @workout.date
	%p= @workout.workout
	%p= @workout.mood
	%p= @workout.length
```








To be continued...