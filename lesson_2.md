# Day 2: Intro to Rails

### Review

Review from yesterday:

* What things are important to pay attention to when coding? Why? 
* What is the difference between a string and an integer? 

Remind students that by the end of this week, students will have created a Twitter-like app to use with their friends. Today, we'll start with creating users for our app. 

### Getting Set Up

Have students open Cloud 9 accounts and go to same workspace from yesterday. Once inside the workspace, have students `gem install rails`. While this is happening, explain what a gem is and what is happening behind the scenes. 

Talk also about what Rails is: I like to think of it as a kit to build a web app. Talk to students about the fact that you could build something without a kit, but it would take a lot more time, patience, and knowledge. If you already have the individual pieces available, it will make your life easier. 

Explain that Rails looks very complicated, so it's ok to feel overwhelmed. However, they can follow simple commands and steps to build a simple web app. 

### Generating a Rails App

Remind students that they are building a site where users can post messages and photos (kind of like Twitter). Have students brainstorm names for their Twitter-like apps. Then type `rails new name-of-app`. 

When students type `ls`, they should see their app as a folder. Have them `cd` into the folder. 

When we want to "start" the app so that you can visit it online, we follow these steps:

1) Type `rails s -b $IP -p $PORT` into the command line. Stress to students that *every* symbol and letter in this command is super important! Watch out for careless errors, additions, or omissions. 
1) Click "Preview" in the top center of the screen and select "Preview Running Application". A new tab should pop open in the text editor area. If you choose to, you can have students copy the web address and paste it into a new tab so that it looks like a real web app in a separate browser window. 

Explain to students that anyone can now go to this web address to see their app! Obviously, our app doesn't do anything yet, but eventually we will see our app here. 

Tell students that in order to develop our app, we will need to shut down the server by pressing `control-c`. 

### Making Users for Our App

Pose the question: Where does Twitter or Facebook or Instagram keep all of its' data? How does it know whether or not you have an account? 

Discuss this with students and get them to the point of understanding that data is kept in a database that the company owns. 

We will need to create our own database, in addition to creating a way to make new users, edit existing users, and delete old users. We will do this with a tool known as a scaffold generator. Before we do that, we need to brainstorm characteristics of a user. 

1) Name
2) School
3) Grade level
... etc? 

Keep it simple -- don't get into dates, etc. for data types. 

Once you have brainstormed some characteristics for a user, have students follow these steps:

1) From the command line, type `rails generate scaffold User name:text school:text grade_level:integer` and discuss why we put quotes and data types after the characteristic. 
1) Look at the output of this command and give a high-level overview of what happened (Rails made a whole bunch of files for us -- it basically wrote the code for our program).
1) Show students that if we made a mistake, we can run `rails destroy scaffold User` and it deletes all of the files it just made. Demonstrate that on your screen. 
1) Redo the scaffold generator to get it back. 
1) Run `rake db:migrate`. Explain to students that this will tell the database that we're ready to add users. 

Have students start up the server using the command `rails s -b $IP -p $PORT`. If they closed the preview tab, they will need to reopen it. Add `/users` to the end of the web address in the preview (like `https://projectname-username.c9users.io/users`). 

Tell students to add themselves and a few friends as users in the program. 

### Validations

Have students click "New User" and tell them to try creating a user that doesn't have a name, but *does* have a school and grade. Then have them discuss:

* Did it work?
* Should it work?
* What should we tell the computer to check for before it actually makes a user? 

Introduce the concept of validations. You can think of validations as a guard who checks to make sure everything is ok before he/she lets you into the system. 

We will need to make a change to one of our files. Remind students that before we make a change, we need to shut down our server. Click in the command line area of Cloud 9 and press `control-c`. 

There are a lot of files in a Rails app, so there's an easy way to find them. Over on the left-hand side in vertical text, there is a tab that says "Navigation". Click on it, then in the search bar, type `user.rb`. Click on the first file. 

Explain that this file will eventually contain any rules that we want for a user. Right now, we would consider this file almost empty since it doesn't contain any rules. 

Explain to students that we want to tell the computer that we want to make sure the user has a name. We do this by modifying the file to say:

```ruby
class User < ApplicationRecord
    validates_presence_of :name
end
```

*MAKE SURE TO SAVE THE FILE!!!*

Now start up the server again (show students that they can click the up arrow in the terminal to bring back the command for starting the server) and try to add a user that doesn't have a name. Did it work? 

*Practice*: 

* Have students try creating users that are missing either a school name or a grade. 
* Can they figure out what to modify so that a user can't be created unless there is a grade and a school? Make sure to save your file!
* Students can use the "Destroy" link to delete any users that should not be in the system. 
* Once they think their app is working, have them save their files, restart the server, and give the project to a friend to see if they can find any bugs. 

*Recap*:

Discuss with students:

* Why did we have to type things like `name:text` and `school:text` and `grade:integer`? 
* What happened when we didn't tell the computer what things to check for on a user? How did we fix this problem? 

### Adding a Root Route

Right now, we have to `/users` every time we want to use our app. Is that annoying? Yes. 

Prompt students to figure out what should happen when they open the app. Hopefully, they'll say that it should go to the list of users. 

Have students open the file navigation tab on the left hand side of Cloud 9 and find the file called `routes.rb`

Students should see something like this:

```ruby
Rails.application.routes.draw do
  resources :users
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
end
```

Tell students they can delete the line `# For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html`. We will add one line in here that will make the root of our app go to the users page:

```ruby
Rails.application.routes.draw do
  resources :users
  root 'users#index'
end
```

You can discuss the syntax of the line a bit if you feel like the group can understand what is happening. 

*MAKE SURE TO SAVE THE FILE!!!*

Start the server and open the page. Does it work? 

### Adding Image Posting

Right now we have users. Pose to students: What steps do you think we'll need to repeat in order to add image posting to our app? 

Guide students to come up with the following:

* `rails generate scaffold Image...`
* an image has a source (or URL -- you can use Google Images to demonstrate how each image has a source)
* we need to know which user posted the image
* the image might need a caption
* we will want to know the date and time the user posted the image -- luckily, Rails will manage this for us

Have students type `rails generate scaffold Image source:text caption:text user:references` and explain that `user:references` will make it so that the image belongs to a specific user.

Once the scaffold generator is complete, have students type `rake db:migrate` to tell the computer we are ready to start accepting images. 

Start up the server using `rails s -b $IP -p $PORT` and have kids navigate to `/images`. Get to the New Image page and show students how to use Google Images to find the URL of an image. If they don't have any ideas for images, they can pick their favorite baby animal. 

Students will paste their image URL into the field that says "Source", then add a caption, and type their name in for the user. When they click save, it should give an error "User must exist". 

Explain to students that the computer doesn't keep track of people by their name, but instead by a number. The first person you entered into the users was person #1; the second was person #2, etc. It will work if we use a number, but then when we have 100 people using our app, it will be hard to remember who had which number. 

Instead, tell students that we will change the way the form works. Open the navigation panel and type `form`. Two files called `_form` will pop up. Guide students to look closely at the small text to figure out which one we need. They should choose the one that has `image` in the file path name.

Have students look at code in form and ask them where they see the form talking about the user. They should point to lines 25 and 26. Out of those two lines, ask them to figure out which one they think represents the actual input box. Hopefully they eliminate the one that says "label" and choose line 26.

Have them delete line 26 and instead type:

```
<%= form.collection_select :user_id, User.all,:id,:name %>
```

If you want, you can walk through what exactly this line does. In a nutshell, it's telling the computer to display all users and show their name in a dropdown. 

Have students save the file, start up the server, and try again. 

*Review*:

* Prompt students to discover: will it let us create an image without a caption and source? How did we fix that last time? 
* Have students open `image.rb` and add the validations for caption and source. 

### Making the Image Display

Right now when an image is created, it just shows the URL, not the actual image. Have students find the file `show.html.erb` using the navigation, and again, guide them to pick out the one that is for images. 

Have them find the line where the source is displayed, and change it to say:

```
<%= image_tag(@image.source) %>
```

Save, then refresh the page. Tada!

*Practice*:

* Navigate back to `/images`. What do you notice is wrong? Do we know how to fix it? 
* Guide students to find `index.html.erb` for images. Have them locate the line that needs fixing to add an image tag. 

### Extra Time? 

If today's lesson finishes early, guide students to codecademy.org and click on the Ruby track for practice. You could even make some sort of challenge to finish the most number of lessons for a prize. 

### Close Out

* What does Rails let us do?
* What was the most interesting thing you learned today?

Preview tomorrow: 

Tomorrow, you will add functionality so that users can also post status updates as long as the are less than 140 characters. You will also add links on the top of your page so that a person can click to get somewhere else in the app without having to type in a new web address. 
