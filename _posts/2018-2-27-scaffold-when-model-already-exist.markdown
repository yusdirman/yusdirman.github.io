---
layout: post
title:  "Rails Scaffold on Existing Model only object"
date:   2018-02-27 09:05:00 +0800
categories: Ruby on Rails
---

### Assalamualaikum.

I often have a model that is from another system and resides in another server. Before I learn that I can use scaffold to generate its controller and view based on the attributes in its table, I helplessly created each of the needed files one by one manually.

Actually with --skip-migration we can use rails generate scaffold for any model that already exist in order to automake the controller and the views for that model.

### Example

I have a model named `"Department"`. The table resides in another server and I already use it in association with my local `"User"` model. But my application also need to at least have proper view for index, show. Since I have become to lazy to create the controller and view nowadays thanks to ruby CLI command, I tend to find another solution to automake these task.

### The Concept

When you run `rails generate scaffold ModelName` with option --skip-migration, Rails will create all files nessescary in model, controller, view, assets and test, but, it wont create a migration.

### How to do it

  1. Get the table attributes in rails manner
```ruby
$\> rails c
Running via Spring preloader in process 21847
Loading development environment (Rails 5.1.3)
2.4.1 :001 > Department.new
=> #<Department code: nil, name: "", full_name: nil, service_code: 0>
2.4.1 :004 >
```
  2. Ready the attributes in Rails way
```ruby
Department name full_name service_code:integer
```
  3. Adding options to the generate script as you prefer
```ruby
Department name full_name service_code:integer --skip-migration --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework
```
  4. run the migration
```ruby
rails generate scaffold Department name full_name service_code:integer --skip-migration --no-helper --no-assets --no-controller-specs --no-view-specs --no-test-framework
```

> DO NOT OVERRIDE THE MODEL FILE

There you have it. After the scaffold, you will have a working controller and most important, the form for your model is created and ready to use. :smile:

Thanks. Assalamualaikum

> yusdirman is very lazy to create html form. :laughing:
