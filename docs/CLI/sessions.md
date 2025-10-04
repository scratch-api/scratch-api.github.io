# Sessions

`sc login` | `sc login --id`

In the scratch CLI, an account can be added by session ID, ot by username/password.
This will be registered as a session within the database. If the registered username already exists, it will be updated.

It will also create a new group with the username, if possible.
There will then be an option `(Y/n)` to add the session to the current group.
You can read more about groups [here](groups.md)
