---
layout: post
title:      "Sinatra Project Post"
date:       2018-10-22 19:19:20 +0000
permalink:  sinatra_project_post
---

Hey Everyone - so I had put off my Sinatra project and went on ahead to Rails and Javascript, but just now came back and completed it.  Turns out, Thin and Shotgun do not work on windows Computers, so I had to keep copying my project from the temp Learn-IDE3 folder into my girhub synced Atom workspace.  This is why I had put it off for awhile, but nonetheless I figured out a workaround.  

So I made a Restaurant manager app!  Managers can sign up, Create a new table with a certain number of people and assign a waiter to the table.  After that, the manager that created that table can edit or delete that table.  Other users can log in - As long as they have a different name - and can do the same to their own tables.  No manager can delete or edit another managers tables though!  The tables belong to a manager!

That's pretty much it.  I'll include some of my code below!

```
#users_controller.rb

require './config/environment'

class UsersController < ApplicationController

  get '/signup' do
    erb :'/users/signup'
  end

  get '/login' do
    if !logged_in?
      erb :'/users/login'
    else
      redirect "/tables"
    end
  end

  get '/users/:slug' do
  @user.find_by_slug(params[:slug])
  erb :'/users/show'
end

get '/logout' do
  if logged_in?
    session.clear
    redirect "/login"
  else
    redirect "/"
  end
end

  post '/signup' do
  if params[:name] != "" && params[:dob] != "" && params[:password] != ""
    @user = User.create(:name => params[:name], :dob => params[:dob], :password => params[:password])
    if @user.save
      session[:user_id] = @user.id
      redirect "/tables"
    else
       redirect "/signup"
     end
  else
    redirect "/signup"
  end
end

  post '/login' do
    user = User.find_by(:name => params[:name])
    if user != nil && user.authenticate(params[:password])
      session[:user_id] = user.id
      redirect "/tables"
    else
      redirect "/login"
    end
  end
end
```

```
#tables_controller.rb

require './config/environment'

class TablesController < ApplicationController

  get '/tables' do
    if logged_in?
      @tables = Table.all

      erb :'/tables/table'
    else
      redirect to '/login'
    end
  end

  get '/tables/new' do
  if logged_in?
    erb :'/tables/create_table'
  else
    redirect '/login'
  end
end

  get '/tables/:id' do
  if logged_in?

    @table = Table.find_by_id(params[:id])
    erb :'/tables/show_table'
  else
    redirect '/login'
  end
end

get '/tables/:id/edit' do
  if logged_in?
    @table = Table.find_by_id(params[:id])
    if @table && @table.user_id == current_user.id
      erb :'tables/edit_table'
    else
      redirect to '/tables'
    end
  else
    redirect to '/login'
  end
end

patch '/tables/:id' do
  if logged_in?
    if params[:content] == ""
      redirect to "/tables/#{params[:id]}/edit"
    else
      @table = Table.find_by_id(params[:id])
      if @table && @table.user_id == current_user.id
        if @table.update(:table_number => params[:table_number], :head_count => params[:head_count], :waiter_name => params[:waiter_name])
          redirect to "/tables/#{@table.id}"
        else
          redirect to "/tables/#{@table.id}/edit"
        end
      else
        redirect to '/tables'
      end
    end
  else
    redirect to '/login'
  end
end


  post '/newtable' do
    if logged_in?
      @table = Table.find_or_create_by(:table_number => params[:table_number], :head_count => params[:head_count], :waiter_name => params[:waiter_name])
      if @table.save
        @table.user_id = current_user.id
        @table.save
        redirect "/tables/#{@table.id}"
      else
         redirect "/tables"
       end
     else
     redirect '/login'
   end
  end

  delete '/tables/:id/delete' do
  if logged_in?
    @table = Table.find_by_id(params[:id])
    if @table && @table.user_id == current_user.id
      @table.delete
    else
      redirect to '/tables'
    end
    redirect to '/tables'
  else
    redirect to '/login'
  end
end

end

```

