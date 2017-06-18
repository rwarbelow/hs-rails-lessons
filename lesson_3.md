# Day 3: Adding Status Updates

### Review

Review from yesterday:

* What did we make our app do yesterday? (Be able to add users, see users, delete users, add images, see images, delete images, make sure that images and users have the correct attributes before saving)

Preview Today:

Today, users will be able to add status updates as long as they are under 140 characters. 

### Creating Statuses

Ask students if they remember what the command to get Rails to make a new part of our app. If not, remind them: `rails generate scaffold...`

Brainstorm what characteristics a status has:

* message
* belongs to a user
* the date and time it was posted (Rails keeps track of this for us)

Tell students to type into the terminal:

```
rails generate scaffold Status message:text user:references
```

Then run `rake db:migrate`. 

*Practice*

* Have students start up the server
* Navigate to `/statuses`
* Have students try adding a few statuses. Are they able to leave out fields? Do they remember how to fix that? Encourage them to reference `user.rb` to check how they validated the presence of certain fields, and do the same to `status.rb`
* Do they remember (or can they reference their _form file) how to make it so that the users all appear in the drop down? Encourage them to find that line and copy-paste. 

### Making the User Name User Friendly

Right now, when someone adds a status, their name shows up something like `#<User:0x007f4126550a00>`. Ask students what they expect it to say. 

Use the navigation tab to open `show.html.erb` for the statuses. Ask students to find the line that says something about a user. Have them change that line from:

```
<%= @image.user %>
```

to

```
<%= @image.user.name %>
```

Refresh the page. 

*Practice*:

* Have students go to `/statuses` and see if they see a similar problem. Guide students to open `index.html.erb` for statuses and fix the problem.
* Have them do the same for show and index of images from yesterday. 

### Navigating Website

Right now, there's no easy way to get from the home page (users index) to statuses and images. Let's add some links. 

Have students go to the `application.html.erb` file using navigation. At the top, add these links:

```
  <%= link_to "Users", users_path %>
  <%= link_to "Statuses", statuses_path %>
  <%= link_to "Images", images_path %>
```

Briefly explain that those lines will create a link with the text in quotes going to the path after the quotes. Be sure to again emphasize that *every character matters* and the computer will not automatically fix any typing mistakes. 

Also point out that now anywhere we go on our web app, we see these three links. This is the purpose of the application.html.erb file -- it wraps the entire page. 

### Making Things Less Ugly

I'm guessing that along the way, students have asked about how to make things less... ugly. We're going to use Bootstrap to make things a little nicer. If you want to talk about what Bootstrap is and what it does, feel free. 

Tell students to stay on application.html.erb and add this line above `stylesheet_link_tag` (line 7):

```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css">
```

If they save and refresh, they might notice that the font changed, but not much else. 

Have students go to the `nav` documentation for [Bootstrap](https://v4-alpha.getbootstrap.com/components/navs/) and choose a navbar that they want for their project. Demonstrate copying in the code and pasting it in the correct place in `application.html.erb`. 

Once every student has saved and refreshed, demonstrate moving the `link_to`s into their correct place and removing any unnecessary classes. Mine looks something like this:

```
<ul class="nav nav-pills nav-fill">
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

Next, we'll clean up the statuses. Have students open the `index.html.erb` page for statuses using the navigation tab and *delete the whole thing*. I don't noramlly like when students copy-paste, but here, I would give them this in a Github gist to copy into their file. 

```
<p id="notice"><%= notice %></p>
<h1>Statuses</h1>

<%= link_to 'New Status', new_status_path %>

<% @statuses.each do |status| %>
  <div class="card status-card" style="width: 50%;">
    <div class="card-block">
      <h4 class="card-title">Posted by <%= status.user.name %></h4>
      <p><%= status.created_at %></p>
      <p class="card-text"><%= status.message %></p>
      <%= link_to 'Edit', edit_status_path(status) %>
      <%= link_to 'Destroy', status, method: :delete, data: { confirm: 'Are you sure?' } %>
    </div>
  </div>
<% end %>
```

Have students save and refresh the page. Tada! It looks marginally better. Ask students what's weird about the way the date displays. Hopefully they say that it is not normally how you write a date. Let's fix that.

Have students go to [For A Good Strftime](http://www.foragoodstrftime.com/) and show them how to build a date. Tell them to either use a preset or click the "build your own" tab to find a date format that they like, then copy the symbols (those %d, %A things) to paste into this line of their index. MAKE SURE THAT IT IS IN QUOTES!

```
  <p><%= status.created_at.strftime("%b %e, %l:%M %p") %></p>
```

Let's change one more funky thing before we finish off with some fancy colors and fonts. Have students create another status. Pose question: what is weird about where it takes you once you post it? Where should it take you? 

Hopefully, students say that it should take you back to the list of all statuses (the index). 

Have students open their `statuses_controller.rb` and find the `def create` method. Currently, line 31 says:


```ruby
  format.html { redirect_to @status, notice: 'Status was successfully created.' }
```

Have students change `@status` to `statuses_path`, like this: 

```ruby
  format.html { redirect_to statuses_path, notice: 'Status was successfully created.' }
```

*Practice*:

Right now, the image creation does something similar -- instead of taking the user back to all of the images, it takes the user to one particular image. Have students open up the `images_controller.rb` file and find the `def create` method. Ask students: "Can you think of what we need to change here? Do you see something similar to what we just changed?"

Make sure that students change the redirect line to say:

```ruby
  format.html { redirect_to images_path, notice: 'Status was successfully created.' }
```

Then pose the question: What is weird about the order that the statuses are displayed in?

Hopefully students say that the newest statuses should be at the top. We can fix that! Have them go to the file `status.rb` and add this line right between the current lines 1 and 2: 

```ruby
  default_scope { order(created_at: :desc) } 
```

*Practice*:

Do the same thing to the `image.rb` file's ordering:

```ruby
  default_scope { order(created_at: :desc) } 
```

### A Splash of Color

Let's close out today by playing around with color and border for our status cards. Tell students to open `application.css` and type this under line 16 (bottom of the file):

```css
.status-card {
  background-color: red;
}
```

Super ugly, right? Point students in the direction of [my favorite color site](http://colours.neilorangepeel.com/) to find a color that they like. Once they find it, they can replace `red` with the color they actually want. 

Finally, let's modify the margin and border. Have students add to the previous CSS to center, create vertical space between statuses, and add a border:

```css
.status-card {
  background-color: red;
  margin: 10px auto 0;
  border: 2px solid blue;
}
```

Give students a few minutes to play around with the amount of margin, the width of the border, and the color of the border. They can also try `dashed` for the border instead of `solid`. 

Once they have something they're happy with, they can trade with a partner and have them test out their app. 

### Extra Time? 

If today's lesson finishes early, here are some ideas: 

* feel free to show students the `font-family` property
* experiment with other CSS properties
* guide students to codecademy.org and click on the Ruby track for practice
* start on Day 4 lesson since there is quite a bit in there

### Close Out

* What were some of the problems we ran into that we needed to solve? In what ways did we solve those problems?

Preview tomorrow: 

Tomorrow, we will clean up the images page to make sure our images are not all different sizes. We'll also modify one final page to make sure we can see both statuses and images posted on the same page. 
