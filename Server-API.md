# cncjs Server API

The cncjs server is an [Express web server](https://expressjs.com) running on [Node.js](https://nodejs.org). The server defines an HTTP API.

# Endpoints

## /api/signin
POST endpoint for making authentication requests.

This is the only endpoint in the API that allows public access.

Method: POST

Arguments:
- token: JWT token
- name: name
- password: password

Response
- HTTP 200,
  ```
  {
    enabled: true,
    token: <JWT>,
    name, <name>
  }
  ```
  Sent if the request succeeds by either JWT or name-password authentication. If no JWT is present in the request the name and password are assessed for authentication.

- HTTP 401,
  ```
  {
    msg: 'Authentication failed'
  }
  ```
  Sent if authentication fails.

- HTTP 500,
  ```
  {
    msg: 'Internal server error'
  }
  ```
  Sent if an error occurs while processing the JWT.

## Authorized-access-only endpoints
Most of the categories follow a (F)CRUD pattern: fetch, create, read, update, delete.

|Category|endpoint|HTTP method|operation|arguments|
|---|---|---|---|---|
|Version|api/version/latest|GET|get|none|
|State|api/state|GET|get|key|
||api/state|POST|set|key|
||api/state|delete|unset|key|
|G-code|api/gcode|GET|fetch|port|
||api/gcode|POST|upload|port, name, gcode, context|
||api/gcode/download|GET/POST|download|port|
|Controllers|api/controllers|GET|get|none|
|Commands|api/commands|GET|fetch|paging, page, pageLength|
||api/commands|POST|create|enabled, title, commands|
||api/commands/:id|GET|read|none|
||api/commands/:id|PUT|update|enable, title, commands|
||api/commands/:id|DELETE|delete|none|
||api/commands/run/:id|POST|run|none|
|Events|api/events|GET|fetch|paging, page, pageLength|
||api/events|POST|create|enabled, event, trigger, commands|
||api/events/:id|GET|read|none|
||api/events/:id|PUT|update|enabled, event, trigger, commands|
||api/events/:id|DELETE|delete|none|
|Machines|api/machines|GET|fetch|paging, page, pageLength|
||api/machines|POST|create|{id, name, limits: {xmin, xmax, ymin, ymax, zmin, zmax}}|
||api/machines/:id|GET|read|none|
||api/machines/:id|PUT|update|{name, limits: {xmin, xmax, ymin, ymax, zmin, zmax}}|
||api/machines/:id|DELETE|delete|none|
|Macros|api/macros|GET|fetch|paging, page, pageLength|
||api/macros|POST|create|name, content|
||api/macros/:id|GET|read|none|
||api/macros/:id|PUT|update|name, content|
||api/macros/:id|DELETE|delete|none|
|MDI|api/mdi|GET|fetch|paging, page, pageLength|
||api/mdi|POST|create|name, command, grid|
||api/mdi|PUT|bulkUpdate|Array({name, command, grid}, ...)
||api/mdi/:id|GET|read|none|
||api/mdi/:id|PUT|update|name, command, grid|
||api/mdi/:id|DELETE|delete|none|
|Users|api/users|GET|fetch|paging, page, pageLength|
||api/users|POST|create|enabled, name, password|
||api/users/:id|GET|read|none|
||api/users/:id|PUT|update|enabled, name, oldPassword, newPassword|
||api/users/:id|DELETE|delete|none|
|Watch|api/watch/files|GET/POST|getFiles|path|
||api/watch/file|GET/POST|readFile|file|