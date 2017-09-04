---
layout: post
title:  "Tic Tac Toe Game - Ruby style"
date:   2017-08-23 16:49:27 -0400
---


![](http://i.imgur.com/GNwr0AJ.jpg)

Argh!!!!!!!!

This was a challenge despite the labs going over each step to build the game. Everything lead up to the final build and play method.

What is a helper method. How does it work?

Here’s what I’m understanding from all this practice:

We start by understanding how the game works in general.

   * A Tic Tac Toe board uses an array of strings (“ “)

   * The board has 9 squares or fields that the player’s characters (X or O) are placed.

   * There are 8 ways or combinations of how a player can win at Tic Tac Toe.
      * Winners win by having matching X’s or O’s in:
         * Top row
         * Middle row
         * Bottom row
         * Left column
         * Middle column
         * Right column
         * Left diagonal
         * Right diagonal
      * No winners happen when
         * The entire board is filled with X’s and O’s but don’t have a matching combination

   * When a winner occurs, they are notified by saying “X wins!” or “O wins!”

I guess you can say the above outline is our pseudocode. Writing things/instructions in simple, plain english or descriptors can help us figure out how to write code for the program.

   - We'll pass the board to every method as an argument so the helper methods can be reused to our logic.

   - Getting user input will be done through "gets" and they will choose a position on the board by entering 1-9.

   - Our program will then fill in the square(as the player's view) on the board with the converted user input (now in an interger) with the player's X or O.

   - We will keep track of which player's turn it is and how many turns have been played. At every turn, we'll check if there is a winner. If there is a winner, we'll congratulate them. If there is a tie, we'll let them know.


**Build the Board**

Taking the pseudocode, I’ll start building my code.

 * *A Tic Tac Toe board uses an array of strings (“ “)*

    So we have to define the variable “board”

    ```puts board = [' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ']```

* *The board has 9 squares or fields that the player’s characters (X or O) are placed.*

   How can we display the board to show that we have 3x3 board? We need to define a method “display_board.” each square or fields represents as a string.
	 
   ```
   def display_board
     puts row = ["   " "|" "   " "|" "   "]
     puts separator = "-----------"
     puts row
     puts separator
     puts row
   end
   ```
	 
	 At this point, we have our board. The board is viewed 2 ways - from the players view and the code as an array.
	 
```
	 board       (viewed by players)     (viewed by program)	 
	 |   |           1 | 2 | 3                0 | 1 | 2  
 -----------       -----------              -----------
     |   |           4 | 5 | 6                3 | 4 | 5  
 -----------       -----------              -----------
     |   |           7 | 8 | 9                6 | 7 | 8
```

 We would display the board from the program's point of view (an array, because programs count starting with 0) as a method (which would then be a helper method).

  ```
  def display_board(board)
    puts " #{board[0]} | #{board[1]} | #{board[2]} "
    puts "-----------"
    puts " #{board[3]} | #{board[4]} | #{board[5]} "
    puts "-----------"
    puts " #{board[6]} | #{board[7]} | #{board[8]} "
  end   #this is the board displaying the proper board[index] in its array format
  ```
	
   Note: We are using string interpolation with #{board[0]} because it'll print (puts) the current state of the board, which method display_board will pass.
		
  Ok, now that we have the board defined, we need to find out how a user will choose a square to enter their X or O. Since our board is in an array, a player wouldn't know that. A player starts counting at 1. When a user choses the field from its point of view, we need to convert it into an interger so it can match the index in the array that the program is seeing. If user enters 5 for the center position of the board, the array corresponding to it should be [4].
	
	To convert the user input into an integer we have to define a method to help us do that. We would use the .to_i method to change numbers to integers and minus 1 because the array counts up to 8, not 9.

   ```
   def input_to_index(user_input)
     user_input.to_i - 1
   end
   ```

   Now that the program is reading the user_input's number as an integer, we can tell it to make a move by placing the player's choice of "X" or "O." We have to define how to move a piece (X or O) on the board.
  
   ```
   def move(board, index, player)
     board[index] = player
   end
   ```
	 
	 When playing this game, you can't place your X or O in a spot that's already taken. In other words, we have to validate the code. We'll run the user's input against the Tic Tac Toe board and check to see if the user's submitted position is free or already filled with X or O.
	 
   ```
   def position_taken?(board, index)
     if (board[index] == " ") || (board[index] == "") || (board[index] == nil)
       return false
     else
       return true   #board[index] is not " " or "" or nil because there's an "X" or "O"
     end
   end
   ```

   NOTE: Methods return either `true` or `false` by adding a literal question mark to the end of the name

   Now we validate the user’s input(now converted to an index). If the user’s input converted index number is between 0-8 (the array index) and its position is NOT taken then the move is valid (true)

   ```
   def valid_move?(board, index)
     if index.between?(0,8) && !position_taken?(board, index)
       return true
     end
   end
   ```
	 
	If validation of the moves are good, then the players will take turn playing the game. Here we would build a method composed of the use of many methods (helper methods) previously defined. Helper Methods are used inside other methods to carry out larger tasks. It can be re-used.
  
	Helper Methods are now display_board(board), valid_move?(board, index), position_taken?(board, index), input_to_index(user_input), and move(board, index, first_player = "X")

While playing the game, we need to know who's turn it is and to place X or O. We do this by defining turn_count(board) and current_player(board) methods. 

```
def turn_count(board)
  counter = 0
  board.each do |spaces|
    if spaces == "X" || spaces == "O"
      counter += 1
    end
  end
  counter
end
```

```
def current_player(board)
  turn_count(board) % 2 == 0 ? "X" : "O"
end
```
	 
	 
**turn method**

```
def turn(board)
  puts "Please enter 1-9:"    #program asks user to enter a position number (1-9) 
  user_input = gets.strip      #user enters a number
  index = input_to_index(user_input)     #that number will convert to an index.
  if valid_move?(board, index)      #  if valid_move? is true ...
     move(board, index, current_player(board))   # ... then move on the board, on the index user chose, the player's X or O.
  else        
     turn(board)  #  if not, until the user’s input number is correctly converted into an index and is true, then the program will continuously asked the user to keep entering a valid number. Once user inputs a correct number within 1-9, then the move is validated. 
   end
  display_board(board)     # Shows the board after the move is made.
end
```

The turn method threw off my test while I was working on the lab. The problem lied in the if statement block of move(board, index, current_player(board)). See below code. What I forgot about ruby were many things - method calls (or sends) values; while defining a method accepts values. We should try to think of methods as a dictionary. A dictionary defines a word and gives it a meaning. So, when we are defining a "word", we are defining a "method" in Ruby. When calling a method or the word, it sends a value. Here's how I figured out the if statement to clear my confusion.

The if statement of method #turn has 3 helper methods - valid_move?(board, index), move(board, index, current_player(board)), and current_player(board). Their individual definitions will help us place an X or an O on the index/square on the board.

**turn method - if statement**

The definition of valid_move?(board, index) is that if an index is between numbers 0 to 8 AND it's position is NOT(!) taken, then it must be true. Given this definition, then the if statement of method #turn says to make a move on the board to the index stated and place the current_player's token. That's all there's to it for the if statement.

**So who's the winner?**

   * There are 8 ways or combinations of how a player can win at Tic Tac Toe.
      * Winners win by having matching X’s or O’s in:
         * Top row
         * Middle row
         * Bottom row
         * Left column
         * Middle column
         * Right column
         * Left diagonal
         * Right diagonal
      * No winners happen when
         * The entire board is filled with X’s and O’s but don’t have a matching combination

   * When a winner occurs, they are notified by saying “X wins!” or “O wins!”

Here are our combinations in its arrays

```
WIN_COMBINATIONS = [
  [0,1,2], # top_row
  [3,4,5], # middle_row
  [6,7,8], # bottom_row
  [0,3,6], # left_column
  [1,4,7], # center_column
  [2,5,8], # right_column
  [0,4,8], # left_diagonal
  [6,4,2]  # right_diagonal
]
```

So how do we determine the combinations to see who won the game? WIN_COMBINATIONS is a constant but it's the parent array to our win_combinations (the children array) A win_combination is a 3 element array of indexes that tells us the matching wins [0,1,2], [3,4,5], etc... So we have to grab each index from the |win_combination| that won.

```
def won?(board)
  WIN_COMBINATIONS.each do |win_combination|
    win_index_1 = win_combination[0]
    win_index_2 = win_combination[1]
    win_index_3 = win_combination[2]

    position_1 = board[win_index_1] # load the value of the board at win_index_1
    position_2 = board[win_index_2] # load the value of the board at win_index_2
    position_3 = board[win_index_3] # load the value of the board at win_index_3

   position_1 == position_2 && position_2 == position_3 && position_taken?(board, win_index_1) #detect returns first element (position_1) & make sure position is taken (that it's either an X or O).
  end
end
```

Is the board full of X's and O's or is it empty?
```
def full?(board)
  board.all? {|i| i == "X" || i == "O"}
end
```

Returns true if the board has not been won and is full and false if the board is not won and the board is not full, and false if the board is won.

```
def draw?(board)
  if !won?(board) && full?(board)
    return true
  elsif !won?(board) && !full?(board)
    return false
  else won?(board)
    return false
  end
end
```

Returns true if the board has been won, is a draw, or is full.

```
def over?(board)
  if draw?(board) || won?(board) || full?(board)
    return true
  end
end
```

#accept a board and return the token, "X" or "O" that has won the game given a winning board.

```
def winner(board)
  if won?(board)
    return board[won?(board)[0]]
  end
end
```

**Time to Play**

When playing the game, the players play until someone wins or there's a tie. It keeps going until there's a condition for it to stop. That condition is either win or tie. In other words, we are looping this play. We start the game at 0 until we fill up the 9 squares or until someone wins or there's a tie.

```
Play Method A

def play(board)
  counter = 0
  until counter == 9   <= until the 9 squares are filled up
    turn(board)
    counter += 1   <= we iterate a move by 1
  end
end
```

Play Method A was the first iteration of the method. Play Method B is the final updated method. We're no longer counting the game is over until all the 9 squares are filled up. We're counting the game until it is over as each player takes their turn. After they play a few turns, either there's a winner or a draw. If there's a winner, we have to call on the winner(board) method and put out a message.

```
Play Method B

def play(board)
  #input = gets
  until over?(board) #until the game is over...
    turn(board) #players will keep taking turns
  end
  #plays the first few turns of the game
    if won?(board) #if there's a winner...
      winner(board) == "X" || winner(board) == "O" #we check who the winner is...
        puts "Congratulations #{winner(board)}!" #and congratulate them
    elsif draw?(board) #if there's a draw, then print the below strings
      puts "Cats Game!"
    end
end

```


END of Tic Tac Toes.


**I'm tired of Tic Tac Toes... -_- **

I hope all this makes sense to someone. Or at least be helpful. I try to be pithy when giving instructions, descriptions, or other forms of info, but I often say more than I should. Maybe with coding and programming, the more info give the better? I have a better understanding of Ruby, tests, and CLI, now, but hope to make it crystal clear soon. I just hope I can apply what I've done within this post to future labs. Otherwise, I don't know what I'm doing....

![](http:///i.imgur.com/KBdf4fl.jpg")
