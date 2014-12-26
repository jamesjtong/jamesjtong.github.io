---
layout: post
title: "BASHing Commands from Within a Ruby Script"
date: 2013-11-20 10:21
comments: true
categories: [Bash, Ruby, Blackjack]
keywords: bash, ruby, blackjack, clear, command line
---

Recently, I wanted to create a blackjack program that incorporates a real life blackjack card-counting situation. It needed to be realistic so it would need a pause. Additionally, I wanted it to clear the screen every once in a while so that the user won't be overwhelmed with the heavy text from previous rounds. I realized that some of the bash commands would be an instant remedy to my problems. Then, I wondered what is the best way to incorporate these bash commands in my script. The 3 ways that I know are using

1) Using system  
    system('ls')

2) Using backticks `  
    `ls`

3) Using built in syntax %x()  
 NOTE: that for x you can replace it with any character; it just acts as a delimiter  
     %x(ls)

There are subtle differences between these 3. The most important difference is regarding there return values. System returns true or false depending on if the command was able to be executed, while the other 2 return the result of the backtick command. This is an extremely important distinction as my 'clear' command in my blackjack game would not work with backticks or the built in syntax. This is because clear needs tooutputs ANSI sequences to clear the screen; if I just returned clear (using backticks or the build in syntax), my screen would not get cleared.If I wanted it to be executed correctly, I would need to do a system('clear'); the other 2 ways of bashing commands wouldn't work.     

An actual code snippet from my Counting Cards Blackjack Game, utilizing BASHing within a Ruby Script:  

    def dealer_hits_until_17
      @new_hand = false
      while hand_score(@dealer_hand) < 17 && @new_hand == false
        puts "Dealer Hits! His hand is now #{@dealer_hand}"
        system('clear')
        hit(@dealer_hand)
        `sleep 1`
      end
      if @new_hand == false
        compare_hand_value
      end
    end