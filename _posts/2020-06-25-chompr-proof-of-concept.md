---
layout: post
title: chompr Pt. 1 -  Proof of concept
description: Pt. 1 - Proof of concept
summary: This is a cool cool blog
tags: [css]
---

### The starting point

I don't think I'm alone in saying that my first foray into software development began when I had an idea for an app that I wanted bad enough
that when I couldn't find it, I decided to try and learn how to make it myself. A few months later I've learned a little enough to know that I know
precious little, but I finally feel like I'm on a path where I can see the light at the end of the tunnel. 

I'm going to use these blog posts as an opportunity to chronicle the process of developing an app _alongside_ developing a skillset. Let's go!

### Chompr - Or, "IDK, what do you feel like?"

I've spent **way** too many nights laying on the couch with a friend or partner doing the back-and-forth dance of "what are you in the mood to eat". It's a vicious cycle of hunger, indecision, and ultimately, the resignation of eventually landing on a choice that no one's particularly excited for. Literally any alternative would be prefarable: a giant plinko board, prize wheel, or even... an **app**. Ideally an app that can help its users choose a meal option

* easily
* quickly
* intuitively
* fairly, with respect to each chooser 

At this point, I've put together a *wish list* of features/functionality that I'd like to pursue, with varying degrees of complexity. Of course, it's all pretty complex for me right now, but I'm attemping to tackle it in small chunks. For functionality, I'm hoping to build out an app that:

* accepts a geographic location (user-input zipcode? cellular location data? Google web location data?)
* allows for either delivery or take-out
* accepts a number of choosers, with names for each (I think the process starts to break down with 5+ choosers)
* provides the choosers with a randomized set of eight food categories (ie Thai, Korean, BBQ, Pub Fare)
* randomly selects a chooser to select a category to eliminate, within a time limit (30 second?)
* Once choosers have eliminated all but one option, the app instantiates a search for nearby restaurants in that category.

I've got some stretch goals too, including user auth and API integration (grubhub? uber eats?). And any mobile development is firmly in the stretch category. 

### Humble Beginnings - Let's write some code

I've got a couple of big ol' lists of stuff I don't know how to do yet up there, but I can start buy building some base funcionality. First, let's figure out a way to randomly generate eight food categories from a much larger "database" of catagories. 
{% highlight ruby %}
def create_choices
    options = %w[Thai Sushi Japanese Southern Korean Greek Armenian Italian Pizza Burgers 
    Sandwiches BBQ Mexican American Fast-Food Diner Chinese Pub-Fare]
    
    mixed = options.shuffle
    
    $choice_set = []

    (0...8).map { |i| $choice_set.push(mixed[i]) }
end 
{% endhighlight %}