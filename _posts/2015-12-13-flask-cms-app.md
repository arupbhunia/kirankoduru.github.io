---
layout: post
title:  "Minimalistic CMS blog with Flask"
date:   2015-12-14 00:39:43
updated:   2015-12-24 10:43:04
categories: python
description : Create a minimal CMS app using flask
---
When I started writing this post, I was going to name it _Writing a minimal Flask CMS app in 10 minutes_, and an hour later I changed my mind. 

Even though, I took an hour to write this app from scratch I am amazed at how quickly you can create a minimal CMS blog with Flask. All code for this project is on [github](https://github.com/kirankoduru/flask-cms-demo), you can view it as we go through it. 

If you don't know Flask, it's a python package that allows you to get started writing web apps in minutes. Initially it may have been a considered threat to Django, but I see it as a different beast. At work, we use it to write RESTful webservices around our internal packages but can be used to create a web apps around any of your python code.

Today, I am going to discuss about how to get started implementing a minimalistic CMS app. Most content management systems work around creating, editing, deleting and updating content. Most content is in HTML form. I wrote this app with the aforementioned assumption.

#### Directory structure
When writing a Flask app you will have a file to run your app, some html files in a __templates__ directory and some python code(_views.py_) that binds the app to your html file. To store the content of the website we will be using a sqlite database.

<pre>
.
├── app
│   ├── __init__.py
│   ├── templates
│   │   ├── edit-page.html
│   │   ├── index.html
│   │   ├── master.html
│   │   ├── new-page.html
│   │   └── page.html
│   └── views.py
├── cms.db  #sqlite database
├── requirements.txt
└── runserver.py
</pre>

#### Views

The _views.py_ contains the pages you would like the flask app to respond to, the connection to the database and SQLAlchemy table objects. I created views for the following pages:

| Page          | Route  | Description  |
|:--------------|:-------|:-------------|
|Index|/|This page will list all the blog posts.|
|Blog|/page/:id|This page shows the contents of a single page _id_.|
|Edit|/edit-page/:id|To edit a particular page _id_.|
|New|/new-page/|To create a new page.|
|Delete|/delete/:id|To delete a particular page _id_.|

The database contains only one table, the `Pages` table that contains all the blog posts with fields `id`, `title` and `content`. The `content` field is a [blob](https://en.wikipedia.org/wiki/Binary_large_object) object that contains the HTML for any given page.


#### Conclusion

The application is pretty basic so I haven't included basic authentication either. I use a `<textarea>` tag to include the content for a blog post which you can replace with light weight [WYSIWYG](https://github.com/cheeaun/mooeditable/wiki/Javascript-WYSIWYG-editors) editors if you like. For any other comments and suggestions please reply to this post, I would love to hear your thoughts & opinions. The source code is available on the following [link](https://github.com/kirankoduru/flask-cms-demo).
