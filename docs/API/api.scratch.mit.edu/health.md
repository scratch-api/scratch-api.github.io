# GET [`/health`](https://api.scratch.mit.edu/health)

This serves information about the uptime and status of some of scratch's servers.

!!! Note

    This does **NOT** include the cloud server health!

Example response:

```json
{
  "version": "f496266dda6a084450996c08cd7c2ba47055ec34",
  "uptime": 1109827.35,
  "load": [0.51, 0.28, 0.21],
  "sql": {
    "main": {
      "primary": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 40,
        "numUsed": 0,
        "numFree": 2,
        "pendingAcquires": 0,
        "pendingCreates": 0
      },
      "replica": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 40,
        "numUsed": 1,
        "numFree": 13,
        "pendingAcquires": 0,
        "pendingCreates": 0
      }
    },
    "project_comments": {
      "primary": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 4,
        "numUsed": 0,
        "numFree": 1,
        "pendingAcquires": 0,
        "pendingCreates": 0
      },
      "replica": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 4,
        "numUsed": 0,
        "numFree": 2,
        "pendingAcquires": 0,
        "pendingCreates": 0
      }
    },
    "gallery_comments": {
      "primary": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 4,
        "numUsed": 0,
        "numFree": 1,
        "pendingAcquires": 0,
        "pendingCreates": 0
      },
      "replica": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 4,
        "numUsed": 0,
        "numFree": 1,
        "pendingAcquires": 0,
        "pendingCreates": 0
      }
    },
    "userprofile_comments": {
      "primary": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 4,
        "numUsed": 0,
        "numFree": 0,
        "pendingAcquires": 0,
        "pendingCreates": 0
      },
      "replica": {
        "ssl": true,
        "destroyed": false,
        "min": 0,
        "max": 4,
        "numUsed": 0,
        "numFree": 0,
        "pendingAcquires": 0,
        "pendingCreates": 0
      }
    }
  },
  "timestamp": 1765026555788,
  "cache": {
    "connected": true,
    "ready": true
  }
}
```