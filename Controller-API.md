# cncjs Controller API
The cncjs server defines an API for sending and receiving events from a CNC controller over websockets. Clients of this API may make use of [cncjs-controller](https://github.com/cncjs/cncjs-controller).

## Description
websockets, send events

## Event handlers
### Controller Events
|event|data|
|---|---|
|startup|{loadedControllers, baudrates, ports}|
|config:change|{config}|
|task:start|taskId|
|task:finish|(taskId, code)|
|task:error|(taskId, err)|
|serialport:list|(ports)|
|serialport:change|{port, inuse=true}|
|serialport:open|{port, baudrate, controllerType, inuse=true}|
|serialport:close|{port, inuse=false}|
|serialport:error|{err, port}|
|serialport:read|(serial output)|
|serialport:write|(data, {...context, source})|
|gcode:load|(name, gcode, context|
|gcode:unload|none|
|feeder:status|{hold, holdReason, queue, pending, changed}|
|sender:status|{sp, hold, holdReason, name, context, size, total, sent, received, startTime, finishTime, elapsedTime, remaniningTime}|
|workflow:state|workflow.state|
|controller:settings|('Grbl', {version, parameters, settings)|
|controller:state|'Grbl', {state, parserstate}|
|message||

### Socket.io events
connect
connect_error
connect_timeout
error
disconnect
reconnect
reconnect_attempt
reconnecting
reconnect_error
reconnect_failed

## Functions
- openPort(port, options, callback)
- closePort(port, callback)
- listPorts(callback)
- command(cmd, ...args)
- write(data, context)
- writeln(data, context)

## Commands

