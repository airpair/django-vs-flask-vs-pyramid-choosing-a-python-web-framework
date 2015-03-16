<div class="workshop-cta">
        <b>FREE WORKSHOP: Django vs Flask vs Pyramid</b>Ryan Brown is giving a free virtual workshop to help guide you on picking the right Python framework and answer your questions.
        <a href="http://docs.google.com/forms/d/1h1gunjoc3FqcPGRWsQFZFvhy9iPluh3paK4tf9H7Edc/viewform?embedded=true" class="trackPostCTA">>> Sign up to secure a spot</a>
      </div>
</br>
<div style="max-width:640px;background-color:#f5f5f5;padding:10px;">
<b>TL;DR: </b>
Pyramid, Django, and Flask are all excellent frameworks, and choosing just one
for a project is hard. We'll see working apps with identical functionality in
all three frameworks to make comparing the three easier. Skip to *Frameworks
in Action* for the <a href="https://github.com/ryansb/wut4lunch_demos">code</a>.
</div>

## 1 Introduction

The world of Python web frameworks is full of choices. Django, Flask, Pyramid,
Tornado, Bottle, Diesel, Pecan, Falcon, and many more are competing for
developer mindshare. As a developer you want to cut the legions of options down
to the one that will help you finish your project and get on to the Next Big
Thing (tm). We'll focus on Flask, Pyramid, and Django. Their ideal cases span
from micro-project to enterprise-size web service.

To help make the choice between the three easier (or at least more informed),
we'll build the same application in each framework and compare the code,
highlighting the strengths and weaknesses of each approach. If you just want
the code, skip straight to *Frameworks in Action* or view the code on
[Github][repolink].

Flask is a "microframework" primarily aimed at small applications with simpler
requirements. Pyramid and Django are both aimed at larger applications, but
take different approaches to extensibility and flexibility. Pyramid targets
flexibility and lets the developer use the right tools for their project. This
means the developer can choose the database, URL structure, templating style,
and more. Django aims to include all the batteries a web application will need
so developers need only open the box and start working, pulling in Django's
many modules as they go.

Django includes an [ORM][ormwat] out of the box, while Pyramid and Flask leave
it to the developer to choose how (or if) they want their data stored. The
most popular ORM for non-Django web applications is [SQLAlchemy][sqla] by far,
but there are plenty of other options from [DynamoDB][dynamo] and
[MongoDB][mongo] to simple local persistence like [LevelDB][leveldb] or plain
[SQLite][sqlite]. Pyramid is designed to use any persistence layer, even
yet-to-be-invented ones.


## 2 About the Frameworks

Django's "batteries included" approach makes it easy for developers who know
Python already to dive in to web applications quickly without needing to make
a lot of decisions about their application's infrastructure ahead of time.
Django has for templating, forms, routing, authentication, basic database
administration, and more built in. In contrast, Pyramid includes routing and
authentication, but templating and database administration require external
libraries.

The extra work up front to choose components for Flask and Pyramid apps yields
more flexibility for developers whose use case doesn't fit a standard ORM, or
who need to interoperate with different workflows or templating systems.

Flask, the youngest of the three frameworks, started in mid-2010. The Pyramid
framework began life in the [Pylons project][pylons] and got the name Pyramid
in late 2010, though the first release was in 2005. Django had its first
release in 2006, shortly after the Pylons (eventually Pyramid) project began.
Pyramid and Django are extremely mature frameworks, and have accumulated
plugins and extensions to meet an incredibly large range of needs.

Though Flask has a shorter history, it has been able to learn from frameworks
that have come before and has set its sights firmly on small projects. It is
clearly used most often in smaller projects with just one or two functions.
One such project is [httpbin][httpbin], a simple (but extremely powerful)
helper for debugging and testing HTTP libraries.

## 3 Community

The prize for most active community goes to Django with 80,000 StackOverflow
questions and a healthy set of blogs from developers and power users. The
Flask and Pyramid communities aren't as large, but their communities are quite
active on their mailing lists and on IRC. With only 5,000 StackOverflow
questions tagged, Flask is 15x smaller than Django. On Github, they have a
nearly identical number of stars with 11,300 for Django, and 10,900 for Flask.

All three frameworks are available under BSD-derived permissive licenses. Both
[Flask's][flaskbsd] and [Django's][bsd] licenses are 3-clause BSD, while
Pyramid's [Repoze Public License RPL][rpl] is a derivative of the 4-clause BSD
license.

## 4 Bootstrapping

Django and Pyramid both come with bootstrapping tools built in. Flask includes
nothing of the sort because Flask's target audience isn't *trying* to build
large [MVC][mvc] applications.

### 4.1 Flask
Flask's `Hello World` app has to be the simplest out there, clocking in at a
puny 7 lines of code in a single Python file.

<!--code lang=python linenums=true-->

    # from http://flask.pocoo.org/ tutorial
    from flask import Flask
    app = Flask(__name__)

    @app.route("/") # take note of this decorator syntax, it's a common pattern
    def hello():
        return "Hello World!"

    if __name__ == "__main__":
        app.run()

This is why there aren't bootstrapping tools for Flask: there isn't a demand
for them. From the above Hello World featured on Flask's homepage, a developer
with no experience building Python web applications can get hacking
immediately.

For projects that need more separation between components, Flask has
[blueprints][blueprints]. For example, you could structure your Flask app with
all user-related functions in `users.py` and your sales-related functions in
`ecommerce.py`, then import them and add them to your app in `site.py`. We
won't go over this functionality, as it's beyond the needs of our demo app.

### 4.2 Pyramid

Pyramid's bootstrapping tool is called `pcreate` which is part of Pyramid.
Previously the [Paste][paste] suite of tools provided bootstrapping for
but has since been replaced with a Pyramid-specific toolchain.

<!--code lang=bash linenums=true-->

    $ pcreate -s starter hello_pyramid # Just make a Pyramid project

Pyramid is intended for bigger and more complex applications than Flask.
Because of this, its bootstrapping tool creates a bigger skeleton project. It
also throws in basic configuration files, an example template, and the files
to package your application for uploading to the [Python Package Index][pypi].

<!--code lang=markup linenums=true-->

    hello_pyramid
    ├── CHANGES.txt
    ├── development.ini
    ├── MANIFEST.in
    ├── production.ini
    ├── hello_pyramid
    │   ├── __init__.py
    │   ├── static
    │   │   ├── pyramid-16x16.png
    │   │   ├── pyramid.png
    │   │   ├── theme.css
    │   │   └── theme.min.css
    │   ├── templates
    │   │   └── mytemplate.pt
    │   ├── tests.py
    │   └── views.py
    ├── README.txt
    └── setup.py

As in the rest of the framework, Pyramid's bootstrapper is incredibly
flexible. It's not limited to one default application; `pcreate` can use any
number of project templates. Included in `pcreate` there is the "starter"
template we used above, along with SQLAlchemy- and [ZODB][zodb]-backed
scaffold projects. On PyPi it's possible to find ready-made scaffolds for
[Google App Engine][appengine], [jQuery Mobile][jqm], [Jinja2
templating][j2a], [modern frontend frameworks][modern], and many more.

### 4.3 Django

Django also has its own bootstrapping tool built in as a part of `django-admin`.

<!--code lang=python linenums=true-->

    django-admin startproject hello_django
    django-admin startapp howdy # make an application within our project

We can already see one of the ways Django differs from Pyramid. Django
separates a project into individual applications, where Pyramid and Flask
expect a project to be a single "application" with several views or models.
It's possible to replicate the project/app distinction in Flask and Pyramid,
but the notion does not exist by default.

<!--code lang=markup linenums=true-->

    hello_django
    ├── hello_django
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── howdy
    │   ├── admin.py
    │   ├── __init__.py
    │   ├── migrations
    │   │   └── __init__.py
    │   ├── models.py
    │   ├── tests.py
    │   └── views.py
    └── manage.py

By default Django only includes empty model and template files, so a new user
sees a bit less example code to start out. It also (unfortunately) leaves the
choice of how to distribute their application to the developer.

The downside of the bootstrap tool not guiding users to package their apps is
that novice users won't. If a developer hasn't packaged an app before, they'll
find themselves rudely surprised upon their first deploy. Projects with a
large community like [django-oscar][oscar] are packaged and available on PyPi,
but smaller projects on Github often to lack uniform packaging.


## 5 Templating

Just having a Python application that can respond to HTTP requests is a great
start, but it's a good bet that most of your users won't be interested in
using `curl` to interact with your web app. Fortunately, all three contenders
provide an easy way to fill in HTML with custom info, and let folks enjoy your
swanky [Bootstrap][bootstrap] frontend.

Templating lets you inject dynamic information directly into your page without
using making AJAX requests. This is nice from a user experience perspective
since you only need to make one round-trip to get the full page and all its
dynamic data. This is especially important on mobile sites where round trips
can take multiple seconds.

All the templating options we'll see rely on a "context" that provides the
dynamic information for the template to render into HTML. The simplest use
case for a template would be to populate a logged-in user's name to greet them
properly. It would be possible to use AJAX to get this sort of dynamic
information, but requiring a whole call just to fill in a user's name would be
a bit excessive when templates are this easy.

### 5.1 Django

Our example use case is about as easy as it gets, assuming that we have a
`user` object that has a `fullname` property containing a user's name. In
Python we'd pass the current user to the template like so:

<!--code lang=python linenums=true-->

    def a_view(request):
        # get the logged in user
        # ... do more things
        return render_to_response(
            "view.html",
            {"user": cur_user}
        )

Populating the template context is as simple as passing a dictionary of the
Python objects and data structures the template should use. Now we need to
render their name to the page, just in case they forget who they are.

<!--code lang=python linenums=true-->

    <!-- view.html -->
    <div class="top-bar row">
      <div class="col-md-10">
      <!-- more top bar things go here -->
      </div>
      {% if user %}
      <div class="col-md-2 whoami">
        You are logged in as {{ user.fullname }}
      </div>
      {% endif %}
    </div>

First, you'll notice the `{% if user %}` construct. In Django templates `{%`
is used for control statements like loops and conditionals. The `if user`
statement is there to guard against cases where there is not a user. Anonymous
users shouldn't see "you are logged in as" in the site header.

Inside the if block, you can see that including the name is as simple as
wrapping the property we want to insert in `{{ }}`. The `{{` is used to insert
actual values into the template, such as `{{ user.fullname }}`.

Another common use for templates is displaying groups of things, like the
inventory page for an ecommerce site.

<!--code lang=python linenums=true-->

    def browse_shop(request):
        # get items
        return render_to_response(
            "browse.html",
            {"inventory": all_items}
        )

In the template we can use the same `{%` to loop over all the items in the
inventory, and to fill in the URL to their individual page.

<!--code lang=python linenums=true-->

    {% for widget in inventory %}
        <li><a href="/widget/{{ widget.slug }}/">{{ widget.displayname }}</a></li>
    {% endfor %}

To do most common templating tasks, Django can accomplish the goal with just a
few constructs, making it easy to get started.

### 5.2 Flask

Flask uses the Django-inspired [Jinja2][jinja] templating language by default
but can be configured to use another language. A programmer in a hurry
couldn't be blamed for mixing up Django and Jinja templates. In fact, both the
Django examples above work in Jinja2. Instead of going over the same examples,
let's look at the places that Jinja2 is more expressive than Django
templating.

Both Jinja and Django templates provide a feature called filtering, where a
list can be passed through a function before being displayed. A blog that
features post categories might make use of filters to display a post's
categories in a comma-separated list.

<!--code lang=python linenums=true-->

    <!-- Django -->
    <div class="categories">Categories: {{ post.categories|join:", " }}</div>

    <!-- now in Jinja -->
    <div class="categories">Categories: {{ post.categories|join(", ") }}</div>

In Jinja's templating language it's possible to pass any number of arguments
to a filter because Jinja treats it like a call to a Python function, with
parenthesis surrounding the arguments. Django uses a colon as a separator
between the filter name and the filter argument, which limits the number of
arguments to just one.

Jinja and Django `for` loops are also similar. Let's see where they differ. In
Jinja2, the for-else-endfor construct lets you iterate over a list, but also
handle the case where there are no items.

 <!--code lang=python linenums=true-->

    {% for item in inventory %}
    <div class="display-item">{{ item.render() }}</div>
    {% else %}
    <div class="display-warn">
    <h3>No items found</h3>
    <p>Try another search, maybe?</p>
    </div>
    {% endfor %}

The Django version of this functionality is identical, but uses
for-empty-endfor instead of for-else-endfor.

 <!--code lang=python linenums=true-->

    {% for item in inventory %}
    <div class="display-item">{{ item.render }}</div>
    {% empty %}
    <div class="display-warn">
    <h3>No items found</h3>
    <p>Try another search, maybe?</p>
    </div>
    {% endfor %}

Other than the syntactic differences above, Jinja2 provides more control over
its execution environment and advanced features. For example, it's possible to
disable potentially dangerous features to safely execute untrusted templates,
or to compile templates ahead of time to ensure their validity.

### 5.3 Pyramid

Like Flask, Pyramid supports many templating languages (including Jinja2 and
Mako) but ships with one by default. Pyramid uses [Chameleon][chameleon], an
implementation of [ZPT][zpt] (the Zope Page Template) templating language.
Let's look back at our first example, adding a user's name to the top bar of
our site. The Python code looks much the same except that we don't need to
explicitly call a `render_template` function.

<!--code lang=python linenums=true-->

    @view_config(renderer='templates/home.pt')
    def my_view(request):
        # do stuff...
        return {'user': user}

But our template looks pretty different. ZPT is an XML-based templating
standard, so we use XSLT-like statements to manipulate data.

<!--code lang=python linenums=true-->

    <div class="top-bar row">
      <div class="col-md-10">
      <!-- more top bar things go here -->
      </div>
      <div tal:condition="user"
           tal:content="string:You are logged in as ${user.fullname}"
           class="col-md-2 whoami">
      </div>
    </div>

Chameleon actually has three different namespaces for template actions. TAL
(template attribute language) provides basics like conditionals, basic string
formatting, and filling in tag contents. The above example only made use of
TAL to complete its work. For more advanced tasks, TALES and METAL are
required. TALES (Template Attribute Language Expression Syntax) provides
expressions like advanced string formatting, evaluation of Python expressions,
and importing expressions and templates.

METAL (Macro Expansion Template Attribute Language) is the most powerful (and
complex) part of Chameleon templating. Macros are extensible, and can be
defined as having slots that are filled when the macro is invoked.

## 6 Frameworks in Action

For each framework let's take a look at making an app called wut4lunch, a
social network to tell the whole internet what you ate for lunch. Free startup
idea right there, totally a gamechanger. The application will be a simple
interface that allows users to post what they had for lunch and to see a list
of what other users ate. The home page will look like this when we're done.

<img src="https://raw.githubusercontent.com/ryansb/wut4lunch_demos/master/static/screenshot.png">


### 6.1 Demo App with Flask

The shortest implementation clocks in at 34 lines of Python and a single 22
line Jinja template. First we have some housekeeping tasks to do, like
initializing our app and pulling in our ORM.

<!--code lang=python linenums=true-->

    from flask import Flask

    # For this example we'll use SQLAlchemy, a popular ORM that supports a
    # variety of backends including SQLite, MySQL, and PostgreSQL
    from flask.ext.sqlalchemy import SQLAlchemy

    app = Flask(__name__)
    # We'll just use SQLite here so we don't need an external database
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'

    db = SQLAlchemy(app)

Now let's take a look at our model, which will remain almost the same for our
other two examples as well.

<!--code lang=python linenums=true-->

    class Lunch(db.Model):
        """A single lunch"""
        id = db.Column(db.Integer, primary_key=True)
        submitter = db.Column(db.String(63))
        food = db.Column(db.String(255))

Wow, that's pretty easy. The hardest part was finding the right [SQLAlchemy
data types][sqladata] and picking a length for our `String` fields in the
database. Using our models is also extremely simple, thanks to the [SQLAlchemy
query syntax][sqlaquery] we'll see later.

Building our submission form is just as easy. After importing
[Flask-WTForms][flaskwtf] and the correct field types, you can see the form
looks quite a bit like our model. The main difference is the new submit button
and prompts for the food and submitter name fields.

The `SECRET_KEY` field in the app config is used by WTForms to create
[CSRF][csrf] tokens. It is also used by [itsdangerous][itsdangerous] (included
in Flask) to sign cookies and other data.

<!--code lang=python linenums=true-->

    from flask.ext.wtf import Form
    from wtforms.fields import StringField, SubmitField

    app.config['SECRET_KEY'] = 'please, tell nobody'

    class LunchForm(Form):
        submitter = StringField(u'Hi, my name is')
        food = StringField(u'and I ate')
        # submit button will read "share my lunch!"
        submit = SubmitField(u'share my lunch!')

Making the form show up in the browser means the template has to have it.
We'll pass that in below.

<!--code lang=python linenums=true-->

    from flask import render_template

    @app.route("/")
    def root():
        lunches = Lunch.query.all()
        form = LunchForm()
        return render_template('index.html', form=form, lunches=lunches)

Alright, what just happened? We got a list of all the lunches that have
already been posted with `Lunch.query.all()`, and instantiated a form to let
the user post their own gastronomic adventure. For simplicity, the variables
are passed into the template with the same name, but this isn't required.

<!--code lang=markup linenums=true-->

    <html>
    <title>Wut 4 Lunch</title>
    <b>What are people eating?</b>

    <p>Wut4Lunch is the latest social network where you can tell all your friends
    about your noontime repast!</p>

Here's the real meat of the template, where we loop through all the lunches
that have been eaten and display them in a `<ul>`. This almost identical to
the looping example we saw earlier.

<!--code lang=python linenums=true-->

    <ul>
    {% for lunch in lunches %}
    <li><strong>{{ lunch.submitter|safe }}</strong> just ate <strong>{{ lunch.food|safe }}</strong>
    {% else %}
    <li><em>Nobody has eaten lunch, you must all be starving!</em></li>
    {% endfor %}
    </ul>

    <b>What are YOU eating?</b>

    <form method="POST" action="/new">
        {{ form.hidden_tag() }}
        {{ form.submitter.label }} {{ form.submitter(size=40) }}
        <br/>
        {{ form.food.label }} {{ form.food(size=50) }}
        <br/>
        {{ form.submit }}
    </form>
    </html>

The `<form>` section of the template just renders the form labels and inputs
from the WTForm object we passed into the template in the `root()` view. When
the form is submitted, it'll send a POST request to the `/new` endpoint which
will be processed by the function below.

<!--code lang=python linenums=true-->

    from flask import url_for, redirect

    @app.route(u'/new', methods=[u'POST'])
    def newlunch():
        form = LunchForm()
        if form.validate_on_submit():
            lunch = Lunch()
            form.populate_obj(lunch)
            db.session.add(lunch)
            db.session.commit()
        return redirect(url_for('root'))

After validating the form data, we put the contents into one of our Model
objects and commit it to the database. Once we've stored the lunch in the
database it'll show up in the list of lunches people have eaten.

<!--code lang=python linenums=true-->

    if __name__ == "__main__":
        db.create_all()  # make our sqlalchemy tables
        app.run()

Finally, we have to do a (very) little bit of work to actually run our app.
Using SQLAlchemy we create the table we use to store lunches, then start
running the route handlers we wrote.

### 6.2 Demo App with Django

The Django version of wut4lunch is similar to the Flask version, but is spread
across several files in the Django project. First, let's look at the most
similar portion: the database model. The only difference between this and the
SQLAlchemy version is the slightly different syntax for declaring a database
field that holds text.

<!--code lang=python linenums=true-->

    # from wut4lunch/models.py
    from django.db import models

    class Lunch(models.Model):
        submitter = models.CharField(max_length=63)
        food = models.CharField(max_length=255)

On to the form system. Unlike Flask, Django has a built-in form system that we
can use. It looks much like the WTForms module we used in Flask with different
syntax.

<!--code lang=python linenums=true-->

    from django import forms
    from django.http import HttpResponse
    from django.shortcuts import render, redirect

    from .models import Lunch

    # Create your views here.

    class LunchForm(forms.Form):
        """Form object. Looks a lot like the WTForms Flask example"""
        submitter = forms.CharField(label='Your name')
        food = forms.CharField(label='What did you eat?')

Now we just need to make an instance of `LunchForm` to pass in to our
template.

<!--code lang=python linenums=true-->

    lunch_form = LunchForm(auto_id=False)

    def index(request):
        lunches = Lunch.objects.all()
        return render(
            request,
            'wut4lunch/index.html',
            {
                'lunches': lunches,
                'form': lunch_form,
            }
        )

The `render` function is a Django shortcut that takes the request, the
template path, and a context `dict`. Similar to Flask's `render_template`, but
it also takes the incoming request.

<!--code lang=python linenums=true-->

    def newlunch(request):
        l = Lunch()
        l.submitter = request.POST['submitter']
        l.food = request.POST['food']
        l.save()
        return redirect('home')

Saving the form response to the database is different, instead of using a
global database session Django lets us call the model's `.save()` method and
handles session management transparently. Neat!

Django provides some nice features for us to manage the lunches that users
have submitted, so we can delete lunches that aren't appropriate for our site.
Flask and Pyramid don't provide this automatically, and not having to write
Yet Another Admin Page when making a Django app is certainly a feature.
Developer time isn't free! All we had to do to tell Django-admin about our
models is add two lines to `wut4lunch/admin.py`.

<!--code lang=python linenums=true-->

    from wut4lunch.models import Lunch
    admin.site.register(Lunch)

Bam. And now we can add and delete entries without doing any extra work.

Lastly, let's take a look at the differences in the homepage template.

<!--code lang=python linenums=true-->

    <ul>
    {% for lunch in lunches %}
    <li><strong>{{ lunch.submitter }}</strong> just ate <strong>{{ lunch.food }}</strong></li>
    {% empty %}
    <em>Nobody has eaten lunch, you must all be starving!</em>
    {% endfor %}
    </ul>

Django has a handy shortcut for referencing other views in your pages. The
`url` tag makes it possible for you to restructure the URLs your application
serves without breaking your views. This works because the `url` tag looks up
the URL of the view mentioned on the fly.

<!--code lang=python linenums=true-->

    <form action="{% url 'newlunch' %}" method="post">
      {% csrf_token %}
      {{ form.as_ul }}
      <input type="submit" value="I ate this!" />
    </form>

The form is rendered with different syntax, and we need to include a CSRF
token manually in the form body, but these differences are mostly cosmetic.

### 6.3 Demo App with Pyramid

Finally, let's take a look at the same program in Pyramid. The biggest
difference from Django and Flask here is the templating. Changing the Jinja2
template very slightly was enough to solve our problem in Django. Not so this
time, Pyramid's Chameleon template syntax is more reminiscent of [XSLT][xslt]
than anything else.

<!--code lang=python linenums=true-->

    <!-- pyramid_wut4lunch/templates/index.pt -->
    <div tal:condition="lunches">
      <ul>
        <div tal:repeat="lunch lunches" tal:omit-tag="">
          <li tal:content="string:${lunch.submitter} just ate ${lunch.food}"/>
        </div>
      </ul>
    </div>
    <div tal:condition="not:lunches">
      <em>Nobody has eaten lunch, you must all be starving!</em>
    </div>

Like in Django templates, a lack of the for-else-endfor construct makes the
logic slightly more verbose. In this case, we end up with if-for and
if-not-for blocks to provide the same functionality. Templates that use XHTML
tags may seem foreign after using Django- and AngularJS-style templates that
use `{{` or `{%` for control structures and conditionals.

One of the big upsides to the Chameleon templating style is that your editor
of choice will highlight the syntax correctly, since the templates are valid
XHTML. For Django and Flask templates your editor needs to have support for
those templating languages to highlight correctly.

<!--code lang=python linenums=true-->

    <b>What are YOU eating?</b>

    <form method="POST" action="/newlunch">
      Name: ${form.text("submitter", size=40)}
      <br/>
      What did you eat? ${form.text("food", size=40)}
      <br/>
      <input type="submit" value="I ate this!" />
    </form>
    </html>

The form rendering is slightly more verbose in Pyramid because the
`pyramid_simpleform` doesn't have an equivalent to Django forms' `form.as_ul`
function, which renders all the form fields automatically.

Now let's see what backs the application. First, we'll define the form we need
and render our homepage.

<!--code lang=python linenums=true-->

    # pyramid_wut4lunch/views.py
    class LunchSchema(Schema):
        submitter = validators.UnicodeString()
        food = validators.UnicodeString()

    @view_config(route_name='home',
                 renderer='templates/index.pt')
    def home(request):
        lunches = DBSession.query(Lunch).all()
        form = Form(request, schema=LunchSchema())
        return {'lunches': lunches, 'form': FormRenderer(form)}

The query syntax to retrieve all the lunches is familiar from Flask because
both demo applications use the popular [SQLAlchemy ORM][sqla] to provide
persistent storage. In Pyramid lets you return your template's context
dictionary directly instead of needing to call a special `render` function.
The `@view_config` decorator automatically passes the returned context to the
template to be rendered. Being able to skip calling the `render` method makes
functions written for Pyramid views easier to test, since the data they return
isn't obscured in a template renderer object.

<!--code lang=python linenums=true-->

    @view_config(route_name='newlunch',
                 renderer='templates/index.pt',
                 request_method='POST')
    def newlunch(request):
        l = Lunch(
            submitter=request.POST.get('submitter', 'nobody'),
            food=request.POST.get('food', 'nothing'),
        )

        with transaction.manager:
            DBSession.add(l)

        raise exc.HTTPSeeOther('/')

Form data is easy to retrieve from Pyramid's request object, which
automatically parsed the form POST data into a `dict` that we can access. To
prevent multiple concurrent requests from all accessing the database at the
same time, the ZopeTransactions module provides [context managers][ctxmanager]
for grouping database writes into logical transactions and prevent threads of
your application from stomping on each others' changes, which can be a problem
if your views share a global session and your app receives a lot of traffic.

## 7 Summary

Pyramid is the most flexible of the three. It can be used for small apps as
we've seen here, but it also powers big-name sites like Dropbox. Open Source
communities like [Fedora][fedora] choose it for applications like their
community [badges system][tahrir], which receives information about events from
many of the project's tools to award achievement-style badges to users. One of
the most common complaints about Pyramid is that it presents so many options it
can be intimidating to start a new project.

By far the most popular framework is Django, and the list of sites that use it
is impressive. Bitbucket, Pinterest, Instagram, and The Onion use Django for
all or part of their sites. For sites that have common requirements, Django
chooses very sane defaults and because of this it has become a popular choice
for mid- to large-sized web applications.

Flask is great for developers working on small projects that need a fast way
to make a simple, Python-powered web site. It powers loads of small one-off
tools, or simple web interfaces built over existing APIs. Backend projects
that need a simple web interface that is fast to develop and will require
little configuration often benefit from Flask on the frontend, like
[jitviewer][jitviewer] which provides a web interface for inspecting PyPy
just-in-time compiler logs.

All three frameworks came up with a solution to our small list of
requirements, and we've been able to see where they differ. Those differences
aren't just cosmetic, and they will change how you design your product and how
fast you ship new features and fixes. Since our example was small, we've seen
where Flask shines and how Django can feel clunky on a small scale. Pyramid's
flexibility didn't become a factor because our requirements stayed the same,
but in the real world new requirements are thrown in constantly.


### 7.1 Credits

Logos in the title image are from the
[Flask](http://flask.pocoo.org/community/logos/)
[Django](https://www.djangoproject.com/community/logos/) and
[Pyramid](http://www.pylonsproject.org/projects/pyramid/about) project web
sites.

This article owes many thanks to its reviewers, Remy DeCausemaker, Ross
Delinger, and Liam Middlebrook, for tolerating many early drafts.

In its current form this article incorporates comments and corrections from
Adam Chainz, bendwarn, Sergei Maertens, Tom Leo, and wichert. (alphabetical
order)

[jitviewer]: https://bitbucket.org/pypy/jitviewer
[tahrir]: https://badges.fedoraproject.org/
[fedora]: https://fedoraproject.org/
[ctxmanager]: https://docs.python.org/2/reference/datamodel.html#context-managers
[rpl]: http://repoze.org/license.html
[pypi]: https://pypi.python.org/
[zpt]: http://wiki.zope.org/ZPT/FrontPage
[paste]: http://pythonpaste.org/
[oscar]: https://github.com/tangentlabs/django-oscar
[csrf]: http://en.wikipedia.org/wiki/Cross-site_request_forgery
[sqlaquery]: http://docs.sqlalchemy.org/en/latest/orm/query.html
[bsd]: https://raw.githubusercontent.com/django/django/master/LICENSE
[blueprints]: http://flask.pocoo.org/docs/0.10/blueprints/
[xslt]: http://en.wikipedia.org/wiki/XSLT
[flaskbsd]: https://raw.githubusercontent.com/mitsuhiko/flask/master/LICENSE
[ormwat]: http://en.wikipedia.org/wiki/Object-relational_mapping
[sqla]: http://www.sqlalchemy.org/
[itsdangerous]: http://pythonhosted.org/itsdangerous/
[flaskwtf]: https://flask-wtf.readthedocs.org/en/latest/
[dynamo]: http://aws.amazon.com/dynamodb/
[mongo]: http://www.mongodb.org/
[leveldb]: https://github.com/google/leveldb
[sqlite]: http://www.sqlite.org/
[mvc]: http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
[pylons]: http://www.pylonsproject.org/about/history
[httpbin]: http://httpbin.org/
[zodb]: http://www.zodb.org/en/latest/
[appengine]: https://pypi.python.org/pypi/pyramid_appengine/
[jqm]: https://github.com/Pylons/pyramid_jqm
[modern]: https://pypi.python.org/pypi/pyramid_modern
[j2a]: https://pypi.python.org/pypi/jinja2-alchemy-starter
[sqladata]: http://docs.sqlalchemy.org/en/rel_0_9/core/types.html
[jinja]: http://jinja.pocoo.org/
[chameleon]: http://docs.pylonsproject.org/projects/pyramid-chameleon/en/latest/
[bootstrap]: http://getbootstrap.com/
[foundation]: http://foundation.zurb.com/
[repolink]: https://github.com/ryansb/wut4lunch_demos
