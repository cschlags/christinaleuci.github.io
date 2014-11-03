---
layout: post
title: "Tictactoe in Ruby"
date: 2014-11-03 14:20:03 -0500
comments: true
categories: [Technical, Ruby]
---
This is a really quick Ruby tictactoe program that really feels like it's playing against you. 

<!--more-->

I created this a couple of months ago so it may not be the most optimized it can be but it works which is the important thing. What you must first do is define the parameters of the board

{%codeblock%}
#all the different slots that you can play
@slots = { 
  "a1"=>" ","a2"=>" ","a3"=>" ",
  "b1"=>" ","b2"=>" ","b3"=>" ",
  "c1"=>" ","c2"=>" ","c3"=>" "
}

#all 8 winning matches
@winning = [ 
  ['a1', 'a2', 'a3'],
  ['b1', 'b2', 'b3'],
  ['c1', 'c2', 'c3'],
  ['a1', 'b1', 'c1'],
  ['a2', 'b2', 'c2'],
  ['a3', 'b3', 'c3'],
  ['a1', 'b2', 'c3'],
  ['c1', 'b2', 'a3']
]
{%endcodeblock%}

The next thing you need to do is initialize the players, you and the computer. In the method below we define the computer's name as Computer and prints out the lines before taking in a user's name.

{%codeblock tictactoe.rb%}
# player names; introduction
@comp_name = "Computer"
puts "TIC TAC TOE"
puts "Player, what is your name?"
STDOUT.flush
@player_name = gets.chomp
{%endcodeblock%}

What we must now decide is who's turn it will be. I decided to create some logic that randomnly selects who will go first.

{%codeblock%}
# determine who is X or O
# use .5 because only 2 options [x or o]
@comp = rand() > 0.5 ? 'X' : 'O'
@player = @comp == 'X' ? 'O' : 'X'

#actually initiating the methods
 if(@player == 'X')
  puts ''
  puts "#{@player_name} has the first turn"
  player_turn
else
  puts ''
  puts "#{@comp_name} has the first turn"
  comp_turn
end
{%endcodeblock%}

At this point we have defined the parameters of the game, who the players are, and who will go first. What we must do now is draw the game and figure out the logic that allows the computer to be a worthy opponent. Let's start with an easy method to draw the board.

{%codeblock%}
#draw game method shows slots, empty and full
def draw_game
  puts "-----------------------------------------------------------------------------"

  puts "TIC TAC TOE"
  puts "#{@comp_name}: #{@comp}"
  puts "#{@player_name}: #{@player}"
  puts ""
  puts "   a   b   c"
  puts ""
  puts " 1 #{@slots["a1"]} | #{@slots["b1"]} | #{@slots["c1"]}"
  puts "   ---------"
  puts " 2 #{@slots["a2"]} | #{@slots["b2"]} | #{@slots["c2"]}"
  puts "   ---------"
  puts " 3 #{@slots["a3"]} | #{@slots["b3"]} | #{@slots["c3"]}"
  puts ""
end
{%endcodeblock%}

What this will print out is a board that contains the slot names ie. "a1...a2" in their slots. At this point we can start dealing with the more complicated logic. Let's begin by starting with the two methods we defined earlier when we figured out which player would go first.

Let's walk through the player_turn method. It begins by drawing the game and then asks the player to make a move by typing in a slot. Inside the method we check if the slot the user inputted is legitimate. If it's not we call the wrong_move or wrong_input method's but we'll cover that in just a second.

{%codeblock%}
def player_turn
  puts ""
  draw_game
  puts "#{@player_name}, please make a move or type 'exit' to quit"
  STDOUT.flush
  input = gets.chomp.downcase
  if input.length == 2
    a = input.split("")
    if(['a','b','c'].include? a[0])
      if(['1','2','3'].include? a[1])
        if @slots[input] == " "
          @slots[input] = @player
          puts ""
          puts "#{@player_name} marks #{input.upcase}"
      #puts times.times_in_column
          check_game(@comp)
        else
          wrong_move
        end
      else
        wrong_input
      end
    else
      wrong_input
    end
  else
    wrong_input unless input == 'exit'
  end
 end
{%endcodeblock%}

The wrong_move and wrong_input methods are very simple. If the input the user gives is wrong then the program prints out that the player inputed the wrong string. It calls the player_turn method again for the player to pick another turn.

{%codeblock%}
def wrong_move
  puts ""
  puts "You must choose an empty slot"
  player_turn
end

def wrong_input
  puts ""
  puts "Please specify a move with the format 'A3' , 'B1' , 'C2' etc."
  player_turn
end
{%endcodeblock%}

If we walk through the code at this point as a player we find ourselves at the check_game method. This is a main method in our program so let's take a break from the player path and delve more into the computer path so we can find ourselves again at the check_game method.

{%codeblock%}
def comp_turn
  move = comp_find_move
  @slots[move] = @comp
  puts ""
  puts "#{@comp_name} marks #{move.upcase}"
  check_game(@player)
end
{%endcodeblock%}

The first method we encounter here is the comp_find_move method. This will decide the next move that the computer will take before we check the game for a winner or loser. Let's go over the comp_find_move method now to go over the logic involved in the computer finding it's next move.

{%codeblock%}
def comp_find_move
  #choosing best comp choice 2 in column
  for column in [@winnings] do
    if times_in_column(column, @comp) == 2
      return empty_in_column column
    end
  end

  #defending against the player first
  for column in [@winnings] do
    if times_in_column(column, @player) == 2
      return empty_in_column column
    end
  end

  #third best comp choice only one in column
  for column in [@winnings] do
    if times_in_column(column, @comp) == 1
      return empty_in_column column
    end
  end

  #choosing an empty slot
  k = @slots.keys;
  j = rand(k.length)
  if @slots[k[j]] == " "
    return k[j]
  else
    #first empty slot
    @slots.each { |k,m| return k if m == " " }
  end
end
{%endcodeblock%}

This method figures out the best moves the computer can make to make sure it beats the player. The first if clause chooses a slot if the computer already has two slots in a line. The next if clause chooses a slot to defend against the player. The last if clause chooses a slot if the computer already has one slot in a row on the board. If all these if clauses have failed then the computer chooses an empty slot.

Let's go over the times_in_column and empty_in_column methods before we go over the check_game method.

{%codeblock%}
def times_in_column arr, item
  times = 0
 #arr.each do |i|
 for i in [arr] do
    if @slots[i] != " "
    times += 1
    unless @slots[i] == item || @slots[i] == " "
      return 0
    end
  end
  return times
  end
end

def empty_in_column arr
  arr.each do |i|
    if @slots[i] == " "
      return i
    end
  end
end
{%endcodeblock%}

These two methods are very simple. The times_in_column method count the slots that are in a column. So if there were three slots in a column it would return three. Similarly the empty_in_column method returns if there is an empty space in that column.

At this point we've handled all the minor methods, we have one more method to go, the check_game method. This will essentially tell us if there is a winner, a tie, or if the game needs to continue.

{%codeblock%}
#checking winnings or loses or continuings
def check_game(next_turn)
  game_over = false
  #@winnings.each do |column|
  for column in [@winnings] do
    #comp has won?
    if times_in_column(column, @comp) == 3 #three in a row
    puts ""
      puts "Game Over Dude, the Computer won!!!"
      game_over = true
    end
  
    #player has won?
    if times_in_column(column, @player) == 3 #three in a row
    puts ""
      puts "Game Over YOU win!!!"
      game_over = true
    end
  end
  unless game_over
    if(moves_left > 0) #no moves left
      if(next_turn == @player) #from comp_turn method
        player_turn
      else
        comp_turn #from player_turn method
    end
  
    else
      puts ""
      puts "Game Over -- DRAW!"
    game_over = true
    end
  end
end
{%endcodeblock%}

This method begins by setting a game_over value to false. In the first if loop we figure out if the computer has three slots in a row, if that's true the program outputs that the computer has won and the program stops. If the player has three slots in a row then the program outputs that the player has won and the program stops.

The final if statement checks to make sure that there are no more moves left. If there are more moves left then it figures out who has the next move. If there are no more moves left it outputs that the game has ended in a draw and the program stops.

If you haven't noticed we have one small problem, we still need to define the moves_left method. This last piece completes the program. This method goes through the board to see if there is another slot open for a move to be completed.

{%codeblock%}
def moves_left
  slots = 0
  @slots.each do |k, v|
    slots += 1 if v == " "
  end
  slots
end
{%endcodeblock%}

Congratulations you've completed a Ruby Tictactoe program! As with any good program there are always ways to optimize the code. Let me know what you think of the code.

