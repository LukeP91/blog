---
title: Post.new.valid?
date: 2016-08-06T20:21:43+01:00
---

Today I would like to share the next step in my Rails journey share with you all. Which is, as you can tell from the title, validation. Or, to be specific, making rails app check if a newly created object is valid in the context of its model and how validation works under the hood.

I was asked to create a simple application with just one model, and a controller. Next, I had to add some validations to it. I chose to make a simple "blog" app with only Post model. There was also one more restriction that [Andrzej Krzywda](https://twitter.com/andrzejkrzywda) gave me. No scaffolds or generators except for the migration generator. That meant  I had to create a model, a controller and all view files from scratch for all basic actions. As it turned out it was a great way to learn how it all works. In the end, I figured out that I could also create migrations without using a generator. From now on all my apps during Rails course will be created that way.

### Model
I decided to split the whole process into more manageable chunks and the first one was easy. Create a model with some simple validations. At the beginning, I decided what fields should it have and how they should be validated. So, a blog post should at least have a title, which I thought should not only be present - it doesn't make sense to create posts without a title, right? But also, it should have length constraints. I thought a way too short title doesn't make any sense and one that is too long, on the other hand, would make it difficult to understand what the post is about. Next thing that is even more important is the actual content of the post. What's the point of creating empty posts? And the last thing which I thought should be optional was the author.

```language-ruby
class Post < ActiveRecord::Base
  validates :title, presence: true, length: { in: 2..50 }
  validates :content, presence: true
end
```

Code for that model is very simple, but the purpose of that task was to learn how validation works under the hood and not how to make the most bulletproof model. Ok, the first thing I did learn was that each model inherits from the _ActiveRecord::Base_ class. That class includes multiple modules and one of them is the module responsible for validations. One of the methods in that module is validation, which in very simple words runs all validations for given fields and checks if any errors were found. Since I am at the beginning of my learning process that was all that I needed to know for now.

### Controller
```language-ruby
def create
  @post  = Post.new(post_params)
  if @post.save
    redirect_to @post
  else
    render 'new'
  end
end
```

Next thing on the to-do list was to create controller that was responsible for all CRUD operations on my posts. From a validation perspective, the most important part was the code that you could find in the create and update methods. To be even more specific the _save_ and _update_attributes_ methods. In very simple words, that code works like this. First, it checks if an object that called it is valid, if that assertion is true, it creates an SQL query that inserts it to the database. If the object is not valid it rerenders a proper template with errors listed and all invalid fields marked.

### View
```language-html
<% if @post.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize( @post.errors.count, "error") %> need to be fixed before saving that post.</h2>
    <ul>
      <% @post.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>
```

Ok, that was last part that needed to be done. The actual views with proper error displaying. To make that happen first we need to check if our object has any errors using _@object.erros.any?_ and later, thanks to a cool method pluralize, return the number of errors with some fancy text. Finally, we have to iterate over the collection of all error messages for our object and display them in a form of a list.

You can find finished app on [Heroku](https://damp-anchorage-94507.herokuapp.com/posts) and [Github](https://github.com/LukeP91/validation_app).
