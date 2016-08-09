# Learning by Doing 

![Ubuntu version](https://img.shields.io/badge/Ubuntu-16.04%20LTS-orange.svg)
![Rails version](https://img.shields.io/badge/Rails-v5.0.0-blue.svg)
![Ruby version](https://img.shields.io/badge/Ruby-v2.3.1p112-red.svg)

Mackenzie Child's video really inspired me. So I decided to follow all of his rails video tutorial to learn how to build a web app. Through the video, I would try to build the web app by my self and record the courses step by step in text to facilitate the review.


# Project 8: How To Build A Workout Log In Rails       
We have the ability to create / edit / destroy workouts, and add multiple exercises to each workout with the amount of sets & reps for that exercise.


https://mackenziechild.me/12-in-12/8/      


![image](https://github.com/TimingJL/workout_log/blob/master/pic/index.jpeg)
![image](https://github.com/TimingJL/workout_log/blob/master/pic/exercise_styling.jpeg)


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
![image](https://github.com/TimingJL/workout_log/blob/master/pic/new_workout.jpeg)


In `app/views/workouts`,we create a new file and save it as `show.html.haml`.
```haml
#workout
	%p= @workout.date
	%p= @workout.workout
	%p= @workout.mood
	%p= @workout.length
```


# List Out All Of Workouts

Next thing we wanna do is if we go to `http://localhost:3000/`, we want to list out all the workouts.           
In `app/views/workouts/index.html.haml`
```haml
- @workouts.each do |workout|
	%h2= link_to workout.date, workout
	%h3= workout.workout
```

And in our controller `app/controllers/workouts_controller.rb`
```ruby
def index
	@workouts = Workout.all.order("created_at DESC")
end
```
![image](https://github.com/TimingJL/workout_log/blob/master/pic/list_out.jpeg)


# Update And Destroy Workouts
Let's next add the ability to update and destroy workouts.


### Update/Edit
In `app/controllers/workouts_controller.rb`
```ruby
def update
	if @workout.update(workout_params)
		redirect_to @workout
	else
		render 'edit'
	end
end
```

And in our `app/views/workouts/show.html.haml`
```haml
#workout
	%p= @workout.date
	%p= @workout.workout
	%p= @workout.mood
	%p= @workout.length

= link_to "Back", root_path |
= link_to "Edit", edit_workout_path(@workout)
```

In `app/views/workouts/`, we're going to create a new file and save it as `edit.html.haml`
```haml
%h1 Edit Workout

= render 'form'

= link_to "Cancel", root_path
```
![image](https://github.com/TimingJL/workout_log/blob/master/pic/edit_workout.jpeg)


### Delete
In `app/controllers/workouts_controller.rb`
```ruby
def destroy
	@workout.destroy
	redirect_to root_path
end
```

Then back in our `app/views/workouts/show.html.haml`
```haml
#workout
	%p= @workout.date
	%p= @workout.workout
	%p= @workout.mood
	%p= @workout.length

= link_to "Back", root_path
= link_to "Edit", edit_workout_path(@workout)
= link_to "Delete", workout_path(@workout), method: :delete, data: { confirm: "Are you sure?" }
```

# Structure And Styling
http://getbootstrap.com/css/       


### Navbar
Under `app/views/layouts/`, we change the `application.html.erb` to `application.html.haml`.
```haml
!!!
%html
%head
	%title Workout Log Application
	= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true
	= javascript_include_tag 'application', 'data-turbolinks-track' => true
	= csrf_meta_tags
%body
	%nav.navbar.navbar-default
		.container
			.navbar-header
				= link_to "Workout Log", root_path, class: "navbar-brand"
			.nav.navbar-nav.navbar-right
				= link_to "New Workout", new_workout_path, class: "nav navbar-link"

	.container
		= yield
```
![image](https://github.com/TimingJL/workout_log/blob/master/pic/navbar.jpeg)

# Add The Exercises
Next, what I wanna do is add the excercises that we did for each workout.        
What we need to do is create an excercise model.     
```console
$ rails g model exercise name:string sets:integer reps:integer workout:references
$ rake db:migrate
```

Now, we need to add an association between exercise and workout.        
In `app/models/workout.rb`
```ruby
class Workout < ApplicationRecord
	has_many :exercises, dependent: :destroy
end
```
Note:
`dependent: destroy` means if you delete a workout, its exercises are deleted as well.

And next, we need to add routes:
In `config/routes.rb`
```ruby
Rails.application.routes.draw do
	resources :workouts do
		resources :exercises
	end
	root 'workouts#index'
end
```

Next, let's generate a controller.           
```console
$ rails g controller exercises
```

In `app/controllers/exercises_controller.rb`
```ruby
class ExercisesController < ApplicationController
	def create
		@workout = Workout.find(params[:workout_id])
		@exercise = @workout.exercises.create(params[:exercise].permit(:name, :sets, :reps))

		redirect_to workout_path(@workout)
	end
end
```

Next, we need to create our views for the exercises.        
Under `app/views/exercises`, we new a file and save as `_exercise_html.haml`. And another one save as `_form.html.haml`.      
In `app/views/exercises/_form.html.haml`
```haml
= simple_form_for([@workout, @workout.exercises.build]) do |f|
	= f.input :name, input_html: { class: "form-control" }
	= f.input :sets, input_html: { class: "form-control" }
	= f.input :reps, input_html: { class: "form-control" }
	%br/
	= f.button :submit
```

In `app/views/exercises/_exercise_html.haml`
```haml
%p= exercise.name
%p= exercise.sets
%p= exercise.reps
```


In `app/views/workouts/show.html.haml`
```haml
#workout
	%p= @workout.date
	%p= @workout.workout
	%p= @workout.mood
	%p= @workout.length

#exercises
	%h2 Exercises
	= render @workout.exercises

	%h3 Add an exercise
	= render "exercises/form"

= link_to "Back", root_path
= link_to "Edit", edit_workout_path(@workout)
= link_to "Delete", workout_path(@workout), method: :delete, data: { confirm: "Are you sure?" }
```
![image](https://github.com/TimingJL/workout_log/blob/master/pic/exercise_form.jpeg)


Let's go back to our `app/views/workouts/index.html.haml`
```haml
#index_workouts
	- @workouts.each do |workout|
		%h2= link_to workout.date.strftime("%A %B %d"), workout
		%h3
			%span Workout:
			= workout.workout
```


### Styling
http://uigradients.com/      

http://css3generator.com/           


In `app/assets/stylesheets/application.css.scss`
```scss
/*
 * This is a manifest file that'll be compiled into application.css, which will include all the files
 * listed below.
 *
 * Any CSS and SCSS file within this directory, lib/assets/stylesheets, vendor/assets/stylesheets,
 * or vendor/assets/stylesheets of plugins, if any, can be referenced here using a relative path.
 *
 * You're free to add application-wide styles to this file and they'll appear at the bottom of the
 * compiled file so the styles you add here take precedence over styles defined in any styles
 * defined in the other CSS/SCSS files in this directory. It is generally better to create a new
 * file per style scope.
 *
 *= require_tree .
 *= require_self
 */

@import "bootstrap-sprockets";
@import "bootstrap";

html {
	height: 100%;
}

body {
	background: -webkit-linear-gradient(90deg, #616161 10%, #9bc5c3 90%); /* Chrome 10+, Saf5.1+ */
  background:    -moz-linear-gradient(90deg, #616161 10%, #9bc5c3 90%); /* FF3.6+ */
  background:     -ms-linear-gradient(90deg, #616161 10%, #9bc5c3 90%); /* IE10 */
  background:      -o-linear-gradient(90deg, #616161 10%, #9bc5c3 90%); /* Opera 11.10+ */
  background:         linear-gradient(90deg, #616161 10%, #9bc5c3 90%); /* W3C */
}

.navbar-default {
	background: rgba(250, 250, 250, 0.5);
	-webkit-box-shadow: 0 1px 1px 0 rgba(0,0,0,.2);
	box-shadow: 0 1px 1px 0 rgba(0,0,0,.2);
	border: none;
	border-radius: 0;
	.navbar-header {
		.navbar-brand {
			color: white;
			font-size: 20px;
			text-transform: uppercase;
			font-weight: 700;
			letter-spacing: -1px;
		}
	}
	.navbar-link {
		line-height: 3.5;
		color: rgb(48, 181, 199);
	}
}

#index_workouts {
	h2 {
		margin-bottom: 0;
		font-weight: 100;
		a {
			color: white;
		}
	}
	h3 {
		margin: 0;
		font-size: 18px;
		span {
			color: rgb(48, 181, 199);
		}
	}
}

```

![image](https://github.com/TimingJL/workout_log/blob/master/pic/index.jpeg)


Finished!