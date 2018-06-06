---
layout: post
title:      "Diving in Deep"
date:       2018-06-06 17:28:48 +0000
permalink:  diving_in_deep
---

After several attempts at learning varying coding languages over the past few years, I finally feel like it's all clicking with Ruby.  I'm sure the basics have crossed over from those previous attempts but I'm very excited about the level of comfort and confidence I'm feeling with Ruby.  

I'm currently in the middle of the Object Oriented Ruby Final Projects in which the Tic-Tac-Toe with Ai resides.  I was stuck on the ai portion of this project for awhile, but recently teamed up with another student and not only succeeded in creating the ai, but also in creating randomizing strategies that even make the computer only games play different games each time, while also never losing in 100 games.  Super proud of that!

Now, I am deep inside of the CLI Data Gem in which I am creating a CLI app for people living around Portland Oregon that want to find fun outdoor activites and get more info on them. The program begins by asking the user to pick between biking, hiking, and skiing.  I have gotten the biking section to work, in which it lists about 12 bike routes in the area and then you can select one and get a description on that specific route... and then return to the bike routes or return to the activities.  I'm having a bit more trouble on the hiking and skiing sections since each activity is based on a different website.  I guess I'm essentially doing the project 3 times over by going this route, but I dont mind the practice and would actually like to make something useful to me and anyone in the area.  

Here's a sneak peak at my Bike list scraping code: 

```
  def bike_list
  site = Nokogiri::HTML(open("https://fitt.co/portland/where-to-ride-best-bike-trails-portland/"))
  site.css(".list-loop__item").each_with_index do |trail, index|
    num = index + 1
    puts "#{num}. #{trail.css("h2").text}."
  end
  bike_info
end
```

The trouble I'm running into on the hiking and skiing sites is in relation to the way the html is laid out... It seems each description isn't a child of the name of each path, but moreso on the same level and just next to it.  So I have to find a way to scrape each title's sibling.  Currently it is scraping, but its just scraping 1 or 2 off from the correct descriptions.  Hopefully, I'll figure that all out today and have it done!

All in all, this learning experience has been very enriching so far, and I look forward to collaborating with others more in the future as I noticed concepts really click in my mind when talking with another coder. Can't wait to get to the Ruby on Rails section!

-Kirby
