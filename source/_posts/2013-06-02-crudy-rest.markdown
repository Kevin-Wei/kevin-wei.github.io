---
layout: post
title: "CRUDy REST"
date: 2013-06-02 21:30
comments: true
categories:
---
###CRUDy RESTful Rails Resources

####Quickly CRUDy

This is a peak into what I've learned so far in my Ruby on Rails bootcamp at Codefellows.

So let me give you a quick overview on CRUD (Create, Read, Update, Delete).

These are the four basic operations you can do to data in a database.

Yup that was it. That was my overview.

####Restful Rails Resources basics

What does it have to do with the RESTful MVC on rails? MVC, in case you need a refresher, is just a paradigm that makes it easier to organize which parts of an application do what. Model should interact with data, and View should format and display it. The Controller is all the logic for **what** data should get fetched to **which** view and **how** it is sent.
HTTP verbs correspond to the CRUD actions, and the MVC in Rails maps them to certain URLs (paths) using an "action".

Unfortunately, if you ever actually used a website you also need "staging areas" so you can state what you're changing your data should be before the CRUD action really happens. So with our slight update to CRUD we get what I call CRUDy RESTful Rails Resources.

CRUD|Http Verb|Action|Path|Use case
:--|:--:|:--|:--|:--
Pre-create | Get | new | /resources/new | staging area to make our new data object
Create | Post | create | /resources | The actual making of it
Read | Get | show | /resources/:id | Show the thing
Read-all | Get | index | /resources/ | Show ALL THE THINGS
Pre-update | Get | edit | /resources/:id/edit | staging for what to change the thing
Update | Put | update | /resources/:id | The actual updating of the thing
Delete | Delete | destroy | /resources/:id | Remove the thing

So to make all this CRUD happen all you have to do is add a line to the routes.rb file:

{% codeblock routes.rb %}
  resources: things
{% endcodeblock %}

#### Some quick CRUDy RESTful tricks

But say you don't want to show ALL THE THINGS because there is only one of said thing. You can avoid the index option like this:

{% codeblock routes.rb %}
  resource: thing
{% endcodeblock %}

You can even nest a resource. This is useful when you have models with has_many and belongs_to relationships. For example, a product has many attributes, and an attribute belongs to a product.

{% codeblock routes.rb %}
  resources: product do
    resources attribute
  end
{% endcodeblock %}

This creates the expected paths that are similarly nested:

Action|Path
:--|:--
new |/products/:products_id/attributes/new
create | /products/:products_id/attributes
show | /products/:products_id/attributes/:id
index |/products/:product_id/attributes
edit | /products/:products_id/attributes/:id/edit
update | /products/:products_id/attributes/:id
destroy | /products/:products_id/attributes/:id

Great, but what if you don't want some of these things to be possible? It's easy to handpick using the rails :only and :except options. You can include / exclude whatever you want.

####Custom CRUDyness

Only want the index?

{% codeblock routes.rb %}
  resources: things :only => :index
{% endcodeblock %}

Want things to be unchangeable (immutable)?

{% codeblock routes.rb %}
  resources: things :except => [:edit, :update]
{% endcodeblock %}

Think the default word choice is awful? Yeah I agree. Rename them like so:

{% codeblock routes.rb %}
  resources: zerg :path_names => { :new => 'spawn', :edit => 'evolve' }
{% endcodeblock %}

This is pretty basic stuff. Feel free to fall asleep reading more about it [here](http://guides.rubyonrails.org/routing.html#overriding-the-named-helpers).

[So I lied in my Intro.](http://kevinthiswei.com/blog/2013/05/30/introduction/) This post was probably boring and humorless. I'm sorry.
