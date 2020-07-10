---
layout: post
title: SOCCER BUDDY 64 
description: A Flatiron Mod 1 CLI application
summary: This is a cool cool blog
tags: [css]
---

### SOCCER BUDDY 64
![Soccer Buddy](https://raw.githubusercontent.com/matthewlawrenceklein/ruby-project-guidelines-chi01-seng-ft-062220/Jacob/Images/N64%20SUPPER%20SOCCER%20BUDDY%2064.jpg)

SOCCER BUDDY 64 is a CLI application built in Ruby using Sinatra ActiveRecord to create our model structure. Our database is seeded from a CSV file of the 2019-2020 English Premiere League soccer schedule. The app allows users to

- Create/Login to their own unique account, persisted in our database
- Search for league matches by team or stadium
- Add matches to their favorites list
- View, sort, and manipulate their favorites list

During the build process we experienced a handful of challenges. Some of the major obstacles included

- Parsing data from our original seed file, an XLS spreadsheet
- Setting up checks to validate user input and not throw errors
- Building out an intuitive and attractive UI

## Spreadsheet Hell - Seeding our database

Because of the current international pandemic most professional soccer leagues are shuttered, or operating on a much reduced capacity. This means that seeding our database from a live API would yield limited results. Instead, a quick search for _soccer database_ returned a fully fleshed out .XLS document file. Because .XLS is an outdated filetype (having been replaced with XLSX), there are only a couple of Ruby gems available that were able to parse the file. we ulitmately settled on the Spreadsheet gem, which worked correctly in a test environment. Unforunately, we faced a number of bugs when attempting to implement it in our working environment, with various errors targeting ActiveRecord and the gem itself. After a few hours of failed troubleshooting, we realized we could simply convert the spreadsheet file from XLS to CSV and use Ruby's built-in CSV library. This solutoin was up and running almost immediately! The only caveat here was the CSV parser was incorrectly interpreting the spreadseet "Date" and "Time" data, instead spitting out static and incorrect data. We decided to scrap implementing time/date functionality and instead focus on what worked. 

## Computers are dumb, and so are we

Another hurdle we faced was attempting to account for user error when at all possible. The first solution was to turn to TTY::Prompt, a ruby gem that enabled us to write user-selectable responses. Instead of asking for user responses, we were able to implement a menu of selectable options. 
{% highlight ruby %}
def add_to_favorites
   answer = $prompt.select("Would you like to add a match to your favorites?".colorize(:color => :black, :background => :light_green), "YES", "NO")
        case
        when answer == "YES"
            puts "Please select a match by ID"
            print "> "
            number = gets.chomp 
            system('clear')
            Favorite.create(user_id: $user.id, match_id: number)
            favoriteMatchesAre = TTY::Box.info("your favorites are now:")
            print favoriteMatchesAre 
            $user.reload
            $user.matches.each { |match| ap "#{match.home_team} play #{match.away_team} at #{match.location}." }
            
            display_options()
            user_input()
        when answer == "NO"
            system('clear')
            display_options()
            user_input()
        end
end
{% endhighlight %}

The other major validation checkpoint was upon login - We needed a way to check our database of users against the input to see if that user exists. If the user exists, than the current user should be assigned to that user ID. If not, the app should kick back an error and prompt the user for new input. The solution we developed is sloppy, but functional. 

{% highlight ruby %}
case
        when answer == "NEW USER"
            puts "Whats your name?".colorize(:color => :black, :background => :light_green)
            print "> "
            name = gets.chomp
            $user = User.create(name: name)
            welcomeUser = TTY::Box.success("Your user ID is #{$user.id}, please remember this number!")
            print welcomeUser
        when answer == "EXISTING USER"
            puts "Please enter your user ID"
            print "> "
            answer_id = gets.chomp   
            allIds = []
            User.all.each { |user| allIds << user.id }

            if allIds.include?(answer_id.to_i)
                    $user = User.all.find{ |user| user.id == answer_id.to_i }    
                    welcomeExistingUser = TTY::Box.success("Welcome back #{$user.name}")
                    print welcomeExistingUser
                else puts TTY::Box.error("USER ID NOT FOUND SORRY FRIEND")
                    
                    welcome()
            end
        end
{% endhighlight %}

If attempting to access an existing account, the user submits an ID #. An empty array is created, and all existing User.id info in the database is shoveled into the array. Then the array is checked to see if it includes the input data (converted to an integer). If it exists, the user is tied to the existing account. 

## And it looks nice, too! 

Once the app was functionally built out, we focused our attention on the user experience. This mainly took the form of contextual colorization, intuitive menu structures, a thorough README, and a demonstration video. 

## Stray thoughts

This Mod 1 project was an excellent crash course on code collaboration, time management, troubleshooting, googling, and all the other foundational skills that SWE careers are built on. I'd say overall we'd give the experience an A, A-. Solid project. Good job. 

![agile development](https://i.imgur.com/rJCPOVV.png)