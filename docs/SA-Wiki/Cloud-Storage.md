Cloud Storage Framework that makes it easy to store project data by sending it over cloud variables

# Basic usage

**Add a Cloud Storage to your Scratch project:**

Download this project file to your computer (click the link to download it): <https://github.com/TimMcCool/scratchattach/raw/main/assets/CloudStorage_Template.sb3>

Then, go to the Scratch website, create a new project and upload the project file from above.

## Create a database in your Python code

A database is a simple key-value storage. It automatically saves the data to a JSON file on your local device. If the specified JSON file does not exist, scratchattach will create an empty one for storing the data, else it will load the existing data from the JSON file.

```py
import scratchattach as sa
from scratchattach import Database

db1 = Database("db_name", json_file_path="path/to/json_file.json", save_interval=10) # save_interval specifies the wait time between the automated saves to the JSON file

...
```

## Start the cloud storage

A cloud storage consists of at least one database.

```py
...

session = sa.login("username", "password")
cloud = session.connect_cloud("project_id")
storage = cloud.storage()

storage.add_database(db1)
# you can add more databases here

storage.start()
```

In your Scratch project, you will find blocks that you can use to get values from the cloud storage or set keys on the cloud storage. They work while the Python backend is running. As you've probably already noticed, cloud storages are based on cloud requests.

# Advanced

**Methods of `sa.Database`:**
```py
db.save_to_json() #saves the data to the JSON file
db.keys() #returns the keys of the key-value db
db.get(key) #gets a value from the db
db.set(key, value)
```

**Database events:**

Can be used to automatically call functions when specific conditions occur.

```py
@db.event
def on_save():
    print("The data was saved")

@db.event
def on_set(key, value):
    print("Key", key, "was set to value", value)
```

**Methods of `sa.CloudStorage`:**

```py
storage.get(db_name, key)
storage.set(db_name, key, value)
storage.keys(db_name)
storage.databases()
storage.database_names()
storage.add_database(database)
storage.get_database(db_name)
storage.save()
```