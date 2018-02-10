---
title: Associations
date: 2016-08-27T20:21:43+01:00
---

An important topic in any application that tries to solve a problem are associations. Objects, items, things are connected in many ways with each other.  And those connections are what today's blog post will be about.  It will show how to create and use basic associations in Rails.

As my next step in [Junior Rails Developer Course](http://blog.arkency.com/junior-rails-developer/), I was asked to create a simple app that will teach me how to use associations in Rails. In order to do that, I have created two models that represent employees in a company and various departments in it.

To create an association in Rails you need to do two things. Add proper method to both or one of the connected models. And then create a proper migration. In my app, I have decided that each Employee belongs to one of existing departments. Department has many employees, but also to make things more interesting each department belongs to the manager.

### Belongs_to
Belongs to is the simplest association which tells us that one object belongs to another one. For example, book belongs to an author or in our case each employee belongs to one of existing departments.

```language-ruby
class Employee < ActiveRecord::Base
  belongs_to :department
end
```

Another usage of belongs to was a bit trickier. I wanted to make each department to have one manager. But since I didn't want to create a separate model for that because a manager is in fact on of department's employees I needed to do it differently. Fortunately _belongs\_to_ method in Rails supports many additional options. One of which I used in that example. Rest of them you can check out in [Rails Guides 4.1.2](http://guides.rubyonrails.org/association_basics.html#belongs-to-association-reference). To make manager work I used _class\_name_.

#### Class_name
Class_name helps when model name can't be figured out using association name. For example in our case association name was manager. But in our app manager is, in fact, an object of Employee class. Using _class\_name_ we can tell Rails that even though our association name is manager, the model that is actually used is called Employee.
```language-ruby
class Department < ActiveRecord::Base
  belongs_to :manager, class_name: "Employee"
end
```
### Has_many
Another type of association used in my project is _has\_many_. Which basically tells us that object of our class has a connection with many objects of another class. In that case, a department has many employees. Usage of that is fairly simple and you can see it in the code below.

```language-ruby
class Department < ActiveRecord::Base
  has_many :employees
end
```

### Migrations
Last part that is required to make it work is to create proper migrations. That part of making proper associations is fairly simple. We create proper fields in the database for those tables that correspond to a model that has a _belongs\_to_ association in it. So in our case, we need to create the _department\_id_ field in employees table and _manager\_id_ in departments table.

```language-ruby
class AddReferences < ActiveRecord::Migration
  def change
    change_table :departments do |t|
      t.integer :manager_id, index: true
    end

    change_table :employees do |t|
      t.integer :department_id, index:true
    end
  end
end

```

There was one problem with a manager when I started to make my app work. I couldn't figure out whether the department should belong to the manager or the other way. Fortunately, guys from [Arkency](https://arkency.com/) helped me with that and explained that if I would put _belongs\_to_ in Employee I would need to create field _managed\_department\_id_. But since not all employees are managers many of them would have _nil_ in it. It should be the department that has the _manger\_id_ field in it and because of that _belongs\_to_ should be in department model not in employee model.

You can check finished app on [Github](https://github.com/LukeP91/association_app) and [Heroku](https://tranquil-fjord-78902.herokuapp.com/).

Cheers Luke!
