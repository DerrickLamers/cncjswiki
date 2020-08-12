# PubSub API
The cncjs frontend webapp uses PubSubJS for communication between widgets. (It appears to be used in an ad hoc way). 

## PubSub publishers
|Component|Topics|
|---|---|
|Settings|updateMachineProfiles|
|Workspace|resize|
||gcode:load|
||updatePrimaryWidgets|
||updateSecondaryWidgets|
|App|message:connect|
||message:resize|
|Visualizer|gcode:bbox|
||gcode:unload|

## Pubsub subscribers
|Component|Topics|
|---|---|
|PrimaryWidgets|updatePrimaryWidgets|
|SecondaryWidgets|updateSecondaryWidgets|
|Console|resize|
|Custom|message:connect|
||message:resize|
|Gcode|gcode:bbox|
|SecondaryToolbar|updateMachineProfiles|
|Visualizer|resize|


