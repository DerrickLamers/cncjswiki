# cncjs Controller API
The cncjs server defines an API for sending and receiving events over websockets to a CNC controller. Clients of this API may wish to make use of the client library [cncjs-controller](https://github.com/cncjs/cncjs-controller).

## Description
websockets, send events

## Controller events emitted
The CNC controller running on the server emits these events. One can use the cncjs-controller#addListener() to register callbacks for events.

|event|data|
|---|---|
|startup|{loadedControllers, baudrates, ports}|
|config:change|config|
|task:start|taskId|
|task:finish|(taskId, code)|
|task:error|(taskId, err)|
|serialport:list|ports|
|serialport:change|{port, inuse=true}|
|serialport:open|{port, baudrate, controllerType, inuse=true}|
|serialport:close|{port, inuse=false}|
|serialport:error|{err, port}|
|serialport:read|serial output|
|serialport:write|(data, {...context, source})|
|gcode:load|(name, gcode, context)|
|gcode:unload|none|
|feeder:status|{hold, holdReason, queue, pending, changed}|
|sender:status|{sp, hold, holdReason, name, context, size, total, sent, received, startTime, finishTime, elapsedTime, remaniningTime}|
|workflow:state|workflow.state|
|controller:settings|('Grbl', {version, parameters, settings)|
|controller:state|'Grbl', {state, parserstate}|
|message||

## Controller events handled
The CNC controller listens for and handles these events. The cncjs-controller has functions that send these events to the server.
|Event|Controller function|
|---|---|
|open|openPort(port, options, callback)|
|close|closePort(port, callback)|
|list|listPorts(callback)|
|command|command(cmd, ...args)|
|write|write(port, data, context)|
|writeln|writeln(port data, context)|

## Commands
The 'command' event can be used to send the following commands to the controller.
|Command String|Parameters|Description|
|---|---|---|
|gcode:load|name, gcode, context, callback|load G-code|
|gcode:unload|none|unload G-code|
|gcode:start|none|start sending G-code|
|gcode:stop|{ force: true }|stop sending G-code|
|gcode:pause|none|pause|
|gcode:resume|none|resume|
|feeder:feed||
|feeder:start||
|feeder:stop||
|feeder:clear||
|feedhold|feed hold|
|cyclestart|||
|statusreport|||
|homing||start a homing cycle|
|sleep||enter sleep|
|unlock||unlock|
|reset||reset|
|feedOverride|||
|spindleOverride|||
|rapidOverride|||
|energizeMotors:on|||
|energizeMotors:off|||
|gcode| gcode, context)||
|macro:load|'\<macro-id\>', context, callback)||
|macro:run|'\<macro-id\>', context, callback)||
|watchdir:load|'/path/to/file', callback)||


### Socket.io events
These events occur in the life cycle of the webocket connection. Full documentation at [socket.io](https://socket.io/docs/client-api)
|Event|Data|
|---|---|
|connect|none|
|connect_error|error|
|connect_timeout|none|
|error|error|
|disconnect|reason|
|reconnect|attempt|
|reconnect_attempt|attempt|
|reconnecting|attempt|
|reconnect_error|error|
|reconnect_failed||
