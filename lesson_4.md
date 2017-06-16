# Day 4: Creating the News Feed

### Review

Review from yesterday:

* What did we make our app do yesterday? (Fix the way user names display, fix the way images display, style the statuses page)

Preview Today:

Today, we will do two things:

* Style the images page
* Overhaul the home page to make it a full news feed with statuses and images

### Styling the Images Page

Have students start up their server and navigate to `/images`. 

*Practice*:

Have students use the "New Image" link and use Google Images to add urls for a few images. 

Then look at the `/images` page. It looks awful. Ask students to brainstorm why it looks so terrible. Hopefully they say spacing, varying height/widths of images, etc. 

Have students open `index.html.erb` for images using the navigation tab on Cloud 9. Again, have this code in a gist or something easily accessible for students to copy and paste:

```
<p id="notice"><%= notice %></p>

<h1>Images</h1>

<%= link_to 'New Image', new_image_path %>
<div>
<% @images.each do |image| %>
  <div class="card image-card" style="width: 50%">
    <div class="card-block">
      <h4 class="card-title">Posted by <%= image.user.name %></h4>
      <p><%= image.created_at %></p>
      <p class="card-text"><%= image_tag(image.source, width: "150") %></p>
      <%= link_to 'Edit', edit_image_path(image) %>
      <%= link_to 'Destroy', image, method: :delete, data: { confirm: 'Are you sure?' } %>
    </div>
  </div>
<% end %>
</div>
```

Save and refresh the page.

*Practice*:

* Have students play around with changing the `width: "150"` property of the image tag to see what they like best. 
* Again, guide students toward [For A Good Strftime](http://www.foragoodstrftime.com/) and have them reference back to `index.html.erb` for statuses to see how they added the formatting for the date and time. Do the same thing for the images' `index.html.erb`. 

### Styling Images

Have students go to `application.css` and add a new section for `.image-card` underneath `.status-card` from yesterday. It should look something like this:

```css
.status-card {
  background-color: red;
  margin: 10px auto 0;
  border: 2px dashed blue;
}

.image-card {
  background-color: orange;
  margin: 10px auto 0;
  border: 2px solid black;
}
```

This is just a starter set of style code. Give students a few minutes to play around with color, margin size, border size, etc. If you want, feel free to introduce font-family as well. 

### Making a News Feed

Have students navigate to the root path. Have them compare to when they log in to Facebook, Twitter, or Instagram. What do you see when you log into those sites? How is the data organized? 

Get them to the point of recognizing that we want this page not to just show the users in the system, but to show all of the posts in the database. 

First, have students go to `routes.rb` and change `root 'users#index' to:

```ruby
root 'newsfeed#index'
```

Then, have students type in the command line:

```
rails g controller Newsfeed index
```

This will make a `newsfeed_controller.rb` (with code setup) and an `index.html.erb` file for the newsfeed. If you feel like the group can handle a discussion of a deeper understanding of what files are created and what we use them for, feel free to go that direction at this point. 

Have students open the `newsfeed_controller.rb`. Explain to students that this is the part of our application that is going to get data ready to show to the user.

Pose question: "What data from our database needs to be pulled out to display on the newsfeed?" If they struggle with this question, ask them to think back to Twitter/Facebook/Instagram. What data is shown on the newsfeed? 

Get students to the point where they understand that we need to pull the photos and the statuses from the database. Inside `def index`, type:

```ruby
class NewsfeedController < ApplicationController
  def index
    @posts = Status.all + Image.all
  end
end
```

Then go to `index.html.erb` for the newsfeed. Review that this is the place where we put what we want the user to see, and ask students: Now that we have the data for all of the statuses and images, what do we do here? 

Hopefully they say that we display that data using HTML. If not, discuss this with them.

This next part is a little tricky. We're going to be doing some copy-paste but changing a few things. I would clearly demonstrate this one piece of a time, then have students do the same for their own. First, in `index.html.erb` for the newsfeed, type:

```
<% @posts.each do |post| %>
  <% if post.kind_of?(Image) %>

  <% elsif post.kind_of?(Status) %>

  <% end %>
<% end %>
```

Discuss the basic concept of what is happening (we take a collection of data -- posts and statuses mixed together), then we ask whether the post is an Image or a Status. If it is an image, what should we do? If it is a status, what should we do? Is there a difference between what the user sees for each type? Remind students that they styled each type differently. 

(And yes, we're going to have some duplicated code which is bad practice, but to avoid confusing students with partials and helpers, just go with it.)

Once students have their if/elsif skeleton set up, then demonstrate *just you* doing the following thing:

Go to `index.html.erb` for images. Select the lines that start with `<div class="card card-image"...` all the way until the line *before* the `<% end %>` but not including that line:

```
<div class="card image-card" style="width: 50%">
  <div class="card-block">
    <h4 class="card-title">Posted by <%= image.user.name %></h4>
    <p><%= image.created_at.strftime("%b %e, %l:%M %p") %></p>
    <p class="card-text"><%= image_tag(image.source, width: "150") %></p>
    <%= link_to 'Edit', edit_image_path(image) %>
    <%= link_to 'Destroy', image, method: :delete, data: { confirm: 'Are you sure?' } %>
  </div>
</div>
```

Then copy it, go back to your index for newsfeed, and paste it in the blank spot after `<% if post.kind_of?(Image) %>`. 

Have students repeat that step. 

Then, demonstrate changing all instances of `image` to `post` so that it looks like this. There are five places where they need to change it:

```
<div class="card image-card" style="width: 50%">
  <div class="card-block">
    <h4 class="card-title">Posted by <%= post.user.name %></h4>
    <p><%= post.created_at.strftime("%b %e, %l:%M %p") %></p>
    <p class="card-text"><%= image_tag(post.source, width: "150") %></p>
    <%= link_to 'Edit', edit_image_path(post) %>
    <%= link_to 'Destroy', post, method: :delete, data: { confirm: 'Are you sure?' } %>
  </div>
</div>
```

Now, have students do the same thing.

*Repeat these steps with the `div` for statuses.*

Once finished, save the `index.html.erb`, start up the server, and go to the root. If all goes as planned, you should see a primitive news feed. 

Pose these questions for discussion: What is wrong with this news feed? Where are the most current statuses and images? How are the statuses and images organized? How should they be organized? 

### Organizing Posts

Go back to `newsfeed_controller.rb`. Discuss how right now, we're just taking *all* of the statuses and then adding on the end *all* images after that. What should we be sorting them by? Hopefully, students say that they should be sorted by when they were created. 

Have students update their line to say:

```ruby
class NewsfeedController < ApplicationController
  def index
    @posts = (Status.all + Image.all).sort_by(&:created_at)
  end
end
```

Save, start the server, and refresh the page. Ask students what they notice. Hopefully, the images and statuses are now mixed together, but it's organized from oldest at the top of the page to newest at the bottom of the page. We want the opposite order. Let's fix that:

```ruby
class NewsfeedController < ApplicationController
  def index
    @posts = (Status.all + Image.all).sort_by(&:created_at).reverse
  end
end
```

Save and refresh. Tada!

### One more thing...

Right now, if a student navigates away from the newsfeed to one of the links on the top, they can't get back to the newsfeed with a click (only by modifying the url). 

Have students to go `application.html.erb` and add a link to the root path:

```
<ul class="nav nav-pills nav-fill">
  <li class="nav-item">
    <%= link_to "Newsfeed", root_path %>
  </li>
  <li class="nav-item">
    <%= link_to "Users", users_path %>
  </li>
  <li class="nav-item">
    <%= link_to "Statuses", statuses_path %>
  </li>
  <li class="nav-item">
    <%= link_to "Images", images_path %>
  </li>
</ul>
```

Save, refresh. 

### Try it out

Have students switch computers and try out adding a few users, statuses, and images. Look at the newsfeed. Is everything working? 

### Have Extra Time? 

Have students create a footer div on the bottom of their `application.html.erb` with a bit of info about themselves and a copyright 2017 symbol. 

OR

Show students how to create an extra page where they can add their name, a bio, and a photo. 

Other ideas from yesterday: 

* feel free to show students the `font-family` property
* experiment with other CSS properties
* guide students to codecademy.org and click on the Ruby track for practice

### Close Out

* What were some of the problems we ran into that we needed to solve? In what ways did we solve those problems?

Preview tomorrow: 

Tomorrow, we will clean up any design/layout bits of our program with some HTML and CSS and practice our presentations. 
