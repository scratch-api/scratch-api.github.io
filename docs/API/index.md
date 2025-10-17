# API

API stands for application-programmer-interface. Essentially what this means is a way to integrate your code
with scratch.

Scratch itself provides a number of urls which can be used for interaction via web requests, shown [below](#web-api).

!!! info

    It is recommended to use an [existing library](existing-libraries.md) to interact with scratch instead of making your own

## Organisation

This section is organised in a similar way to [scratchattach](https://github.com/TimMcCool/scratchattach/)

todo list:

- [ ] Cloud protocol
- [ ] Other apis
- [ ] Site
  - [ ] Activity
  - [ ] (Educator) Alert
  - [ ] Backpack Assets
  - [ ] Classrooms
  - [ ] Comments
  - [ ] Forums
  - [ ] TW Placeholder
  - [ ] Project
  - [ ] Session
  - [ ] Studio
  - [ ] User

## Web API

In order to make requests to scratch, there are a multitude of different urls where you can make
`GET`, `POST`, `PUT`, `DELETE` and other types of request.

These urls can be generally categorized by their subdomain and/or path.

!!! warning

    This is not a complete list.

### Scratch

| URL                                        | Purpose                                                                                                                         |
|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| https://scratch.mit.edu/                   | The main website                                                                                                                |
| https://api.scratch.mit.edu/               | The main API for public use, documented [on the scratch wiki](https://en.scratch-wiki.info/wiki/API)                            |
| https://assets.scratch.mit.edu/            | Interaction with assets (costumes and sounds)                                                                                   |
| https://backpack.scratch.mit.edu/          | Interaction with the backpack                                                                                                   |
| https://scratch.mit.edu/site-api/          | An API used internally by the scratch website. Provides access for the same things, but less restrictive, e.g. classroom alerts |
| wss://clouddata.scratch.mit.edu/           | This is where the scratch cloud server websocket lives                                                                          |
| https://projects.scratch.mit.edu           | Downloading projects                                                                                                            |
| https://uploads.scratch.mit.edu            | Thumbnails/profile images                                                                                                       |
| https://translate-service.scratch.mit.edu/ | Translation requests - which the translate extension uses                                                                       |
| https://cdn.scratch.mit.edu                | Content delivery network - hosts images, css, js, emojis, etc.                                                                  |
| https://synthesis-service.scratch.mit.edu  | Text2Speech requests - which the Text2Speech extension uses                                                                     |

### Ocular

| URL                                | Purpose                                       |
|------------------------------------|-----------------------------------------------|
| https://jeffalo.net/api/           | Jeffalo's API                                 |
| https://my-ocular.jeffalo.net/api/ | API for [ocular](https://ocular.jeffalo.net/) |

### TurboWarp

| URL                                   | Purpose                                                                                                                             |
|---------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| https://trampoline.turbowarp.org/api/ | Serves as a proxy for the scratch api                                                                                               |
| wss://clouddata.turbowarp.org         | This is where the turbowarp cloud server websocket lives                                                                            |
| https://share.turbowarp.org/          | Turbowarp placeholder. Has endpoints for uploading, downloading, and deleting projects. Note this is 'Not ready for mainstream use' |
