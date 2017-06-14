# Day 1: Getting Set Up and Ruby Basics

### Intro

Give students an overview of the week:

* Monday: Getting set up with Cloud9 IDE and Ruby Basics
* Tuesday: Your First Rails App
* Wednesday: Rails, Part II 
* Thursday: Rails, Part III
* Friday: Project Work Time & Demos

By the end of this week, students will have created a Twitter-like app to use with their friends.

### Getting Set Up

Send out the Cloud9 invites and have them sign up for an account. Once every student is signed up, walk them through creating a workspace:

1) Click "Create a new Workspace"
1) Type "learntocode" for the workspace name
1) Change the team to "Don't set a team for this workspace"
1) Click "Blank" for the template
1) Click "create workspace". This step might take a little while. 

Once each student has created a workspace, explain each of the three areas: 

1) The file tree: for organizing files and folders
1) The Command Line (or Terminal): for telling the computer to run a program (or do something)
1) The Text Editor: for writing code

### Practice with Command Line

Tell students that the command line is where we give the computer commands. For example: we can tell the computer to make a folder from the command line. Show students the command `mkdir myfolder` and watch it appear in the file tree.

*Practice*: Have students make three folders called music, photos, and apps.

Show students how to delete a folder with the command `rm -rf myfolder`. Warn students to be super careful when using this command, and explain why it can be dangerous.

*Practice*: Have students delete music, photos, and apps. 

Show students how to make a file with `touch my_file.txt` and watch it appear in the file tree. Explain the concept of a file extension (like .txt, .docx, .xls, etc.)

*Practice*: Have students create three files called english_essay.txt, research_paper.txt, and todo_list.txt.

Show students how to delete a file with `rm my_file.txt`. 

*Practice*: Have students delete their three files. 

Mid-lesson Recap: Ask students the following questions: 

* What is the command line for?
* What is the command for making a folder?
* What is the command for making a file?
* What is the command for deleting a file?
* What is the command for deleting a folder?

Show students the command `ls`. Right now, nothing should appear since everything has been deleted. Create a few files and folders, then try the `ls` command again. Have students come up with a definition for what `ls` does. 

Pose the question: What might we need to do if we wanted to make a file inside one of our folders? Show the command `cd foldername` and point out that the command line prompt changes to show where we are. At this point, feel free to draw a tree diagram on the board if you feel it is helpful to explain the file system. 

Show students how to get out of the folder with the command `cd ..`. 

*Practice*: Give students these instructions:

* Create a folder called `examplefolder`. 
* Change directories into the folder.
* Make a file inside the folder called `examplefile.txt`
* Type a command to display the contents of the folder
* Get back out of the folder
* Remove the folder

### Writing a Simple Ruby Program

Explain to students that we are going to write a very simple program in the Ruby programming language. Explain that Ruby is a programming language that is popular for writing web applications (give some examples of web applications). It's also a relatively easy programming language in comparison to other programming languages. 

Tell students that our very simple program today will help us learn the basics of Ruby which we will then be able to use when we build our own web applications. 

First, have students create a file: `touch my_program.rb`. Inside the file, have students type:

```ruby
puts "Hello, world! This is my first program."
```

Make *sure* to have students save work either with command-s or File -> Save. Move into the command line and type `ruby my_program.rb`. Discuss what happened when they ran that command. Ask students to figure out a definition for what the `puts` command does. They can experiment with taking away `puts` and saving and running the program to see what happens. 

You can also have students experiment to see what happens when they remove the quotes or forget a closing quote in order for them to develop an understanding of what is necessary to create a working program. Show students the trick of getting back the last command typed by pressing the up arrow. 

*Practice*: Tell students to modify the program so that a different message prints out to the screen. Have students share their modifications with a partner. 

Next, have students modify program to ask the user for their name. Underneath the first line, have the students type:

```ruby
name = gets.chomp
```

Explain that `gets.chomp` will get an answer from the user. The part `name = ` makes the computer remember whatever the user types in as their name. We call that part a variable. Compare this to math class where we can use variables to represent different numbers. 

On the third line, have students type:

```ruby
puts "Nice to meet you, #{name}!"
```

Save and run the program. Have students run the program a few times and encourage them to type in different names when the computer asks their name. Then, ask students: "What does the last bit of the line (the part with a hashtag, curly braces, and name) do? How do you know?"

*Practice*: Tell students to duplicate first two lines so that the program also asks for an age. Then, modify the final line of the program so that it prints out something like "Your name is Maria and you are 15 years old." Obviously, the name and the age should be dynamic values. 

*Recap*: Discuss the following questions:

* What does "puts" do in a Ruby program? When and why would we use "puts" in our programs?
* What does gets.chomp do? When and why would we use it?
* What does it mean when we see something like `name = ` or `email = ` or `message = `? Why do we need these? 
* How do we put a variable into a sentence in Ruby? 
* How do you run a program once you've written and saved it? 
* What are things that might make a program not run? (examples: misplaced quote marks, missing equals sign, extra random characters, etc.)

### A More Complex Ruby Program

Explain to students that programs become powerful when you can make decisions based on what input the user gives you. Tell students that we will practice making a more complicated program using what's called control flow. 

First, make a new file: `touch game.rb`. 

Explain to students that we will start with a very simple game, then make it more complicated. Students will type the following code into their file:

```ruby
puts "Which word am I thinking of? Type dog, hat, or pen."
answer = gets.chomp

if answer == "pen"
  puts "Correct! You guessed my word."
else
  puts "Too bad. That wasn't the word I was thinking of."
end
```

Have students walk through each line of code in pairs to guess what will happen. Pose the question: What do you think will happen if I guess the word "hat"? What about "pen"? Why do you think that?

Have students save the program and run it a few times. 

*Practice*: Have students modify the program with a different set of three words. Have them share their new program with a partner. 

Next, have students change the program to the following:

```ruby
secret_word = ["dog", "hat", "pen"].sample

puts "Which word am I thinking of? Type dog, hat, or pen."
answer = gets.chomp

if answer == secret_word
  puts "Correct! You guessed my word."
else
  puts "Too bad. The word I was thinking of was #{secret_word}"
end
```

*Practice*: Have students modify the program to include six words of their choice. When they think their program works, have them trade with a friend and try it out. 

### Strings vs. Integers

Tell students that so far, the data we've dealt with has been a string. That means anything inside of quote marks. Let's modify our game to make it a number guessing game. 

Prompt students to figure out what needs to change about line one (where the array is assigned). Students should figure out that it should look something like this:

```ruby
secret_number = [1,2,3,4,5].sample
```

Explain that we don't use quotes because we want these to be numbers, not text.

Next, step away from the code to have students walk through the possible conditions for a number guessing game. They should come up with three possibilities: number is correct, number is too high, number is too low. 

Walk students through the process of modifying the if statement to look like this (and briefly talk about `.to_i`):


```
secret_number = [1,2,3,4,5].sample

puts "Which number am I thinking of? Type 1,2,3,4, or 5."
answer = gets.chomp.to_i

if answer == secret_number
  puts "Correct! You guessed my number."
elsif answer > secret_number
  puts "Your guess was too big! I was thinking of #{secret_number}."
else
  puts "Your guess was too small! I was thinking of #{secret_number}."
end
```

Have students run the program a few times. 

*Recap*:

* What do we need in our programs if we want the computer to make a choice? 
* Why do we have `else` without anything on the same line with it?

### Extra Time? 

If today's lesson finishes early, guide students to codecademy.org and click on the Ruby track for practice. You could even make some sort of challenge to finish the most number of lessons for a prize. 

### Close Out

* What was the most interesting thing you learned today?
* What was the most confusing thing you learned today?

Preview tomorrow:

We will start on our Twitter-like apps tomorrow, so think of a good name for your app!
