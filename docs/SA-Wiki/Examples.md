# Example scripts

**Set a cloud var with scratchattach:**
```py
# Example: Set Cloud Variable

import scratchattach as sa# Import the scratchattach module

session = sa.login("username", "password") # Login with Scratch login information (replace with your username and password)
cloud = session.connect_cloud("project_id") # Replace project_id with your project ID

cloud.set_var("variable", value) # Set a cloud variable to a certain value (int, float or string consisting of numbers). The variable name is specified without the cloud emoji
```

**Love a Scratch project:**
```py
import scratchattach as sa
session = sa.login("username", "password") # Login with Scratch login information (replace with your username and password)
project = session.connect_project("project_id") # Replace project_id with your project ID
project.love()
```

**Cloud event handler:**
```py
import scratchattach as sa # Import the scratchattach module
events = sa.get_cloud("project_id").events() # Replace project_id with your project id. No login required.

@events.event
def on_set(activity): #Called when a cloud var is set
    # Things inside here only happen if a cloud variable is set. 
    print(f"{activity.user} set the variable {activity.var} to the value {activity.value} at {activity.timestamp}")

events.start() # Start listening for events. 
```

**Comment on a project:**
```py
import scratchattach as sa # Import the scratchattach module

session = sa.login("username", "password") # Log in with Scratch login information (replace with your username and password)
project = session.connect_project("project_id") # Replace project_id with your project id

comment = project.post_comment("Hi guys!") # Post a comment on the project and save the posted comment in a variable
```

**Follow a user:**
```py
import scratchattach as sa
session = sa.login("username", "password") # Login with Scratch login information (replace with your username and password)
user = session.connect_user("user_to_follow")
user.follow()
```

**Automatically update your profile with your follower count:**
```py
import scratchattach as sa # Import the scratchattach module
import time # Import time module.

session = sa.login("username", "password") # Log in with Scratch login information (replace with your username and password).
user = session.connect_linked_user() # Get the your Scratch profile as User object.

while True:
    follower_count = user.follower_count() # Get follower count
    user.set_bio(f"My follower count: {follower_count}") # Updates "What I'm working on" section on your profile.
    time.sleep(60) # Waits 60 seconds before updating the section again. 
```

# Example projects

Warning: These examples still use scratchattach v1 and are therefore outdated

[Check out this repo: https://github.com/mas6y6/scratchattach-examples](https://github.com/mas6y6/scratchattach-examples)

[Also: https://github.com/programORdie2/servers24-7](https://github.com/programORdie2/servers24-7)

More coming soon 
