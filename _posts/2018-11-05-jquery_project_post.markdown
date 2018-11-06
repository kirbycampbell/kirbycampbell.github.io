---
layout: post
title:      "jQuery Project Post"
date:       2018-11-06 00:06:52 +0000
permalink:  jquery_project_post
---


I had so many inspiring breakthroughs while working on this jQuery-Rails project!  

For this project I built a SpaceX manager web app.  I began by building out the standard rails login functions, but quickly realized I should just focus on the assignment at hand, and build some json Serializers, API endpoints, and add javascript/Ajax dynamic functionality!

So I built a Rocket model, which had one pilot and many engineers.  

In the Rocket Index Page I placed the Names of the Rockets through Ruby and allowed the user to click the "More Info" button and populate the rest of the info with Ajax.  It looked like this:

```
<b>Rockets Index</b>


<% @rockets.each do |rocket| %>
  <h3 class="rocketName"><%= link_to rocket.name, rocket %></h3>
  <button class="js-more" data-id="<%= rocket.id %>">More Info</button>
  <ul id="rocketSpeed-<%= rocket.id %>"></ul>
  <ul id="rocketCapacity-<%= rocket.id %>"></ul>
  <ul id="rocketPilot-<%= rocket.id %>"></ul><br>

<% end %>

<script type="text/javascript" charset="utf-8">
$(document).ready(function() {
  $(function() {
    $(".js-more").on("click", function() {
      var id = $(this).data("id");
    $.get("/rockets/" + id + ".json", function(data) {
      var rocket = data;
      var rocketSpeed = "<p>" + "Top Speed: " + rocket["top_speed"] + "</p>";
      $("#rocketSpeed-" + id).html(rocketSpeed);
      var rocketCapacity = "<p>" + "Capacity: " + rocket["capacity"] + "</p>";
      $("#rocketCapacity-" + id).html(rocketCapacity);
      var rocketPilot = "<p>" + "Pilot Name: " + rocket["pilot"]["last_name"] + "</p>";
      $("#rocketPilot-" + id).html(rocketPilot);
    });
  });
});
});
</script>

```

Similarly, I had the rockets/new page update with the newly created rocket without reloading as well.  I also experimented with adding a drop down selection for the pilots, since the pilot options should be already populated.  

```
<%= form_for @rocket do |f| %>
  <% if @rocket.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@rocket.errors.count, "error") %> prohibited this Product from being saved:</h2>

      <ul>
      <% @rocket.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= f.label :name %><br>
    <%= f.text_field :name %>
  </div>

  <div class="field">
    <%= f.label :top_speed %><br>
    <%= f.number_field :top_speed %>
  </div>
  <div class="field">
    <%= f.label :capacity %><br>
    <%= f.number_field :capacity %>
  </div>
  <div class="field">
    <%= f.label :pilot %><br>
    <%= f.collection_select :pilot_id, Pilot.order(:first_name), :id, :last_name %><br>
  </div>

    <%= f.submit %>
<% end %>

<div id="rocketResult">
  <h2>Rocket Name: </h2><h2 id="rocketName"></h2>
  <p>Rocket Speed: </p><p id="rocketSpeed"></p>
  <p>Rocket Pilot: </p><p id="rocketPilot"></p>
</div>

<script type="text/javascript" charset="utf-8">
$(document).ready(function() {
$(function () {
  $('form').submit(function(event) {
    event.preventDefault();

    var values = $(this).serialize();
    var posting = $.post('/rockets', values);

    posting.done(function(data){
      var rocket = data;
      $("#rocketName").text(rocket["name"]);
      $("#rocketSpeed").text(rocket["top_speed"]);
      $("#rocketCapacity").text(rocket["capcity"]);
      $("#rocketPilot").text(rocket["pilot"]["last_name"]);
    });
  });
});
});
</script>

```

Finally, the show page and it's next rocket javascript function:

```
<a href=" " class="js-next" data-id="<%=@rocket.id%>">Next Rocket</a>
<h1 class="rocketName"><%= @rocket.name %></h1>
<p class="rocketSpeed">Top Speed: <%= @rocket.top_speed %></p>
<p class="rocketCapacity">Capacity: <%= @rocket.capacity %></p>
<p class="rocketPilot">Pilot: <%= @rocket.pilot.last_name %></p>
<p class="rocketEngineers">Engineers:<% @rocket.engineers.each do |engineer| %></p>
  <p><%= engineer.first_name %> <%= engineer.last_name %></p>

  <% end %>

<script type="text/javascript" charset="utf-8">
  $(document).ready(function() {
  $(function () {
    $(".js-next").on("click", function(event) {
      event.preventDefault();

      var nextId = parseInt($(".js-next").attr("data-id")) + 1;
      $.get("/rockets/" + nextId + ".json", function(data) {
        var rocket = data;
        $(".rocketName").text(rocket["name"]);
        $(".rocketSpeed").text(rocket["top_speed"]);
        $(".rocketCapacity").text(rocket["capcity"]);
        $(".rocketPilot").text(rocket["pilot"]["last_name"]);
        //$(".rocketEngineers").text(rocket["engineers"][0]["first_name"]);

        // re-set the id to current on the link
        $(".js-next").attr("data-id", nextId);
      });
    });
  });
  });
  </script>

```

I had a lot of fun building out this project!  The API endpoint came out looking like this:

{
id: 1,
name: "Apollo 49",
top_speed: 153,
capacity: 4,
updated_at: "2018-11-05T23:19:18.587Z",
pilot: {
first_name: "Tanya",
last_name: "Harding",
age: 35,
slogan: "Break a Leg!",
updated_at: "2018-11-05T23:19:18.585Z"
},
engineers: [
{
first_name: "Michio",
last_name: "Kaku",
age: 75,
credentials: "Harvard Space Institute"
},
{
first_name: "Popo",
last_name: "Misterson",
age: 55,
credentials: "Harvard Space Program"
}
]
}
