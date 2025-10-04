# Scratch CLI

!!! Warning

    This documentation currently does not apply. It is currently documenting a specification for the tool.

The [Scratch CLI](https://github.com/scratch-api/scratch-cli) is a command line client for scratch that wraps 
scratchattach.

## Why Scratch CLI?

Scratch CLI is built to be good for account management. It makes it easy to:

- Switch between accounts
- Perform actions with multiple accounts
- Manage certain groups of accounts

For more info about multi account management, go [here](sessions.md)

It's recommended to add an alias `sc` to Scratch CLI as will be used in this documentation. 
On linux, you can do this using `.bashrc`.

## Implementation details

- The CLI will use `sqlite` (subject to change?)
- Each command will be implemented in a separate file in the `cmd/` directory
- For `sessionable` commands, run the command for every session in the group.

## Scratchattach objects

format: `-{object-type} {identifying info} ({other info})`

- Users: `-U {username}`
- Projects `-P {id} (name)`
- Studios `-S {id} (name)`

You can view more information about a user/project/studio using `sc` and then their format, e.g. `sc -P 104`.
Doing this will also save the object's identifying information in the database, so that any subsequent command can relate to that project.
e.g. Loading `-P 104`, then doing `sc comment`, creating a prompt for a comment on the project with ID 104.

## Commands

If called without any params (`sc`), just display the help menu.

---

`sc messages -L [LIMIT] -O [OFFSET]`

Print out message activity with an offset & limit.

!!! Question
    
    consider adding message type filters

---

`sc find -U [USERNAME] -S [STUDIO_ID] -P [PROJECT_ID] [SEARCH_PARAM]`

If no search param is provided, just show the user/project/session page instead.
If no USERNAME is provided, default to the session username.

Options for search param:

- `lovefeed` - Projects loved by scratchers I'm following
- `loves` | `loved` - Loved projects (requires USERNAME)
- `faves` | `faved` | `favorited` | `favorites` - Favorited projects (requires USERNAME)
- `shared` - Shared projects (requires USERNAME)
