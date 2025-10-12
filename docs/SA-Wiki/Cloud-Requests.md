Cloud Requests Framework (inspired by di*****.py) that allows Scratch projects and Python to interact

New in v2.0: Cloud Requests now use threading.Event which massively lowers CPU usage.

# Basic usage

**Add Cloud Requests to your Scratch project:**

Download this project file to your computer (click the link to download it): <https://github.com/TimMcCool/scratchattach/raw/main/assets/CloudRequests_Template.sb3>

Then, go to the Scratch website, create a new project and upload the project file from above.

**How to use with Scratch:**

Copy this code to your Python editor. [How to get your session id](Get-your-session-id.md)

```py
import scratchattach as sa

session = sa.login_by_id("session_id", username="username") #replace with your session_id and username
cloud = session.connect_cloud("project_id") #replace with your project id
client = cloud.requests()

@client.request
def ping(): #called when client receives request
    print("Ping request received")
    return "pong" #sends back 'pong' to the Scratch project

@client.event
def on_ready():
    print("Request handler is running")

client.start(thread=True) # thread=True is an optional argument. It makes the cloud requests handler run in a thread
```

Replace "session_id" and "username" with your data and "project_id" with the id of the project you created on Scratch.
Then, run the code.

Now go to the Scratch project. In the `Cloud Requests` sprite, you will find this block:

![image](https://raw.githubusercontent.com/TimMcCool/scratchattach/refs/heads/main/wiki/images/cr_tut_block.png)

When active, it sends a "ping" request to the Python client. This will call the `ping()` function. The data returned by the function will be sent back to the project.
Try it out by clicking the block!

![image](https://raw.githubusercontent.com/TimMcCool/scratchattach/refs/heads/main/wiki/images/cr_tut_restult.png)

**How to use with TurboWarp:**

```python
import scratchattach as sa

cloud = sa.get_tw_cloud("project_id") #replace with your project id
client = cloud.requests()

...
```

# Examples

**Example 1: Script that loads your message count**

Scratch code:

![image](https://raw.githubusercontent.com/TimMcCool/scratchattach/refs/heads/main/wiki/images/cr_tut_example1.png)

```python
@client.request
def message_count(argument1):
    print(f"Message count requested for user {argument1}")
    user = sa.get_user(argument1)
    return user.message_count()
```

The arguments you specify in the Scratch code are given to the Python function.

# Basic features

- TCP-like packet loss prevention (new in v2.0)
- **No length limitation** for the request or the returned data! (If it is too long for one cloud variable, it will be split into multiple cloud variables)
- Cloud Requests can handle **multiple requests sent at the same time**
- Requests can also **return lists,** these will be decoded as list in the Scratch project
- You can freely choose the name of your requests
- Requests that only return numbers won't be encoded (-> 50% faster!)

# Advanced features

**Send data to the Scratch project** (new in v2.0)

It's now possible to send data to the Scratch project anytime without a priorly received request:

```py
client.send("message to send")
```

**Get the request metadata:**

In your requests, you can use these functions (since v2.0, they also work for requests running in threads):
```py
client.get_requester() #Returns the name of the user who sent the request
client.get_timestamp() #Returns the timestamp of when the request was sent (in milliseconds since 1970)
client.get_exact_timestamp() #Returns the exact timestamp of when the request was sent (fetches it from the clouddata logs). New in v1.2.6
```

**Change the used cloud variables:**
```py
client = cloud.requests(used_cloud_vars=["1", "2", "3", "4", "5", "6", "7", "8", "9"])
```
**Enable no-packet-loss:**

No packet loss will make cloud requests reconnect before every sent back request. This eliminates packet loss.
```py
client = cloud.requests(no_packet_loss=True)
```
**Send more than two arguments:**

The seperator used to join the different arguments is "&". To send more than three arguments from Scratch, join them using "&".

# Advanced requests
(new in v1.0.0, updated in v2.0.0)

**Add arguments to the decorator:**

Above requests, you put the decorator `@client.request`.
You can use this decorator to customize your requests!

*Modify the response priority*
It is possible to change the way sending back request responses is being prioritised. There are three options to choose from:

1. _Respond in receive order (default):_ Requests received first will be sent back to the Scratch project first

```py
client = cloud.requests(respond_order="receive")
```

2. _Respond in finish order:_ Requests that finished first will be sent back to the Scratch project first

```py
client = cloud.requests(respond_order="finish")
```

3. _Respond based on individual priority scores:_ The lower the request's priorit score, the sooner it will be sent back to the Scratch project

```py
client = cloud.requests(respond_order="priority")
```
Use this code to assign priorities to the individual requests:
```py
@client.request(response_priority=1)
```

*Disable running request in thread*
By default, requests are ran in threads in v2.0. However, you may want to disable this if your client is getting lots of requests that can be finished within less than 0.1 seconds.

```py
@client.request(thread=False)
```

*Disable request:*
Put this decorator above a request to disable it:
```py
@client.request(enabled=False)
```

*Overwrite the request name:*
Put this decorator above a request to overwrite its name with something else:
```py
@client.request(name="new_name")
```

**Manually add, edit and remove requests:**

*Add requests:*
To add requests, you can also use this method:

```py
client.add_request(function, thread=False, enabled=True, name="request_name")
```

*Edit / Remove requests:*
```py
client.edit_request("request_name", thread=False, enabled=True, new_name="request_name", new_function=new_function) #The keyword arguments are optional and can be removed if they are not needed
client.remove_request("request_name")
```

# Advanced events

Events will be called when specific things happen.
There are more events than just `on_ready`!

*Called after the client connected to the cloud:*
```py
@client.event
def on_ready():
    print("Request handler is ready")
```

*Called every time the client receives any request:*
```py
@client.event
def on_request(request):
    print("Received request", request.name, request.requester, request.arguments, request.timestamp, request.request_id)
```

*Called when the client receives an unknown / undefined request:*
```py
@client.event
def on_unknown_request(request):
    print("Received unknown request", request.name, request.requester, request.arguments, request.timestamp, request.request_id)
```

*Called when an error occurs in a request:*
```py
@client.event
def on_error(request, e):
    print("Request: ", request.name, request.requester, request.arguments, request.timestamp, request.request_id)
    print("Error that occured: ", e)
```

*Called when the client receives a disabled request:*
```py
@client.event
def on_disabled_request(request):
    print("Received disabled request", request.name, request.requester, request.arguments, request.timestamp, request.request_id)
```
