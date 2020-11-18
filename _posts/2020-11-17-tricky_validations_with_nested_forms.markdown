---
layout: post
title:      "Tricky Validations with Nested Forms"
date:       2020-11-18 02:38:29 +0000
permalink:  tricky_validations_with_nested_forms
---


I've just finished building my first Rails web application, a simple to-do app built for collaboration among friends. I faced a few interesting challenges along the way, but perhaps my favorite was when I decided to introduce a nested form. See, for the purposes of streamlining the relationships in this app, I wanted Projects and Users to only be related through Tasks. If a User has created a Task associated with a Project, that Project should then show up on their homepage. The problem arises, though, when a User creates a new Project that has no Tasks. So, to fix this, I made sure that new Projects were created with a new Task with the help of a nested form. 

Originally, I set things up like this:

The nested form:
```ruby
<%= form_for @project do |f| %>
    <%= f.label :title %>
    <%= f.text_field :title %> <br>
    <%= f.label :private, "Private?" %> 
    <%= f.check_box :private, checked: false, unchecked: true %> <br>

    <%= f.fields_for :tasks do |task| %>
        <div>Make sure to add a task to get your project started!</div>
        <%= task.hidden_field :user_id, value: current_user.id %>        
        <%= task.label :title %>
        <%= task.text_field :title %> <br>
        <%= task.label :due_date %>
        <%= task.date_field :due_date %> <br>
    <% end %>

    <%= f.submit %>
<% end %>
```

The Projects controller:
```ruby
class ProjectsController < ApplicationController
	...
    def new
        @project = Project.new
        @project.tasks.build
    end

    def create
        @project = Project.create(project_params)
        if @project.save
            flash[:success] = "Your project was created."
            redirect_to project_path(@project)
        else
            flash[:error] = "Something went wrong! Please try again."
            redirect_to new_project_path
        end
    end
	...
	private
	
    def project_params
        params.require(:project).permit(
            :title,
            :private,
            tasks_attributes: [
                :user_id,
                :title,
                :due_date
            ]
        )
    end
end
```

The models:
```ruby
class Project < ApplicationRecord
    has_many :tasks
    accepts_nested_attributes_for :tasks
    has_many :users, through: :tasks
    has_many :comments, as: :commentable
    
    validates :title, presence: true
    validates :private, inclusion: [true, false]
end
```
```ruby
class Task < ApplicationRecord
    belongs_to :user
    belongs_to :project
    has_many :comments, as: :commentable

    validates :title, :user_id, :project_id, presence: true
    validates :completed, inclusion: [true, false]
    ...
end
```

So, what happens here is the Projects controller creates a new blank Project, then builds a new blank Task on that Project before sending everything over to the template where the logged in User can create a new Project and Task. Then, thanks to `accepts_nested_attributes_for` in our Project model, and the Task relevant strong params in our Projects Controller, a new Project and Task should be created and saved to the database, right? 

Unfortunately, the "Something went wrong!" message is displayed to the User. If we `pry` into the error messages for our `@project` variable, we see this...

```
pry(#<ProjectsController>)> @project.errors.messages
=> {:"tasks.project_id"=>["can't be blank"]}
```

What seems to be happening here is that ActiveRecord wants to check all of the validations before it goes to the database to save the information about these two new objects. When it runs the validations for a Task, it finds that there is no `:project_id`, because the Project object hasn't been saved yet either. At this point, it cancels the entire transaction, and we get neither a new Project, nor a new Task. 

According to my research, since the release of Rails 5, `belongs_to` automatically validates for the presence of a parent object when saving a child object to the database. Since we have built our child object on the parent, the additional validation becomes obsolete, and in the case of validating for the presence of an ID, problematic.

So the Task model that we end up with looks like this:

```ruby
class Task < ApplicationRecord
    belongs_to :user
    belongs_to :project
    has_many :comments, as: :commentable

    validates :title, presence: true
    validates :completed, inclusion: [true, false]
    ...
end
```

Notice that in addition to removing the validation for `:project_id`, we can now also remove the `:user_id` validation, as that is also taken care of by `belongs_to :user` in the model.

The moral of the story, or at the very least, my takeaway, is that more is not always better when it comes to validations. In an attempt to be thorough, I ended up disregarding the built in validations and causing myself trouble down the road. Always be sure to [check out the docs](https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html#method-i-belongs_to) for what code is generated for you by your ActiveRecord macros and other Rails generated code as to avoid unexpected behavior.
