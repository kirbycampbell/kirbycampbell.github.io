---
layout: post
title:      "Rails Project"
date:       2018-10-08 19:39:03 +0000
permalink:  rails_project
---


It's been awhile since I've posted here, but nonetheless, I've made it to the Rails Project.  

This Project really pushed me to solidify my knowledge of the past 6 months of FlatIron learning.  

I decided to build a Rails app similar to a Tumblr or Instagram... My database includes Users, Topics, Photos, and Statements.  

The Users may signup/login or use Google OAuth to sign in.  The User may then create a Topic, which can contain photos or writings (statements).  In a way its sort of a portfolio/blog website.  

The main roadblock I ran into creating this app was the OAuth process:  First :provider was missing, as well as :email, and :password.  The email and password were an easy fix with `raise auth.inspect`'s help.  


```
#From class User
  def self.from_omniauth(auth)
   where(provider: auth.provider, uid: auth.uid).first_or_initialize.tap do |user|
     user.provider = auth.provider
     user.uid = auth.uid
     user.email = auth.info.email
     user.name = auth.info.name
     user.password = 'whatever'
     user.oauth_token = auth.credentials.token
     user.oauth_expires_at = Time.at(auth.credentials.expires_at)
     user.save!
   end
 end
```

In the sessions controller I built an #oauth method to handle the logic when logging in through Google:

```
#From class SessionsController
  def oauth
    #raise request.env["omniauth.auth"].inspect
    user = User.from_omniauth(request.env["omniauth.auth"])
    if user
      log_in user
      redirect_to user
    else
      render 'new'
    end
  end
```
In this method I had to add "request" before the env["omniauth.auth"] to get the process to work..

Also once logged in, the user was able to view any other User's page and have access to deleting their posts.  So I did some googling and found a method I was able to alter:

```
#From Topics Controller
  before_action :only_see_own_page, only: [:show]
	
	private 
	  def only_see_own_page
    @topic = Topic.find(params[:id])
    @user = @topic.user

    if current_user != @user
      redirect_to root_path, notice: "Sorry, but you are only allowed to view your own profile page."
    end
  end
```

In order for User's to view other User's work, I created a "Topic Feed" Page which shows the most recent posts in DESC order from when created_at.

This project also pushed me to revisit CSS for the first time in awhile, and it was definitely fun getting back into the design aspect again.  

```
#The navbar and button CSS from Application.css
.top-bar{
  padding: 40px;
  margin: 0px;
  width: 100%;
  background: linear-gradient(-45deg, #57cfb0, #2ab5d3);
  box-sizing: border-box;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  grid-gap: 5px;
  list-style: none;
}

.button-words{
  color: white;
  display: flex;
  font-size: 20px;
  flex-grow: 1;
  flex-shrink: 1;
  justify-content: center;
  padding: 20px;
  text-decoration: none;
  border-radius: 5px;
  background: #49295e;
}

```

Which connected to:

```
    <ul class="top-bar">
      <% if !logged_in? %>
        <%= link_to "Home", topic_all_path, :class => "button-words" %>
        <%= link_to "Topic Feed", topic_all_path, :class => "button-words" %>
        <%= link_to "Signup ", new_user_path, :class => "button-words" %>
        <%= link_to "Login", login_path, :class => "button-words" %>
      <% else %>
        <%= link_to "My Home", user_path(current_user), :class => "button-words" %>
        <%= link_to "Topic Feed", topic_all_path, :class => "button-words" %>
        <%= link_to "Create New Topic", new_topic_path, :class => "button-words" %>
        <%= link_to "Logout", logout_path, method: :destroy, :class => "button-words" %>
      <% end %>
    </ul>
```
I was able to use logic from the sessions helper #logged_in? method to determine what the buttons at the navbar would contain, while still applying the same CSS design to them.

Thanks for Reading!


