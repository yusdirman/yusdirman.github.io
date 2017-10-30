---
layout: post
title:  "rails generate scaffold with existing table"
date:   2017-10-29 15:30:00 +0800
categories: Ruby on Rails, scaffolding
---

# Generate scaffold with existing table

When we integrate other system's databases with our rails application, it was a strenuous task to generate view and controller based on tables that already existed and on top of that, was build by scratch and not following the rules of thumb in database structure.

For that, in Ruby on Rails spirit as to make development fast, there are methods to run scaffolding script to automake the view, contoller and model.


Here, I will show one way to do it / how I did it

### 1. Create connection setting to the db alongside with our application


```ruby
# database.yml
...
#local app setting

identity_development:
  adapter: mysql2
  encoding: utf8
  pool: 5
  host: localhost
  port: 3306
  username: a
  password: b
  database: identity


#below also setting for test and production
```

### 2. Make parent model in separated folder / module

```ruby
  # in model/identity/users.rb
  module Identity
    class User < ApplicationRecord
      establish_connection ("identity_#{Rails.env}").to_sym
      self.table_name = 'staffs'
      after_initialize :readonly!
    end
  end
```

> I've set as `:readonly` to secure any unwanted script that would write to the database. But, actually the mysql username only have read access to the database.

### 3. Get the table parameters / variables using consoles

```bash

03:46 PM: Oct 30[user1@diman]: ~/Project/performance_monitor: 15-create-controller-view-for-users-in-identity*
$\> rails c
Running via Spring preloader in process 28864
Loading development environment (Rails 4.2.6)
2.2.4 :002 > Identity::User.new
 => #<Identity::Staff id, stafno: nil, passwd: nil, name: nil, gender: nil, position_id: nil, position_name: nil, grade_no: nil, grade_no: nil, grade_name:nil, date_appointed: nil, email: nil, pic: nil, phone_ext: nil, is_admin: false, active: true, synced_at: nil
2.2.4 :003 >
```

> by running `rails console` and `executing ModelName.new` in the console, every parameters for the table will shown in sequence order. That will make things easier.


### 4. Generate scaffold

Inside you project folder, run scaffold and copy paste (edit) the parameters you got from rails console before.

```bash

04:12 PM: Oct 30[user1@diman]: ~/Project/staff_monitor: 15-create-controller-view-for-users-in-identity*
$\> rails g scaffold Staff id:integer stafno, passwd, name, gender:integer, position_id:integer, position_name, grade_no:integer, grade_name, date_appointed:date, email, pic, phone_ext, is_admin:boolean, active:boolean, synced_at:date --migration=false --parent=identity/user

Running via Spring preloader in process 10222
      invoke  active_record
      create    app/models/staff.rb
      invoke    test_unit
      create      test/models/staff_test.rb
      create      test/fixtures/staffs.yml
      invoke  resource_route
       route    resources :staffs
      invoke  scaffold_controller
      create    app/controllers/staffs_controller.rb
      invoke    haml
      create      app/views/staffs
      create      app/views/staffs/index.html.haml
      create      app/views/staffs/edit.html.haml
      create      app/views/staffs/show.html.haml
      create      app/views/staffs/new.html.haml
      create      app/views/staffs/_form.html.haml
      invoke    test_unit
      create      test/controllers/staffs_controller_test.rb
      invoke    helper
      create      app/helpers/staffs_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/staffs/index.json.jbuilder
      create      app/views/staffs/show.json.jbuilder
      create      app/views/staffs/_staff.json.jbuilder
      invoke  test_unit
      create    test/system/staffs_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/staffs.coffee
      invoke    scss
      create      app/assets/stylesheets/staffs.scss
      invoke  scss
   identical    app/assets/stylesheets/scaffolds.scss
04:12 PM: Oct 30[user1@diman]: ~/Project/staff_monitor: 15-create-controller-view-for-users-in-identity*
$\>

```

let's see the `Staff` model
```ruby
class Staff < Identity::User

end
```

> with `migration=false` option will tell rails script NOT TO generate a migration file. and with `--parent` option will make the model created inherit from another model that we created earlier.

Noticed that rails has created all required files and method in controllers that match every single columns in the table even the table from an obsolete old system.

with `rails generate scaffold` script, it can help us to create common files for our project, for that, we can focus on the database structure development and its association which are far more complicated and require more time to focus.




Thanks..

yusdirman

> p/s you need to manually set primary_key column for the model.
