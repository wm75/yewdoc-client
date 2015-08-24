YEWDOC
======

Yewdoc is a personal document manager that makes creating and editing
text documents from the command line easier than using an editor and
filesystem commands.

It should be emphasised that it's for *text* documents: plain text,
restructuredText, markdown, html, css, etc. It offers these features:

* Filesystem transparency: the user doesn't need to know where files
  are stored.

* Entirely keyboard driven.

* Hyperlink awareness: documents in Yewdoc can link to other documents
  in Yewdoc. This makes it possible to manage a set of documents as a
  mutually referencing set.

* Tags: you can tag documents with tags you define. 

* Familiar commands: yewdoc has commands like 'ls', 'head', 'tail',
  that are familiar to most shell users.
  
* Optional cloud storage for synchronising to multiple devices/workstations.

* Document conversions: it will generate any format that pandoc is
  able to convert to/from. There is special support for generation of
  html and the use of templates.
  
* Supports attachments: it supports attaching any file types but in
  particular graphics files that might be referenced in-line by the
  document.

* Integration with other command line utilities: you can do normal
  shell piping in and out, grep, etc.
 
You might think of it as a personal wiki, though the capabilities go
beyond that. The target users are those who prefer to work in a single
context, such as command-line without the mental overhead of switching
back and forth between a shell and the host OS GUI. When working
regularly on the command line, it is a considerable annoyance to have
to break out to use the host OS file management app to find files and
use the mouse. Yewdoc lets the user seamlessly browse and operate on
her collection of text files. These can be snippets, larger documents,
notes, etc. Exporting to other formats is easy and natural.

It's possible to maintain text documents on a server and sync to any
local device that supports Python (>= 2.7) and one of the common *nix
shells.

You can edit and manage any kind of text file, txt, rst, md, conf,
etc. Yewdoc does have a slight prejudice towards Markdown, but this
will be hardly noticeable by setting a simple user variable to the
preferred type.

Installation
============

Make sure you are in the directory and execute the install command:

    ./install.sh

> Currently pandoc must be installed following the instructions specific
> to your operating system.

That should be all that is required. 

Usage
=====

Create a new document and start editing: 

    yd create foo

Yewdoc uses the EDITOR environment to determine what editor to
launch. You can also have yewdoc launch an editor from the host OS and
let it decide which application handles that file type. This might be
Atom, Sublime Text or whatever editor you choose to associate with the
file type.

Edit the file we just created:

    yd edit foo

Edit the file with Sublime or whatever you have specified in your host
OS to handle this type of file:

    yd edit -o foo

You don't have to provide the whole title of the document. If the
fragment, in this case "my", matches case-insensitively to a document,
it will be loaded in the editor. Otherwise, the user is presented with
a choice of all matching files.

    yd show foo

will dump the contents of "foo" to stdout. Create a new document now from stdin:

    yd read bar

Type some content and enter end of file (ctrl-d usually). 

Copy a document to a new one: 

    yd show foo | yd read --create bar

A copy of the contents of foo will appear in a new document, bar. 

List all your documents: 

    yd ls

List all documents on the server (if you are using the Yewdoc server):

    yd ls -r

Sync your documents to/from a Yewdoc server:

    yd sync

You must have previously registered with Yewdoc server. This is
entirely optional.

Tags
====

Tags are an important featureset. You can create tags and
associate them with documents for making file organisation and
publishing easier.

Create a tag:

    yd tag -c red

Assign the 'red' tag to the doc 'foo':

    yd tag red foo

List documents with the red tag in long format, humanized:

    yd ls -lh -t red

Dissociate the tag 'red' from the document 'foo':

    yd tag -u red foo

How many documents with the foo tag:

    yd ls -t foo | wc

List all tags:

    yd tag

Configuration
=============

No configuration is necessary to work locally without mirroring
changes to the cloud (to a server through an internet connection). If
you want to have changes saved remotely, you need to provide certain
information, like:

* Email: your email that has not been registered to Remote
* URL: the endpoint for the remote yewdoc service. Currently, that is just https://doc.yew.io
* username: this is the username you desire to have on the remote service
* first name: first name (optional)
* last name: your last name (optional)

You can enter this via the `configure` command: 

    yd configure

Or immediately attempt to register: 
  
    yd register

In both cases, yewdoc will collect the required information from you. 

In the case that a registration succeeds, you will have set a token
that forthwith enables transparent access to the remote server. The
token is secret and should be protected. It's located in your home
directory in an sqlite database table.

You can check all user preferences via the `user_pref` command:

```
$ yd user_pref
location.default.url = https://doc.yew.io
location.default.email = paul.wolf@yewleaf.com
location.default.username = yewser
location.default.password = None
location.default.first_name = Paul
location.default.last_name = Wolf
location.default.token = fe0be94826b451bba72j54tjnwlr2jrn2o5cd9b
```

