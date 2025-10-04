# SCLI_CONFIG

Scli config is a means of saving extra information for the scratchattach CLI for a user.
It is stored in a shared project on that user's profile.

The project ID for scli config is stored using base128 or something using ASCII chars and some emojis to use as few chars as possible.
It will be separated from everything else by a space or newline.

Example of a SCLI-Config compliant `About me` for [faretek1](https://scratch.mit.edu/users/faretek1/)
```html
That the true meaning of life is to expend every resource, every second, and every thought, so as to spend as much time on thy computer as possible.

1216591875 <!-- TODO: change this to the special encoded text --> <!-- The associated project: https://scratch.mit.edu/projects/1216591875/ -->
```

The actual scli config data will be stored in a comment inside the project.

the project must be named `_SCLI_CONFIG_` without no sprites - only the stage - with only 1 comment, 
which would be the scli config data, which must end in `_SCLI_CONFIG_`

The project must be owned by the associated user or be in the saved sessions list (can be outside the group).
