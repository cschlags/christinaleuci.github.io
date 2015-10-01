---
layout: post
title: "When Ruby sends you some X's and O's"
date: 2015-09-30 15:37:10 -0400
comments: true
categories: [technical, ruby, rails]
---

New and improved Tic Tac Toe program! Now with Rails! Call now and receive a second one for 50% off!
<!--more-->

I recently interviewed at a company that asked that my first coding challenge assignment be an object-oriented Tic Tac Toe game. Good news I had a Tic Tac Toe game from when I applied to Flatiron. Bad news it wasn't object oriented. So it was time to map out how my new code was going to work, what new classes I would need to create, and what logic I would need to optimize. This will be a little longer than my current posts so you should probably grab a cup of hot chocolate and settle in for some intense programming and my subtle corny jokes.

The first thing I did was map out what classes I would need because as an object-oriented program I would need to have one class interact with another class. I ultimately decided on having a Game, Player, and Computer class. The Game class is where I have all my intial setup like creating the board, creating the players, and accessing the player moves. The Player class is where I have the logic behind assigning a Player's move. Lastly, the Computer class is where I have the logic behind the Computer's move.


## The Setup
Let me walk you through the Game class. Since this is the main class that will access the other classes I first required the Player and Computer classes using `require_relative`, then I created a game initialization at the bottom.

{% codeblock [game.rb] %}
  require_relative "./player"
  require_relative "./computer"

  class Game
    def initialize

    end
  end

  game = Game.new
{% endcodeblock %}

From there I created a `create_game` method that called on the setup methods and game play methods that the game would use. I called this method in the initialize method.

{% codeblock [game.rb] %}
  require_relative "./player"
  require_relative "./computer"

  class Game
    def initialize
      create_game
    end

    def create_game
      design_board
      create_players
      game_play
    end

    def design_board
    end

    def create_players
    end

    def game_play
    end
  end

  game = Game.new
{% endcodeblock %}

I'll start with the board design method. Again I needed this to be a customizably sized board so I began by asking the user what size board they wanted. What I received was a string so I needed to change this string into an integer and I set that to @slots. The reason why I made slots an instance variable was because ultimately this variable was going to be shared between all the methods in this class and even outside the class in choosing a move. Now that slots was an instance variable I needed to created an attr_accessor at the beginning of the Game class. To ensure the user doesn't break the program I created an if clause that made sure @slots was between 3 and 5. If it wasn't then the `design_board` method was called again. If @slots was between 3 and 5 then I made @board, another instance variable, equal to `(1..@slots**2).to_a`. I made board an instance variable for the same reasons I made slots an instance variable.

Let me explain the logic of why I did what I did. As a personal decision, I kept it between 3 and 5 because can you imagine playing a 20x20 slotted game? YEESH! For the @board logic I went through each number between 1 and however many slots the user wanted squared. Let's say the user put in '4' this would create an array of [1, 2, 3 ... 15, 16]. I used this logic for the user when they end up seeing the board.

{% codeblock [game.rb] %}
class Game
  attr_accessor :board, :slots

  ##code##

  def design_board
    #customizable board size
    puts "How big would you like the board to be? < 3 / 4 / 5 >"
    #changes slots size to integer
    @slots = gets.to_i
    if @slots <= 0 || @slots > 5
      puts "That is not valid."
      design_board
    end
    #creates array of slot ^ 2. eg. 5 becomes 1..25
    @board = (1..@slots**2).to_a
  end

   ##code##
end
{% endcodeblock %}


## Create Your Very Own Lvl. 60 Paladin
Next, I moved on to the `create_players` method. In the assignment guidelines it asked that I let the user choose between playing against another human or a computer. So the first line of business is asking the user that and taking in that response. I then created three if clauses, one for is the user wants a human, one for if the user wants a computer, and one if the user wants to break my program. If the user wants another human then that's simple, create two players using Player.new. Similarly simple, if the user puts in the wrong input then through recursion the method will be called again. For the moments when the user wants to play the computer I created another method called `random_start`; this method will end up asking the user who will play first.

{% codeblock [game.rb] %}
  def create_players
    #decides if you will play computer or human
    puts "Would you like your opponent to be a human or computer? < h / c>"
    response = gets.chomp
    if response == "h"
      @player1 = Player.new(self)
      @player2 = Player.new(self)
    elsif response == "c"
      random_start
    else
      puts "Invalid input"
      create_players
    end
  end
{% endcodeblock %}

Now, for the `random_start` method. Like with the `create_player` method I asked the user who should go first and chomped that response. If the player said they wanted to play then Player 1 was the human and Player 2 was the computer, if they wanted the computer to start then Player 1 was the computer and Player 2 was the human. If the user wants to randomize then I decided on creating an if statement using the rand() Math method. This method will generate a float greater than or equal to 0.0 and less than 1.0, when compared to .5 this gives an equal 50/50 chance perfect for randomizing players.

{% codeblock [game.rb] %}
  def random_start
    puts "Who should go first? < you / computer / random"
    response = gets.chomp
    if response == "you"
      @player1 = Player.new(self)
      @player2 = Computer.new(self)
    elsif response == "computer"
      @player1 = Computer.new(self)
      @player2 = Player.new(self)
    elsif response == "random"
      #from past code 
      #determine who is player1 and who is player2
      @player1 = rand() > 0.5 ? Computer.new(self) : Player.new(self)
      @player2 = "#{@player1.class}" == "Computer" ? Player.new(self) : Computer.new(self)
    else
      puts "Invalid input"
      random_start
    end
  end
{% endcodeblock %}


## Deciding The Fate of the Universe
Right now we have completed the initial setup of the application and we move onto the game play. So let's finish that final method, `game_play` that's called in `create_game`. Game play will be where the actual game steps are accessed, this is will involve counting turns, the player moves, checking for a winner, and exiting or playing the game again. Let's create some variables with an intial value before we work on the logic. As I metioned earlier we need to track the turns so let's make @turn = 0 at the top of the `game_play` method. We will also need to check for a winner during the game so let's set the winner variable equal to false.

To follow the logic we just put down let's create a while loop using both winner and @turn. We can continue playing the game if there is no winner and if @turn less than the amount of spaces on the board (remember @slots**2 is the number of spaces). If both of these comparisions are true then we can continue with the game. For an aethestic purpose we will need to print the board, so let's create a method `print_board`. To follow good programming practices leave the logic for the methods.

Now that the user can see the board let's work on the logic of the game play. If @turn is one less than the amount of spaces on the board and a winner has not yet been found then that means the game is a time. Let's output a string to the user that it's a tie and ask them if they want to play again by calling the `play_again` method. If @turn is not equal to the amoutn of spaces then we should get the player moves by calling `get_player_moves`. To finish this game play logic let's increase @turn by 1 before the while loop ends.

{% codeblock [game.rb] %}
  def game_play
    #set turns equal to 0
    @turn = 0
    #will need to check for winner in this method to ensure it does not contiue playing
    winner = false
    #if winner is false and turns are less than slots?
    while winner == false && @turn < @slots**2
      print_board
      if @turn == (@slots**2)-1
        #else if turns are equal to slots and no winner then draw
        puts "It's a tie!"
        play_again
      else
        #get the player move
        get_player_moves
      end
      #turns will need to be increased somewhere
      @turn += 1
    end
  end
{% endcodeblock %}

Before we move onto the the player move logic let's work on these two new methods `print_board` and `play_again`. The board printing method is formulating a large string for the user to see. Let's begin by making the board variable equal to @board.in_groups_of(@slots). If you remember from the `design_board` method earlier @board is an array from 1 to however many total spaces there are; using in_groups_of will divide that total number of spaces by the amount of slots. For instance if the number of @slots is 3 then @board is [1, 2, 3, 4, 5, 6, 7, 8, 9]. If you divide 9 by 3 then there will be 3 equal groups of 3 making board a multi-dimensional array, [[1, 2, 3],[4, 5, 6],[7, 8, 9]].

We'll make printed_board equal to two new lines and then go through each value of the board arrays and add those values to that printed_board variable. Lastly to keep things neat we'll add an additional two new lines to printed_board and return that variable. This means each time the printed_board method is called then this board will print to the terminal.

{% codeblock [game.rb] %}
  def print_board
    board = @board.in_groups_of(@slots)
    printed_board = "\n\n"
    board.each_with_index do |row, i|
      row.each do |char|
        #designing the actual board
        printed_board += char.to_s.rjust(@slots**2.to_s.length, " ")
      end
      printed_board += "\n\n" 
    end
    puts printed_board
  end
{% endcodeblock %}

The `play_again` method is very simple. We'll first ask if the user wants to play again and chomp that response. If the user answers 'y' then a new game is created and the user can play again. If they don't answer 'y' then the script is exited.

{% codeblock [game.rb] %}
  def play_again
    puts "Play again? y/n"
    response = gets.chomp
    response == "y" ? Game.new : exit
  end
{% endcodeblock %}

Now that our two side methods are finished let's move onto the `get_player_moves` method. We want to show who is currently moving so let's print that to the screen using the about of turns%2 + 1. The modulo division gives the remainder of an integer division which shows "Player 1" or "Player 2". Depending on the @turn variable we decide who moves and pass along the type of mark, 'X' or 'O'. After the players have moved we check if someone has won in `winner_check`. For the sake of continuity let's work on the player moves in the next section.

{% codeblock [game.rb] %}
  def get_player_moves
    #players move after each other
    puts "Player #{(@turn%2)+1}"
    #ie. if player1 moves then player2 will move
    if @turn % 2 == 0 
      @player1.play_move("X")
    else
      @player2.play_move("O")
    end
    #check if someone has won
    winner_check
  end
{% endcodeblock %}


## Playah! Playah!
As you saw in the previous code block the `play_move` method is used in both the Player and Computer classes. We'll work with the Player class, let's initialize it with @game =  game. If you notice in the Game class when a player was initialize we passed along the current game using 'self'.

Let's work on that `play_move` method, the argument is mark and takes in that 'X' or 'O' passed along from when it's called in the Game class. We ask the user where they would like to move and pass that and the @game.board along to the `slot_play` method.  The return of the `slot_play` method returns the space number if it is correct. We then set that space number on the @game.board to the 'X' or 'O' mark.

Before the codeblock let's go over the `slot_play` method, as seen in the `play_move` it takes in the current game board. We make a variable slot equal to that string we received from the user which has been converted into an integer. If slot is not between the range of the board spaces then the the user needs to input a number again. If the slot is between that board range but another play currently has a mark there then it asks the player to input another number. Finally, if the user has done things correctly then slot is decreased by 1.

{% codeblock [Player.rb] %}
class Player
  def initialize(game)
    @game = game
  end

  def play_move(mark)
    puts "Where would you like to move?"
    #need to get the board index that includes that number
    slot = slot_play(@game.board)
    @game.board[slot] = mark
    #another method?
  end

  def slot_play(board) 
    #needs to get the slot into a number
    slot = gets.to_i
    # if number is greater than the amount of slots 
    while slot < 1 or slot > @game.slots**2
      #return invalid input
      puts "Invalid game slot. Try again"
      slot = gets.to_i
    end
    #if number is taken
    if @game.board[slot-1].is_a?(String)
      #return taken
      puts "Someone is there. Try again"
      slot = gets.to_i
    end
    #redo
    slot -= 1
  end
end
{% endcodeblock %}


## I For One Welcome Our Computer Overlords
Let's move on to the Computer class and the logic behind that. Like the Player class when the Computer class is initialized it takes in the current game. Like the `play_move` method in Player thi one will take in the mark 'X' or 'O'. We let the human know that the computer is moving, we add an aethestic quality of a sleep command of two seconds to it appears the computer is thinking. We make move equal to the return of the `mark_move` method and on that sspace number on the game board we put the mark.

The `mark_move` method is pure logic, although this program does not let the Computer just randomly pick a spot this is a simple slot selection process. Generally, the user will pick the @slot number as 3 so I used the logic based on that. The smartest move the computer can do is use the center of the board, if another mark is there then it moves to the next if clause. The next four if clauses test if a mark is in one of the fours corners of the board. From there we figured out if a mark is in the other slots of the 3 group board. If it still fails the previous if clauses then the computer will choose a random spot.

{% codeblock [Computer.rb] %}
class Computer

  def initialize(game)
    @game = game
  end

  def play_move(mark)
    #take the symbol "x" or "o"
    puts "The computer is moving..."
    sleep 2
    move = mark_move
    #the index of the game board gets that mark
    @game.board[move] = mark
  end

  def mark_move
    if @game.board[4].is_a?(Integer)
      4
    elsif !@game.board[5].is_a?(Integer) && @game.board[8].is_a?(Integer)
      8
    elsif @game.board[0].is_a?(Integer)
      0
    elsif @game.board[6].is_a?(Integer)
      6
    elsif @game.board[1].is_a?(Integer)
      1 
    elsif !@game.board[1].is_a?(Integer) && @game.board[2].is_a?(Integer)
      2
    elsif @game.board[3].is_a?(Integer)
      3
    elsif @game.board[5].is_a?(Integer)
      5
    elsif @game.board[7].is_a?(Integer)
      7
    else
      @game.board.select { |i| i.is_a?(Integer) }.sample - 1
    end
  end 
end
{% endcodeblock %}


## Checking Who's The Big Pumba
We've just finished the Player and Computer classes! Tired yet? We're almost done. In the Game class we left off with the `winner_check` method, so let's create that. The `winner_check` method will access the methods that will test if there is a winner, if there is then the script will announce a winner and ask if the user wants to play again.

There are three possible winning situations, a horizontal line, a vertical column, or a diagonal. I have a concise explanation on my <a href="">script's README</a> that describes the winning situations but let me go over these methods as I go over each one.

{% codeblock [Game.rb] %}
  def winner_check
    #since there are no hard-coded wins there should be a line/column/diagonal win
    board = @board.in_groups_of(slots)
    #need to keep track of the players points
    @point = 1
    if line(board) || column(board) || diagonal(board)
      @winner = true
      puts "Player #{(@turn%2)+1} wins!"
      play_again
    end
  end
{% endcodeblock %}

The `line` method takes in the current game's board. We will go through each array of the board. Testing the values of each array in the second dimension, each time the same value is found then a point is added. We then call the `points` method which I will go over at the very end, if there are enough points then the `line` method returns true and a winner is announced. If there are not enough points then the method returns false and we move on to the `column` method.

{% codeblock [Game.rb] %}
  def line(board)
    (0...@slots).each do |y|
      (0...(@slots-1)).each do |x|
        #if one slot is equal to the slot to the right add point
        @point += 1 if board[y][x] == board[y][x+1]
      end
      if points == true
        return true
      end
    end
    false
  end
{% endcodeblock %}

The `column` method takes in the current game's board. We will go through each array of the board. Testing the values of the board where the second array value is equal to another value in that same second array value. Each time the same value is found then a point is added. We then call the `points` method which I will go over at the very end, if there are enough points then the `column` method returns true and a winner is announced. If there are not enough points then the method returns false and we move on to the `diagonal` method.

{% codeblock [Game.rb] %}
  def column(board)
    #going through each slot
    (0...@slots).each do |x|
      #going through each slot
      (0...(@slots-1)).each do |y|
        #if one slot is equal to the slot below it add point
        @point += 1 if board[y][x] == board[y+1][x]
      end
      if points == true
        return true
      end
    end
    false
  end
{% endcodeblock %}

The `column` method takes in the current game's board. We will go through each array of the board. First, if the values are equal to each other in a downward right fashion then a point is added. `Points` is then called and may or may not return true. If it does not return true then we check if the values are equal to each other in a downward left fashion then a point is added. `Points` is then called and may or may not return true. In this case if `points` returns false then the `check_winner` method also returns false.

{% codeblock [Game.rb] %}
  def diagonal(board)
    (0...(@slots-1)).each do |i|
      #if board slot is equal going in downward right fashion
      @point += 1 if board[i][i] == board[i+1][i+1]
    end

    if points == true
      true
    end

    (0...(@slots-1)).each do |i|
      #diagonal board slot is equal going in downward left fashion
      @point += 1 if board[@slots-(i+1)][i] == board[@slots-(i+2)][i+1]
    end

    points == true ? true : false
  end
{% endcodeblock %}

The `points` method compares the @point variable to the @slots variable. If they are not equal then @point returns to 1.

{% codeblock [Game.rb] %}
  def points
    if @point == @slots 
      true
    else 
      @point = 1
    end 
  end
end
{% endcodeblock %}

And that is the completion of the Tic Tac Toe Ruby script! For the future I'm going to setup a difficulty setting for the computer, where beginner let's the Computer randomnly select a board space, medium selects the board space using the current logic, and extreme selects the board space depending on both the computer and the human player. I hope this walkthrough aids you in creating your own Tic Tac Toe Ruby script and that you enjoy playing this one.

Keep on being badass programmers!