This project is based on the Python crash course book. Most of the rough notes below are copied from the book too for reference.

## Running project
- activate virtual env
- pip3 install django
- pip3 install django_heroku
- pip3 install bootstrap4
- pip3 install django-bootstrap4
- python3 manage.py migrate
- python3 manage.py runserver

### Writing a web spec
A spec needs to:
- detail project goals
- project functionality
- discuss appearance
- user interface

### Project spec
We’ll write a blog app  that allows users to blog on topics they’re interested in and to make journal entries. The blog  home page will describe the site and invite users to either register or log in. Once logged in, a user can create new blogs, add new entries, and read and edit existing entries.

### Django website
- manage.py - file takes in commands and feeds them to parts of django that needs it
- settings.py - controls how django interacts with your system and manages your project
- urls.py - tells django which build in response to browser requests
- wsgi.py - helps django serve the file it creates

Database
----
- django stores info in databases
command: python manage.py migrate
- we modify a database, we say we’re migrating the database
- The SQLite is a database that runs off a single file

Viewing the project
---
- running the project:
command:python manage.py runserver
- The URL http://127.0.0.1:8000/ indicates that the project is listening for requests on port 8000 on your computer, which is called a localhost
starting app
command: python3 manage startapp blog_website
- A class called Topic, which inherits from Model
    —a parent class included in Django that defines a model’s basic functionality
    - We add two attributes to the Topic class: text and date_added
    - We tell Django which attribute to use by default when it displays information about a topic. Django calls a __str__() method to display a simple representation of a mode

Activating models
----
- To use our models, we have to tell Django to include our app in the overall project
- Add our app to this list by modifying INSTALLED_APP
- Grouping apps together in a project helps to keep track of them as the
project grows to include more apps
- Django to modify the database so it can store information related to the model Topic
- command line: python manage.py makemigrations app_name
- The command makemigrations tells Django to figure out how to modify
the database so it can store the data associated with any new models we’ve
defined
- apply this migration and have Django modify the database
- Whenever we want to modify the data that Learning Log manages, we’ll follow these three steps: modify models.py, call makemigrations on learning_logs, and tell Django to migrate the project

Admin site
----
- We’ll set up the admin site and use it to add some topics through the
Topic model.
The most restrictive privilege settings allow a user to only read public information on the site. Registered users typically have the privilege of reading their own private data and some selected information available only to
members. To effectively administer a web application, the site owner usuall needs access to all information stored on the site. A good administrator is careful with their users’ sensitive information, because users put a lot of trust into the apps they access
- creating a superuser
python manage.py createsuperuser

Registering a Model with the Admin Site
----
- Django includes some models in the admin site automatically, such as User
and Group, but the models we create need to be added manually
- register Topic with the admin site
- The code admin.site.register() tells Django to manage our model
through the admin site
- Then you can add topics e.g. Daily blog and Weekly review

Entry model
----
- E.g. For a user to record what they’ve been learning about chess and rock climbing, we need to define a model for the kinds of entries users can make in their learning logs. Each entry needs to be associated with a particular
topic. This relationship is called a many-to-one relationship, meaning many entries can be associated with one topic
- The Entry class inherits from Django’s base Model class, ju
- The first attribute, topic, is a ForeignKey instance
  - A foreign key is a database term; it’s a reference to another record in the database
  - This is the code that connects each entry to a specific topic.

Migrating the entry model
----
- Because we’ve added a new model, we need to migrate the database again.
command: python manage.py makemigrations app name
command:python manage.py migrate
Registering Entry with the Admin Site
add to admin.py

Making pages
----
- making pages consists of three stages:
  - defining URLs
  - writing views
  - writing templates
- Define a URL pattern - A URL pattern describes the way the URL is laid out. 
- It also tells Django what to look for when matching a browser request with a site URL so it knows which page to
return.
- URL then maps to a particular view
- the view function retrieves and processes the data needed for that page
- The view function often renders the page using a template, which contains the overall structure of the page.
- We’ll define the URL for the home page, write its view function, and create a simple template

Maping a URL
----
Users request pages by entering URLs into a browser and clicking links, so we’ll need to decide what URLs are needed.
The home page URL is first:it’s the base URL people use to access the project. At the moment the base URL, http://localhost:8000/, returns the default Django site that lets us know the project was set up correctly. We’ll change this by mapping the base URL to blog page.

View
----
- a view takes in informatio from a request, prepares the data needed to generate a page, and then sends the data back to the browser, often by using a template that defines what the page will look like
- When a URL request matches the pattern we just defined, Django looks for a function called index() in the views.py file. Django then passes the request object to this view function. 
- The render() function here passes two arguments—the original request object and a template it can use to build the page

Template
----
The template defines what the page should look like, and Django fills in the relevant data each time the page is requested. A template allows you to access any data provided by the view. Because our view for the home page provided no data, this template is fairly simple. 
- add index file in subfolder
- Now when you request the project’s base URL, http://localhost:8000/, you should see the page we just built instead of the default Django page. Django
will take the requested URL, and that URL will match the pattern ''; then
Django will call the function views.index(), which will render the page using the template contained in index.html.
- Although it might seem like a complicated process for creating one page, this separation between URLs, views, and templates works quite well. It allows you to think about each aspect of a project separately. In larger projects, it allows individuals working on the project to focus on the areas
in which they’re strongest. For example, a database specialist can focus on the models, a programmer can focus on the view code, and a web designer can focus on the templates


Additional pages
----
- a page that lists all topics and a page that shows all the entries
- We’ll create a base template that all templates in the project can inherit from.
  - When building a website, some elements will always need to be repeated on each page. Rather than writing these elements directly into each page, you can write a base template containing the repeated elements and then have each page inherit from the base
  - a template tag, which is indicated by braces and percent signs {% %}. A template tag generates information to be displayed on a page
  - In a simple HTML page, a link is surrounded by the anchor tag <a>
  - Having the template tag generate the URL for us makes it much easier to keep our links up to date. We only need to change the URL pattern in urls.py, and Django will automatically insert the updated URL the next time
  the page is requested
  - Every page in our project will inherit from base.html,
  so from now on, every page will have a link back to the home page
  - we insert a pair of block tags - placeholder the child template will define the content
  - update index html, this line pulls in everything contained in the base.html template and allows index.html to define what goes in the space reserved by the content block
  - benefit of template inheritance: in a child template, we only need to include content that’s unique to that page. This not only simplifies each template, but also makes it much easier to modify the site. To modify an element common to many pages, you only need to modify the parent template. Your changes are then carried over to every
  page that inherits from that template 
- For each page, we’ll specify a URL pattern, write a view function, and write a template
  - We’ll use the word topics, so the URL http:/localhost:8000/topics/ will return this page
  - We’ve simply added topics/ into the string argument used for the home page URL
  - When Django examines a requested URL, this pattern will match any URL that has the base URL followed by topics
  - Any request with a URL that matches this pattern will then be passed to the function topics() in views.py
  - The topics() function needs to retrieve some data from the database and send it to the template
  - we query the database by asking for the Topic objects,
sorted by the date_added attribute
  - A context is a dictionary in which the keys are names we’ll use in the template to access the data, and the values are the data we need to send to the template
  - When building a page that uses data, we pass the context variable to render() as well as the request object and the path to the template

Topics template
---
- The template for the topics page receives the context dictionary, so the template can use the data that topics() provides
- We use the {% extends %} tag to inherit from base.html
- To print a variable in a template, wrap the variable name in double braces. The braces won’t appear on the page; they just indicate to Django that we’re using a template variable. So the code {{ topic }} at will be replaced by the value of topic on each pass through the loop
- modify the base template to include a link to the topics page
- We add a dash after the link to the home page u, and then add a link to the topics page using the {% url %} template tag again v. This linetells Django to generate a link matching the URL pattern with the name 'topics' in learning_logs/urls.py.

Individual topic pages
----
- We’ll again define a new URL pattern, write a view, and create a template
- We’ll also modify the topics page so each item in the bulleted list links to its corresponding topic page.
- Let’s examine the string 'topics/<int:topic_id>/' in this RL pattern. The first part of the string tells Django to look for URLs that have the wordtopics after the base URL. The second part of the string, /<int:topic_id>/,matches an integer between two forward slashes and stores the integer value in an argument called topic_i
- When Django finds a URL that matches this pattern, it calls the view function topic() with the value stored in topic_id as an argument. We’ll use the value of topic_id to get the correct topic inside the function.
- We store the topic and entries in the context dictionary x and send context to the template topic.html
- The template needs to display the name of the topic and the entries. We also need to inform the user if no entries have been made yet for this topic
- we need to modify the topics template so each topic links to the appropriate page
- We use the URL template tag to generate the proper link, based on the URL pattern in learning_logs with the name 'topic'. This URL pattern requires a topic_id argument, so we add the attribute topic.id to the URL template tag. Now each topic in the list of topics is a link to a topic page,such as http://localhost:8000/topics/1/. 
----

you learned how to build simple web applications using the
Django framework. You wrote a brief project specification, installed Django to a virtual environment, set up a project, and checked that the project was set up correctly. You set up an app and defined models to represent the data for your app. You learned about databases and how Django helps you migrate your database after you make a change to your models. You created a superuser for the admin site, and you used the admin site to enter some initial data. You also explored the Django shell, which allows you to work with your project’s data in a terminal session. You learned to define URLs, create view
functions, and write templates to make pages for your site. You also used template inheritance to simplify the structure of individual templates and make it easier to modify the site as the project evolves


# User accounts
You’ll build forms so users can add their own topics and entries, and edit existing entries. You’ll also learn how Django guards against common attacks to form-based pages so you
don’t have to spend much time thinking about securing your apps. You’ll also implement a user authentication system. You’ll build a registration page for users to create accounts, and then restrict access to certain
pages to logged-in users only. 

## Allow users to enter data
- add pages that allow users to enter their own data
- users can add new topic, add entry and edit previous entries

### adding new topic
----
Adding a form-based page works in much the same way as the pages we’ve already built: we define a URL, write a view function, and write a template. Add forms.py.

forms.py
----
-  simplest way to build a form in Django is to use a ModelForm, which uses the information from the models to automatically build a form
-  a class called TopicForm, which inherits from forms.ModelForm
-  a ModelForm consists of a nested Meta class telling Django which model to base the form on and which fields to include in the form
-  Django not to generate a label for the text
field
- new topic URL - add url pattern.URL pattern sends requests to the view function new_topic().
- The new_topic() function needs to handle two different situations:
  - initial requests for the new_topic page (in which case it should show a blank form) and the processing of any data submitted in the form. 
  - After data from a submitted form is processed, it needs to redirect the user back to the topics page.
  - GET requests - for pages that only read data from the server
  - POST requests - when the user needs to submit information through a form. 
  - The new_topic() function takes in the request object as a parameter. When the user initially requests this page, their browser will send a GET request. Once the user has filled out and submitted the form, their browser will submit a POST request
  - Depending on the request, we’ll know whether the user is requesting a blank form (a GET request) or asking us to process a completed form (a POST request)
new topic html
----
- we define an HTML form. The action argument tells the browser where to send the data submitted in the form; in this case, we send it back to the view function new_topic()
- The method argument tells the browser to submit the data as a POST request
- Django uses the template tag {% csrf_token %} v to prevent attackers from using the form to gain unauthorized access to the server (this kind of attack is called a cross-site request forgery)
- we display the form; here you see how simple Django can make certain tasks, such as displaying a form. We only need to include the template variable {{ form.as_p }} for Django to create all the fields necessary to display the form automatically. The as_p modifier tells Django to render all the form elements in paragraph format, as a simple way to display the form neatly

Adding new entries
----
- Now that the user can add a new topic, they’ll want to add new entries too
- define a URL, view function, template and link to page
- forms.py
  - we include the widgets attribute
  - A widget is an HTML form element, such as a single-line text box,multi-line text area, or drop-down list.
- new entry urls
  - This URL pattern matches any URL with the form http://localhost:
8000/new_entry/id/, where id is a number matching the topic ID

editting entries
----
- The URL for the page needs to pass the ID of the entry to be edited
- The URL pattern sends requests that match this format to the view function edit_entry()
- view function 
  - when it received get request - returns a form for editing the entry
  - when it received post request - it saves modified text into database
- We then redirect to the topic page, where the user should see the updated version of the entry they edited


setting up user accounts
----
- user registration and authorisation system
- create a new app to contain the funcitonality and related to working with users.
user app
---
- create new app: startapp
- add app to INSTALLED_APPS in settings.py
- include URLs from users 
- we add line to include the file urls.py from users. This will match URL that starts with users.
Login page
----
- use default login view that django has
- import path function, then import include function so include some default authentical urls.
login template
----
- we want the login view to process the form so we set the acion argument as the URL of the login page
- the login view sends a form to the template, and it's up to us to display the form and add a submit button
- we include a hidden form element 'next' the vlaue argument tells django where to redirect the user after they've lgoged in successfully.

loggin out
---
- we'll put a link to log out in base.html. 

registration page
----
- registration URL - add URL patterns for registration page.
- the pattern for the registration page matches the URL - sends requests ot register function
- add register function to views -


Allow users to enter their own data
----
We’ll modify the Topic model so every topic belongs to a specific user.
This will also take care of entries, because every entry belongs to a specific
topic. We’ll start by restricting access to certain pages

Django makes it easy to restrict access to certain pages to logged-in users
through the @login_required decorator.

A decorator is a directive placed just
before a function definition that Python applies to the function before it
runs, to alter how the function code behaves

 As a result, Python knows to run the code in login_required()
before the code in topics().

Next, we need to connect the data to the user who submitted it. We need to
connect only the data highest in the hierarchy to a user, and the lower-level
data will follow. 

Identifying Existing Users
python manage.py shell

Migrating the Database
Now that we know the IDs, we can migrate the database. When we do this,
Python will ask us to connect the Topic model to a particular owner temporarily or to add a default to our models.py file to tell it what to do

python manage.py makemigrations learning_logs

To associate all existing topics with the original admin user, ll_admin, I
entered the user ID of 1 at z. You can use the ID of any user you’ve created;
it doesn’t have to be a superuser. Django then migrates the database using
this value and generates the migration file 0003_topic_owner.py, which adds
the field owner to the Topic model.

When a user is logged in, the request object has a request.user attribute set that stores information about the user. The query Topic.objects
.filter(owner=request.user) tells Django to retrieve only the Topic objects
from the database whose owner attribute matches the current use

Protecting a User’s Topics
---
We haven’t restricted access to the topic pages yet, so any registered user
could try a bunch of URLs, like http://localhost:8000/topics/1/, and retrieve
topic pages that happen to match
Associating New Topics with the Current User
we have access to the current user through the request object

When we first call form.save(), we pass the commit=False argument because
we need to modify the new topic before saving it to the database u. We then
set the new topic’s owner attribute to the current user v. Finally, we call save()
on the topic instance just defined w. Now the topic has all the required data
and will save successfully.

In this chapter, you learned to use forms to allow users to add new topics and entries, and edit existing entries. You then learned how to implement user accounts. You allowed existing users to log in and out, and used
Django’s default UserCreationForm to let people create new accounts.
After building a simple user authentication and registration system, you
restricted access to logged-in users for certain pages using the @login_required
decorator. You then attributed data to specific users through a foreign key
relationship. You also learned to migrate the database when the migration
requires you to specify some default data.
Finally, you learned how to make sure a user can only see data that
belongs to them by modifying the view functions. You retrieved appropriate data using the filter() method and compared the owner of the
requested data to the currently logged in user.
It might not always be immediately obvious what data you should make
available and what data you should protect, but this skill will come with practice. The decisions we’ve made in this chapter to secure our users’ data also
illustrate why working with others is a good idea when building a project:
having someone else look over your project makes it more likely that you’ll
spot vulnerable areas.
You now have a fully functioning project running on your local machine.

Styling
----
For the styling we’ll use the Bootstrap library- django-bootstrap4 app
deploy using Heroku - lets you push your project to one of its servers

Bootstrap is a large collection of styling tools. It also has a number of templates you can apply to your project to create an overall style

we load the collection of template tags available in djangobootstrap4.
Next, we declare this file as an HTML document.

An HTML file is divided into two main parts, the head and the body.
header
----
- The head of an HTML file doesn’t contain any content: it just tells the browser what it needs to know to display the page correctly
- we include a title element for the page, which will display in the browser’s title bar whenever Learning Log is open
- we use one of django-bootstrap4’s custom template tags, which tells Django to include all the Bootstrap style files
- The tag that follows enables all the interactive behavior you might use on a page, such as collapsible navigation bars.

navigation bar
---
The body of an HTML file contains the content users will see on a page.
A <nav> element that indicates the page’s navigation links section.
A selector determines which elements on a page a certain style rule applies to

we set the project’s name to appear at the far left of the navigation
bar and make it a link to the home page; it will appear on every page in the
project. The navbar-brand selector styles this link so it stands out from the
rest of the links and is a way of branding the site.

the template defines a button that appears if the browser window
is too narrow to display the whole navigation bar horizontally. 

we open a new section of the navigation bar. The term div is short
for division; you build a web page by dividing it into sections and defining
style and behavior rules that apply to that section

Any styling or behavior rules that are defined in an opening div tag affect everything you see until the next closing div tag, which is written as </div>. This is the beginning of the part of the navigation bar that will be collapsed on narrow screens and windows.

we define a new set of links. Bootstrap defines navigation elements as items in an unordered list with style rules that make it look nothing like a list. Every link or element you need on the bar can be included as
an item in one of these lists. Here, the only item in the list is our link to the
Topics page


we begin a new set of links by using another opening <ul> tag.
You can have as many groups of links as you need on a page. This will be
the group of links related to login and registration that appears on the
right side of the navigation bar. The selector ml-auto is short for margin-leftautomatic: this selector examines the other elements in the navigation bar
and works out a left margin that pushes this group of links to the right side
of the screen

defining main part of page
----
The main element is used for the most significant part of the body of a page.
bootstrap selector container - group elements - place 2 divs
- The first div element v contains a page_header block. We’ll use this block
to title most pages
- To make this section stand out from the rest of the page,
we place some padding below the header. Padding refers to space between
an element’s content and its border.


Deploying the app!
----

Heroku - a webbased platform that allows you to manage the deployment of web applications. 
Django is a dynamic web framework that requires server-side processing

make an heroku account

install heroku CLI: brew tap heroku/brew && brew install heroku
Modifying settings.py for Heroku

The psycopg2 package is required to manage the database that Heroku
uses. The django-heroku package handles almost the entire configuration our
app needs to run properly on Heroku servers. This includes managing the
database and storing static files in a place where they can be served properly.
Static files contain style rules and JavaScript files. The gunicorn package provides a server capable of serving apps in a live environment.

Making a Procfile to Start Processe

A Procfile tells Heroku which processes to start to properly serve the project.
Save this one-line file as Procfile, with an uppercase P and no file extension,
in the same directory as manage.py

web: gunicorn blog_website.wsgi --log-file -

This line tells Heroku to use gunicorn as a server and to use the settings in learning_log/wsgi.py to launch the app.

We don’t track changes to the local database, because it’s
a bad habit: if you’re ever using SQLite on a server, you might accidentally
overwrite the live database with your local test database when you push the
project to the server. The asterisk in *.sqlite3 tells Git to ignore any file that
ends with the extension .sqlite3

Pushing to Heroku - requires payment.... 
so went to PythonAnywhere. Followed instructions from:
https://help.pythonanywhere.com/pages/DeployExistingDjangoProject/
