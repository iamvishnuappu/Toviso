### Content
- [Stack](#stack)
  - [RUBY ON RAILS](#RUBY-ON-RAILS)
  - [PostGreSQL](#PostGreSQL)
  - [PostMan](#Postman)
- [RUBY ON RAILS + POSTGRESQL](RUBY-ON-RAILS-+-POSTGRESQL)
  - [Creating a New Rails Project](#Creating-a-New-Rails-Project)
    - [Installing Rails](#Installing-Rails)
    - [Installing Bundler](#Installing-Bundler)
    - [Creating the Application](Creating-the-Application)
  - [Cleanup Gems](Cleanup-Gems)
  - [Database](#Database)
    - [Creating Connection to database](Creating-Connection-to-database)
     - [New PG Database](New-PG-Database)
  - [Start Rails Server](Start-Rails-Server)
  - [Routes](#Routes)
  - [Controller](#Controller)
    - [Files and Folders](#Files-and-Folders)
    - [Pages](#pages)
    - [API](#Api)
  - [Migration](#Migration)
    - [Create New Migration](#Create-New-Migration)
    - [Creating Tables](#Creating-Tables)
    - [Creating Schema](#Creating-Schema)
  - [Models](#Models)
     - [Files and Folder](#Files-and-Folder)
    - [Application Record](#Application-Record)
    - [Project](#project)
    - [User](#User)
    - [Task](#task)
  - [Record](#Record)
     - [Create new Record](#create-new-Record)
     - [Render Record](#Render-Record)
        - [JSON Object](JSON-Object)
        - [Row to JSON](Row-to-JSON)
    
## Stack
[Go back to contents](#content)
- Before you install Rails, you should check to make sure that your system has the proper prerequisites installed. These include:
   - Ruby on Rails
   - postman
   - PostgreSQL
 - ## Ruby on Rails
   - Ruby on Rails is open source software, so not only is it free to use, you can also help make it better.
   - Link: https://rubyonrails.org/
- ## PostgreSQL
   - PostgreSQL is a powerful, open source object-relational database system with over 30 years of active development that has earned it a strong reputation for reliability, feature robustness, and performance.
   - Link: https://www.postgresql.org/
- ## PostMan
   - Postman is a collaboration platform for API development. Postman's features simplify each step of building an API and streamline collaboration so you can create better APIs—faster.
   - Link: https://www.postman.com/
    

____
# RUBY ON RAILS + POSTGRESQL
## Creating a New Rails Project
 [Go back to contents](#content)
 - Open up a command line prompt. On macOS open Terminal.app; on Windows choose "Run" from your Start menu and type cmd.exe. Any commands prefaced with a dollar sign $ should be run in the command line. Verify that you have a current version of Ruby installed:
 ```
 $ ruby --version
   ruby 2.7.2
 ```
## Installing Rails
  - To install Rails, use the gem install command provided by RubyGems:
  ```
 $ gem install rails
  ```

## Installing Bundler
- To install Rails, use the gem install command provided by RubyGems:
``` 
 $ gem install bundler

  ```
## Creating the Application
 - open a terminal, navigate to a directory where you have rights to create files, and run:
```
 rails new my _app
```
  *This will create a Rails application called my_app in a blog directory*
  - After you create the my_app application, switch to its folder:
```
$ cd blog
```
## Cleanup Gem
- In \Gemfile 
  - Add 
   ```
  gem 'figaro'
  gem 'pg'
   ```

- Remove -> *<mark style="background-color: lightgrey">WebPacker</mark> , <mark style="background-color: lightgrey">turbolinks</mark> , <mark style="background-color: lightgrey">Jbuilder</mark>*
``` ruby
source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '2.7.2'

gem 'figaro'

gem 'pg'

gem 'rails', '~> 6.1.3'

gem 'sqlite3', '~> 1.4'

gem 'puma', '~> 5.0'

gem 'sass-rails', '>= 6'

gem 'bootsnap', '>= 1.4.4', require: false

group :development, :test do

  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
end

group :development do
  gem 'web-console', '>= 4.1.0'

  gem 'rack-mini-profiler', '~> 2.0'
end

group :test do
  gem 'capybara', '>= 3.26'
  gem 'selenium-webdriver'
  gem 'webdrivers'
end

gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

```
 - Go into app\views\layouts\application.html.erb
 - clean up the turbo links from stylesheet_tag and javscript tag
``` JS

    <%= stylesheet_link_tag 'application', media: 'all',  %>
    'reload' %>

    <%= javaScript_tag 'application'%>

```
## Figaro
  - Figaro installation is easy:
    ```$ bundle exec figaro install```

    *This creates a commented config/application.yml file*
---
## Database
[Go back to contents](#content)
## Creating Connection to database
- In config\application.yml
 ``` yml 
   development:
     DATABASE_URL: postgres://postgres@localhost/myapp_dev
   test:
     DATABASE_URL: postgres://postgres@localhost/myapp_test
 ```
   *This is a databse connection url*

- In config\database.yml
  
  ```yml
    development:
      adapter: postgresql
      encoding: utf8
 
    test: &test
      adapter: postgresql
      encoding: utf8
  
    production:
      adapter: postgresql
      encoding: utf8
   cucumber:
       <<: *test
  
  ```
 ## New PG Database
  - To create a new Database
     - On Terminal write

  ```
  Rake db create
  ```
  *This create a database named myapp_dev on Postgres\


  - To List the Database
    - On Terminal write
  ```
  psql -u postgres
   \l
  ```
  *This list the tables on postgres*

---
## Start Rails Server
 - On terminal write one of the following
 ```
 Rails s
 Rails server
 ```
 *This starts default rails page on localhost*

---
## Routes
[Go back to contents](#content)
 - To create a new route go into *config\routes.rb*
```Ruby
Rails.application.routes.draw do
  root "pagesHome"
  namespace: api do
    get '/test' => 'pages#test'
  end
end
```
- To check for available routes
   - On terminal
    ```
    Rake Routes
    ```
## Controller
[Go back to contents](#content)
### Files and Folders

- Create a new files and folders
``` 
app\controllers\pages_controller.rb
app\controllers\api\base_controller.rb
app\controllers\api\pages_controller.rb
app\controllers\api\tasks_controller.rb

```
### Pages 
In app\controllers\pages_controller.rb
``` ruby
    class PagesController < ApplicationController
       def pagesHome
           @message = "Hello World!"
```
 *This Creates a message called Hello World on your localhost*
 	
   ![alt text](Images/1.jpg)

---

### API
- In app\controllers\api\base_controller.rb
```Ruby
   class Api::BaseController < ApplicationController
    
   end
```
- app\controllers\api\pages_controller.rb
```Ruby
  class Api::PagesController < Api::BaseController
    def test
        
        render json: {success:true}
    end
end
```
- In app\controllers\api\tasks_controller.rb
```  Ruby
  class Api::TasksController < :: BaseController
    def index
        
        render json: Task.index_json

    end
end
```
---
## Migration
[Go back to contents](#content)
- Migrations are a feature of Active Record that allows you to evolve your database schema over time. Rather than write schema modifications in pure SQL

### Create new Migration
- To create a new Rails Migration
  - On terminal
  ```
     Rails g migration setup_tables
  ```
   *This create a new migration in *db\migrate\2021031818999_setup_tables.rb*

### Creating Tables
- In db\migrate\2021031818999_setup_tables.rb define
```Ruby
 class SetupTables < ActiveRecord::Migration[6.0]
  def change
    create_table: projects do |t|
      t.string: name
      t.integer :tasks_count, default: 0
      t.timestamps
    end

    create_table :tasks do|t|
      t.string: name
      t.boolean: completed, default: false
      t.integer: user_id
      t.integer: project_id
      t.timestamps
    end

    create_table : user do |t|
      t.string: name
      t.integer: tasks_count, default: 0
      t.timestamps
    end
end

```
### Creating Schema
- To create a new Schema
  - On Terminal
    ```
     Rails db:migrate 
    ```
  - This helps to migrate and create a new file in *db\schema.rb*
```Ruby
  ActiveRecord::Schema.define(version: 2021_03_18_1 89999)do
  enable_extension "plpgsql"

  create_table "projects", force : :cascade do |t|
    t.string "name"
    t.integer "tasks_count", default: 0
    t.datetime "created_at", precision:6, null: false
    t.datetime "updated_at", precision:6, null: false
  end
   
  create_table "tasks", force: :cascade do |t|
    t.string "name"
    t.boolean "completed", default: false
    t.integer "user_id"
    t.integer "project_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision:6, null: false
  end

  create_table "users", force : :cascade do |t|
    t.string "name"
    t.integer "tasks_count", default: 0
    t.datetime "created_at", precision:6, null: false
    t.datetime "updated_at", precision:6, null: false
  end
end
```
 - To see the tables
    - On Terminal
     ``` 
        psql -u postgres
        /dt
        Select * from #Table_name
     ```

    - On Sql plus
       - Connect to database
       - Check for the created database
---

## Models
[Go back to contents](#content)
- We need to create models of the schema
### Files and Folder
- Create the following file and folders
```
app\models\application_record.rb
app\models\project.rb
app\models\task.rb
app\models\task.rb
```
### Application Record
- In app\models\application_record.rb
``` Ruby
class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
end
```
### Project
- In app\models\project.rb
 ``` Ruby
 class Project < ApplicationRecord
    has_many: task
    validate: name, presence: true
end
```
### User
- In app\models\user.rb
 ``` Ruby
 class User < ApplicationRecord
    has_many: tasks

    validate: name, presence: true
end
```
### Task
- In app\models\task.rb
 ``` Ruby
 class task < ApplicationRecord
    belong_to :user, counter_cacher:true
    belong_to :project, counter_cache: true  
    validate: name, presence: true

    def self.index_json
      self.select_value 
        
    end
end
```
___

## Record
 - ## Create new Record
   - we can use Rail Migration
     
        or
   - On Terminal open Rails Console
     ```
     Rails c
     Tablename.create()
     ```
  - To create a new project 
      ``` 
      Project.create(name:Myapp)
      ```
      - *this creates a new project named **Myapp** in the project table
  - To create a new User
       ```
       User.Create name:'Vishnu'
      ```
    - *this creates a new user named **Vishnu** in the user table*
  - To create a new task
       ```
       Task.Create name: 'Setup' user.findby_Name('Vishnu'), Project_id : 1
       ```
       - *this creates a new task named Setup with user-> **Vishnu** and projectid->**1** in Task table*
    
    **We can see the newly created values on SQl Plus or on  Rail Terminal**
    ![alt text](Images\5.jpg)
  ---
## Render Record
[Go back to contents](#content)
   ### JSON object
      - In app\controllers\api\tasks_controller.rb
  ```Ruby
     class Api::TasksController < :: BaseController
       def index
         collection = Task.order('id ASC').all
         render json: Task.index_json
       end
     end
   ```
   *This sort the task by Ascending order by Id and list all the data in the task by an Array of Json Object*
   - Open Postman and paste localhost:3000/api/tasks to check for output
  ![alt text](Images\9.jpg) 
  
  ---
  ### Row to JSON
  - In /Gemfile Add the following statement
       
       ```
       gem 'monster-queries', github: 'monsterboxpro/monster-queries'
       ```
  *On Terminal*
       ``` Bundle install```

  - Create app\queries\tasks\index.sql
  - In app\queries\tasks\index.sql
    ```SQL
      SELECT
      tasks.name,
      project.name as project
      FROM tasks
      LEFT JOIN projects ON project.id = tasks.project_id
      ORDER BY taks.id ASC
      ```
      -In app\models\task.rb
      ```Ruby
      class task < ApplicationRecord
         belong_to :user, counter_cacher:true
         belong_to :project, counter_cache: true  
         validate: name, presence: true

         def self.index_json
           sql= Q.tasks.index
           select_array sql
         end
        end
      ```
     *This creates JSON as an Array*
     	
       ![alt text](Images\10.jpg)
       
       





    
   