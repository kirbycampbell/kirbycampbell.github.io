---
layout: post
title:      "My first dive into Artificial Intelligence"
date:       2018-07-10 03:08:20 +0000
permalink:  my_first_dive_into_artificial_intelligence
---



   For the Ruby Tic Tac Toe Final Project, we had to create a computer AI to play in a one player game and in a computer vs. computer game.  Initially I began designing the AI through testing the computer vs. computer situation, but quickly realized I wasn’t able to control the responses I was getting.  So, I made myself into the control unit.  I played a one player game, and I would play the same game over and over until I was able to get the computer to react in strategic and non-repetitive ways.  
   Initially, I began with hardcoding the order of moves for the computer to make. It would aim for the middle space first, and then the corners.  But it was 1 dimensional and predictable.  Next I pushed the AI to learn which token it was and which token the opponent was, that way I could make it play defensively first, then offensively if no defensive moves existed.  

```
def move_option
	result = nil
	@pattern.detect do |combo|
		if @board.cells[combo[0]] == @opponent && @board.cells[combo[1]] == @opponent && !@board.taken?(combo[2]+1)
			result = combo[2]+1
		elsif @board.cells[combo[1]] == @opponent && @board.cells[combo[2]] == @opponent && !@board.taken?(combo[0]+1)
			result = combo[0]+1
		elsif @board.cells[combo[0]] == @opponent && @board.cells[combo[2]] == @opponent && !@board.taken?(combo[1]+1)
			result = combo[1]+1
		end
	end
	result
end
```


The above code is an example of the defensive strategy.  The Offensive strategy is the exact same except that @opponent is replaced with self.token which makes the computer aim to win if possible. 
	
   My Third strategy was “Begin completing a winning combination if one of the computer’s tokens were already within that combo”.  This looked like this:
	
```
    def move_option_22
      result = nil
      @pattern.detect do |combo|
        if @board.cells[combo[0]] == self.token && !@board.taken?(combo[1]+1) && !@board.taken?(combo[2]+1)
          result = combo[2]+1
        elsif @board.cells[combo[1]] == self.token && !@board.taken?(combo[0]+1) && !@board.taken?(combo[2]+1)
          result = combo[0]+1
        elsif @board.cells[combo[2]] == self.token && !@board.taken?(combo[1]+1) && !@board.taken?(combo[0]+1)
          result = combo[1]+1
        end
      end
      result
    end
```

The above code looks to see if one of the spots is taken, while two other spots within that win combination are available.  It only reaches this code if there were no defensive win blocks or winning moves to make.  

  After that, the computer falls back to general tic tac toe strategy of going for the middle space if its available, or going for corner spots if the opponent has the middle space, and lastly aiming for the top/left/right/bottom spaces.

   Upon completion of this foolproof and unbeatable strategy, I realized the computer was scrolling through the patterns of win combinations in the same order each game, resulting in identical games when the computer plays itself over and over.  
	I fixed this by adding a .shuffle extension onto all arrays of combos.  That way each game would load with a new order of arrays of win combos.  This looked like this: 
	
```
@pattern.shuffle.detect do |combo|
```

And: 

```
[1, 3, 7, 9].shuffle.detect {|n| !@board.taken?(n)} || [2, 4, 6, 8].shuffle.detect {|n| !@board.taken?(n)}
```

   Now the computer can play itself 100 times with varying gameplay moves, and continuously ends in a tie, as a perfect strategy of tic tac toe should always yield. 

   This project was by far the most rewarding and challenging code project yet, and I look forward to many more similar projects.  I’m deeply fascinated with logic and intelligence and would love to dive into these concepts deeper in the near future!
	


