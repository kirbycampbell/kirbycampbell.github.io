---
layout: post
title:      "Change is Good"
date:       2018-04-23 18:30:53 +0000
permalink:  change_is_good
---


I've spent the past decade working mainly on computers in a creative capacity, while never learning to properly code.  Most of my work was music or video related, utilizing programs like Pro Tools, Logic, Ableton, Adobe Premiere, Unity, Maya, Photoshop, and many others.  (Those are roughly in order from mastered to generally able to operate). I was mainly focused on the production side of music yet spent about 50% of my time touring the world and playing the music I had made with my previous band.  I left the group in 2015 to focus on my own long-term career goals, and thus I was drawn towards a career oriented with computers.  I then went on to focus heavily on Virtual Reality creation and design using Maya and Unity, absorbing as much from tutorials online and a course I eventually found to be sub-par at Udacity. 
I also continued mixing and producing music since 2015 as well, but am now going full force towards a coding career. 

Coding seems very natural to me, as its so similar to the processes found in Pro Tools or Logic via the inputs and outputs of sending sounds into other sounds to interact with each other, much like coding with methods or piping in and out of arrays. 

As for my personal progress at FlatIron - I've been putting lots of hours in over my first 2 weeks, essentially trying to connect the dots to Ruby with what I've experienced over the years when looking into Objective-C (books), Swift (teamtreehouse),  and C# (In Unity).  So far, its been challenging at a few spots like the Tic-Tac-Toe project and the Oxford Comma Lab, which I just finished.

When I began on the oxford_comma method the first two tests passed easily with 
```
def oxford_comma(array)
  if array.length <= 1
    array.join
  elsif array.length == 2
    array.join(" and ")
		end
```
		The trouble began when I wanted to place commas between every item in the array, add an "and" after the second to last item, while not receiving a comma after the "and". 
		I tried pulling the second to last item out of the final string, which did not work.. I then tried to use
		```
last = array.length -2
		array.insert(0..last, ", ")
```
Yet this gave me errors since .insert requires an integer, not a range.  

Next I resorted to some internet searching - I found lots on removing the last item from an array, or pulling out every comma in a string, but I realized that manipulating the string after the fact was out of the question, this job needed to happen in the array itself. 

Thus I found a process that I still don't fully understand how it works, but as a formula I understand what it is doing.  It included a .flat_map on the array, some code, and then .tap and pop.  I found this method to intersperse whatever element I would like in between each item of an array, and then using pop, could consistently remove the final iteration of  this process.  This was now adding commas between every array item by literally adding strings to the array as ", " and then it would leave no comma between the last two items in the array.  That left me room to run array_insert(-2, "and ") thus giving me the output I wanted.  Here is the full code I wrote:

```
def oxford_comma(array)
  if array.length <= 1
    array.join
  elsif array.length == 2
    array.join(" and ")
  elsif array.length >= 3
    new_array = array.flat_map { |i| [i, ", "]}.tap(&:pop)
    new_array.insert(-2, "and ")
    new_array.join()
  end
end
```



		
