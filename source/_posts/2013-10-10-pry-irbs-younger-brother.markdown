---
layout: post
title: "Pry - IRB's Smarter Younger Brother"
date: 2013-10-10 11:58
comments: true
categories: [Learning pry, pry gem, pry ruby, installing pry, byebug, pry-byebug ruby]
keywords: Learning pry, pry gem, pry ruby, installing pry, byebug, pry-byebug ruby
---
---
Pry is one of my favorite gems. It is a REPL (it reads, evaluates, prints, loops). Pry is a replacement for Ruby's default REPL, irb. Pry's advantages over irb are Pry adds additional features such as syntax-highlighting, auto indenting, and super useful debugging features. To install pry and its plugins (in this guide, I will also be using pry-byebug), perform the following commands:  

    $ gem install pry  
    $ gem install pry-byebug

Once you have those gems installed, you can enter into a session with pry, by just typing pry in your Terminal.

    $ pry

My absolute favorite feature of pry is its ability to load any portion of a file you are working on in your text editor directly into your REPL session. Let's say you are working in sublime text 2, so below is your prime.rb file:  

```ruby
#prime.rb
def prime?(num)
  (1..(num/2)).select {|element| num % element == 0}.length == 1
end
binding.pry
def every_prime(num)
  (1..num).each {|element| puts "#{element} is a prime? #{prime?(element)} "}
end
```

If I wanted to play around with these methods, I normally would have had to copy and paste everything into my REPL session. Right now, this doesn't seem like a big issue for my file as it is literally less than 10 lines long. However, imagine if I had a file that contains thousands of lines. Try copying and pasting something like that into your terminal (Don't actually try it, it may take a while).

So how does pry tackle this problem?  
*binding.pry*  

So how do we use binding.pry?

The way you do this is by requiring the pry gem at the beginning of the file and then use binding.pry wherever you want to REPL to load into.

```ruby
#prime.rb
def prime?(num)
  (1..(num/2)).select {|element| num % element == 0}.length == 1
end
binding.pry
def every_prime(num)
  (1..num).each {|element| puts "#{element} is a prime? #{prime?(element)} "}
end
```

*(notice how we put "binding.pry" after our first method "prime?(num)"; this causes the repl to only load the first method. If we wanted to play around with the "every_prime(num)" method, then we would have to put change the location of "binding.pry to the end of the file.).*     

All we need to do now is type "ruby primes.rb" in our terminal.   

    $ ruby primes.rb

Ok, so we've went through pry a little. Now, let's go through pry-byebug(use pry-debugger for ruby versions before 2.0).  We use pry-byebug in much the same way as using pry. Pry-byebug is really useful if you want to step through loops. Pry lets you see what certain variables are equal to at different points of your code.

To realize the full utility of pry-byebug, let's take a look at slightly more complex and messy code.

```ruby
#pigeon_organizer.rb
require 'pry'

pigeon_data = {
  :color => {
    :purple => ["Theo", "Peter Jr.", "Lucky"],
    :grey => ["Theo", "Peter Jr.", "Ms .K"],
    :white => ["Queenie", "Andrew", "Ms .K", "Alex"],
    :brown => ["Queenie", "Alex"]
  },
  :gender => {
    :male => ["Alex", "Theo", "Peter Jr.", "Andrew", "Lucky"],
    :female => ["Queenie", "Ms .K"]
  },
  :lives => {
    "Subway" => ["Theo", "Queenie"],
    "Central Park" => ["Alex", "Ms .K", "Lucky"],
    "Library" => ["Peter Jr."],
    "City Hall" => ["Andrew"]
  }
}

def reorganize(data)
  new_hash={}
  data.each do |attribute, specific_attrib|
    specific_attrib.each do |specific_attrib, birds_array|
      birds_array.each do |bird|
        binding.pry
        new_hash[bird] ||= {}
        if data[attribute][specific_attrib].include?(bird)
          if attribute != :color
            new_hash[bird][attribute] ||= specific_attrib.to_s
          else 
            if new_hash[bird][attribute]
              new_hash[bird][attribute].push(specific_attrib.to_s)
            else
              new_hash[bird][attribute] = [specific_attrib.to_s]
            end
          end
        end
      end
    end
  end
  new_hash
end

reorganize(pigeon_data)
```

What pry-byebug allows you to do is to step through your code. Notice how I put my binding.pry in the middle of my method. I do this so that when I open my file I go to that specific line of my code. I use pry so that I am able to see what variables equal at that specific point of code. This is extremely useful for debugging. Now open your ruby file again normally:

    $ ruby pigeon_organizer.rb
  
You will end up with something like this:  

{% img /images/screenshotforpry.png %}  

First type 'q' to enter back into your pry session.
    q

You can see a group of your local variables/methods with the 'ls' command. You can even type in the name of the variable to see what it is currently defined as (shown below).  

{% img /images/pryss2.png %}  

As this is just an intro. to Pry and Pry-byebug and there are so many different things to learn about them, I will let you explore these two gems on your own. In this pry session, some other commands that you may want to try on your own are 'help', 'cd', 'quit', 'step', 'next'.  

##**Other resources**  
Watch this video [Pry by Railscasts](http://www.youtube.com/watch?v=A494WFSi6HU)  
Read more about pry at [Pry github repo](https://github.com/pry/pry)  
Read more about pry-byebug at [Pry-byebug github repo](https://github.com/deivid-rodriguez/pry-byebug)  

Thanks for reading!

