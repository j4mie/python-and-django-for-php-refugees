
# Python & Django for PHP Refugees
## Jamie Matthews
## Brighton & Hove Python User Group
## 12th April 2011

---

# About me

* Used both Python and PHP (amongst other things) for ~8 years, on and off.
* Web freelancing using PHP while at uni.
* Python for MSc work: genetic algorithms, gluing together C code.
* ~3 years full-time PHP developer.
* Co-founded **DabApps**, using Python and Django.

![Tesco Cars](images/tescocars.png)

---

# Outline

* PHP ain't so bad
* PHP and Python: compare and contrast
* PHP and Django: views, templates, models, other goodies
* Demo time
* Homework

---

.fx: image

![Elephant](images/elephant.jpeg)

---

# PHP ain't so bad

* PHP is a programming language and a web framework and a templating language.
* PHP is always going to be easier to deploy than Python (thanks to `mod_php`).
* It is perfectly possible to write good code in PHP. You just have to be very careful.

---

# For beginners

* Python is easier to learn than PHP.
* Web programming with PHP is easier to learn than Python and Django.
* ...but, web programming isn't meant to be easy. Web applications are complex.
* Most important: get your head around the concepts. The details come with experience.

---

# Compare

* Both are interpreted, high-level languages with dynamic typing.
* Both are **multiparadigm** languages. Support procedural, object-oriented and functional styles.

## PHP

    !php
    <?php
    items = array('spam', 'eggs', 'beans');
    foreach ($items as $item) {
        echo $item . "\n";
    }
    ?>

## Python

    !python
    items = ['spam', 'eggs', 'beans']
    for item in items:
        print item

* WTF? No semicolons?
* OMG! Significant whitespace!

---

# Conventions & Consistency

## PHP

* `CamelCase`
* `lowercase_with_underscores`
* `something_Else_Weird($MY_VAR);`
* Intentation: tabs? Two spaces? Four spaces?

## Python ##

**PEP8: Style Guide for Python Code**

<http://www.python.org/dev/peps/pep-0008/>

No arguments.

---

# Namespacing

## PHP < 5.3

...what namespaces?

Everything is global. The only way to namespace code is to use a class.

## PHP >=5.3

Namespaces are optional. No namespace declaration? No namespace.

## Python

> Namespaces are one honking great idea -- let's do more of those!

A file containing Python code (a module) gets its own namespace.

*firstfile.py*

    !python
    some_variable = "Hello Brighton"

*secondfile.py*

    !python
    from firstfile import some_variable
    print some_variable

---

# Everything is an object

## Strings are objects

    !python
    my_string = 'hello'
    print my_string
    print my_string.upper()
    print 'hello'.upper()

## Functions are objects

    !python
    def say_hello(name):
        print 'hello ' + name

    def say_goodbye(name):
        print 'goodbye ' + name

    greetings = [hello, goodbye]
    for greeting in greetings:
        print greeting('brighton')

## Classes are objects

... but let's not go there.

---

## Python global built-in functions: 74

![Python built-in functions](images/pyfunctions.png)

---

## PHP global built-in functions

![PHP built-in functions](images/phpfunctions1.png)

---

## PHP global built-in functions (2)

![PHP built-in functions](images/phpfunctions2.png)

---

## PHP global built-in functions (3)

![PHP built-in functions](images/phpfunctions3.png)

---

## PHP global built-in functions: 5845

![PHP built-in functions](images/phpfunctionsall.png)

---

## Personal favourite

![date sunrise](images/phpdatesunrise.png)

---

# This is not good language design.

---

.fx: image

![Pony](images/pony.png)

---

# MVC

---

.fx: image

![MTV](images/mtv.jpg)

---

# Code and URLs: PHP

In PHP, it's tempting to use Apache and the filesystem to map a URL to some code.

This goes in `myfile.php`

    !html+php
    <h1><?php echo "Hello!"; ?></h1>

Your URL becomes `http://www.yourserver.com/myfile.php`

# Problems

* Code becomes a tangled mess of `include "header.php";`
* Your URLs are ugly.

---

# Code and URLs: Django

All requests get sent to the framework. It is responsible for routing URLs to pieces of code.

Functions that handle requests and return responses are called **views** (other frameworks might call a similar concept *controllers*).

views.py:

    !python
    from django.http import HttpResponse

    def my_view(request):
        return HttpResponse('<h1>Hello!</h1>')

urls.py:

    !python
    from django.conf.urls import patterns, url
    from views import my_view

    urlpatterns = patterns(
        url(r'^$', my_view),
    )

---

# Templates: PHP

PHP is its own templating language.

    !html+php
    <?php
        $title = "Saying hello!";
        $names = array("Rod", "Jane", "Freddy");
    ?>

    <h1><?php echo $title; ?></h1>
    <ul>
    <?php foreach ($name as $name): ?>
        <li><?php echo $name; ?></li>
    <?php endforeach; ?>
    </ul>

With great power comes great responsibility. Most developers are lazy.

Ever seen a file with HTML, CSS, JavaScript, PHP and SQL in it? Me too.

---

# Templates: Django

views.py:

    !python
    from django.shortcuts import render

    def my_view(request):
        return render(request, 'mytemplate.html', {
            'title': "Saying hello!",
            'names': ['Rod', 'Jane', 'Freddy'],
        })

templates/mytemplate.html:

    !html+django
    <h1>{{ title }}</h1>
    <ul>
    {% for name in names %}
        <li>{{ name }}</li>
    {% endfor %}
    </ul>

Can add power through **tags** and **filters**. Many built in, can define your own.

    !html+django
    {{ value|date:"D d M Y" }}

---

# Models

Many PHP frameworks provide an **object-relational mapper**. So does Django.

... but Django's is nicer.

* Django considers your *code* to be the canonical representation of the structure of your data.
* Django *generates* the necessary `create table` statements to set up your database schema.

(As far as I know, no PHP ORMs do this. I wish they would).

Supports MySQL, PostgreSQL, SQLite and Oracle out of the box. Third-party backends available (Sybase, IBM DB2, Microsoft SQL Server, Firebird, ODBC...)

---

# Model fields

* Fields can map directly onto database types (`CharField` or `IntegerField`) or be rich objects with custom validation rules (`URLField`, `ImageField`).
* Fields can represent relationships: `ForeignKey`, `ManyToManyField`, `OneToOneField`.

---

# Declarative API

    !python
    from django.db import models

    class Note(models.Model):
        title = models.CharField(max_length=50)
        content = models.TextField()
        author_name = models.CharField(max_length=100)
        created_at = models.DateTimeField(auto_now_add=True)

This is impossible in PHP (please prove me wrong!)

---

# Making queries

    !python
    from models import Note

    first_note = Note.objects.get(pk=1)

    all_notes = Note.objects.all()

    notes_by_jamie = Note.objects.filter(author_name='Jamie')

    first_note_by_jamie = notes_by_jamie[0]

    new_note = Note()
    new_note.author_name = 'Jamie'
    new_note.title = 'My new note'
    new_note.content = 'Content of my new note'
    new_note.save()

---

# Other goodies: free admin interface

Just by defining your models, you get a rich editing interface for free.

![admin](images/admin.png)

---

# Other goodies: forms

Django lets you create HTML forms in a high-level object-oriented way.

Similar declarative API to models:

    !python
    from django import forms

    class ContactForm(forms.Form):
        name = forms.CharField()
        email_address = forms.EmailField()
        message = forms.CharField(widget=forms.Textarea)

Then, in your template...

    !html+django
    {{ form.as_p }}

Generates...

    !html
    <p>
        <label for="id_name">Name:</label>
        <input type="text" name="name" id="id_name" />
    </p>
    <p>
        <label for="id_email_address">

    ... etc

---

# Other goodies: form validation

Built-in form fields know how to validate themselves (`EmailField` checks for a valid email address, etc).

You can specify your own validation rules, on a per-field and/or per-form basis.

    !python
    from forms import ContactForm

    def my_view(request):
        if request.method == 'POST':
            form = ContactForm(request.POST)
            if form.is_valid():
                # process form, send email etc..

You can also use a `ModelForm` to create a form based on a model. No need to define the form fields twice (this is how the automatic admin works).

---

# Other goodies

* Authentication
* Cache system
* Geospatial database extensions
* Internationalisation, localisation
* Testing
* Signals
* Generic views

and, of course

* Documentation
* Huge community
* Open source culture
* Jazz

---

# Demo time

---

# Homework

Read up on:

* pip
* virtualenv
* bpython
* gunicorn

---

# Thanks!

* <http://j4mie.org>
* <http://twitter.com/j4mie>
* <http://brightonpy.org>
* <http://dabapps.com> (coming soon)

---

# The Zen of Python

    >>> import this
    The Zen of Python, by Tim Peters

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
