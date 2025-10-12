**You need to have the coding language Python installed on your device.**
*Download Python here if you don't have it: https://www.python.org/downloads/*

## It is *strongly* advised to learn Python before using scratchattach.

# Installation

Run the following command in your command prompt / shell:

```
pip install -U scratchattach
```

Then, import the library in Python using:

```python
import scratchattach as sa
```

# Usage with and without a login

**Entrypoint with a login:**

Scratchattach provides methods for logging in (see below). These methods all return a `sa.Session` object saving the login / session. This object can be used to connect users, projects, clouds and lots of other stuff.

Methods starting with `session.connect_` (like, for example, `session.connect_user("username")` will return an object that is connected to the session and allows performing actions that require a login (like `user.follow()`).

The session used to connect the object will also be connected to objects emerging from it (like the `sa.Project` objects returned by `user.projects()`), which hence also can be used to perform actions that requiring authentication. This makes it possible to run code like this without having to worry about staying authenticated:

```py
sa.login("user", "password").connect_user("user2").projects()[0].comment_by_id("id").reply("content")
```

**Entrypoint without a login:**

Scratchattach also allows getting data without logging in, by using methods starting with `sa.get_` (like `sa.get_user("username")`). However, the returned objects will NOT be connected to a session and therefore can't be used to perfom actions that require authentication, **so be careful when to use these methods.**

## Logging in

**Logging in with username / password:**

```python
session = sa.login("username", "password") #Returns a sa.Session object
```

**Logging in with a session id:**

*You can get your session id from your browser's cookies. [More information](https://github.com/TimMcCool/scratchattach/wiki/Get-your-session-id)*

```python
session = sa.login_by_id("session", username="username") #Returns a sa.Session object. The username field is case sensitive and optional (you only need to set it when you're on replit)
```

**Logging in with a session string:**

*This is a scratchattach-specific authentication method. After running `session = sa.login("username", "password")`, your session string is saved in the `session.session_string` attribute. This encoded string contains both your username / password and session. When using it to log in, scratchattach will first try authenticating using the session id, then attempt logging in by username / password.*

```python
session = sa.login_by_session_string("session_string") #Returns a sa.Session object
```

## sa.Session class

Represents and stores a Scratch login / session. Located in `scratchattach.site.session`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```py
session.id #The session id associated with the login
session.username #The username associated with the login
session.xtoken
session.new_scratcher #Returns True if the associated account is a New Scratcher
session.mute_status #Information about commenting restrictions of the associated account
session.banned #Returns True if the associated account is banned
session.email #Returns the email address associated with the logged in account
session.new_email_address #If an email change was requested, this returns the new (not verified yet) email. New in 2.1.6
session.has_outstanding_email_confirmation #Returns whether the associated account verified their email. New in v2.1.6
# ----- ----- #
user.update() #Updates the above data
```

# Cloud variables

**Connect a cloud:**

```python
cloud = session.connect_cloud("project_id") # connects any cloud (by default Scratch one's). Returns a sa.ScratchCloud object by default
cloud = session.connect_scratch_cloud("project_id") # connects Scratch's cloud. Returns a sa.ScratchCloud object by default

cloud = session.connect_tw_cloud("project_id", purpose="(optional) your use case", contact="(optional) your Scratch account or other contact info")
# connects TurboWarp's cloud. Returns a sa.TwCloud object by default.
# Optional arguments: purpose and contact. Providing these arguments allows TurboWarp to understand what you're using their cloud server for.
# Optional argument: cloud_host (for connecting to custom websockets). To connect to forkphorus's cloud server, use cloud_host="wss://stratus.turbowarp.org"
```

**Get a cloud without logging in:** (methods that require authentication will not work on the returned object)
 
```python
cloud = sa.get_cloud("project_id") #Returns a sa.ScratchCloud object by default
cloud = sa.get_scratch_cloud("project_id") #Returns a sa.ScratchCloud object
cloud = sa.get_tw_cloud("project_id") #Returns a sa.TwCloud object
```

## sa.ScratchCloud, sa.TwCloud, sa.CustomCloud classes

Represent a Scratch cloud, TurboWarp cloud or another cloud variable server. Located in `scratchattach.cloud.cloud`.

`sa.ScratchCloud`, `sa.TwCloud` and `sa.CustomCloud` all inherit from the base class `sa.BaseCloud`. All classes inheriting from `sa.BaseCloud` have these methods:

**Set one cloud variable:**
```py
cloud.set_var("variable", "value") # the variable name is specified without the cloud emoji
```

**Set multiple cloud variables simultaneously:** (new in v2.0)
```py
cloud.set_vars({
    "var1_name" : "var1_value",
    "var2_name" : "var2_value",
    ...
}) # optional argument: intelligent_waits=True. When setting it to False, you can set cloud variables much faster, but you will get rate limited by Scratch.
```

**Get cloud variables:** (new in v2.0)
```python
value = cloud.get_var("variable") # returns the current value of the variable. the variable name is specified without the cloud emoji. In v2.0, you can safely use this method in a loop without spamming the API
variables = cloud.get_all_vars() # returns a dict with the current values of all cloud variables. In v2.0, you can safely use this method in a loop without spamming the API
```

**Disconnect / reconnect:**
```py
cloud.reconnect()
cloud.disconnect()
```

**Get the clouddata logs:**
```py
logs = cloud.logs() # returns the clouddata logs as list of sa.CloudActivity objects
```

[**Fully customized clouds (advanced)**](https://github.com/TimMcCool/scratchattach/wiki/Documentation#fully-customized-clouds)

## sa.CloudActivity class

Represents a cloud activity (a cloud variable set / creation / deletion). Located in `scratchattach.site.cloud_activity`. Inherited from `sa.BaseSiteComponent`.

**Attributes:**
```py
cloud_activity.timestamp # The timestamp of when the activity was performed
cloud_activity.username # The username of the user who added / set / deleted the cloud variable
cloud_activity.type # The activity type ("set", "create" or "del")
cloud_activity.var # The name of the cloud variable that was updated (specified without the cloud emoji)
cloud_activity.value # If the activity is a variable set, this attribute provides the value it was set to
cloud_activity.cloud # The cloud this activity was performed on as sa.ScratchCloud, sa.TwCloud or sa.CustomCloud object
```

**Methods:**
```py
cloud_activity.load_log_data() # Checks the clouddata logs to find the user who performed the activity. Also loads the exact timestamp
cloud_activity.actor() # Returns the user who performed the activity as sa.User object
cloud_activity.project() # Returns the project the activity was performed on as sa.Project object
```

## Encoding / Decoding

Scratchattach has a built in encoder. Scratch sprite to decode texts encoded with scratchattach: [Click here to download](https://github.com/TimMcCool/scratchattach/raw/main/assets/Encoder.sprite3)

```python
from scratchattach import Encoding

Encoding.encode("input") #will return the encoded text
Encoding.decode("encoded") #will decode an encoded text

sa.encoder.letters #returns the list with letters used by the encoder. You can set indices of this list to add / remove letters. The indices of the letters in this list correspond to the indices of costumes in the Scratch sprite
Encoding.replace_char("old_char", "new_char") #replaces a character in the above list. Don't forget to replace the character in the customes of the Scratch sprite too.
```

## Cloud Events

*Cloud events allow reacting to cloud events in real time. If a Scratcher
sets / creates / deletes a cloud var on the given project, an
event will be called.*

All classes inheriting from `sa.BaseCloud` have the `.events()` method that returns a cloud event handler.

The cloud event handler's class is `scratchattach.event_handlers.cloud_events.CloudEvents`. This class is inheriting from the base class `sa.BaseEventHandler`.

**How to use with Scratch's cloud (data from websocket):**

```python
import scratchattach as sa

session = sa.login("username", "password") # Log in to Scratch
cloud = session.connect_scratch_cloud("project_id") # Connect Scratch's cloud
events = cloud.events()

@events.event
def on_set(activity): #Called when a cloud var is set
    print(f"Variable {activity.var} was set to the value {activity.value} at {activity.timestamp}")
    # `activity` is a sa.CloudActivity object
    # To get the user who set the variable, call activity.load_log_data() which saves the username to the activity.username attribute

@events.event
def on_del(activity):
    print(f"{activity.user} deleted variable {activity.var}")

@events.event
def on_create(activity):
    print(f"{activity.user} created variable {activity.var}")

@events.event #Called when the event listener is ready
def on_ready():
   print("Event listener ready!")

events.start()
```

**How to use with Scratch's cloud (data from cloud logs):**

```python
import scratchattach as sa

cloud = sa.get_scratch_cloud("project_id")
events = cloud.events()
...
```

**How to use with TurboWarp's cloud:**

```python
import scratchattach as sa

cloud = sa.get_tw_cloud("project_id")
events = cloud.events()
...
```

In scratchattach, all event handlers inheriting from `sa.BaseEventHandler` have these methods:

```py
events.start(thread=True, ignore_exceptions=True)
events.pause()
events.resume()
events.stop()
```

**Note:** In scratchattach v2.0, all events are ran in threads by default. You can disable this using `@events.event(thread=False)`.

## Cloud Requests

Scratchattach provides a Cloud Requests Framework (inspired by discord.py) that allows Scratch projects and Python to interact freely and exchange any kind of data, without having to worry about encoding / decoding or exceeding cloud variable length limits. (Based on Cloud Events)

*This makes it possible to access data like message counts, user stats and more from Scratch projects! Uses cloud variables to transmit data.*

**[Click here to go to Cloud Requests documentation](https://github.com/TimMcCool/scratchattach/wiki/Cloud-Requests)**

If you want to access external information in Scratch projects or store data on an external database, scratchattach's Cloud Requests are ideal for your project:
- Similar to cloud events, but send back data to the project
- Automatically encode / decode sent data
- Tons of extra features

## Cloud Storage

Scratchattach also provides a Cloud Storage Framework that makes it easy to store project data by sending it over cloud variables. The data is stored in JSON files saved on your computer and can be accessed and modified by the Scratch project. (Based on Cloud Requests)

*This makes it possible to connect a database to your Scratch project!*

**[Click here to go to Cloud Storage documentation](https://github.com/TimMcCool/scratchattach/wiki/Cloud-Storage)**

If you need a simple key-value storage for storing highscores or user data, scratchattach's Cloud Storages are ideal for your project.

# Users

**Connect a user by their username:**
```python
user = session.connect_user("username") # Returns a sa.User object
```

**Connect a user by their user id:** (new in v2.0)
```python
user = session.connect_user_by_id("user_id") # Returns a sa.User object. Avoid spamming this method, it posts a comment (deleted automatically) every time it is called (no rate limit is activated because the comment is deleted immediately, but still avoid spamming it)
```

**Connect the user you are logged in with:**
```python
user = session.connect_linked_user() # Returns a sa.User object
```

**Get a user without logging in:** (methods that require authentication, like user.follow() or user.post_comment(), will not work on the returned object)
```python
user = sa.get_user("username") # Returns a sa.User object. Warning: Any methods that require authentication will not work on the returned object
```

## sa.User class

Represents a Scratch user. Located in `scratchattach.site.user`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```python
user.join_date
user.about_me
user.username
user.wiwo #Returns the user's 'What I'm working on' section
user.country #Returns the country from the user profile
user.icon_url #Returns the link to the user's pfp (90x90)
user.id #Returns the id of the user
user.scratchteam #Retuns True if the user is in the Scratch team
# ----- ----- #
user.update() #Updates the above data
```

**Methods:**
```python
user.message_count()
user.featured_data() #Returns info on the user's featured project as dict
user.does_exist() #Returns True if the user exists and False if the user is deleted. New in v1.7.3
user.is_new_scratcher() #Returns whether the user is a new Scratcher. New in v1.7.3

user.follower_count()
user.following_count()
user.project_count()
user.loves_count() # Returns the amount of projects the user has loved. New in v2.1.1
user.favorites_count() #Returns the amount of projects the user has favorited
user.studio_count() #Returns the amount of studios the user is curating
user.studio_following_count()

user.follower_names(limit=40, offset=0) #Returns the followers as list of usernames (strings). New in v1.1.5
user.following_names(limit=40, offset=0) #Returns the people the user is following as a list of usernames (strings). New in v1.1.5
user.followers(limit=40, offset=0) #Returns the followers as list of sa.User objects
user.following(limit=40, offset=0) #Returns the people the user is following as list of sa.User objects

user.projects(limit=40, offset=0) #Returns the projects the user has shared as list of sa.Project objects
user.favorites(limit=40, offset=0) #Returns the projects the user has favorited as list of sa.Project objects
user.studios(limit=40, offset=0) #Returns the studios the user is curating as list of sa.Studio objects
user.viewed_projects(limit=24, offset=0) #Returns the projects the user has recently viewed as list of sa.Project objects. To use this, you need to be logged in as the user.
user.loves(limit=40, offset=0, get_full_project=False)

user.follow()
user.unfollow()
user.is_following("scratcher") #Returns True if user is following the specified Scratcher
user.is_followed_by("scratcher") #Returns True if user is followed by the specified Scratcher

user.activity(limit=1000) # Returns the user's activity as list of sa.Activity objects
user.activity_html(limit=1000) # Returns the user's activity as raw HTML document

user.comments(limit=20, page=1) #Returns the user's profile comments as list of sa.Comment objects
user.comment_by_id("comment_id") #Finds the comment and returns it as as sa.Comment object

user.post_comment("comment content") #Posts a comment on the user's profile and returns it as sa.Comment object. Requires logging in.
user.reply_comment("comment content", parent_id="parent_id", commentee_id="") #Warning: Your comment will only be shown on Scratch if parent_id is the comment id of a top level comment
user.delete_comment(comment_id="comment_id")
user.report_comment(comment_id="comment_id")

user.toggle_commenting()
user.set_bio(text) #Changes the 'About me' of the user
user.set_wiwo(text)
user.set_featured("project_id", label="") #Changes the featured project
user.set_forum_signature(text) # Changes the discuss forum signature of the user. New in v1.7.3

user.ocular_status() #Returns information about the user's ocular status, like the status text, the color, and the time of the last update.
```

**Deprecated methods**: (may not work anymore)
```py
user.stats() #Returns the user's statistics as dict. Fetched from ScratchDB
user.ranks() #Returns the user's ranks as dict. Fetched from ScratchDB
user.followers_over_time(segment=1, range=30) #Fetched from ScratchDB
```

## Verifying a user's identity

In some applications, it may be necessary to verify a user's Scratch identity. This is usually done by telling the user to comment a code on a project, and then checking if the user actually commented the code.

scratchattach makes it easy to develop this kind of system. **It provides a `user.verify_identity()` function that returns a `Verificator` object:**

```py
verificator = user.verify_identity() # The project id where the user has to comment can be specified as `verification_project_id` keyword argument 
```

**The `Verificator` object has these arguments:**

```py
verificator.projecturl # The link to the project where the user has to go to verify
verificator.project # The project where the user has to go to verify as sa.Project object
verificator.code # The code the user has to comment on the project
```

Tell your user to comment the code saved in the `verificator.code` attribute. To check if the user verified successfully, call the `verificator.check()` function. If the user commented the code, it will return `True`.

## Message Events

_Message events allow reacting to messages in real time. Every time a Scratch receives a message, an event will be called._

The message event handler's class is `scratchattach.event_handlers.message_events.MessageEvents`. This class is inheriting from the base class `sa.BaseEventHandler`.

**Observe message count changes on other users:**

```py
import scratchattach as sa

user = sa.get_user("username") # Get the user you want to observe
events = user.message_events()

@events.event
def on_count_change(old_count, new_count): #Called when the user's message count changes.
    print("message count changed from", old_count, "to", new_count)

@events.event #Called when the event listener is ready
def on_ready():
   print("Event listener ready!")

events.start()
```

**Call events when you receive messages on your account:**

```py
import scratchattach as sa

session = sa.login("username", "password")
events = session.connect_message_events()

@events.event
def on_message(message):
    # `message` is a sa.Activity object.
    # All attributes and methods of `message` can be found in the documentation of the sa.Activity class.
    print(message.actor_username, "performed action", self.type)

...

events.start()
```

In scratchattach, all event handlers inheriting from `sa.BaseEventHandler` have these methods:

```py
events.start(thread=True, ignore_exceptions=True)
events.pause()
events.resume()
events.stop()
```

## Filterbot

Scratchattach provides a filterbot framework that can be used to automatically delete spam comments. You can either use pre-made filter profiles (like *f4f filter* or *advertising filter*) or set up your own, custom filter rules. (Based on Message Events)

**[Click here to go to Filterbot documentation](https://github.com/TimMcCool/scratchattach/wiki/Filterbot)**

# Projects

**Connect a project:**
```py
project = session.connect_project("project_id") # Returns a sa.Project object
```

**Create a new project:** (never spam this method)
```py
project = session.create_project(title="title") # Returns a sa.Project object representing the created project. Optional keyword arguments: parent_id and project_json
```

**Get a project without logging in:** (methods that require authentication, like project.love(), will not work on the returned object)

```py
project = sa.get_project("project_id") # Returns a sa.Project object. Warning: Any methods that require authentication will not work on the returned object
```

## sa.Project class

Represents a Scratch project. Located in `scratchattach.site.project`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**

```python
project.id  #Returns the project id
project.url  #Returns the project url
project.title #Returns the project title
project.author_name  #Returns the username of the author
project.comments_allowed  #Returns True if comments are enabled
project.instructions
project.notes  #Returns the 'Notes and Credits' section
project.created  #Returns the date of the project creation
project.last_modified  #Returns the date when the project was modified the last time
project.share_date
project.thumbnail_url
project.remix_parent
project.remix_root
project.loves  #Returns the love count
project.favorites #Returns the project's favorite count
project.remix_count  #Returns the number of remixes
project.views  #Returns the view count
project.project_token
project.embed_url #New in v2.1.6
# ----- ----- #
project.update()  #Updates the above data
```

Depending on how the object was created, some attributes may not exist or may be None. After running `project.update()`, all attributes are available.

**Methods:**
```python
project.author() #Returns the author as sa.User object
project.create_remix() #Creates a remix of the project. Never spam this method.
project.moderation_status() #Returns the project's moderation status (either "safe" or "notsafe" (nfe)). Fetched from jeffalo.net
project.visibility() #Returns info on the project's visibility. You have to be the project owner to use this.
project.is_shared() #Returns True if the project is currently shared

project.comments(limit=40, offset=0) #Returns all project comments as list of sa.Comment objects
project.comment_by_id("comment_id") #Finds the comment and returns it as as sa.Comment object
project.comment_replies(comment_id="comment_id", limit=40, offset=0) #Returns all replies to the specified comment as list of sa.Comment objects

project.post_comment("comment content") #Returns the posted comment as sa.Comment object
project.reply_comment("comment content", parent_id="parent_id", commentee_id="") #Warning: Your comment will only be shown on Scratch if parent_id is the comment id of a top level comment
project.delete_comment(comment_id="comment_id")
project.report_comment(comment_id="comment_id")

project.love()
project.unlove()
project.favorite()
project.unfavorite()
project.post_view() #Does not require authentication

project.set_title("new title")
project.set_instructions("new instructions")
project.set_notes("new notes and credits")  #Sets the notes and credits section of the project
project.set_thumbnail(file="filename.png") #File must be .png and fit Scratch's thumbnail guidelines
project.share()
project.unshare()
project.set_fields({ ... }, use_site_api=False) #Allows setting multiple fileds (like "title", "instructions" etc.) simultaneously through scratch's API. When setting use_site_api to True, the deprecated site api will be used (allows setting more fields, but may not work anymore).

project.body() #Downloads and parses the content of the project and returns it as sa.ProjectBody object (new in v2.0)
project.download(filename="project_name.sb3", dir="") #Downloads the project to your computer. The downloaded file will only work in the online editor
project.raw_json() #Returns the JSON of the project content as dict
project.creator_agent() #Returns the user-agent of the user who created the project (with information about their browser and OS)

project.set_body(project_body) #Sets the project JSON to a sa.ProjectBody object (which is representing the content of a project)
project.set_json(json_data) #Sets the project JSON. Can be used to upload projects. json_data must be a dict or an encoded JSON object with the project JSON.
project.upload_from_json(project_id)

project.turn_off_commenting()
project.turn_on_commenting()
project.toggle_commenting()

project.remixes(limit=None, offset=0) #Returns the remixes as list of sa.Project objects
project.studios(limit=None, offset=0) #Returns the studios sa list of sa.Studio objects
```

## sa.PartialProject class

When connecting / getting a project that you can't access, a `sa.PartialProject` object is returned. Located in `scratchattach.site.project`. Inherited from the base class `sa.BaseSiteComponent`.

**Methods:**
```python
project.create_remix() #Creates a remix of the unshared project. Never spam this method.
project.load_description() #Gets the title and the instructions of the unshared project and saves them to the project.title and project.instructions attributes. It is unclear if the Scratch team allows using this method because it uses an API glitch. Requires authentication.

project.remixes(limit=None, offset=0) #Returns the unshared project's remixes as list of sa.Project objects
project.is_shared() # Will always return False for PartialProject objects
```

# Studios

**Connect a studio:**
```py
studio = session.connect_studio("studio_id") # Returns a sa.Studio object
```

**Get a studio without logging in:** (methods that require authentication, like studio.post_comment, will not work on the returned object)
```py
studio = sa.get_studio("studio_id") # Returns a sa.Project object. Warning: Any methods that require authentication will not work on the returned object
```

## sa.Studio class

Represents a Scratch studio. Located in `scratchattach.site.studio`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```python
studio.id
studio.title
studio.description
studio.host_id #The user id of the studio host
studio.open_to_all #Whether everyone is allowed to add projects
studio.comments_allowed
studio.image_url
studio.created
studio.modified
studio.follower_count
studio.manager_count
studio.project_count
# ----- ----- #
studio.update()  #Updates the above data
```

Depending on how the object was created, some attributes may not exist or may be `None`. After running `studio.update()`, all attributes are available.

**Methods:**
```python
studio.follow()
studio.unfollow()

studio.comments(limit=40, offset=0)  #Returns the studio comments as list of sa.Comment objects
studio.comment_by_id(comment_id="comment_id") #Finds the comment and returns it as as sa.Comment object
studio.comment_replies(comment_id="comment_id", limit=40, offset=0) #Returns all replies to the specified comment as list of sa.Comment objects

studio.post_comment("comment content") #Returns the posted comment as sa.Comment object
studio.reply_comment("comment content", parent_id="parent_id", commentee_id="") #Warning: Your comment will only be shown on Scratch if parent_id is the comment id of a top level comment
studio.delete_comment(comment_id="comment_id")
studio.report_comment(comment_id="comment_id")

studio.add_project("project_id")
studio.remove_project("project_id")

studio.set_description("new description")
studio.set_title("new title")
studio.set_thumbnail(file="filename.png") # New in v1.4.5. File must fit Scratch's thumbnail guidelines. Returns a link to the uploaded thumbnail.
studio.open_projects() #Allows everyone to add projects
studio.close_projects()
studio.set_fields({ ... }) #Sets fields using the scratch.mit.edu/site-api PUT API

studio.turn_off_commenting() # New in v1.0.1
studio.turn_on_commenting()
studio.toggle_commenting()

studio.invite_curator("username")
studio.promote_curator("username")
studio.remove_curator("username")
studio.accept_invite() #If there is a pending invite for you, this function will accept it
studio.leave() #Removes yourself from the studio

studio.projects(limit=40, offset=0) #Returns the studio's projects as list of sa.Project objects
studio.activity(limit=24, offset=0) #Returns the studio's activity as list of sa.Activity objects
studio.curators(limit=24, offset=0) #Returns the curators as list of sa.User objects
studio.managers(limit=24, offset=0) #Returns the managers as list of sa.User objects
studio.host() #Returns the studio host as sa.User object

studio.transfer_ownership("new_owner", password="password") #Irreversibly makes another Scratcher studio host. You need to specify your password to do this.
```

# Comments

**Connect / get a comment:**

```py
comment = project.comment_by_id("comment_id") # Returns a sa.Comment object representing a project comment
comment = user.comment_by_id("comment_id") # Returns a sa.Comment object representing a profile comment
comment = studio.comment_by_id("comment_id") # Returns a sa.Comment object representing a studio comment
```

## sa.Comment class

Represents a Scratch comment. Located in `scratchattach.site.comment`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```py
comment.id # The comment id
comment.parent_id # The id of the corresponding top level comment if this comment is a reply, else None
comment.commentee_id # The id of the user that this comment mentions
comment.content # The comment content
comment.datetime_created
comment.author_name # The username of the comment author
comment.author_id # The user id of the comment author
comment.written_by_scratchteam # Whether a Scratch team member posted this comment
comment.reply_count
comment.source # The type of place where this comment was posted (either "studio", "profile" or "project")
comment.source_id # An ID identifying the place where this comment was posted (either a studio id, a username or a project id)
```

**Methods:**
```py
comment.author() # Returns the author of the comment as sa.User object
comment.place() # Returns the place where the comment was posted as sa.Project, sa.User or sa.Studio project. If the place wasn't saved, this returns None.
comment.parent_comment() # Returns the corresponding top-level comment as sa.Comment object if this comment is a reply, else None
comment.replies() # Returns the replies to this comment as list of sa.Comment objects

comment.reply("comment content") # Posts a reply to this comment and returns it as sa.Comment object. Automatically sets the parent_id and commentee_id fields correctly
comment.delete() # Deletes the comment
comment.report() # Reports the comment to the Scratch team
```

# Activites / Messages

**Connect your messages:**
```py
activites = session.messages(limit=40, offset=0, date_limit=None, filter_by=None) # Returns your Scratch messages as list of sa.Activity objects. You can use date_limit to only include activities from after a specific date. Options for filter_by are None (no filter), “comments”, “projects”, “studios” or “forums”
```

**Connect your front page "What's happening" feed:**
```py
activities = session.feed(limit=20, offset=0, date_limit=None) # Returns your feed as list of sa.Activity objects. You can use date_limit to only include activities from after a specific date.
```

**Connect / get user or studio activities:**
```py
activites = user.activity() # Returns the user's activities as list of sa.Activity objects
activites = studio.activity() # Returns the studio's activities as list of sa.Activity objects
```

## sa.Activity class

Represents a message, a "What's happening"-activity, a user profile activity or a studio activity. Scratch uses the same class for representing these kind of objects, therefore scratchattach does too.

Located in `scratchattach.site.activity`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```py
activity.id # The ID of the activity
activity.type # Specificies the activity type ("loveproject", "becomecurator", "followuser", "addcomment", and many others)
activity.actor_username # The name of the user who performed the activity
activity.actor_id # The id of the user who performed the activity
activity.datetime_created # When the activity was performed

# If the activity type is addcomment:
activity.comment_fragment # The comment content
activity.comment_obj_title # The id of the place where the comment was posted
activity.comment_obj_id # The comment id
...

# If the activity type is loveproject:
activity.project_id # The loved project
activity.project_title

# If the activity type is followstudio:
activity.gallery_id # The studio id
activity.title # The studio title
...
```
Depending on how the object was created, there may be more attributes than listed above, or some attributes may not exist.

Tip: Use `activity.__dict__` to convert an `sa.Activity` to a `dict`. You can use `print(activity.__dict__)` to see the object's attributes and values.

**Methods:**
```
activity.actor() # Returns the user who performed the activity as sa.User object
activity.target() # Returns the activity's target (depending on the activity, this is either a sa.User, sa.Project, sa.Studio or sa.Comment object). May also return None if the activity type is unknown
```

# Data from Scratch's main pages

## Front page

```py
sa.get_news(limit=10, offset=0) #Returns the news from the Scratch front page as list

sa.featured_data() #Returns all front page data as dict

sa.featured_projects() #Returns the featured projects from the Scratch homepage as list of sa.Project objects
sa.featured_studios() #Returns the featured studios from the Scratch homepage as list of sa.Studio objects
sa.top_loved()
sa.top_remixed()
sa.newest_projects() #Returns a list with the newest Scratch projects as list of sa.Project objects. This list is not present on the Scratch home page, but the API still provides it.
sa.design_studio_projects()
sa.curated_projects()

session.feed(limit=20, offset=0) #Returns your "What's happening" section from the Scratch front page as list of sa.Activity objects
session.loved_by_followed_users(limit=40, offset=0) #Returns the projects loved by users you are following as list of sa.Project objects
```

## Search

```py
session.search_projects(query="query", mode="trending", language="en", limit=40, offset=0)
sa.search_projects(query="query", mode="trending", language="en", limit=40, offset=0) # Search projects without logging in. Methods requiring authentication won't work on the returned objects

session.search_studios(query="query", mode="trending", language="en", limit=40, offset=0)
sa.search_studios(query="query", mode="trending", language="en", limit=40, offset=0) # Search studios without logging in. Methods requiring authentication won't work on the returned objects
```

## Explore

```py
session.explore_projects(query="query", mode="trending", language="en", limit=40, offset=0)
sa.explore_projects(query="query", mode="trending", language="en", limit=40, offset=0) # Search projects without logging in. Methods requiring authentication won't work on the returned objects

session.explore_studios(query="query", mode="trending", language="en", limit=40, offset=0)
sa.explore_studios(query="query", mode="trending", language="en", limit=40, offset=0) # Search studios without logging in. Methods requiring authentication won't work on the returned objects
```

## Messages

```py
session.messages(limit=40, offset=0, date_limit=None, filter_by=None) #Returns your messages as list of sa.Activity objects
session.admin_messages() #Returns your unread admin notifications / alerts from the Scratch team
session.clear_messages() #Marks your messages as read
session.get_message_count() #Returns your message count. Fetched from the API used by 2.0-style pages.
```

## My stuff

```py
projects = session.mystuff_projects("all", page=1, sort_by="", descending=True) # Returns a list of sa.Project objects
studios = session.mystuff_studios("all", page=1, sort_by="", descending=True) # Returns a list of sa.Studio objects
```

**Arguments:**
* filter_arg (str): Possible values for this parameter are "all", "shared", "unshared" and "trashed"

**Keyword Arguments:**
* page (int): The page of the `My Stuff` projects or studios that should be returned
* sort_by (str): The key the projects should be sorted based on. Possible values for this parameter are "" (then the projects are sorted based on last modified), "view_count", love_count", "remixers_count" (then the projects are sorted based on remix count) and "title" (then the projects are sorted based on title)
* descending (boolean): Determines if the element with the highest key value (the key is specified in the `sort_by` argument) should be returned first. Defaults to `True`.

## Statistics

```py
sa.total_site_stats()
sa.monthly_site_traffic()
sa.country_counts()
sa.age_distribution()
sa.monthly_comment_activity()
sa.monthly_project_shares()
sa.monthly_active_users()
sa.monthly_activity_trends()
```

# Multi-event handler

If you want to run cloud events, cloud requests or message events on multiple projects / users simultaneously without having to define everything multiple times, use scratchattach's `sa.MultiEventHandler` class:

```py
from scratchattach import MultiEventHandler

# Initialize your cloud events / cloud requests / message events instances:
# (In this example, cloud requests are used, but the same can be done with any scratchattach event handler)

session = sa.login("username", "password")
client1 = session.connect_cloud("project_id_1").requests()
client2 = session.connect_cloud("project_id_2").requests()
client3 = session.connect_cloud("project_id_3").requests()
client4 = session.connect_cloud("project_id_4").requests()

# Combine the cloud requests using MultiEventHandler:

combined = MultiEventHandler(client1, client2, client3, client4)

# Define your events and requests:

@combined.event # defines the event for all eventhandlers that were combined
def on_ready():
    print("Cloud requests are ready")

@combined.request # defines the request for all request handlers that were combined
def your_request():
    ...

# Start all event / request handlers:
combined.start()
```

# Hosting a cloud server

TurboWarp allows you to use a custom cloud variable websocket server by setting the `?cloud_host=` URL parameter to its address.

**Example:** When hosting a websocket server on `127.0.0.1:8080`, you can open `turbowarp.org/702543294?cloud_host=ws://127.0.0.1:8080` in your browser to play griffpatch's slither.io remake while using your locally hosted server as cloud server (instead of TurboWarp's cloud server which is used by default).

Scratchattach makes it easy to set up a cloud websocket server which works with TurboWarp's cloud and gives you lots of customization.

## Why host your own cloud variable server?

- You have full control over who is playing your cloud project: You can see all players (with their IP addresses), disconnect players, ban IP addresses anytime etc.
- You have full control over what cloud values can be set: For example, you can allow non-numeric cloud values (values containing letters) and set a custom length limit for cloud values
- You can freely manipulate the behavior of the cloud server
- You can set and get cloud variables server-side any time
- You have full control over when your cloud server is running

## Getting started

To host a cloud server locally, simply run:

```py
server = sa.init_cloud_server()
server.start()
```

By default, your server will be running on `127.0.0.1:8080`. To use it on TurboWarp, add the `?cloud_host=ws://127.0.0.1:8080` parameter to the URL.

**Warning:** The server is only available locally on your computer. To make it possible for others to join it, you need to host it somewhere. More info on how to do this is coming soon.

## Features

**Customize your server:**

```py
server = sa.init_cloud_server(
    '127.0.0.1', 8080, # set IP address and port
    thread=True, # if set to True, the server will run in a thread
    length_limit=None, allow_non_numeric=True, # customize what cloud values are allowed
    whitelisted_projects=None, allow_nonscratch_names=True, blocked_ips=[],
    sync_players=True, # when set to False, other players will no longer be notified about cloud updates (only the server will see and parse them)
    log_var_sets=True # when set to True, all var sets will be printed to the console (can be spammy)
)
```
**Get the cloud variable data from the currently active projects:**
```py
data = server.active_projects()
```
**Get the users that are currently connected:**
```py
usernames = server.active_user_names(project_id) # gets users currently active on the specified project
user_ips = server.active_user_ips(project_id)
```
**Get cloud variables from your server:**
```py
variables = server.get_global_vars()
variables = server.get_project_vars(project_id)
value = server.get_var(project_id, var_name)
```
**Set cloud variables:** (all connected users will autom. receive the cloud variable update)
```py
server.set_global_vars({
    "project_id1":{"var1":"value1", "var2":"value", ... },
    ... 
})

server.set_project_vars("project_id", {"var1":"value1", "var2":"value", ... })
server.set_var("project_id", "variable_name", "value")
```

Also available:

```py
server.pause()
server.resume()
server.stop()
```

# Project JSON editing

**Load the project JSON of a project from the website:**
```py
pb = session.connect_project("project_id").project_body() # Returns a sa.ProjectBody object
pb = sa.get_project("project_id").project_body() # Returns a sa.ProjectBody object. Doesn't require a login / session
```
**Load the project JSON from a sb3 file:**
```py
pb = sa.read_sb3_file("path_to_file") # Returns a sa.ProjectBody object
```
**Load an empty project JSON:**
```py
pb = session.connect_empty_project_pb() # Returns a sa.ProjectBody object
pb = sa.get_empty_project_pb() # Returns a sa.ProjectBody object. Doesn't require a login / session
```

When using `session.connect_` methods for fetching the `sa.ProjectBody` object, assets you add using the `sa.ProjectBody.Sprite.add_asset` method will automatically be uploaded to the Scratch website.

## sa.ProjectBody class

Represents and saves a parsed project JSON. The components of the project are loaded into `sa.ProjectBody.Block`, `sa.ProjectBody.Sprite`, `sa.ProjectBody.Variable` etc. objects.

**Attributes:**
```py
pb.sprites # A list of the project sprites represented by sa.ProjectBody.Sprite objects
pb.monitors # A list of the project's variable and list monitors represented by sa.ProjectBody.Monitor objects
pb.extensions # A list saving the names of the project's extensions ("pen" etc.)
pb.meta # Metadata of the project
```

**[Methods and sub-classes of sa.ProjectBody](https://scratchattach.readthedocs.io/en/latest/scratchattach.html#module-scratchattach.other.project_json_capabilities)**

This section will be expanded soon

# Backpack

**Connect the assets in your backpack:**
```py
backpack = session.backpack(limit=20, offset=0) # Returns a list of sa.BackpackAsset objects
```
**Delete an asset from your backpack by id:**
```py
session.delete_from_backpack("backpack_asset_id")
```

## sa.BackpackAsset class

Represents an asset from the backpack. Located in `scratchattach.site.backpack_asset`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```py
bpasset.id # The asset id
bpasset.type # The asset type (custome, script, etc.)
bpasset.mime # The format in which the content of the backpack asset is saved
bpasset.name # The name of the backpack asset
bpasset.filename # Filename of the file containing the content of the backpack asset
bpasset.thumbnail_url # Link that leads to the asset's thumbnail (the image shown in the backpack UI)
bpasset.download_url # Link that leads to a file containg the content of the backpack asset
```
**Methods:**
```py
bpasset.download(dir="") # Downloads the asset content to the given directory
bpasset.delete() # Deletes the asset from the backpack
```

# Forum

**Connect a forum topic:**
```py
topic = session.connect_topic("topic_id") # Returns a sa.ForumTopic object
```
**Get a forum topic without logging in:** (methods that require authentication will not work on the returned object)
```py
topic = sa.get_topic("topic_id") # Returns a sa.ForumTopic object
```
**Connect a list of the topics in a category:**
```py
topic_list = session.connect_topic_list(category_id, page=0) # Returns a list of sa.ForumTopic objects
```
**Get a list of the topics in a category without logging in:**
```py
topic_list = sa.get_topic_list(category_id, page=0) # Returns a list of sa.ForumTopic objects
```
**Change your forum signature:**
```py
session.connect_linked_user().set_forum_signature("new signature text")
```

Getting a forum post by ID is no longer possible because ScratchDB is down indefinitely.

## sa.ForumTopic class

Represents a Scratch forum topic. Located in `scratchattach.site.forum`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```python
topic.id
topic.title
topic.category_name
topic.last_updated
# Attributes only available if the object was created using sa.get_topic_list or session.connect_topic_list:
topic.reply_count
topic.view_count
# ----- ----- #
topic.update()  #Updates the above data (except for topic.reply_count and topic.view_count)
```

**Methods:**
```python
topic.posts(page=1) #Returns the topic posts as list of sa.ForumPost objects (oldest post first). First page is at index 1.
topic.first_post() #Returns the first topic post as sa.ForumPost object
```

To prevent spam, adding posts to topics is not a scratchattach feature and never will be.

## sa.ForumPost class

Represents a Scratch forum post. Located in `scratchattach.site.forum`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```python
post.id
post.author_name
post.author_avatar_url
post.posted #The date the post was made
post.topic_id # The id of the topic this post is in
post.topic_name # The name of the topic the post is in
post.topic_category # The name of the category the post topic is in
post.topic_num_pages # The number of pages the post topic has
post.deleted # Whether the post was deleted (always False because deleted posts can't be retrieved anymore)
post.html_content #Returns the content as HTML
post.content #Returns the content as text
post.post_index # The index that the post has in the topic
# ----- ----- #
post.update()  #Updates the above data
```

**Methods:**
```python
post.topic() #Returns the topic the post is in as sa.ForumTopic object
post.author() #Returns the post author as sa.User object

post.edit("new content") #Requires you to be the post author.
post.ocular_reactions()
```

# Classrooms

**Connect a classroom by its id:**
```py
classroom = session.connect_classroom("classroom_id") # Returns a sa.Classroom object
```

**Connect a classroom by its token:**
```py
classroom = session.connect_classroom_from_token("classroom_token") # Returns a sa.Classroom object
```

**Get a classroom without logging in:**
```py
classroom = sa.get_classroom("classroom_id") # Returns a sa.Classroom object
classroom = sa.get_classroom_from_token("classroom_token") # Returns a sa.Classroom object
```

Only classrooms that have not been ended can be fetched.

## sa.Classroom class

Represents a Scratch classroom. Located in `scratchattach.site.classroom`. Inherited from the base class `sa.BaseSiteComponent`.

**Attributes:**
```python
classroom.id
classroom.title
classroom.about_class # classroom description "about" section
classroom.working_on # classroom description "working on" section
classroom.datetime # when the classroom was created. datetime.datetime object
classroom.author # the user who created the classroom as sa.User object. Some attributes of this object are not available immediately - to load them, use classroom.author.update()
# ----- ----- #
classroom.update()  #Updates the above data
```

**Methods:**
```python
classroom.student_count()
classroom.student_names() # Returns a list with the usernames of the students
classroom.class_studio_count()
classroom.class_studio_ids() # Returns a list with the ids of the studios
```

# Other APIs

```python
session.become_scratcher_invite() #If you are a new Scratcher and have been invited for becoming a Scratcher, this method will return more info on the invite
session.set_country("Antarctica") #Changes the account's country
session.resend_email("password") #Resends the verification email. Requires providing your password.
session.logout() #Destroys the session saved by this Session object. Warning: The session is not entirely gone and can still be used to do some things, like setting cloud variables.

sa.get_health() #Returns Scratch's health data
sa.get_total_project_count() #May not work sometimes (timeout error)
sa.check_username("username") #Checks if the username is available and allowed for a new account
sa.check_password("password") #Checks if the password is eligible for being set as Scratch account password

sa.get_csrf_token() #Returns a scratchcsrftoken which is fetched from Scratch's API
sa.get_resource_urls() #New in v2.1.6
sa.scratch_team_members() #New in v2.1.6

sa.youtube_link_to_scratch("youtube_url") #Converts a YouTube url (in multiple formats) to a link like https://scratch.mit.edu/discuss/youtube/1JTgg4WVAX8

sa.aprilfools_get_counter() #See: https://en.scratch-wiki.info/wiki/April_Fools%27_Day#Recurring_Pranks
sa.aprilfools_increment_counter()

sa.translate(sa.Languages.Hungarian, "hello world") #Translates the 2nd argument to the language provided in the 1st argument, using the API of Scratch's translate blocks
sa.text2speech("text", "female", "en-US") #Returns an audio file in which the given text is spoken. The 2nd argument provides the speaker's voice type, the 3rd argument provides the language. Uses the API of Scratch's TTS blocks.
```

To prevent account stealing, changing password / email is not a scratchattach feature and never will be.

# Advanced

## Proxies

```py
from scratchattach.utils.requests import Requests
Requests.proxies = {} # set this to a dict that contains your proxies
```

Your proxies will be applies to all HTTP requests performed by scratchattach (except for websocket connects)

## Fully customized clouds

**Option 1: sa.CustomCloud class**

```py
cloud = sa.CustomCloud(
    project_id = "project_id", cloud_host = "wss://...", username = "username", length_limit = 10000, allow_non_numeric = False, _session = None, header = None, cookie = None, origin = None, print_connect_messages = False
)
```

All keyword arguments (kwargs) provided to the constructor will be set as attributes of the CustomCloud object

**Option 2: Create your own class inheriting from sa.BaseCloud**

This makes it possible to override methods. For example, you can define your own .set_var method:

```py
class MyCloud(sa.BaseCloud):

    def __init__(self, *, project_id, cloud_host, ... ): # you can add more arguments
        super().__init__()
        
        # Attributes that must be set in every class inheriting from sa.BaseCloud:
        self.project_id = project_id
        self.cloud_host = cloud_host

        # Set other attributes, like self.allow_non_numeric or self.length_limit (click the link below for full list)

    def set_var(self, variable, value):

        # Insert your custom code for the set_var function here

        super().set_var(variable, value)
```

[Attributes you can set in the constructor of your class](https://scratchattach.readthedocs.io/en/latest/scratchattach.html#scratchattach.cloud._base.BaseCloud)

Initialize an object of the new class:

```py
cloud = MyCloud(project_id="project_id", cloud_host="wss://...", ...)
```

The methods that can be used on `sa.ScratchCloud` and `sa.TwCloud` objects (like `.set_var` or `.get_var`) can also be used on `sa.CustomCloud` objects and all other objects that inherit from the `sa.BaseCloud` class.

## Note regarding iterative APIs (like `method(limit=40, offset=0)`)

In v2.0, you can set the `limit` argument to any value. scratchattach will automatically do the API queries required for getting the requested amount of objects. For big `limit` parameter values (>400), this will take some time tho.