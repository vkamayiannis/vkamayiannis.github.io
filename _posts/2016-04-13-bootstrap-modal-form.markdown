---
layout: post
title:  "Bootstrap modal form"
date:   2016-04-13 23:34:26
categories: jekyll update
comments: true
---
I started creating again some simple applications in Rails so I tried do enhance things a bit with some Bootstrap 
modal forms. But since I'm not a Javascript/jQuery/I_don't_know_what_else guy, I tried to find resources in order to
understand and implement something like that. Since it was really hard to find some solutions, I came up to the idea to
create this post in order to have something as a future reference (together with a demo app in Github).

<h3>The concept</h3>
The concept was to create a task list (yeah I know, such an original idea). I wanted to have the ability to create a
new task or see the contents of a task using modal forms. Anyways, enough with the talking. 

First, we need to create a new Rails app. So
{% highlight powershell %}
rails new app modal-form-demo
{% endhighlight %}

At this point we've created our new application. Before we bundle install it, we can change the Gemfile in order to 
add the gems to enable Bootstrap support. Add the following in the end of your Gemfile

{% highlight ruby %}
gem 'coffee-script-source', '1.8.0'
{% endhighlight %}

Then run bundle install and the bootstrap gem will be installed. 

Then our first model. We need to create a model Task with a title, a summary and a due date

{% highlight powershell %}
rails g scaffold Task title:string summary:text due_date:date
rake db:migrate
{% endhighlight %}

So we've generated the model, the controller and the views (scaffolding) and also we ran the migration on the database.
So let's run the server in order to create a couple of tasks just to make our changes for bootstrap.

Before we make any other change we must first enable our application to use Bootstrap. First in out app/assets/javascripts/application.js file
{% highlight ruby %}
//= require bootstrap-sprockets
{% endhighlight %}
we need to add this line before the require_tree.

After that on app/assets/stylesheets/application.css we need to first rename the file to have the scss extension 
and then add the following at the end of the file:

{% highlight ruby %}
@import "bootstrap-sprockets";
@import "bootstrap";
{% endhighlight %}


[koding]:      http://koding.com