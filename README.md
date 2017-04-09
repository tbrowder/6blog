Blog engine written in Perl 6
==============================

Description of the planned system
=================================

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
  basename (derived automatically from the title) - does not change once the article was published even if the title is changed.
  format: (of abstract and body)
      "HTML"        (limit tags to a set of tags defined by the site admin)
      "Markdown"
  abstract (free text, will be shown on the main page)
  body (free text)
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
   text
   (date)
   (user)
   (reply-to another comment)

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
* Administrator - Can unpublish articles of 


Administration
------------------
A command line interface that can 
When the system is installed we can designate one or more 





