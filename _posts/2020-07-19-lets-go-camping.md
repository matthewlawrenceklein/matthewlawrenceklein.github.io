---
layout: post
title: Everything's on Fire - Let's go camping!
description: first steps into the Camping Ruby Micro Framework
summary: This is a cool cool blog
tags: [css]
---

## A visual introduction to Ruby frameworks

As we begin moving away from the terminal and into full-fledged web development, we start to examine and work within some Ruby frameworks. First, we touched on Sinatra. 
![a regular cat](https://images.unsplash.com/photo-1514888286974-6c03e2ca1dba?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2827&q=80)
Sinatra is a regular cat - not too smart, not too dumb, knows how to do some stuff, but you're gonna have to teach it to do most anything useful. I like regular cats. They're great to play with, and can even provide all sorts of teachable moments! As a first pet...i mean framework, Sinatra/regular cat is a solid jumping off point. 
### Then there's Rails
![a big lion](https://images.unsplash.com/photo-1471123327422-e370dc57a3da?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2422&q=80)
Rails is a lion - big, powerful, intimidating, and definitely not a candidate for your first pet. But, as we maybe (kind of?) learned from the Netflix series Tiger King, those that can harness the power of big cats can accomplish truly great (?) things. Github itself was built on a Rails backend! The first time you run `rails new Project` in your console and a few hundred files, folders, dependencies, and sub-libraries are generated in a flash is truly an awe-inspiring experience. Learning to navigate the file structures that rails builds for you, and slowly incorporating the various helper methods that rails provide is immensley gratifying. Heck, the first time I ran `rails g scaffold Student name age:integer....` and watched as it created all the requisite MVC files, classes, db migration, and so on that I had previously spent _hours_ learning to build out in a basic Sinatra app, I really began to understand all this talk of **rails magic**.

But ultimate, rails is an undertaking. It's extensive, layered, and has taken (me at least) hours upon hours of trial and error, googling, and headshaking to make progress in. I can imagine a scenario where I have an idea for a simple app, but maybe don't want to do all the legwork involved with getting a rails project off the ground. 
### Enter Camping
![kitten](https://images.unsplash.com/photo-1542736143-29a8432162bc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=3150&q=80)
Camping is a ruby micro-framework created by, as far as I can tell, a few weirdos who have more or less removed their presence from the internet. Camping is tiny - the entire framework source code is less than **4 kb**. What's more, Camping's core ethos is that your entire app can/should be contained in a single ruby document. Your MVC all exist simultaneously in one place. While you're free to build out other pages and `require_relative` them, Camping excels at being an agile launch pad for ideas. 

With the camping gem installed (it's recommended you install the camping omnibus, as it includes markaby - more on that in a bit), All you need to do to get rolling is drop `Camping.goes :AppName` at the top of your ruby document. Running `$ camping appname.rb` in console will fire up a live server, and your project is up and running! Start by building out some views 

{% highlight ruby %}
module SandwichLad::Views
    def layout
        html do
            title { 'Sandwich Lad' }
            body { 
                h1 'Welcome to Sandwich Lad'
                h2 "it's #{Time.now}, time to talk-a sandwich"  
                self << yield 
            
            }
            footer {
                h4 "COPYRIGHT IN PERPETUITY"
            }
        end
    end

    def landing
        body :id => :frontpage do
            h3 'its my sandwich world', :class => :main
            ul do    
                div do a 'add sandwich', :href => '/sandwiches/new' end 
                div do a 'see sandwiches', :href => '/sandwiches' end 
            end 
        end
    end
end 
{% endhighlight %}

and then associate those views with some controllers via routes 

{% highlight ruby %}
module SandwichLad::Controllers
    class Index < R '/'
        def get
            render :landing
        end
    end

    class NewSandwich < R '/sandwiches/new'
        def get
            render :new_sandwich    
        end
    end

    class IndexSandwich < R '/sandwiches'
        def get
            render :sandwiches 
        end
    end 
end
{% endhighlight %}

Notice all that inline html? That's markaby! Markaby (Markup as Ruby) is a relatively straightforward alternative to erb with lousy documentation, but solid usability! We can even link to an external css stylesheet to style our app. Now we're up to a whopping 2 files! 

Finally, we can build out a model and initial table migration. 

{% highlight ruby %}
module SandwichLad::Models
    class Sandwich < Base
    end

    class BasicFields < ActiveRecord::Migration[4.2]
        def self.up
          create_table Sandwich.create_sandwiches do |t|
            t.string :name
            t.text   :bread
            t.text   :cheese
            t.text   :proteins
            t.text   :condiments
            # This gives us created_at and updated_at
            t.timestamps
          end 
        end

        def self.down
            drop_table Sandwich.create_sandwiches
        end
    end
end


def SandwichLad.create
    SandwichLad::Models.create_schema
end

{% endhighlight %}

Our Basicfield class inherits from Active Record, and our schema gets created once the app is instanciated via the `SandwichLad.create` method. At this point we've written out a very basic MVC pattern in under 100 lines of code, in a single ruby document. Neat! And while Camping is surely not an alternative for larger, more in-depth projects, it's comforting to know there's an option to speedily map out app ideas. As the saying goes, the right tool for the job, the right cat for the app. 
![mirror cat](https://www.northumbriacommunity.org/wp-content/uploads/2013/05/kitten-lion-mirror.jpg)
