Scratchattach v2 is not backwards compatible. But upgrading is worth it, because it comes with many new features.

This page lists the main non-backwards-compatible changes to make it easier for you to upgrade. If you're running into unexpected errors, check the documentation to find out how the specific feature works in scratchattach v2.

1. Generally, `scratchattach` is now imported as / referenced as `sa` (no longer as `scratch3`) in the docs.
2. `session = scratch3.Session("session_id")` was changed to `session = sa.login_by_id("session_id")`
3. `events = scratch3.CloudEvents("project_id")` was changed to `events = sa.get_cloud("project_id").events()`
4. `client = scratch3.CloudRequests(conn)` was changed to `client = session.connect_cloud("project_id").requests()`
5. `value = scratch3.get_var("project_id", "variable")` was changed to
```py
cloud = sa.get_cloud("project_id") # This function is ran once
value = cloud.get_var("variable") # This function can safely be called in a loop without spamming an API
```
The same applies to getting TurboWarp cloud variables.

6. All comments are now represented by `sa.Comment` objects
7. All activites and messages are now represented by `sa.Activity` objects
8. Projects and studios are now always represented by `sa.Project` and `sa.Studio` objects, never as dicts
9. project.get_comment was changed to project.comment_by_id
10. Generally, many functions that used to start with ".get_" had the "get_" removed from their names.
11. If an attribute refers to a username, it is now always ".username" and not ".user"
12. `CloudConnection` and `TwCloudConnection` don't exist anymore. Instead, there are now `Cloud` and `TwCloud` objects that not only handle setting cloud variables, but also have built in functions for getting variables very frequently without spamming the API (new CloudRecorder infrastracture).
13. `TwCloudEvents` and `WsCloudEvents` were merged into `CloudEvents`. The formerly called `CloudEvents` are now `CloudLogEvents`
14. `TwCloudRequests` and `CloudRequests` were merged into `CloudRequests`
15. All events are now always ran in threads by default. You can disable this using `@events.event(thread=False)`
16. Cloud Request Events don't use the `id` attribute anymore. Instead, they use the `request_id` attribute.