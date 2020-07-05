---
layout: post
title: chompr Pt. 1 -  Proof of concept
description: Pt. 1 - Proof of concept
summary: This is a cool cool blog
tags: [css]
---

### A Starting Point

I don't think I'm alone in saying that my first foray into software development began when I had an idea for an app that I wanted bad enough
that when I couldn't find it, I decided to try and learn how to make it myself. A few months later I've learned enough to know that I know
precious little, but I finally feel like I'm on a path where I can see the _app_ at the end of the tunnel. 

I'm going to use these blog posts as an opportunity to chronicle the process of developing an app _alongside_ developing a skillset. Let's go!

### chompr - Or, "IDK, what do you feel like?"

I've spent **way** too many nights laying on the couch with a friend or partner doing the back-and-forth dance of "what are you in the mood to eat". It's a vicious cycle of hunger, indecision, and ultimately, the resignation of eventually landing on a choice that no one's particularly excited for. Literally any alternative would be prefarable: a giant plinko board, prize wheel, or even... an **app**. Ideally an app that can help its users choose a meal option

![spin the choice](https://external-preview.redd.it/zC3dcSZFa0BOSk-AbVIqN3vwy7lIKwKnlzs5DGpAz5E.jpg?width=1024&auto=webp&s=2cb4f9c634b5a431978867eeaa765887f4b4bab8){:class="img-responsive"}


* easily
* quickly
* intuitively
* fairly, with respect to each chooser 

![lots of food](https://rciemecdn.imgix.net/API/Blog/tapas_800px.jpg){:class="img-responsive"}

At this point, I've put together a *wish list* of features/functionality that I'd like to pursue, with varying degrees of complexity. Of course, it's all pretty complex for me right now, but I'm attemping to tackle it in small chunks. For functionality, I'm hoping to build out an app that:

* accepts a geographic location (user-input zipcode? cellular location data? Google web location data?)
* allows for either delivery or take-out
* accepts a number of choosers, with names for each (I think the process starts to break down with 5+ choosers)
* provides the choosers with a randomized set of eight food categories (ie Thai, Korean, BBQ, Pub Fare)
* randomly selects a chooser to select a category to eliminate, within a time limit (30 second?)
* Once choosers have eliminated all but one option, the app instantiates a search for nearby restaurants in that category.

I've got some stretch goals too, including user auth and API integration (grubhub? uber eats?). And any mobile development is firmly in the stretch category. 

### Humble Beginnings - Let's write some code

I've got a couple of big ol' lists of stuff I don't know how to do yet up there, but I can start by building some base functionality. First, let's figure out a way to randomly generate eight food categories from a much larger "database" of catagories. 
{% highlight ruby %}
def create_choices
    options = %w[Thai Sushi Japanese Southern Korean Greek Armenian Italian Pizza Burgers 
    Sandwiches BBQ Mexican American Fast-Food Diner Chinese Pub-Fare]
    
    mixed = options.shuffle
    
    $choice_set = []

    (0...8).map { |i| $choice_set.push(mixed[i]) }
end 
{% endhighlight %}

Cool, so now we've got the ability to grab a new and randomized choice set every time. Now we should try and grab the number of chompers, and allow for naming each of them. Again, we'll probably want to keep the max chompers to 4. 

{% highlight ruby %}
    def get_users
        print "> "
        $users = $stdin.gets.chomp 

        case 
        when $users.to_i <= 4
            puts "~" * 15
            puts "super"
        when $users.to_i > 4
            puts "~" * 15
            puts "too many folx"
            get_users()
        else
            puts "~" * 15
            puts "invalid input"
            get_users()
        end
        puts "~" * 15
    end
{% endhighlight %}

And let's get names for each chomper

{% highlight ruby %}
    def name_users

        $user_array = []
        i = 0
    
        while i < $users.to_i do
            print "user #{i + 1} name > "
            $user_array[i] = $stdin.gets.chomp
            i += 1
        end

``
        puts "super, we've got"
        puts "~" * 15
        puts $user_array
        puts "~" * 15
    end
{% endhighlight %}

Finally, let's layout the logic for each elimination round. We want to 
* randomly select a chomper to make a selection
* show the chomper the current food options
* take input for the option they'd like to eliminate
* remove the option from the options set
* repeat until one option remains

Here's what I've got so far

{% highlight ruby %}
    def elimination_round
    
        while $choice_set.length > 1
            shuffled_users = $user_array.shuffle()
            puts "#{shuffled_users[0]}, you've been selected to eliminate an option"
            puts "what would you like to eliminate?"
            print "> "
            eliminate = $stdin.gets.chomp

            $choice_set.each do |name|
                if name == eliminate
                    $choice_set.delete(name)
                end
            end

            puts "your options are now"
            puts "~" * 15
            puts $choice_set
            puts "~" * 15
        end
    end
{% endhighlight %}

Excellent! Now we're making progress!
![happy racoon](https://media.giphy.com/media/40F4fLvOkInEk/giphy.gif)

### Coming Up Next

We've not got the basic logic functioning, and we're set to take user input from the command line. My next step is going to be packaging this functionality into a 
CLI ruby gem, and _shipping_ a first _completed_ product - so to speak. C'yall in a few weeks!