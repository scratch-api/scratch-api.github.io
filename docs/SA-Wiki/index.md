# Home

**Scratch API wrapper with support for almost all site features.** Created by [TimMcCool](https://scratch.mit.edu/users/TimMcCool/).

This library can set cloud variables, follow Scratchers, post comments and do so much more! It has special features that make it easy to transmit data through cloud variables.

<p align="left">
  <img width="120" src="https://raw.githubusercontent.com/TimMcCool/scratchattach/refs/heads/main/logos/logo.svg">
</p>

[![PyPI status](https://img.shields.io/pypi/status/scratchattach.svg)](https://pypi.python.org/pypi/scratchattach/)
[![PyPI download month](https://img.shields.io/pypi/dm/scratchattach.svg)](https://pypi.python.org/pypi/scratchattach/)
[![PyPI version shields.io](https://img.shields.io/pypi/v/scratchattach.svg)](https://pypi.python.org/pypi/scratchattach/)
[![GitHub license](https://badgen.net/github/license/TimMcCool/scratchattach)](https://github.com/TimMcCool/scratchattach/blob/master/LICENSE)
[![Documentation Status](https://readthedocs.org/projects/scratchattach/badge/?version=latest)](https://scratchattach.readthedocs.io/en/latest/?badge=latest)

## It is *strongly* advised to learn Python before using scratchattach.

# Documentation

- **[Documentation](Documentation.md)**

- [Cloud Variables](Documentation.md#cloud-variables)
- [Cloud Requests](Cloud-Requests.md)
- [Cloud Storage](Cloud-Storage.md)
- [Filterbot](Filterbot.md)
- [Self-hosting a TW cloud websocket](Documentation.md#hosting-a-cloud-server)

# Helpful resources

- [Get your session id](Get-your-session-id.md)

- [Examples](Examples.md)
- [Hosting](Hosting.md)

- [Migrating to v2](Migrating-to-v2.md)

Report bugs by opening an issue on this repository. If you need help or guideance, leave a comment in the [official forum topic](https://scratch.mit.edu/discuss/topic/603418/
). Projects made using scratchattach can be added to [this Scratch studio](https://scratch.mit.edu/studios/31478892/).

# Helpful for contributors

- **[Structure of the library](Structure-of-the-library.md)**

- [Extended documentation (WIP)](https://scratchattach.readthedocs.io/en/latest/)

- [Change log](https://github.com/TimMcCool/scratchattach/blob/main/CHANGELOG.md)

Contribute code by opening a pull request on this repository.

# ️Example usage

Set a cloud variable:

```py
import scratchattach as sa

session = sa.login("username", "password")
cloud = session.connect_cloud("project_id")

cloud.set_var("variable", value)
```

**[More examples](Examples.md)**

# Getting started

**Installation:**

Run the following command in your command prompt / shell:

```
pip install -U scratchattach
```

If this doesn't work, try running:
```
python -m pip install -U scratchattach
```


**Logging in with username / password:**

```python
import scratchattach as sa

session = sa.login("username", "password")
```

`login()` returns a `Session` object that saves your login and can be used to connect objects like users, projects, clouds etc.

**Logging in with a sessionId:** *You can get your session id from your browser's cookies. [More information](Get-your-session-id.md)*
```python
import scratchattach as sa

session = sa.login_by_id("sessionId", username="username") #The username field is case sensitive
```

**Cloud variables:**

```py
cloud = session.connect_cloud("project_id") # connect to the cloud

value = cloud.get_var("variable")
cloud.set_var("variable", "value") # the variable name is specified without the cloud emoji
```

**Cloud events:**

```py
cloud = session.connect_cloud('project_id')
events = cloud.events()

@events.event
def on_set(activity):
    print("variable", activity.var, "was set to", activity.value)
events.start()
```

**Follow users, love their projects and comment:**

```py
user = session.connect_user('username')
user.follow()

project = user.projects()[0]
project.love()
project.post_comment('Great project!')
```

**All scratchattach features are documented in the [documentation](Documentation.md).**
