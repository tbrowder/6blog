Blog engine written in Perl 6
==============================

Description of the planned system
=================================

Setup
-------
* Docker image that contains everything needed to run the blog engine. An easy way to upgrade the engine.
* Linux: Unizp, run a shell script that will install everything needed. (Rakudo Star, the Perl 6 modules, the engine)
* Windows: Manual instructions to install Rakudo Star, the prerequisites, and the engine.

Administration
------------------
Both a command line interface and a web interface for the following actions:

* Reset password of any user.
* Set any user as "administrator".
* Remove the "administrator" bit from any user.
* Mark a blog post as "spam" that will hide the post from the world. It will also hide all the comments already added.
* Mark a comment as "spam" that will hide the comment. If the comment already have replies those will be hidden as well.


Modes of working:
-----------------
1) Sinlge author.
2) Multiple authors sharing a namespace.
3) Multiple authors each having their own namespace.  "Multi-user blog engine".


Users:
---------
  username          (Unique. Lowercase letters, digits, undersore, max 20 chars)
  password          (Bcrypt-ed)
  display name      (Any UTF-8 character, max 50 chars)
  email address     (Stored in all lower case. Check for uniqueness.)
  Website url and title
  About tag line    (plain text, max 100 chars)
  User picture

  Register:
      In order to register the user must provide a unique username, email, password.
      We send a verification code with a link. The user needs to click on it to verify
      their e-mail address. If not clicked on it, the verification code will expire in 24 hours.

  Change of password (logged in user)
      Form to type new password.

  Forgot password: type in username or e-mail, system sends a reset code to the e-mail address.
      Clicking on that link the user gest to the "Set new password" form.
      Code expires in 24 hours or when it is used.

  Change e-mail address. System sends verification code to new e-mail address.
      The user needs to click on the link in order to verify the new e-mail address.
 
Posts:
--------
  title:
  basename: (derived automatically from the title) - does not change once the article was published even if the title is changes.
  abstract: (free text, will be shown on the main page and other "index" pages.
  body: (free text)
  format: (of abstract and body)
      "HTML"        (limit tags to a set of tags defined by the site admin)
      "Markdown"
  tags: a comma separated list of expressions
  Status:
    Unpublished (draft)
	Published
	Scheduled
  Publish date: (date and time can be selected)
  Feedback: Accept comments (poster can enable disable comments per post)

* Create new entry.
* Publish entry.
* Unpublish entry.

Articles are: /PERMALINK
          or: /USERNAME/PERMALINK
          or: /prefix/USERNAME/PERMALINK (where prefix is a system-wide prefix)

The permalink is automatically derived from the title of the article,
but the author can change it.
Optionally the system can enforce   /users/USERNAME/YEAR/MONTH/PERMALINK.html

As long as an entry is not published we get 404 if we visit its (future) URL.
If an entry was unpublished, the page will show 404 "Unpublished".
If the PERMALINK of an article is changed we automatically install redirections from
the old URLs to the real URL.

Comments:
----------
Either using external engine (e.g. Disqus) or internal commenting engine for registered users.
In case of internal commenting we have the following about each comment:
   ID: Unique id of the comment.
   text: (free text)
   date:
   user: The ID of the user who creates this comment.
   reply-to: The ID of another comment. (optional)

Images and other non-html files
-------------------------------
Authors can upload files to their home directory. (This is for uploading images)

Index pages:
--------------
/    shows a list of recent posts:
   <a href="PERMALINK">TITLE</a>
   By <a href="/USERNAME">DISPLAY_NAME</a> on DATE
   POST.ABSTRACT
   <hr>
   COUNT comments  <a href="PERMALINK">Continue reading</a>
   <hr>

/USERNAME  or /prefix/USERNAME
   like the / page but shows a list of recent post of the specific author
   it also shows more information about the author.

Roles
-------
* Commenter     - a user who can only comment
* Author        - a user who can write articles
* Publisher     - a user who can publish articles
* Administrator - can unpublish articles.


Text-Editor
-------------
* In the basic version of this application the editor will be plain HTML5 without any JavaScript. The user will need to press "Preview" that will send the content to the server that will redisplay the editor and the formatted text. During this time the draft will be saved on the server.
* In the more advanced version we can add some JaveScript code that will regularily send the current version of the text to the server, where the server code can save the draft and can send back the formatted version of the article.
* Wysiwyg editor (There are lot's of implementation out there. One need to be selected and hooked up.)


Development
------------
perl6 -I../Bailador/lib ../Bailador/bin/bailador bin/app.pl

To run the test (assuming Bailador is not installed, but can be found next to the root directory of this project)
PERL6LIB=../Bailador/lib prove6 -l



Minimal plan
=============
/register: email, username, password, full name  (no verification for now)
Hash the password
Store in database:
   Email (lowercase, /^ <[a-z0-9.-]>\@\<[a-z0-9.-]>+$ , username (lowercase /^<[a-z0-9]>{3,}$/ ), hashed password,
date_of_registration=now(), verified=false, (id auto generated)

/account
     Show username, email, allow editing full name
     Only accessible if logged in

/login
  Username, password
  Session expires in 20 days of inactivity
   (user_id, last_seen time)

/logout
  Remove session
/edit
/edit/ID
   Title
   url (automatically derived from the title)
   id (automatically generated on the first save)
   Published True/False
   Publish_date = when it was first published.
   Abstract (will show in listing)
   Body
   Both can have some pre-defined html tags:
      <a href=””></a>
     <b></b>
    <h2></h2>
    Button to save
    Button to Publish (alternatively, a button to unpublish)

/users/USERNAME/url

(logged in user should be able to see her unpublished posts as well, and they should have some clear indication that the page is
not published)

/   shows the abstract of the N=10 most recent posts


Backup: cron-job off-site that will access the server every 10 minutes and copy the database file to the local disk with a timestamp
in the filename.


