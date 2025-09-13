# Accounts

## Sessions

A session represents a single 'log in' for a scratch account.

The session object:
```json
{
  "username": "faretek1",
  "id": "..."
}
```

### `scratch login`

Login to a scratch account and add it to the current group. You will be prompted for a username and password

### `scratch login --sessid`

Login to a scratch account and add it to the current group. You will be prompted for a session ID.

## Account groups

The CLI allows you to configure account groups. These are collections of 1+ accounts which you can perform actions with.

The group object:
```json
{
  "name": "FA",
  "sessions": [
    {
      "username": "faretek1",
      "id": "..."
    },
    {
      "username": "faretek",
      "id": "..."
    }
  ]
}
```

These are stored in the cookies as values for an object, not a list. The key is the name but lowercase.

### `scratch ungroup`

This exits the current group and into the 'global' group. If you login now, it will make a new group with only 1 member.

### `scratch group`

Get the members of the group. 