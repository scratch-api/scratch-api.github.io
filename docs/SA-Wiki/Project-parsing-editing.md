Scratchattach provides a submodule `editor` for project parsing/editing. It is useful for extracting data from projects, or performing repetitive operations.

> [!NOTE]
> Scratchattach.editor is work-in-progress. If there is a feature missing or a strange bug, [please add an issue!](https://github.com/TimMcCool/scratchattach/issues)

# Basic usage

### import scratchattach.editor:
```py
from scratchattach import editor
```

### Load a project
```py
# from a sb3 filepath:
proj = editor.Project.from_sb3("test proj.sb3")

# from a binary file
with open("test proj.sb3", "rb") as f: 
    proj = editor.Project.from_sb3(f)

# from a bytes
with open("test proj.sb3", "rb") as f:
    sb3_bytes = f.read()
proj = editor.Project.from_sb3(sb3_bytes)

# from a project id
proj = editor.Project.from_id(104)  # Project<name=Weekend, meta=Meta<3.0.0 : 0.1.0 : scratchattach.editor by https://scratch.mit.edu/users/timmccool/>>
```

### View project attributes
```py
# print project name, if applicable (inferred from project title, or file name)
print(proj.name)  # test proj

# print project assets (costumes and sounds in all sprites)
for asset in proj.assets:
    print(asset)

# View turbowarp configuration
print(proj.tw_config)

# Get comment instance storing twconfig
print(proj.tw_config_comment)

# Get a variable, list or broadcast (including for-this-sprite-only vars)
print(proj.find_vlb("my variable"))

# Get all variables/lists/broadcasts with name
print(proj.find_vlb("my variable", multiple=True))  # if you have a variable and list called 'my variable' this will return a list including both

# print project monitors (variable/list displays)
print(proj.monitors)

# print project metadata
# prints in format {semver} : {vm} : {agent} : {Optional[Platform metadata (used by turbowarp)]}
print(proj.meta)  # Meta<3.0.0 : 0.1.0 : scratchattach.editor by https://scratch.mit.edu/users/timmccool/: PlatformMeta(name='TurboWarp', url='https://turbowarp.org/')>

# view extensions that are used by the project (this may not work with old scratch projects)
print(proj.extensions)  # [Extension(code='pen', name='Pen Extension')]

# SUBJECT TO CHANGE: Add a monitor to the project
vlb = proj.find_vlb('var')
m = editor.Monitor(vlb, visible=True, params={
    "VARIABLE": vlb.name
})

proj.add_monitor(m)
```

> [!WARNING]
> Loading scripts from the backpack requires a special function: editor.load_script_from_backpack, because the backpack uses a 
> slightly different block syntax 