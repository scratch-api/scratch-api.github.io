# Base classes

Scratchattach consists out of three base classes: `BaseEventHandler`, `BaseSiteComponent` and `BaseCloud`. All classes are grouped in folders based on which base class they are built upon.

> [!NOTE]
> The scratchattach.editor module uses its own base classes and is structured separately to the rest of scratchattach. This is possibly subject to change.

## BaseEventHandler:

`CloudEvents` is built upon / inheriting from `BaseEventHandler`, `CloudRequests` is built upon `CloudEvents`, and `CloudStorage` is built upon `CloudRequests`.
Also `MessageEvents` is built upon `BaseEventHandler`, and `Filterbot` is build upon `MessageEvents`.
This reduces boilerplate code a lot compared to v1.

## BaseSiteComponent:

All site components (Session, User, Project, Activity, CloudActivity etc.) inherit from this class. They all have a `._session` atribute saving the Session connected with the object (or `None` if there is no session connected)
BaseSiteComponent provides an update method that calls the corresponding JSON API to get updated information about the represented site component and then calls the _update_from_dictmethod, providing the API response as argument (This method is defined as abstract method in the `BaseSiteComponent` class. In the classes inheriting from `BaseSiteComponent`, it is overridden with a class-specific method for parsing the API response.)

Objects of a class `C` inheriting from `BaseSiteComponent` are always initialized this way (`object_id` is the ID that identificates the object in the API (like the project or studio id) and session is the `sa.Session` object that should be connected with the object):

```py
object = C(id=object_id, _session=session)
object.update()
```

To reduce boilerplate code, this generalized process is implemented in the `_get_object` function in _util _> ._commons.py_.
In _commons.py_, there's also a generalized method for parsing a list with dicts representing users, studios etc. to a list of objects inheriting these users, studios etc..

## BaseCloud:

A generalized base class for representing the cloud of any cloud variable server. ScratchCloud and TwCloud inherit from this class and are optimized for using Scratch / TurboWarp cloud variables. There's also CustomCloud (also inheriting from BaseCloud) which allows setting all attributes yourself in the constructor.
If you need even more customizability (like defining your own set_var function etc) you can create your own class inheriting from BaseCloud (there's more info about this in the docstring of BaseCloud).

# scratchattach.get_xyz and Session.connect_xyz

In v2.0, "get_" always means that there's no Session connected to the returned object and that the returned object therefore can't be used for performing operations (like .love(), .follow()) that require authentication.
"connect_" always means that the Session will be connected to the returned object / saved in the object's ._session attribute.

This is used consistently throughout the whole library: scratchattach.get_cloud vs. Session.connect_cloud, scratchattach.get_user vs. Session.connect_user, scratchattach.get_project vs. Session.connect_project
