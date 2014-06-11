# Backbone Rails Blog example

##Version##
A small example using  `rails-backbone gem`


## Example Usage

Created a new rails application called `blog`.

    rails new blog

Edit your Gemfile and add

    gem 'rails-backbone'

Install the gem and generate scaffolding.

    bundle install
    rails g backbone:install
    rails g scaffold Post title:string content:string
    rake db:migrate
    rails g backbone:scaffold Post title:string content:string
    
You now have installed the backbone-rails gem, setup a default directory structure for your frontend backbone code. 
Then you generated the usual rails server side crud scaffolding and finally generated backbone.js code to provide a simple single page crud app.
You have one last step:

Edit your posts index view `app/views/posts/index.html.erb` with the following contents:

    <div id="posts"></div>

    <script type="text/javascript">
      $(function() {
        // Blog is the app name
        window.router = new Blog.Routers.PostsRouter({posts: <%= @posts.to_json.html_safe -%>});
        Backbone.history.start();
      });
    </script>
    
If you prefer haml, this is equivalent to inserting the following code into `app/views/posts/index.html.haml`:

    #posts
    
    :javascript
      $(function() {
        // Blog is the app name
        window.router = new Blog.Routers.PostsRouter({posts: #{@posts.to_json.html_safe}});
        Backbone.history.start();
      });

    
Now start your server `rails s` and browse to [localhost:3000/posts](http://localhost:3000/posts)
You should now have a fully functioning single page crud app for Post models.

####With Rails 4:####
If you are using the default Rails 4 scaffold generators, you will need to adjust the default JSON show view (IE, 'show.json') to render the 'id' attribute.

default rails generated show.json.jbuilder

`json.extract! @post, :title, :content, :created_at, :updated_at`

Change it to add `id` attribute as well

`json.extract! @post, :id, :title, :content, :created_at, :updated_at`

Without adjusting the JSON show view, you will be redirected to a "undefined" url after creating an object.
